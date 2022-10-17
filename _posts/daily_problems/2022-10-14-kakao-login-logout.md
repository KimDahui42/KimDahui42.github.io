---
layout: post
title: "카카오 로그인/로그아웃"
date: 2022-10-14
excerpt: "소셜 로그인 생성을 위한 사전 학습(내가 보려고 필요한 부분을 카카오 문서에서 가져옴)"
tags: [study, REST API, 카카오]
category: [Daily Problem]
comments: false
---

[참고](https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api)

<aside>
👩‍💻 ***18.4월 [공지]  카카오 계정 정책 변경에 따른 공지***

카카오톡 7.2.0 버전부터 이메일이 없어도 카카오계정을 생성할 수 있습니다.즉, 이후부터는 서비스에 신규로 카카오계정 연결 시, 이메일이 없는 상태로 연결이 완료되는 경우가 존재할 수 있게 됩니다.

이에 따라 기존에 이메일을 필수로 설정한 서비스에서는 이메일이 없는 사용자의 신규 연결이 실패하게 됩니다.(이메일 소유자의 경우는 기존과 동일하게 동작합니다. 기존에 이미 연결된 사용자는 아무 문제 없이 서비스를 사용하실 수 있습니다. 또한 이메일이 존재하는 사용자의 경우도 서비스 신규 연결이 문제없이 가능합니다.)

서비스에서 이메일이 없는 상황에 대응이 되어 있다면, 이메일 항목을 선택으로 변경해주시고, 그 이후엔 이메일 없는 유저도 신규연결할 수 있습니다.

또한 앞으로 개발자사이트에서 신규 앱을 생성하거나, 기존 앱의 동의 항목을 변경할 때 이메일을 선택 동의로만 설정할 수 있으니 참고 부탁드립니다.

**이메일 사용시 주의사항은 [가이드 41](https://developers.kakao.com/docs/latest/ko/user-mgmt/common#properties)를 참고해주세요.**카카오계정에 사용자의 이메일이 없거나 사용자가 동의하지 않은 경우 서비스에서 자체적으로 이메일을 수집하시길 권장합니다.

카카오톡 7.2.0 버전은 5월 셋째주에 릴리즈 될 예정입니다.

</aside>

# 로그인

## Step 1. 인가 코드 받기

1. 서비스 서버가 카카오 인증 서버로 인가 코드 받기를 요청한다.

    ```jsx
    GET /oauth/authorize?client_id=${REST_API_KEY}&redirect_uri=${REDIRECT_URI}&response_type=code HTTP/1.1
    Host : kauth.kakao.com
    ```

    인가 코드는 동의 화면을 통해 인가받은 동의 항목 정보를 갖고 있으며, 인가 코드를 사용해 토큰 받기를 요청할 수 있다.

    - 사용자 동의 화면에서 동의→로그인/로그인 취소가 가능
        - 사용자 선택에 따라 요청 처리 결과를 담은 쿼리 스트링을 `redirect_uri` 로 `HTTP 302` 리다이렉트한다.
        - 동의 : `redirect_uri`로 인가 코드 담은 쿼리 스트링 전달 → `code`의 인가 코드 값으로 토큰 받기 요청
        - 비동의 : redirect_uri로 에러 정보를 담을 쿼리 스트링 전달 → 에러 원인별 상황에 맞는 처리

    ### Request

    | Name          | Type   | Description                                                                                           | Required |
    | ------------- | ------ | ----------------------------------------------------------------------------------------------------- | -------- |
    | client_id     | String | 앱 REST API 키                                                                                        | O        |
    | redirect_uri  | String | 인가 코드를 전달받을 서비스 서버의 URI                                                                | O        |
    | response_type | String | code로 고정                                                                                           | O        |
    | scope         | String | 추가 항목 동의 받기 요청 시 사용자에게 동의 요청할 동의 항목 ID 목록, 쉼표로 구분해 여러 개 전달 가능 | X        |
    | prompt        | String | 카카오톡에서 자동 로그인, 기존 로그인 여부와 상관없이 로그인 요청 시 사용                             |

    login : 기존 사용자 인증 여부와 상관없이 사용자에게 카카오계정 로그인 화면을 출력하여 다시 사용자 인증을 수행하고자 할 때 사용
    none : 사용자에게 동의 화면과 같은 대화형 UI를 노출하지 않고 인가 코드 발급을 요청할 때 사용, 인가 코드 발급을 위해 사용자의 동작이 필요한 경우 에러 응답 전달 | X |
    | service_terms | String | 약관 선택해 동의 받기 요청 시 사용 동의받을 약관 태그 목록, 쉼표로 구분된 문자열 값 목록 | X |
    | state | String | 로그인 과정 중 동일한 값을 유지하는 임의의 문자열, 각 사용자의 로그인 요청에 대한 state 값은 공유해야함 | X |
    | nonce | String | OpenID Connect를 통해 ID 토큰을 함께 발급받을 경우, ID 토큰 재생 공격을 방지하기 위해 사용 | X |

    ### Response

    | Name              | Type   | Description                         | Required |
    | ----------------- | ------ | ----------------------------------- | -------- |
    | code              | String | 토큰 받기 요청에 필요한 인가 코드   | X        |
    | state             | String | 요청 시 전달한 state 값과 동일한 값 | X        |
    | error             | String | 인증 실패 시 반환되는 에러 코드     | X        |
    | error_description | String | 인증 실패 시 반환되는 에러 메시지   | X        |

2. 카카오 인증 서버가 사용자에게 카카오 계정 로그인을 통한 인증을 요청한다.
    - 클라이언트에 유효한 카카오계정 세션이 있거나, 카카오톡 인앱 브라우저에서의 요청인 경우 4단계로 넘어간다.
3. 사용자가 카카오계정으로 로그인한다.
4. 카카오 인증 서버가 사용자에게 동의 화면을 출력하여 인가를 위한 사용자 동의를 요청한다.
    - 동의 화면은 서비스 애플리케이션의 동의 항목 설정에 따라 구성된다.
5. 사용자가 필수 동의 항목, 이외 원하는 동의 항목에 동의한 뒤 [동의하고 계속하기] 버튼을 누른다.
6. 카카오 인증 서버는 서비스 서버의 Redirect URI로 인가 코드를 전달한다.

## Step 2. 토큰 받기

1. 서비스 서버가 Redirect URI로 전달받은 인가 코드로 토큰 받기를 요청한다.
2. 카카오 인증 서버가 토큰을 발급해 서비스 서버에 전달한다.

    ```jsx
    POST /oauth/token HTTP/1.1
    HOST: kauth.kakao.com
    Content-type: application/x-www-form-urlencoded;charset=utf-8
    ```

    인가 코드 받기만으로는 카카오 로그인이 완료되지 않으며, 토큰 받기까지 마쳐야 카카오 로그인을 정상적으로 완료할 수 있다.

    - POST 로 요청, 요청 성공 시 응답은 토큰과 토큰 정보를 포함한다.
    - access token으로 사용자 정보 가져오기와 같은 카카오API를 호출할 수 있다.
        1. 토큰 정보 보기(토큰 유효성 검증)
        2. 사용자 정보 가져오기 (필요한 사용자 정보를 받는다)

    ### Request

    | Name                                                   | Type   | Description                                           | Required |
    | ------------------------------------------------------ | ------ | ----------------------------------------------------- | -------- |
    | grant_type                                             | String | authorization_code로 고정                             | O        |
    | client_id                                              | String | 앱 REST API 키                                        | O        |
    | redirect_uri                                           | String | 인가 코드가 리다이렉트된 URI                          | O        |
    | code                                                   | String | 인가 코드 받기 요청으로 얻은 인가 코드                | O        |
    | client_secret                                          | String | 토큰 발급 시, 보안을 강화하기 위해 추가 확인하는 코드 |
    | [내 애플리케이션] > [보안]에서 사용설정한 경우 필수 값 | △      |

    ### Response

    | Name                                                                                     | Type    | Description                           | Required |
    | ---------------------------------------------------------------------------------------- | ------- | ------------------------------------- | -------- |
    | token_type                                                                               | String  | 토큰 타입, bearer로 고정              | O        |
    | access_token                                                                             | String  | 사용자 액세스 토큰 값                 | O        |
    | id_token                                                                                 | String  | ID 토큰 값                            |
    | OpenID Connect 확장 기능을 통해 발급되는 ID 토큰, Base64 인코딩 된 사용자 인증 정보 포함 | △       |
    | expires_in                                                                               | Integer | 액세스 토큰과 ID 토큰의 만료 시간(초) | O        |
    | refresh_token                                                                            | String  | 사용자 리프레시 토큰 값               | O        |
    | refresh_token_expires_in                                                                 | Integer | 리프레시 토큰 만료 시간(초)           | O        |
    | scope                                                                                    | String  | 인증된 사용자의 정보 조회 권한 범위   |
    | 범위가 여러 개일 경우, 공백으로 구분                                                     | X       |

## Step 3. 사용자 로그인 처리

사용자 로그인 처리는 자체 구현해야하며 이는 참고 자료 입니다.

1. 서비스 서버가 발급받은 액세스 토큰을 사용자 정보 가져오기를 요청해 사용자의 회원번호 및 정보를 조회하여 서비스 회원인지 확인한다.

    ```jsx
    GET/POST /v2/user/me HTTP/1.1
    Host: kapi.kakao.com
    Authorization: Bearer ${ACCESS_TOKEN}/KakaoAK ${APP_ADMIN_KEY}
    Content-type: application/x-www-form-urlencoded;charset=utf-8
    ```

    ### Request : 액세스 토큰 사용

    - Header
        | --- | --- | --- |
    - Parameter
        | --- | --- | --- | --- |

    ### Request : 어드민 키 사용

    - Header
        | --- | --- | --- |
    - Parameter
        | --- | --- | --- | --- |
    - Property Keys
        | --- | --- |

    ### Response

    | --- | --- | --- | --- |

    ### KakaoAccount

    | --- | --- | --- | --- |

2. 서비스 회원 정보 확인 결과에 따라 서비스 로그인 또는 회원가입과정을 진행한다.
3. 이 외 서비스에서 필요한 로그인 절차를 수행한 후, 카카오 로그인한 사용자의 서비스 로그인 처리를 완료한다.

# 로그아웃

```jsx
POST /v1/user/logout HTTP/1.1
Host: kapi.kakao.com
Authorization: Bearer ${ACCESS_TOKEN}/KakaoAK ${APP_ADMIN_KEY}
```

사용자 액세스 토큰과 리프레시 토큰을 모두 만료시킨다.

-   로그아웃은 요청 방법에 따라 다음과 같이 동작합니다.
    -   액세스 토큰으로 요청
        -   해당 액세스 토큰만 만료 처리
        -   만료된 액세스 토큰을 사용하는 모든 기기에서 로그아웃됨
    -   어드민 키로 요청
        -   해당 사용자의 모든 토큰 만료 처리
        -   모든 기기에서 로그아웃됨
-   로그아웃 요청 성공 시, 응답 코드와 로그아웃된 사용자의 회원번호를 받는다.
    -   로그아웃 시에도 웹 브라우저의 카카오계정 세션은 만료되지 않고, 로그아웃을 호출한 앱의 토큰만 만료된다.
        -   따라서 웹 브라우저의 카카오계정 로그인 상태는 로그아웃을 호출해도 유지됨
    -   로그아웃 후에는 서비스 초기 화면으로 리다이렉트하는 등 후속 조치를 취하도록 합니다.
-   서비스에서 필요에 따라 웹 브라우저의 카카오계정 로그인 상태 또한 로그아웃 처리하여야 할 때는 추가 기능인 [카카오계정과 함께 로그아웃](https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api#logout-of-service-and-kakaoaccount)을 사용한다.
    ### Request: 액세스 토큰 사용
    -   Header
        | --- | --- | --- |
    ### Request: 어드민 키 사용
    -   Header
        | --- | --- | --- |
    -   Parameter
        | --- | --- | --- | --- |
    ### Response
    | --- | --- | --- | --- |

## 카카오계정과 함께 로그아웃

서비스 서버의 `Logout Redirect URI`로 전달된 서비스 로그아웃 요청에 대한 처리는 자체적으로 구현해야 한다.

```jsx
GET /oauth/logout?client_id=${REST_API_KEY}&logout_redirect_uri=${LOGOUT_REDIRECT_URI} HTTP/1.1
Host: kauth.kakao.com
```

카카오계정과 함께 로그아웃은 웹 브라우저에 로그인된 카카오계정의 세션을 만료시키고, 서비스에서도 로그아웃 처리할 때 사용하는 로그아웃 추가 기능이다.

-   카카오계정과 함께 로그아웃 기능은 카카오계정 로그아웃 처리 후 `Logout Redirect URI`로 302 리다이렉트(Redirect)하여 서비스 로그아웃까지 연속해서 수행할 수 있도록 구성돼 있다.
-   앱의 REST API 키를 `client_id`, 서비스 로그아웃 처리를 하는 서비스 서버의 주소를 `Logout Redirect URI` 파라미터에 담아 `GET`으로 요청한다.
-   로그아웃 과정 중 유지하고자 하는 특정 값이 있다면 `state` 파라미터에 담아 요청 시 함께 전달할 수 있다.
-   카카오 인증 서버는 서비스 로그아웃 처리 결과를 전달받지 않는다.
    ### Request
    -   Parameter
        | --- | --- | --- | --- |
    ### Response
    | --- | --- | --- | --- |

## 추가 공부

간편 로그인

---
layout: post
title: "카카오 로그인 (1)"
date: 2022-10-18
excerpt: "장고로 카카오 로그인 연결할 방법 생각해보기"
tags: [study, REST API, 카카오, Django]
category: [Daily Problem]
comments: false
---

S(serializer) M(model) U(urls) V(view)

이메일이 없이 가입이 가능하던데 그런 경우 기존에 있는 유저인지 여부는 어떻게 판단 가능? → 이메일 동의 필수로 받음

## 1. User 모델 생성

-   User Model
    | Name | Type | Description | Required |
    | -------- | ------- | -------------------------- | -------- |
    | id | Integer | pk , 자동생성 | O |
    | name | String | 사용자 이름 | X |
    | age | Integer | 사용자 나이 | X |
    | provider | String | 회원가입 경로 | O |
    | email | String | 사용자 이메일, 중복 허용 X | O |
-   User Manager : User를 생성할 때 사용하는 헬퍼 클래스 (User data가 UserManager를 거쳐 생성된다.)
    -   BaseUserManager 상속
    -   create_user : 관리자를 포함한 모든 사용자를 생성할 때 실행되는 함수
        -   User에 해당하는 필드 값들을 받고 이를 db에 저장
        -   is_superuser, is_staff : False
    -   create_superuser : 관리자를 생성할 때 실행되는 함수
        -   create_user에서 사용자 정보를 db에 저장 → is_superuser, is_staff를 True로 변경

## 2. Serializer 설정

-   모델 참고하여 생성

## 3. Url 설정

-   메인 urls에 include
-   url 경로 생성
    -   kakao/login/ : 콜백 경로로 redirect 됨
    -   kakao/logout/ : logout
    -   kakao/login/callback/ : 카카오 콜백 경로

## 4. Permission 설정

-   API 접근권한을 지정

## 5. Views 설정

-   KakaoLogIn : 인가 코드 받기 요청
    ![Untitled](/assets/etc/인턴/step1.png)

    ```jsx
    https://kauth.kakao.com/oauth/authorize?response_type=code&client_id=${REST_API_KEY}&redirect_uri=${REDIRECT_URI}

    GET /oauth/authorize?client_id=${REST_API_KEY}&redirect_uri=${REDIRECT_URI}&response_type=code HTTP/1.1
    Host: kauth.kakao.com
    ```

-   KakaoCallback :
    ![Untitled](/assets/etc/인턴/step2.png)

    1. 서비스 서버가 Redirect URI로 전달받은 인가 코드로 토큰 받기를 요청
    2. POST 로 요청, 요청 성공 시 응답은 토큰과 토큰 정보를 포함

        ```jsx
        https://kauth.kakao.com/oauth/token/grant_type=authorization_code&client_id={SOCIAL_AUTH_KAKAO_CLIENT_ID}&redirect_uri={KAKAO_CALLBACK_URI}&response={code}

        curl -v -X POST "https://kauth.kakao.com/oauth/token" \
         -H "Content-Type: application/x-www-form-urlencoded" \
         -d "grant_type=authorization_code" \
         -d "client_id=${REST_API_KEY}" \
         --data-urlencode "redirect_uri=${REDIRECT_URI}" \
         -d "code=${AUTHORIZE_CODE}"
        ```

    3. 카카오 인증 서버가 토큰을 발급해 서비스 서버에 전달
    4. 사용자 정보 가져오기를 요청

        ![Untitled](/assets/etc/인턴/step3.png)

        1. 사용자의 회원번호 및 정보를 조회하여 서비스 회원인지 확인
        2. 서비스 회원 정보 확인 결과에 따라 서비스 로그인 또는 회원가입과정을 진행
        3. 이 외 서비스에서 필요한 로그인 절차를 수행
        4. 카카오 로그인한 사용자의 서비스 로그인 처리를 완료

-   UserViewSet
    생성한 모델과 권한 레벨에 따라

1. 서비스 서버가 발급받은 액세스 토큰 유효성 검증

    1. 토큰 정보 보기 (아래 표는 에러 발생 시 응답 코드)

    | Code | Description                                                                                                                                                                                  | HTTP Status |
    | ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
    | -1   | 카카오 플랫폼 서비스의 일시적 내부 장애 상태토큰을 강제 만료(폐기) 또는 로그아웃 처리하지 않고 일시적인 장애 메시지로 처리 권장                                                              | 400         |
    | -2   | 필수 인자가 포함되지 않은 경우나 호출 인자값의 데이터 타입이 적절하지 않거나 허용된 범위를 벗어난 경우요청 시 주어진 액세스 토큰 정보가 잘못된 형식인 경우로 올바른 형식으로 요청했는지 확인 | 400         |
    | -401 | 유효하지 않은 앱키나 액세스 토큰으로 요청한 경우토큰 값이 잘못되었거나 만료되어 유효하지 않은 경우로 토큰 갱신 필요                                                                          | 401         |

---
layout: post
title: "Django REST Framework"
date: 2022-09-27
excerpt: "인프런 Django REST Framework 강의를 듣고 정리한 내용"
tags: [study, RESTful API, REST]
category: [Daily Problem]
comments: false
---

## DRF는 REST api 규칙을 따른다.

-   url 패턴은 테이블 접근과 연관되어있다.
    -   url 이름을 정할 때 테이블 기준으로 이름을 지어주면 url이 많은 경우에도 일관성있게 이름을 지을 수 있다.
-   RESTful API 특징 : 동작을 url에 표현하는게 아니라 HTTP 메서드로 표현한다.
    -   drf 내부적으로는 list, retrieve, create, update, delete, partial update와 같은 용어를 쓰기도 한다.

[특이사항]
REST나 RESTful에서는 url 끝에 슬래시를 붙이지 않는 걸 권고하지만 장고는 붙이는 형태를 권고한다.
c.f. `/api/post`,`/api/post/`
-> drf는 장고 패턴을 따른다.

### DRF의 default mode

1. 브라우저에서는 api로 응답 : browsable api
2. client에서는 json으로 응답
    - client : 브라우저 이외의 요청(curl/httppie/postman/vuejs/reactjs 등)

```python
'DEFAULT_PERMISSION_CLASSES': [
    'rest_framework.permissions.DjangoModelPermissionsOrAnonReadOnly',
]
```

: 로그인 유저에게는 CRUD 모두 허용, 비로그인 유저에게는 Read 만 허용
->기본 설정은 AllowAny: 모든 사용자에게 CRUD 허용

### DefaultRouter vs SimpleRouter

#### 공통

-   GET (list)
-   GET (retrieve)
-   POST (create)
-   PUT (update)
-   PATCH (partial_update)
-   DELETE (destroy)

#### DefaultRouter는 SimpleRouter를 포함한다

##### 추가되는 점

-   API root : router에 의해 자동 생성되는 API 루트 페이지

cdrf.co : DRF와 관련된 소스들이 정리된 사이트

| 클래스상속           |
| :------------------- |
| View                 |
| API View             |
| GenericAPIView       |
| GenericAPIView + ... |

### CRUD와 View

-   Create : CreateAPIView | POST
-   Read
    -   ListAPIView | GET
    -   RetrieveAPIView | GET
-   Update : UpdateAPIView | PUT+PATCH
-   Delete : DestroyAPIView | DELETE

#### Serializer에서는 HyperlinkedModelSerializer 보다 ModelSerializer를 더 선호한다.

### GenericView의 구조

1. DB에서 data를 가져옴
2. serialize
3. response

-   ListAPIView : PostSerializer(instance=xx,many=True)
-   RetrieveAPIView : PostSerializer(instance=xx,many=False)

#### DRF Serializer는 딕셔너리 기반으로 동작한다.

-   <a href="https://kimdahui42.github.io/create-update/">직렬화 관련 포스팅</a>
-   장고의 form 클래스 for HTML <form>
-   장고의 Model 클래스 for DB Table

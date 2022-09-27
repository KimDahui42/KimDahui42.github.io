---
layout: post
title: "BaseSerializer과 ModelViewSet에서의 create,update 메서드 비교"
date: 2022-09-27
excerpt: "REST API를 정리"
tags: [study, Django, Django REST Framework]
category: [Daily Problem]
comments: false
---

## Serialization : 직렬화

drf에서는 다음과 같이 설명하고 있다.

> serializer를 사용하면 쿼리셋이나 모델 인스턴스와 같은 복잡한 데이터를 네이티브 Python 데이터 유형으로 변환한 다음 JSON, XML또는 다른 콘텐츠 유형으로 쉽게 렌더링할 수 있습니다. serializer는 역직렬화 기능을 제공하여 수신되는 데이터의 유효성을 검사한 후 구문 분석된 데이터를 복합 유형으로 다시 변환할 수 있습니다.

직렬화는 아래의 이유에서 수행된다.

-   서로 다른 메모리 외부/내부 환경
    -   사용하는 타입이 다르다(내부:객체, 외부:문자열)
-   복원(직렬화->역직렬화)시 정보를 유지해야함

메모리 내부에 저장되는 형태와 출력되는 형태가 다르기 때문에 객체의 구조를 유지하는 것이 중요하다.

#### 과정

-   직렬화 : instance -> Serializer(instance=xx) -> 딕셔너리(dict) -> json data -> response -> read operation
-   역직렬화 : json data -> 딕셔너리(dict) -> Serializer(data=dict) -> 유효성 검사 is_valid(),validated_data -> instance -> save(), create(), update() : write operations

### BaseSerializer

DRF의 Serializer은 BaseSerializer 클래스를 상속받아 구현된다.

baseserializer의 네 가지 메서드를 재정의하는 것으로 새로운 serializer 클래스의 기능을 정의한다.
method |설명
:--:|:--:
.to_representation()|읽기 작업, 직렬화 관련
.to_internal_value()|쓰기 작업, 역직렬화 관련
.create() .update()|인스턴스 저장 관련

---

## BaseSerializer과 ModelViewSet의 create,update 메서드

### BaseSerializer - create,update

```python
def create(self, validated_data):
        raise NotImplementedError('`create()` must be implemented.')

def update(self, instance, validated_data):
        raise NotImplementedError('`update()` must be implemented.')
```

개체에 세부 정보를 추가하거나 각 모델 필드에 유효성이 검증된 값을 집어넣기 위해 사용된다. serializer에서 create,update 하고 뷰에서 출력하는게 단순한 뷰를 유지할 수 있어 가장 이상적이다. 메서드(인스턴스 생성/수정 동작)를 정의하여 사용해야한다.

### ModelViewSet - create,update

```python
def create(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        self.perform_create(serializer)
        headers = self.get_success_headers(serializer.data)
        return Response(serializer.data, status=status.HTTP_201_CREATED, headers=headers)

def update(self, request, *args, **kwargs):
        partial = kwargs.pop('partial', False)
        instance = self.get_object()
        serializer = self.get_serializer(instance, data=request.data, partial=partial)
        serializer.is_valid(raise_exception=True)
        self.perform_update(serializer)

        if getattr(instance, '_prefetched_objects_cache', None):
            # If 'prefetch_related' has been applied to a queryset, we need to
            # forcibly invalidate the prefetch cache on the instance.
            instance._prefetched_objects_cache = {}

        return Response(serializer.data)
```

create와 update 메서드는 각각 ModelViewSet의 부모 클래스 CreateModelMixin과 UpdateModelMixin에 정의되어 있다. 이는 serializer의 유효성 검사를 호출하기 때문에 애플리케이션 로직을 분리할 수 있고, 잦은 유효성 검사 호출과 응답 출력에 대해 신경쓰지 않을 수 있다.

#### 과정

-   serializer 호출 -> 유효성 검사 -> perform_create | perform_update

---

#### perform_create,perform_update

```python
def perform_create(self, serializer):
        serializer.save()

def perform_update(self, serializer):
        serializer.save()
```

create와 update 메서드 내부에서 호출되어 유효성 검증이 완료된 값을 저장하기 위해 serializer.save()를 호출한다. 이 메서드로 create와 update의 부분적 재정의가 가능해진다.

#### get_object,get_queryset

```python
def get_object(self):
        """
        Returns the object the view is displaying.

        You may want to override this if you need to provide non-standard
        queryset lookups.  Eg if objects are referenced using multiple
        keyword arguments in the url conf.
        """
        queryset = self.filter_queryset(self.get_queryset())

        # Perform the lookup filtering.
        lookup_url_kwarg = self.lookup_url_kwarg or self.lookup_field

        assert lookup_url_kwarg in self.kwargs, (
            'Expected view %s to be called with a URL keyword argument '
            'named "%s". Fix your URL conf, or set the `.lookup_field` '
            'attribute on the view correctly.' %
            (self.__class__.__name__, lookup_url_kwarg)
        )

        filter_kwargs = {self.lookup_field: self.kwargs[lookup_url_kwarg]}
        obj = get_object_or_404(queryset, **filter_kwargs)

        # May raise a permission denied
        self.check_object_permissions(self.request, obj)

        return obj
```

뷰에서 출력하는 개체를 반환한다. 필요에 따라 다른 쿼리셋을 제공할 때 재정의하여 사용한다.

<details>
<summary style="cursor:pointer;"><h4 style="display:inline-block">filter_queryset</h4></summary>
<p>
<pre>
def filter_queryset(self, queryset):
    for backend in list(self.filter_backends):
    queryset = backend().filter_queryset(self.request, queryset, self)
    return queryset
</pre>
쿼리셋이 주어지면 사용중인 filter backed를 통해 필터링한다
</p>
</details>

```python
def get_queryset(self):
        assert self.queryset is not None, (
            "'%s' should either include a `queryset` attribute, "
            "or override the `get_queryset()` method."
            % self.__class__.__name__
        )

        queryset = self.queryset
        if isinstance(queryset, QuerySet):
            # Ensure queryset is re-evaluated on each request.
            queryset = queryset.all()
        return queryset
```

default는 self.queryset이지만 queryset은 한 번만 동작하고 get_queryset은 request 마다 동작하기 때문에 queryset 사용보다 get_queryset 사용이 권장된다. 요청에 따라 다른 쿼리셋을 제공할때 재정의하여 사용한다.

#### get_serializer

```python
def get_serializer(self, *args, **kwargs):
        """
        Return the serializer instance that should be used for validating and
        deserializing input, and for serializing output.
        """
        serializer_class = self.get_serializer_class()
        kwargs.setdefault('context', self.get_serializer_context())
        return serializer_class(*args, **kwargs)
```

get_serializer_class를 통해 사용되어야할 serializer 클래스를 파악해 get_serializer_context로 serializer context를 전달해 해당 serializer 클래스를 반환하는 메서드다.

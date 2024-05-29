# URL과 View(뷰)

### Django의 기본적인 흐름

![[Screenshot 2024-05-21 at 2.14.25 PM.png]]
1. 브라우저에서 로컬 서버로 `http://localhost:8000/pybo` 페이지를 요청하면
2. `url.py` 파일에서 `/pybo` URL 매핑을 확인하여 views.py 파일의 index 함수를 호출하고
3. 호출한 결과를 브라우저에 반영한다

### URL 매핑

```django
# config/urls.py
urlpatterns = [
	# 1. 'pybo/'에 대한 url은 view.index로 연결되도록 한다
	path('pybo/', views.index)

	# 2. 'pybo/'에 대한 url은 pybo.urls 파일 내의 매핑 결과에 따라 연결되도록 한다(깔끔)
	path('pybo/', include('pybo.urls'))
]

# pybo/urls.py
urlpatterns = [
	# 위의 1번과 같은 내용!!!
	path('', views.index)
]
```

- 보통은 2번과 같이 해당 앱 내의 url 매핑은 내부의 `urls.py`에 따라 연결되도록 하는 방법을 사용
- `pybo/`를 `pybo`라고 하지 않고 뒤에 슬래시(`/`)를 붙여줌
  -> 이 경우, 뒤에 슬래시를 붙이지 않고 주소창에 입력하더라도 자동으로 슬래시가 붙여서 변환됨

 > [!NOTE]
 > **URL 정규화**
 > : 사용자가 URL에 슬래시(`/`)를 붙이거나 붙이지 않아도 원하는 페이지에 접근할 수 있도록 해주는 기능 (== Django의 `APPEND_SLASH` 설정에 의해 제어됨)
 > 1. URL의 기본 이해
 >    : 웹 애플리케이션에서 URL은 사용자가 웹 페이지에 접근하기 위해 사용하는 주소로 Django에서 URL을 처리하려면 `urls.py` 파일에 URL 패턴을 정의해야 한다
 > 2. 슬래시와 URL 매핑
 >    : 슬래시의 유무에 따라 URL 매핑이 달라질 수 있다.
 >    `http://example.com/about/`는 슬래시를 입력하지 않았을 경우 유효하지 않게 된다.
 > 3. URL 정규화
 >    : Django는 `APPEND_SLASH` 설정을 통해 URL 정규화를 지원한다. 이 설정이 `True`로 되어 있으며, 사용자가 URL 뒤에 슬래시를 빼먹었을 때 자동으로 슬래시를 추가하여 올바른 URL로 리다이렉트 한다.
 > 4. `APPEND_SLASH` 설정
 >    : Django의 `settings.py` 파일에 정의되어 있으며 default 값으로 `True`로 되어 있다.
 >
 > 즉, URL 정규화란 사용자가 URL 뒤에 슬래시를 빼먹어도 자동으로 슬래시를 추가하여 올바를 URL로 리다이렉트 하는 기능을 의미한다 (사용자 편의를 위한 것이라 할 수 있음)

### URL 매핑 분리
 ![[Screenshot 2024-05-21 at 3.02.15 PM.png]]

# Django에서는 Model로 데이터를 관리한다

: Django는 `Model`로 데이터를 관리한다.
> `Model`로 데이터를 관리한다
> == 애플리케이션에서 사용할 데이터베이스의 구조를 정의하고, 이 구조를 기반으로 데이터를 생성, 읽기, 업데이트, 삭제(CRUD) 작업을 수행하는 것을 의미
> `Model`은 장고의 ORM(Object-Relational Mapping)을 통해 데이터베이스와의 상호작용을 추상화하여, 개발자가 SQL 쿼리를 직접 작성하지 않고도 데이터베이스 작업을 수행할 수 있게 한다

### Model의 주요 역할

1. 데이터베이스 테이블 정의: 모델 클래스를 상속받아 데이터베이스 테이블의 구조를 정의한다.
2. 데이터베이스 쿼리 관리: 장고의 ORM을 통해 데이터베이스와 상호작용하고 SQL 쿼리를 추상화하여 파이썬 코드로 데이터 조작을 쉽게 할 수 있다.
3. 데이터 검증: 모델 필드를 통해 데이터 타입과 유효성 검사를 정의할 수 있다.

### Model Migration

: 모델을 정의하거나 수정한 후, 해당 변경 사항을 데이터베이스에 반영하기 위해 마이그레이션(migration)을 생성하고 적용해야 한다

```bash
$> python manage.py makemigration
$> python manage.py migrate
```

## Model을 사용하여 데이터베이스 구조 정의 및 사용

### 데이터베이스 구조 정의

: 모델을 사용하여 데이터베이스의 테이블 구조를 정의한다. 각 모델 클래스는 데이터베이스의 테이블에 해당하며, 모델의 속성(필드)은 테이블의 컬럼에 해당한다.

- 모델 클래스는 `django.db.models.Model`을 상속받아 정의하며, `models` 모듈에서 제공하는 다양한 필드를 사용하여 데이터베이스의 테이블 열로 정의된다.

```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100)  # 문자열 필드
    price = models.DecimalField(max_digits=10, decimal_places=2)  # 소수점 필드
    description = models.TextField()  # 텍스트 필드
    created_at = models.DateTimeField(auto_now_add=True)  # 자동으로 현재 시간 추가
    updated_at = models.DateTimeField(auto_now=True)  # 자동으로 현재 시간 업데이트

	# 해당 db 생성 이후 확인할 때 보이는 형태(spring에서 toString()과 비슷)
    def __str__(self):
        return self.name

```

### 모델 사용 예시

```python
# 데이터 생성
product = Product.objects.create(name="Laptop", price=999.99, description="High-end gaming laptop")

# 데이터 조회
all_products = Product.objects.all()

# 데이터 업데이트
product = Product.objects.get(id=1)
product.price = 899.99
product.save()

# 데이터 삭제
product = Product.objects.get(id=1)
product.delete()
```

### 데이터베이스에 저장된 정보를 수정

: DB에 이미 저장되어 있는 데이터를 수정할 때 다음과 같은 방식으로 진행된다.

```python
>>> p = Product.objects.get(id=2)
>>> p.name = 'aaaaa'
>>> p.save()
```

- 이때, 반드시 `save()` 함수를 사용해야만 변경된 데이터가 실제 DB에 저장된다.
  -> Django 데이터베이스 사용 원칙

> [!IMPORTANT]
> `Django`에서는 **변경된 내용을 데이터베이스에 반영하기 위해 명시적으로 `save()`를 호출**해야 한다.
> : ORM(Object-Relational Mapping) 에서는 데이터베이스의 레코드를 수정할 때 변경된 내용을 데이터베이스에 반영하기 위해 명시적으로 `save()` 메서드를 호출해야 한다.
> 1. **트랜젝선 관리**
>    : `save()` 메서드는 변경 사항을 데이터베이스 트랜잭션으로 처리한다. 트랜잭션은 데이터베이스에서 일련의 작업들이 모두 성공적으로 완료되거나 모두 롤백 되는 것을 보장하여 데이터 무결성을 유지할 수 있다.
> 2. **명시적인 저장**
>    : 장고에서 객체를 변경한 후, 자동으로 데이터베이스에 반영하지 않는다. 이는 변경사항이 의도한 대로 정확히 저장될 때까지 명시적인 호출을 통해 제어할 수 있도록 한다.
> 3. **효율성**
>    : 자동으로 모든 변경사항을 즉시 데이터베이스에 반영한다면, 불필요한 데이터베이스 호출이 증가하여 성능에 악영향을 미칠 수 있다. 따라서 명시적으로 `save()`를 호출하는 방식은 필요한 시점에만 데이터베이스와 상호작용하도록 하여 효율성을 높인다.
> 4. **일관성 유지**
>    : `save()`를 호출하면 코드가 명확해지고, 데이터가 변경되었을 때 저장되는 시점을 명확하게 알 수 있다. 이는 코드의 일관성을 유지할 수 있다.

- 추가적인 메서드는 다음과 같다
```python
# save(): 객체를 데이터베이스에 저장하거나 업데이트 한다

# update(): 쿼리셋에 있는 모든 객체를 한 번에 업데이트 한다. 이는 여러 객체를 동시에 업데이트할 때 유용하다
Question.objects.filter(id=2).update(name='aaa')

# save(commit=False): 객체를 데이터베이스에 저장하지 않고 인스턴스를 반환하며, 이후 save()를 통해 저장할 수 있다
q = Question.objects.get(id=2)
q.subject = 'aaa'
q.save(commit=False)  # 변경 사항을 데이터베이스에 반영하지 않고 객체를 반환
	# 추가 작업 후
q.save()  # 최종적으로 데이터베이스에 저장
```


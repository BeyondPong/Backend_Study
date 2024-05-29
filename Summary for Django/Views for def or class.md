# Function-Based Views, FBVs(`def`)

: 함수 기반 뷰는 단순히 `python` 함수를 사용하여 요청을 처리한다. 이 함수는 일반적으로 요청 객체를 매개 변수로 받고, HttpResponse 객체를 반환한다.

- 장점
	1. **간결함**: 간단한 뷰를 작성할 때 더 직관적이고 빠르게 작성할 수 있다.
	2. **명확함**: 요청을 처리하는 로직이 한 곳에 집중되어 있어 이해하기 쉽다.
- 단점
	1. **재사용성 낮음**: 중복 코드를 피하기 어려울 수 있다.
	2. **확장성 제한**: 복잡한 기능을 추가할 때 코드가 길어지고 복잡해질 수 있다.


# Class-Based Views, CBVs(`class`)

: 클래스 기반 뷰는 `python` 클래스를 사용하여 요청을 처리한다. 장고에서는 이 방식에서 사용하기 편리한 여러 제네릭 뷰와 믹스인을 제공한다.

- 장점
	1. **재사용성**: 공통 기능을 재사용하기 쉬우며, 클래스 상속을 통해 코드 중복을 줄일 수 있다.
	2. **구조화**: 코드가 더 구조화되고, 복잡한 뷰 로직을 여러 메소드로 나누어 관리할 수 있다.
	3. **확장성**: 기본 제공되는 제네릭 뷰와 믹스인을 사용하여 기능을 쉽게 확장할 수 있다.
- 단점
	1. **복잡성**: 함수 기반 뷰보다 구조가 복잡할 수 있으며, 클래스와 메소드의 개념을 잘 이해해야 한다.
	2. **가독성**: 작은 프로젝트나 간단한 뷰에는 과도할 수 있다.


---

### 주요 차이점
- **간결함** vs **구조화**
  : 함수 기반 뷰는 간단하고 빠르게 작성할 수 있는 반면, 클래스 기반 뷰는 코드 구조화를 통해 유지보수성과 재사용성을 높인다.
- **단일 책임 원칙**
  : 클래스 기반 뷰는 단일 책임 원칙(Single Responsibility Principle)을 따르기 쉬우며, 각 HTTP 메소드(GET, POST 등)를 별도의 메소드로 나누어 처리할 수 있다.
- **제네릭 뷰 활용**
  : 클래스 기반 뷰는 장고의 제네릭 뷰를 활용하여 CRUD(Create, Read, Update, Delete) 작업을 쉽게 구현할 수 있다.

---

# CBVs: 클래스 기반 뷰를 사용하면...

1. 클래스 기반 뷰를 먼저 정의
```python
# View.py

from django.http import HttpResponse
from django.views import View

class MyView(View):
	def get(self, request):
		return HttpResponse("This is a Get request")

	def post(self, request):
		return HttpResponse("This is a Post request")
```

2. URL 패턴 매핑
```python
from django.urls import path
from .views import MyView

urlpatterns = [
	path('my-view/', MyView.as_view(), name='my_view'),
]
```

3. 동작 방식
	- `GET` 요청이 `my-view/` URL로 들어오면, `MyView` 클래스의 `get` 메서드가 호출된다
	- `POST` 요청이 `my-view/` URL로 들어오면, `MyView` 클래스의 `post` 메서드가 호출된다

클래스 기반 뷰를 사용하면 여러 HTTP 메서드를 하나의 클래스에서 정의할 수 있어 코드가 더 구조화되고 깔끔해진다. Django의 제네릭 뷰와 믹스인을 사용하면 더욱 효율적으로 다양한 기능을 구현할 수 있다.

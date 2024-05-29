# 파이썬이란?

### 인터프리터 언어

: 인터프리터 언어는 소스코드를 바로 실행하여 한 줄씩 읽어 별도의 실행 파일이 존재하지 않으며, 별도의 시작점이 존재하지 않는다.

동적 언어로, 변수의 라이프 사이클에 따라 변수의 type이 계속 변한다.

프로그램 실행과 컴파일이 동시에 이루어지기에 컴파일 과정이 별도로 필요하지 않지만, 구동 시간이 오래걸린다.

### 모듈 vs 패키지 vs 라이브러리

1. 모듈

    : Python에서 기능적으로 관련된 코드들을 모아놓은 파일

    즉, 변수, 함수, 클래스 등을 포함할 수 있는 `.py` 확장자를 가진 하나의 Script-file

    모듈은 재사용 가능한 코드를 한 곳에 정리하여 다른 Python script에서 쉽게 import하여 사용할 수 있게 해준다.

    ```python
    # math.py
    def add(x, y):
    	return x + y

    # another-python file
    import math             # 외부에서 정의한 함수를 포함한 math.py를 import하여 사용
    print(math.add(3, 4))   # print-result: 7
    ```

2. 패키지

    : 하나 이상의 모듈을 포함하고 조직화할 수 있는 폴더로, python의 네임스페이스를 사용하여 모듈을 구조화하는 방법

    → 이때, `__init__.py` 파일을 포함하는 디렉토리로, 이 파일은 해당 디렉토리가 패키지의 일부임을 python에 알리는 역할을 하며, 비어있을 수 있다

    ```python
    # ./numerics/math.py (같은 위치에 __init__.py도 같이 존재)
    def add(x, y):
    	return x + y

    # ./another.py
    from numerics import math    # 하위의 패키지 폴더 명과 함께 원하는 파일 import
    print(math.add(3, 4))        # print-result: 7
    ```

3. 라이브러리

    : 특정 작업을 위해 미리 작성된 코드 집합을 의미

    → 하나 이상의 모듈 또는 패키지를 포함할 수 있으며, 특정 기능을 제공하기 위해 함수, 클래스, 객체 등을 포함한다. 라이브러리는 큰 프레임워크의 일부일 수도 있고, 독립적인 기능을 제공하는 작은 도구일 수도 있다.

    ex) `Numpy`, `Pandas`, etc…


<aside>
💡 모듈 ≤ 패키지 ≤ 라이브러리

</aside>

### 함수(function) vs 메서드(method)

- 함수(function)

    : 독립적으로 존재하는 코드의 블록으로, 특정 작업을 수행하거나 값을 계산하여 결과를 반환하는 데 사용
	`def` 키워드를 사용해서 정의되며, `argument`로 인자를 받아, `return` 키워드를 통해 결과를 반환

- 메서드(method)
	: 메서드는 클래스 내에 정의된 함수로, 클래스의 객체(인스턴스)에 속함
	객체의 데이터를 조작하거나, 객체의 행동을 정의하는 데 사용됨
	→ 메서드는 항상 클래스 정의의 일부이며, 호출 시 첫 번째 매개변수로 자신이 속한 객체의 참조(`self`)를 자동으로 받는다
```python
	class ClassName:
    def method_name(self, parameters):
        # 실행할 코드
        return result
```

- 함수와 메서드의 주요 차이점
	1. 정의: 함수는 독립적으로 정의되지만, 메서드는 클래스의 일부로 정의됨
	2. 호출 방식: 함수는 이름만 사용하여 호출되지만, 메서드는 객체를 통해 호출됨
	3. 접근 방식: 메서드는 클래스의 다른 속성이나 메서드에 접근할 수 있지만(`self`를 통해), 함수는 전달된 인자나 전역 변수를 통해서만 작업을 수행할 수 있음



---


# 기본 자료형

## 숫자 형 변수

### 숫자형을 활용하기 위한 연산자

1. 사칙 연산: `+`, `-`, `*`, `/` (C와 달리 몫 연산이 아닌 실제 나눗셈 연산 결과)
2. 제곱 연산: `**`
3. 나머지 연산: `%`
4. 몫 연산: `//`


## 문자열 형 변수

### 문자열(string)

: 따옴표로 둘러싸여 있으면 모두 문자열

→ 파이썬에서 문자열을 만드는 방법은 다음과 같이 4가지이다

```python
# 큰 따옴표로 양쪽 둘러싸기
"Hello World"

# 작은 따옴표로 양쪽 둘러싸기
'Python is fun'

# 큰 따옴표 3개를 연속으로 써서 양쪽 둘러싸기
"""Life is too short..."""

# 작은 따옴표 3개를 연속으로 써서 양쪽 둘러싸기
'''Life is too short...'''
```

### 이스케이프 코드

: 프로그래밍할 때 사용할 수 있도록 미리 정의해둔 문자 조합을 의미

주로 출력물을 보기좋게 정렬하는 용도로 사용

ex) `\n`, `\t`, `\\`, `\'`, `\"`

### 문자열 연산 및 길이

- 문자열 곱셈

    : `*` 을 문자열에 사용시, 해당 숫자만큼 문자열이 복사되어 붙여진다

- 문자열 길이 구하기

    : `len(변수)` → 이미 내장되어있는 함수로 문자열에서 공백까지 포함한 길이를 return


### 문자열의 인덱싱

```python
>>> a = "Life is too short, You need Python"

>>> a[0]
'L'
>>> a[12]
's'
>>> a[-1]             # python에서는 음수 인덱스가 존재(반대로 counting)
'n'
```

### 문자열의 슬라이싱 기법

: `a[시작_번호:끝_번호]` 형태로 슬라이싱을 할 수 있으며, 시작번호만 포함한다

→ 이때도 인덱스이므로 음수 인덱스를 사용할 수 있다

```python
>>> a = "Life is too short, You need Python"
>>> a[0:4]
'Life'

>>> a = "20230331Rainy"
>>> year = a[:4]
>>> day = a[4:8]
>>> weather = a[8:]
>>> year
'2023'
>>> day
'0331'
>>> weather
'Rainy'
```

### 문자열 포맷팅

```python
# 숫자 바로 대입(변수 대체 가능)
>>> "I eat %d apples." % 3
'I eat 3 apples.'

# 문자열 바로 대입(변수 대체 가능)
>>> "I eat %s apples." % "five"
'I eat five apples.'

# 2개 이상의 값 넣기
>>> number = 10
>>> day = "three"
>>> "I ate %d apples. so I was sick for %s days." % (number, day)
'I ate 10 apples. so I was sick for three days.'
```

### format 함수를 사용한 포멧팅

```python
# 숫자 바로 대입(변수 대체 가능)
>>> "I eat {0} apples".format(3)
'I eat 3 apples'

# 문자열 바로 대입(변수 대체 가능)
>>> "I eat {0} apples".format("five")
'I eat five apples'

# 2개 이상의 값 넣기
>>> number = 10
>>> day = "three"
>>> "I ate {0} apples. so I was sick for {1} days.".format(number, day)
'I ate 10 apples. so I was sick for three days.'

# 이름으로 넣기
>>> "I ate {number} apples. so I was sick for {day} days.".format(number=10, day=3)
'I ate 10 apples. so I was sick for 3 days.'

# 인덱스와 이름을 혼용해서 넣기
>>> "I ate {0} apples. so I was sick for {day} days.".format(10, day=3)
'I ate 10 apples. so I was sick for 3 days.'
```

→ f 문자열 포멧팅을 사용할 경우.. 추가로 찾아보기(3.6버전 이상만 사용 가능)

## 문자열 관련 함수들

### 1. 문자 개수 세기: `count` → return(number)

```python
>>> a = "hobby"
>>> a.count('b')
2
```

### 2. 위치 알려주기 1: `find` → return(first index or -1(x))

```python
>>> a = "Python is the best choice"
>>> a.find('b')
14
>>> a.find('k')
-1
```

### 3. 위치 알려주기 2: `index` → return(first index or ERROR)

```python
>>> a = "Life is too short"
>>> a.index('t')
8
>>> a.index('k')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ValueError: substring not found
```

### 4. 문자열 삽입: `join`

: 해당 문자열의 각각에 삽입

```python
>>> ",".join('abcd')
'a,b,c,d'

# as same as...
>>> ",".join(['a', 'b', 'c', 'd'])
'a,b,c,d'
```

### 5. 문자열 나누기: `split`

: default → 공백(`[Space]`, `[Tab]`, `[Enter]`)을 기준으로 문자열 split

```python
>>> a = "Life is too short"
>>> a.split()
['Life', 'is', 'too', 'short']
>>> b = "a:b:c:d"
>>> b.split(':')
['a', 'b', 'c', 'd']
```

---

# 리스트 자료형

: 리스트 자료형을 사용하여 같은 형태의 자료형 모음을 쉽게 표현하고 관리할 수 있다

### list의 여러가지 표현 방식

: 이때, 빈 리스트는 `a = list()`와 같이 표현할 수 있다

```python
>>> a = []
>>> b = [1, 2, 3]
>>> c = ['Life', 'is', 'too', 'short']
>>> d = [1, 2, 'Life', 'is']
>>> e = [1, 2, ['Life', 'is']]
```

### 다중 구조의 list

: 다음과 같이 작성하면 다중 구조의 리스트를 작성하고 접근할 수 있다(하지만, 매우 복잡하여 자주 사용하지 않는다)

```python
# 선언
>>> a = [1, 2, ['a', 'b', ['Life', 'is']]]

# 접근
>>> a[2][2][0]
'Life'
```

### list의 슬라이싱

: 문자열과 마찬가지로 리스트에서도 슬라이싱 기법을 적용할 수 있다

```python
>>> a = [1, 2, 3, 4, 5]
>>> a[0:2]
[1, 2]
```

### list 연산

- 리스트 더하기(+)
```python
>>> a = [1, 2, 3]
>>> b = [4, 5, 6]
>>> a + b
[1, 2, 3, 4, 5, 6]

```

- 리스트 반복하기(* )
```python
>>> a = [1, 2, 3]
>>> a * 3
[1, 2, 3, 1, 2, 3, 1, 2, 3]
```

- 리스트 길이 구하기
```python
>>> a = [1, 2, 3]
>>> len(a)
3
```

---
# bool 자료형

```python
>>> a = True
>>> b = False

>>> type(a)
<class 'bool'>
>>> type(b)
<class 'bool'>
```

- 자료형에서의 참(`True`)과 거짓(`False`)
![![42Seoul/ft_transcendence/#^Table1]]
---

# 변수













---
# 클래스와 메서드

: 클래스를 통해 데이터와 함수를 하나의 유닛으로 캡슐화하여 사용할 수 있으며, 코드의 재사용성을 높이고 관리를 용이하게 한다

> [!important]
> `Python`에서 클래스를 선언하고 구현하는 것은 (C++와 달리) 하나의 파일에서 모두 수행하는 것이 일반적이다
> 1. **Python의 동적 타이핑**: Python은 동적 타이핑 언어로, 변수의 타입을 명시적으로 선언하지 않고 필요한 곳에서 즉시 정의하고 사용할 수 있다.
> 2. **Python의 간결성**: Python은 간결하고 읽기 쉬운 코드를 강조한다. 파일을 분리하면 코드를 읽고 유지보수하는 데 불필요한 복잡성을 추가할 수 있다.
> 3. **모듈 시스템**: Python은 모듈 시스템을 통해 코드를 구조화한다. 모듈은 하나의 파일로 구성되거나 여러 파일로 구성된 패키지일 수 있다. 각 모듈은 필요한 클래스를 정의하고 구현을 포함할 수 있다.

### 클래스(Class) vs 인스턴스(Instance)

- `Class`: **객체를 생성하기 위한 "틀" 또는 "블루프린트"로, 데이터와 그 데이터를 조작하는 메서드를 정의**한다.
  이는 특정 타입의 객체를 생성하기 위한 설계도와 같으며, 어떤 속성을 가지고 있어야 하며, 객체가 수행할 수 있는 행동은 무엇인지 정의한다.
  클래스는 데이터 멤버(변수)와 메서드를 포함할 수 있다. 데이터 멤버는 객체의 상태를 저장하는데 사용되며, 메서드는 객체의 행동을 정의한다.

- `Instance`: 인스턴스는 **클래스에 정의된 구조를 기반으로 생성된 실제 객체**로, 클래스를 통해 데이터 타입의 특성의 정의하고, 이 클래스의 인스턴스를 생성하여 실제 프로그램에서 사용한다.
  각 인스턴스는 클래스가 정의한 속성과 메서드의 구체적인 실현체로서, 서로 다른 데이터를 저장할 수 있지만 동일한 메서드를 공유한다.

> 즉, 클래스와 인스턴스는 객체 지향 프로그래밍에서 데이터와 행동을 하나의 유닛으로 묶어 관리할 수 있게 해주며, 코드의 모듈성과 재사용성을 높여준다.
> 클래스는 일반적인 틀을 제공하며, 인스턴스는 실제 데이터와 함께 클래스의 행동을 실현한다.

### 클래스의 구조

: 클래스는 `class` 키워드를 사용해서 정의하며, 내부에서 데이터를 저장하는 변수와 클래스의 기능을 정의하는 함수인 메서드를 선언할 수 있다

```python
# class
class ClassName:
	# constructor
	def __init__(self, parameter1, parameter2):
		self.attribute1 = parameters
		self.attribute2 = parameters

	# member functions
	def method_1(self, paramter):
		# do something
		return somthing

	# destructor
	def __del__(self):
		# do something
```

### member variables

python에서 클래스를 정의할 때 **맴버 변수를 명시적으로 선언할 필요가 없다.**
맴버 변수들은 클래스 메서드 내에서 처음 할당될 때 자동으로 생성되기 때문이다.

### constructor

: `__init__` 메서드로 정의되는데, 객체가 생성될 때 자동으로 호출되며, 주로 인스턴스 변수를 초기화하는 데 사용된다.
이때, 내부에서 `self.variable1`, `self.variable2`, ... 과 같은 형태로 변수를 초기화 함과 동시에 맴버 변수가 생성된다.

### destructor

: `__del__` 메서드로 정의되며, 이는 객체의 참조 카운트가 0이 되어 객체가 소멸될 때 자동으로 호출된다. 하지만, python에서는 가비지 컬렉션이 메모리 관리를 담당하기 때문에, 소멸자의 사용은 일반적이지 않으며 권장되지 않는다.

### self

> [!important]
> python's `self` == c++'s `this`

- 파이썬에서 `self` 키워드는 c++의 `this` 포인터와 비슷한 역할을 한다.
  둘 다 클래스의 메서드에서 현재 객체의 인스턴스에 접근하기 위해 사용되며, c++에서는 `this`가 자동으로 각 클래스 메서드에 전달되지만, **python에서 인스턴스 메서드는 `self`를 메서드의 첫 번째 매개변수로 명시적으로 선언해야** 한다.

- `self` 키워드는 클래스의 인스턴스, 즉 클래스로 구현된 실제 해당 객체 자신을 참조하는 변수이다.
  클래스 내의 메서드 정의에서 `self`를 사용하면, 그 메서드가 호출된 인스턴스 객체에 접근할 수 있다.
  이는 인스턴스 변수에 접근하거나, 다른 인스턴스 메서드를 호출하는 데 사용된다.
> [!NOTE]
> Q) 아니근데 왜 파이썬에서는 C++에서의 this를 굳이 안쓰는 것과 달리 명시를 해주어야 본인 인스턴스에 접근할 수 있는가?
> A) `python` 철학에서는 명시적인 것이 암시적인 것보다 낫다는 것을 기반하기 때문에, `self`를 명시적으로 메서드의 첫 번째 매개변수로 지정해야 한다.

### 파이썬에서의 캡슐화

: python에서 캡슐화는 엄격한 접근 제어자를 사용하지 않고도, 명명 규칙과 약속을 통해 캡슐화를 유도하며, 이는 보다 유연하게 데이터와 메서드를 관리할 수 있게 해준다.
다음 예시와 같이, 멤버 변수의 이름 앞에 `_`의 개수에 따라 공개 여부가 달라진다

```python
class Account:
    def __init__(self, owner, amount):
        self.owner = owner         # 공개 속성 (어디서든 직접 접근 가능)
        self._balance = amount     # 내부 사용을 위한 속성
        self.__secret = "123456"   # 이름 변경을 통해 숨겨진 속성

    def deposit(self, amount):
        if amount > 0:
            self._balance += amount
            return True
        return False

    def _internal_method(self):
        # 내부에서만 사용될 메서드
        pass

    def __very_private_method(self):
        # 매우 개인적인 메서드, 이름 변경을 통해 외부 접근 어려움
        pass
```

1. **`_`** + `variableName`: **내부 사용**
   : 해당 속성이나 메서드가 공개 API의 일부가 아님을 의미
   즉, `내부 사용용`이기에 외부에서 접근은 가능하나, 권장하지 않음
   **API나 프레임워크에서는 사용자에 의해 호출되지 않도록 의도된 것**

2. **`_`** + **`_`** + `variableName`: **이름 변경 (자동으로 파이썬 인터프리터가 처리)**
   : 해당 멤버가 파생 클래스에서 **오버라이딩 되는 것을 방지**하려는 목적
   C++에서 **private**으로 선언한 멤버와 같은 역할을 함

3. `variableName`: 일반 공개 멤버
   : 이름에 어떤 특별한 접두어도 붙지 않은 멤버는 공개적으로 접근이 가능(어디서든 접근 가능)

> [!important]
> 즉, 파이썬에서는 접근 제어자가 명시적으로 없는 대신, 관례와 이름 변경 기능을 통해 캡슐화를 지원

## Python Class Method

: `python`에서 클래스 메서드를 구현할 때, 내부 메서드들이 어느 메서드인지에 따라 `self`와 `cls`를 명시적으로 사용해야 한다.
클래스 내부의 메서드를 인스턴스 메서드, 클래스 메서드, 정적 메서드로 구분할 수 있다.

1. `Instance Method: 인스턴스 메서드` (C++의 일반 맴버 함수와 비슷)
   : 클래스의 인스턴스에 속한 메서드로, 항상 첫 번째 매개변수로 인스턴스를 가리키는 `self` 를 받아야 한다.
   이는 객체의 상태에 접근하거나 변경할 때 사용된다.
   **인스턴스가 생성된 상태에서 해당 함수를 호출 가능**

2. `Class Method: 클래스 메서드` (C++의 static 맴버 함수와 비슷) -> `@classmethod`
   : 클래스 자체와 관련된 메서드로, 항상 첫 번째 매개변수로 클래스를 가리키는 `cls` 를 받아야 한다.
   이는 클래스 변수 자체를 조작하거나 클래스 레벨에서 동작을 수행할 때 사용되며, 인스턴스 변수에는 접근할 수 없다.
   **인스턴스가 생성되지 않은 상태에서도 호출 가능**

3. `Static Method: 정적 메서드` (== C++의 static 맴버 함수와 비슷) -> `staticmethod`
   : 클래스와 관련된 독립적인 함수로, 클래스나 인스턴스의 상태를 참조하지 않고, 작업을 수행한다

```python
class MyClass:
    class_variable = 0

    def __init__(self, value):
        self.value = value

    def instance_method(self):
        # 인스턴스 변수 접근 가능
        print(f"Instance method called with value: {self.value}")

    @classmethod
    def class_method(cls, value):
        # 클래스 변수 접근 가능
        cls.class_variable = value
        print(f"Class method called with class_variable: {cls.class_variable}")

    @staticmethod
    def static_method(arg1, arg2):
        # 클래스나 인스턴스 상태와 무관한 작업 수행
        return arg1 + arg2

# 사용
obj = MyClass(10)
obj.instance_method()  # Output: Instance method called with value: 10
MyClass.class_method(20)  # Output: Class method called with class_variable: 20
result = MyClass.static_method(5, 7)
print(result)  # Output: 12

```

**여기서 파이썬의 클래스 메소드에 대해 설명한 이유는, 장고에서는 위와 같은 내용으로 이해하면 혼란이 생기기 때문!  >> 실제 장고에서는 비슷한 형태이지만 작동하는 방식이 조금 다르며, 상속을 사용하므로 구분을 잘 해야한다!!!**


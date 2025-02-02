## Better Way 52. 순환 의존성을 없애는 방법을 알자

#### 266쪽

* Created : 2019/07/02
* Modified: 2019/07/02

<br>

## 1. 파이썬에서 순환 의존성의 문제

불가피하게 다른 이들과 협력하는 동안 모듈 사이의 `순환 의존성`(Circular Dependency)을 발견하기 마련이다. 심지어 혼자서 작업할 때 발생하기도 한다. 일례로 내가 Django 개발을 할 때 서로 다른 app의 views.py 사이에서 순환 의존성을 발견한 기억이 있다.

오늘은 순환 의존성과 그에 대한 해결책을 살펴보도록 한다. 예를 들어 문서를 저장할 위치를 선택하는 대화상자를 보여주는 GUI 어플리케이션을 만든다고 해보자. 대화상자로 표시할 데이터는 이벤트 핸들러의 인수로 설정할 수 있다. 하지만 대화상자는 올바르게 렌더링하는 방법을 알아내려고 사용자 설정과 같은 전역 상태도 읽어야 한다. 즉, **두 개의 모듈이 상대방에 대한 정보를 모두 필요로 하는 것. 전형적인 순환 의존성의 상황이다.**

먼저 다음 대화상자는 전역 설정에서 기본으로 문서를 저장할 위치를 얻어온다.

```python
# dialog.py
import app


class Dialog:
    def __init__(self, save_dir):
        self.save_dir = save_dir


save_dialog = Dialog(app.prefs.get('save_dir'))


def show():
    pass
```

문제는 **프로그램에서 사용자의 선호도와 관련된 _prefs(preferences)_ 객체를 포함하는 app 모듈도 프로그램을 시작할 때 대화상자를 보여주려고 dialog 모듈을 임포트한다는 것이다.**


```python
# app.py
import dialog


class Prefs:
    # ...
    def get(self, name):
        # ...
        pass

prefs = Prefs()
dialog.show()
```

메인 프로그램에서 app 모듈을 임포트하려고 하면 예외가 일어난다. 순환 의존성이 발생했다. 

```python
import app

AttributeError: module 'app' has no attribute 'prefs'
```

**여기서 정확히 어떤 일이 일어나는지, 그리고 에러의 구체적인 내용, 왜 하필 _app_ 모듈에서 _prefs_ 속성이 없다는  _AttributeError_ 가 raise 됐는지 파악해야 한다.** 우리는 app.py에서 분명이 prefs 변수를 정의했기 때문이다.

이를 위해서는 먼저 파이썬의 모듈 임포트 절차를 자세히 알아봐야 한다. 다음은 파이썬에서 깊이 우선 방식으로 모듈을 임포트하면서 실제로 하는 일이다.

중요하니 제대로 살펴보자.

<br>

1. **_sys.path_ 에 들어 있는 경로들에서 요구한 모듈이 존재하는지 검색한다.**
  * sys 내장 모듈의 path는 list로서 파이썬 인터프리터가 임포트할 모듈들을 검색하는 실제 경로들을 담고 있다. 모든 임포트할 모듈은 이 리스트 안에 있는 경로에 위치해야 한다.
2. **모듈을 찾았으면 모듈에서 코드를 로드하고 코드가 컴파일되게 한다.**
3. **모듈과 관련된 기능에 접근가능하게 할 빈 모듈 객체를 생성한다.**
4. **모듈 객체를 sys.modules에 삽입한다.**
  * sys.modules는 dict로서 현재 임포트되어 사용가능한 모듈들에 대해 이름을 key, 실제 경로를 value로 해서 저장하고 있다.
5. **빈 객체에 코드를 실행하면서 모듈의 내용(함수, 클래스, 상수)을 정의한다.(모듈의 변수가 된다.)**

<bR>

**순환 의존성의 문제는 모듈의 속성이 해당 속성의 코드를 실행하기 전에는 정의되지 않는다는 점이다.(과정 5 이후에 정의 완료) 하지만 모듈을 sys.modules에 삽입한 후에는 import 문에서 즉시 로드할 수 있다.(과정 4 이후)**

이에 대한 이해를 바탕으로 앞선 코드의 에러를 파악해보자. 

1. `import app`를 입력하면 app 모듈은 다른 것을 정의하기 전에 첫 번째 줄에 있는 `import dialog`를 먼저 실행한다. 그러면 메인 스레드는 dialog 모듈로 이동해서 관련 내용을 정의하려 한다.
1. dialog 모듈에서 `import app`을 다시 호출한다. **근데 이때 app은 아직 실행 중이므로(현재 dialog를 임포트하는 중), app 모듈은 그냥 비어 있다.(과정 4)**
1. **app 모듈에 prefs를 정의하는 코드가 아직 실행되지 않았기 때문에(app에 대한 과정 5가 완료되지 않음) _AttributeError_ 가 발생하게 된다.**

<br>

이 문제를 해결하는 **최선의 방법은 코드를 리팩토링해서 _prefs_ 자료구조가 의존성 트리의 아래에 오게 하는 것이다.** 그러면 app과 dialog는 같은 유틸리티 모듈을 임포트하므로 순환 의존성을 피한다. 하지만 항상 이처럼 명쾌하게 분리할 수는 없으며, 얻는 장점에 비해 리팩토링 작업에 너무 많은 노력이 들 수도 있다.  

따라서 순환 의존성을 없앨 수 있는 다른 대안 3가지를 살펴보도록 하자.

<br>
<br>

## 2. 순환 의존성을 해결하는 세 가지 방법

### 2.1. 임포트 재정렬

첫 번째 방법은 임포트 순서를 변경하는 것이다. 예를 들어 app 모듈의 끝에서 dialog 모듈을 임포트한다면 그 내용이 실행되고 AttributeError가 사라진다.

```python
# app.py
class Prefs:
    def get(self, name):
        pass

prefs = Prefs()
import dialog # 임포트 위치 변경
dialog.show()
```

dialog.py는 그대로 두고, app.py에서 `import dialog`문만 아래로 내린다. 이 방법은 dialog 모듈이 나중에 로드될 때 app에 대한 재귀 임포트 부분에서 app.prefs가 이미 정의된 상태이기 때문에 에러를 없앤다.  

이 방법으로 AttributeError를 피할 수 있지만, 이는 PEP 8 스타일 가이드를 어기는 것이다. **PEP 8에서는 항상 파이썬 파일의 가장 위에서 임포트하라고 제안한다.** 코드를 처음 접하는 사람이 모듈 의존성을 명확하게 파악하게 하기 위해서다. 또한 의존하는 모듈이 무엇이든 맨 위 스코프에 있게 하여 그 아래에 있는 모든 코드에서 사용할 수 있게 해준다.  

파일에서 나중에 임포트하는 방법은 불안정하기도 하고 코드의 순서를 약간만 바꿔도 모듈이 동작하지 않는 원인이 된다. 따라서 순환 의존성 문제를 해결하려고 임포트 순서를 변경하는 방법은 피해야 한다.


<br>

### 2.2. 임포트, 설정, 실행

두 번째 해결책은 임포트하는 시점에 모듈에서 부작용을 최소화하는 것이다. 모듈에는 함수, 클래스, 상수만 정의해야 한다. 임포트 시점에 실제 함수를 실행하는 일은 피해야 한다. 그런 다음 각 모듈은 다른 모듈이 모두 임포트되고 나서 한 번만 실행할 configure 함수를 제공하는 방식이다. 

**configure의 목적은 다른 모듈의 속성에 접근해서 각 모듈의 상태를 준비하는 것이다. 모든 모듈이 임포트된 후(과정 5 완료) configure를 실행하므로 모든 속성이 반드시 정의되어 있어야 한다.**

이번에는 configure를 호출할 때 prefs 객체만 접근하도록 dialog 모듈을 재정의한다.

```python
# dialog.py
import app

class Dialog:
    def __init__(self, save_dir=None):
        self.save_dir = save_dir

save_dialog = Dialog()

def show():
    pass

def configure():
    save_dialog.save_dir = app.prefs.get('save_dir')
```

**_Dialog_ 클래스의 생성자 인자의 기본값을 _None_ 으로 둔 것을 눈여겨보자. 구체적인 저장경로 설정은 나중에 설정 단계(configure) 함수에서 진행할 것이기 때문에 저장경로가 빈 상태에서도 대화창이 생성 가능하게 한다.**

다음으로 임포트할 때 아무 기능도 실행하지 않도록 app 모듈을 재정의한다.

```python
# app.py
import dialog

class Prefs:
    def get(self, name):
        pass

prefs = Prefs()
dialog.show()


def configure():
    # 무슨 일을 함.
    pass
```

**마지막으로 이들을 종합해 실행할 main 모듈을 만들어서 별개의 실행 단계 세 개(모든 것을 임포트하는 단계, 설정하는 단계, 액션을 실행하는 단계)로 구성한다.**

```python
# 1. 임포트 단계
import app
import dialog

# 2. 설정 단계
app.configure()
dialog.configure()

# 3. 액션 시작 단계
dialog.show()
```

**이 방법은 많은 상황에서 잘 동작하며 [의존성 주입](https://en.wikipedia.org/wiki/Dependency_injection)(Dependency injection)과 같은 패턴을 가능하게 한다.** 하지만 명시적인 configure 단계가 가능한 형태로 코드를 구성하는 건 어려울 수 있다. 모듈에 별개의 두 단계를 두면 설정에서 객체의 정의가 분리되기 때문에 코드를 이해하기 어렵게 하기도 한다.

<br>

### 2.3. 동적 임포트

마지막으로 가장 간단한 방법은 **import 문을 함수나 메소드에서 사용하는 것이다.** 이 방법은 프로그램이 실행하는 동안 모듈 임포트가 발생하며, 프로그램이 처음 시작해서 모듈을 초기화하는 동안에는 발생하지 않으므로 `동적 임포트`(dynamic import)라고 한다.

동적 임포트를 사용해서 dialog 모듈을 재정의하자. 초기화 시전에 dialog 모듈이 app을 임포트하는 대신 dialog.show 함수로 런타임에 app 모듈을 임포트한다.


```python
# dialog.py
class Dialog:
    def __init__(self, save_dir=None):
        self.save_dir = save_dir


save_dialog = Dialog()


def show():
    import app # 동적 import 
    save_dialog.save_dir = app.prefs.get('save_dir')
```

app 모듈은 원래 예제와 같다. 맨 위에서 dialog를 임포트하고 아래에서 dialog.show()를 호출한다.

이 방법은 앞에서 설명한 '임포트, 설정, 실행' 단계와 비슷한 효과를 낸다. **차이는 이 방법을 이용하면 모듈을 정의하고 임포트하는 방식을 구조적으로 변경할 필요가 없다는 것이다.** 단순히 다른 모듈에 접근해야 하는 시점까지 순환 임포트를 미루는 방식이다. 그 시점에 이르면 다른 모듈은 모두 이미 초기화되어 있다고 확신할 수 있다.(과정 5 완료)

**일반적으로 이런 동적 임포트는 피하는 게 좋다.** 그 이유는 import 문의 `비용` 때문이다. 이 비용은 무시못할 정도이며, 특히 복잡한 루프에서는 좋지 않다. 동적 임포트는 실행을 미루는 동작으로 런타임에 상당히 심각한 실패를 야기한다. 예를 들어 SyntaxError 예외가 프로그램이 실행을 시작한 이후에 일어난다. 하지만 이런 단점이 보통 전체 프로그램을 대체하거나 재구성하는 것보다 낫기는 하다.

<br>

정리하자면 순환 의존성은 서로 다른 모듈이 각자 초기화를 마치지 않은 상태에서 상대방을 호출할 때 발생하며 그에 대한 쉬운 해결책은 크게 다음과 같다:

1. import 문 위치 조정
1. 임포트 단계를 임포트, 설정, 실행 단계로 세분화
1. 동적 임포트


<br>
<br>


## 3. 핵심 정리

* 순환 의존성은 두 모듈이 임포트 시점에 서로 호출할 때 일어난다. 이런 의존성은 프로그램이 시작할 때 동작을 멈추는 원인이 된다.
* 순환 의존성을 없애는 가장 좋은 방법은 순환 의존성을 의존성 트리의 아래에 있는 분리된 모듈로 리팩토링하는 것이다.
* 동적 임포트는 리팩토링과 복잡도를 최소화해서 모듈 간의 순환 의존성을 없애는 가장 간단한 해결책이다.

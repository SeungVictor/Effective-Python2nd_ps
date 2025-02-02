## Better Way 57. pdb를 이용한 대화식 디버깅을 고려하자

#### 293쪽

* Created : 2017/08/18
* Modified: 2019/07/04

<br>

## 1. pdb를 통한 대화식 디버깅

누구나 프로그램을 개발할 때 코드에서 버그를 접하기 마련이다. 보통 디버그 할 때 우리는 print를 남발해서 버그 실행 위치를 찾고 올바르게 동작하는지 확인한다. 사정이 더 나으면 테스트 코드를 작성할 수도 있다.  

하지만 이런 도구만으로는 근본적인 원인을 모두 찾아내지 못할 때도 있다. 더 강력한 도구가 필요하다면  파이썬에 내장된 대화식 디버거(interactive debugger)를 사용해볼만 하다.  디버거를 이용하면 프로그램의 상태를 조사하고, 지역변수를 출력하고, 파이썬 프로그램을  한 문장씩 실행해볼 수 있다.  

대부분의 언어와 또 Pycharm 같은 고성능 IDE의 경우에서는 멈추게 할 소스 파일의 줄을 설정한 다음  프로그램을 실행하는 방법으로 디버거를 실행한다.  이와 달리 파이썬에서는 디버거를 사용하는 가장 쉬운 방법은 프로그램을 수정하여  조사할 만한 문제를 만나기 전에 직접 디버거를 실행하는 것이다. 디버거에서 파이썬을 실행하는 것과  평소처럼 실행하는 것 사이에는 별다른 차이가 없다.  **디버거를 시작하려면 내장 모듈 [pdb](https://docs.python.org/3.7/library/pdb.html)를 import한 후 set\_trace 함수를 실행하기만 하면 된다.**

가령 숫자의 짝홀을 판별하는 함수를 만든다고 해보자.

```python
def is_even():
    n = int(input("정수를 입력하세요. : "))
    if n % 2 == 0:
        print("짝수")
    else:
        print("홀수")
```

<br>

이 함수를 pdb로 추적하고 싶다면 함수 안에 다음 문장을 삽입한다.

```python
def is_even()
    import pdb; pdb.set_trace()
    # ... 나머지 코드
```

'import pdb'와 'pdb.set\_trace()' 두 문장을 '#' 하나로 주석화하기 쉽도록 보통 한 문장으로 이어서 작성한다.  이 상태에서 is\_even 함수를 실행하자마자 set\_trace가 있는 지점에서 프로그램은 실행을 멈춘다. 그리고 프로그램을 시작한 터미널은 대화식 파이썬 쉘로 바뀐다.

```python
is_even()

-> n = int(input("정수를 입력하세요. : "))
(Pdb) 
```

이 (Pdb) 뒤에는 파이썬 쉘처럼 명령어들을 입력할 수 있다. 그리고 그 윗줄의 '->'은 파이썬 디버거의 포인터가 현재 함수의 이 줄에 있다는 것을 알 수 있게 한다.  

프롬프트에서 변수의 값을 출력하려면 지역 변수의 이름을 입력하면 된다. 모든 지역 변수의 리스트를 확인하려면 내장 locals 함수를 사용할 수도 있다.  

이 외에도 모듈 import, 전역 상태 조사, 새 객체 생성, help 함수 실행, 심지어 프로그램의 일부를 수정하는 등  디버깅을 보조하는 작업은 무엇이든 다 할 수 있다. 게다가 디버거는 실행 중인 프로그램을 더 쉽게 조사할 수 있는 명령 세 개를 제공한다.

* bt   : 현재 실행 호출 스택의 추적 정보를 출력한다. 이 정보는 프로그램의 어느 부분에 있고 pdb.set\_trace 트리거 지점에 어떻게 도달했는지 보여준다.
* up   : 현재 함수의 호출자 쪽으로 함수 호출 스택의 스코프를 이동한다. 이 동작으로 호출 스택의 상위 레벨에서 지역 변수를 조사할 수 있다.
* down : 함수 호출 스택을 한 단계 낮춘다.

<br>

현재 상태를 조사하고 나면 다음과 같은 디버거 명령을 사용하여 좀더 세밀한 제어로 프로그램 실행을 재개할 수 있다.

* step    : 프로그램의 다음 줄까지 실행하고 제어를 디버거에 돌려준다. 다음 줄의 실행이 함수 호출을 포함하고 있다면 디버거는 호출된 함수 안에서 바로 멈춘다.
* next    : 현재 함수에서 다음 줄까지 프로그램을 실행한 다음 제어를 디버거에 돌려준다. 다음 줄의 실행이 함수 호출을 포함하고 있다면 호출된 함수가 반환할 때까지 디버거가 멈추지 않는다.
* return  : 현재 함수가 값을 반환할 때까지 프로그램을 실행하고 다음 제어를 디버거에 돌려준다.
* continue: 다음 중단점(혹은 set\_trace가 다시 호출될 때)까지 프로그램 실행을 계속한다.

이 함수는 꼭 써볼 것을 권장한다. 여러 함수들을 실행하는 실제 코드에서 동작을 하면 동작 내용을 잘 알 수 있다.  
나도 연습이 더 필요한 것으로 보인다.  

<br>

## 2. 핵심 정리

* *import pdb; pdb.set\_trace()* 문으로 프로그램의 관심 지점에서 직접 파이썬 대화식 디버거를 실행할 수 있다.
* 파이썬 디버거 프롬프트는 실행 중인 프로그램의 상태를 조사하거나 수정할 수 있도록 해주는 파이썬 쉘이다.
* pdb 쉘 명령을 이용하면 프로그램 실행을 세밀하게 제어할 수 있다. 또한 프로그램의 상태를 조사하거나 프로그램 실행을 이어가는 과정을 교대로 반복할 수 있다.

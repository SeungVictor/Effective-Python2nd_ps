# Better way 1. 사용 중인 파이썬의 버전을 알아두라


#### 28쪽

* Created : 2022/07/31

<br>

파이썬2는 더 이상 사용하지 않는다. 파이썬3도 2018년6월 배포된 3.7 이상 사용
<br>

CLI에서 버전 인자를 통해 확인하기

이는 파이썬뿐만 아니라 다른 프로그램들에서도 일반적인 경우이다.  
우리는 `python` 이라는 명령어를 입력하고 파이썬 인터프리터를 실행하는데(CLI에서),  
알다시피 다양한 옵션을 줄 수 있다. 이때 파이썬의 버전을 확인할 수 있는 옵션은 다음 두 가지이다.

<br>

```sh   
python --version
python -V
>>> Python 3.9.12
```

둘은 완벽히 같은 결과를 낸다. 두 번째 `V`는 대문자이다.  
이 명령어를 통해 파이썬 버전이 3.9.12라는 것을 확인했다.  

<br>

내장모듈 sys 사용하기

버전을 확인하는 다른 방법은 파이썬의 내장 모듈 `sys`를 사용하는 것이다.  
이 모듈에는 파이썬의 버전을 출력해주는 특성이 두 개 있다.  

하나는 `sys.version`으로 파이썬 버전을 문자열로 출력해준다.  

<br>

```python
import sys

print(sys.version)

>>> 3.9.12 (main, Apr  4 2022, 05:22:27) [MSC v.1916 64 bit (AMD64)]
```

`sys.version`는 순수한 문자열이다.  

또 다른 방법은 `sys.version_info`를 사용하는 것이다.  
이런 경우가 있을 수 있다. 파이썬 코드로 파이썬 프로그램을 실행하는데,  
파이썬 버전에 따라 실행하는 코드를 다르게 한다고 하자.  

파이썬 2와 3일 때를 구분해야 하는데, `sys.version`을 쓴다면 이 문자열을 슬라이싱하려고 할 것이다.  
근데 그것뿐만 아니라 '3.2'인지, '3.6'인지도 구분해야 한다면 슬라이싱이 보다 복잡해질 것이다.  
그때 `sys.version_info`를 쓴다.  

<br>

가령 '3.9.12'라는 버전에서 '.'을 사이로 각 버전 정보가 구분되는데,

* 3은 major
* 9는 minor
* 12는 micro

라고 .불린다. `version_info`는 [named tuple](https://docs.python.org/3/library/collections.html#collections.namedtuple){:target="_blank"}로서,  **major, minor, micro라는 key로 각 숫자에 접근할 수 있다.**  
자세히 살펴보면 다음과 같다.  


```python
import sys

sys.version_info

>>> sys.version_info
sys.version_info(major=3, minor=9, micro=12, releaselevel='final', serial=0)

```

위와 같은 방법으로는 파이썬 프로그램의 major, minor, micro 버전 값을 정확하고 쉽게 파악할 수 있다.


<br>
<br>
<br>


#### 요점

* Python3 이상 사용 할 것
* 파이썬 실행 파일이 원하는 버전인지 확인


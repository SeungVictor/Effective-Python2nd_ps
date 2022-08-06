# Better way 03. bytes와 str의 차이를 알아두라

#### 34쪽

* Created : 2022/08/06  

<br>

**파이썬(3.0이상)에서 문자열 데이터의 시퀀스를 나타내는 방식은 bytes와 str 2가지다.**
<br>
[1bytes 설명참고](https://zepeh.tistory.com/313)<br>
[__str__,__repr__](https://vaert.tistory.com/196)<br>
[데이터 표현방법,ascii,unicode](https://slow-and-steady-wins-the-race.tistory.com/34)<br>

## 1. bytes(1byte = 8bits)란?
컴퓨터 메모리에서 1byte 당 하나의 주소값이 할 당 된다.<br>
1byte에는 하나의 영문자, 숫자, 기호, 공백을 담을 수 있다.<br>
(한글이나 한자 등의 문자는 2 bytes나 그 이상으로 표현해야 함)<br>
bytes type의 instance에는 부호가 없는 8bit([unsigned char, 0~255](https://dojang.io/mod/page/view.php?id=30))가 그대로 들어간다.<br>
문자, 숫자, 기호 등을 컴퓨터가 이해 할 수 있는 0과 1의 조합으로 나태내려면 
일정한 규칙(encoding)이 필요하다.
<br>
-> ASCII(가장 처음 만들어진 encoding)

### 1.1 ASCII코드란?
<br>
ASCII코드는 1Byte를 이용하여 데이터를 표현하는데 error를 탐지하는 [Parity Bit](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ansdbtls4067&logNo=220886661657)를 제외하고
<br>
2^7(128)개의 문자를 표현한다.<br>

```python
a = b'h\x65llo'
print(list(a))
print(a)
>>>
[104, 101, 108, 108, 111]
b'hello'
```

ASCII코드는 다양한 언어를 컴퓨터 속 데이터로 표현하기에는 한계가 있으므로 거의 모든 언어를 표현할 수 있는 코드가 탄생한다.
<br>
-> Unicode

### 1.2 Unicode란?
[Unicode참고](https://norux.me/31)<br>
[한글과유니코드](https://gist.github.com/Pusnow/aa865fa21f9557fa58d691a8b79f8a6d)<br>
[유니코드howto](https://docs.python.org/ko/3/howto/unicode.html#the-string-type)
<br>
유니코드(https://www.unicode.org/)는 인간 언어에서 사용하는 모든 문자를 나열하고 각 문자에 고유한 코드를 부여하고자 하는 명세입니다.<br>
새로운 언어와 기호를 추가하기 위해 유니코드 명세가 계속 개정되고 갱신됩니다.<br>
Unicode는 2bites+a를 사용하여 2^16 + a 개수만큼 표현 가능<br>
U+라는 접두어가 붙어있으면 Unicode라는 의미이다. ASCII 코드의 0x41은 대문자 A이고
이를 Unicode로 표현하면 U+0041이다.
<br>

## 2. str타입이란?
python의 str instance에는 사람이 사용하는 언어의 문자를 표현하는 unicode point가 들어 있다.
```python
a = 'a\u0300 propos'
print(list(a))
print(a)
>>>
['a', '̀', ' ', 'p', 'r', 'o', 'p', 'o', 's']
à propos
```
유니코드 문자열은 코드 포인트의 시퀀스이며, 0부터 0x10FFFF (십진수 1,114,111)의 범위를 갖는 수이다. 
이 코드 포인트의 시퀀스는 메모리에서 코드 단위(code units)의 집합으로 표현될 필요가 있고, 그런 다음 코드 단위는 8비트 바이트로 매핑된다. 
유니코드 문자열을 바이트 시퀀스로 변환하는 규칙을 문자 인코딩(character encoding), 또는 단지 인코딩(encoding)이라고 부른다.
<br>

## 3. 정리
* Python 3.0부터 str형은 unicode문자를 포함함-> 어떤 문자열이든 "",''를 사용해서 묶는다면 유니코드로 저장됨을 뜻함.
```python
something = 'hello world'
this = something.encode('utf-8')

print(type(this))
-> <class 'bytes'>

print(type(this.decode('utf-8')))
-> <class 'str'>
```

```python
#bytes 방식
a = b'\xed\x8c\x8c\xec\x9d\xb4\xec\x8d\xac'
print(a)
print(a.decode())
>>>
b'\xed\x8c\x8c\xec\x9d\xb4\xec\x8d\xac'
파이썬
```
```python
#str방식
b = '파이썬'
print(b)
print(b.encode())
```
* 시스템 인코딩 검사 

```python
#python에서 검사시
import sys
print(sys.stdin.encoding)
print(sys.stdout.encoding)
->
utf-8
UTF-8

#terminal에서 시스템인코딩 검사(윈도우이므로 cp949)
python -c 'import locale; print(locale.getpreferredencoding())'
->cp949
```
* python의 bytes방식이 ASCII를 뜻하는 것이 아님 ASCII는 encoding규약임
* ASCII에는 한글을 표현할 수 없음 python의 bytes방식으로 한글 '파'를 표현할때 
utf-8(default encoding(항상default는아님), hex(16진수)로 표현해줌
```python
'파'.encode()
-> b'\xed\x8c\x8c'
'파'.encode('euc-kr')
-> b'\xc6\xc4'
'파'.encode('utf-8')
-> b'\xed\x8c\x8c'
'파'.encode('ascii')
->UnicodeEncodeError: 'ascii' codec can't encode character '\ud30c' in position 0: ordinal not in range(128)
```

* bytes에는 8bit값의 sequence가 들어 있고, str에는 unicode code point의 sequence가 들어있다.
* 처리할 입력이 원하는 문자 sequence(8bit 값, UTF-8로 encoding된 문자열, unicode point들)인지 확실히 하려면
도우미함수를 사용하라.
* bytes와 str instance를( >, ==, +, %와 같은) 연산자에 섞어서 사용할 수 없다.
* 이진 데이터(binary data)를 파일에서 읽거나 쓰고 싶으면 항상 binary mode('rb'나 'wb')로 파일을 열어라.






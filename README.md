# __Effective Python 2nd__

파이썬 문법 검색하다가 우연히 [shoark7](https://github.com/shoark7)님이 깃헙을 발견 <br>
책을 구매해놓고 펴볼생각조차 못했던 effective python을 아래처럼 정리하신걸 보고<br>
기존에 정리하신걸 참고하면서 정리하면 많은 공부가 될거같아 이곳에 기록해보려고 합니다.<br>



* [『파이썬 코딩의 기술』](https://book.naver.com/bookdb/book_detail.naver?bid=16876960)의 90장의 내용을 <ins>__개인공부 목적으로__</ins> 정리해보고자 함
* 지은이 브렛 슬라킨 
* 옮긴이 오현석
* (주) 도서출판 길벗
* 초판 4쇄(2021-12-20)




<br>

## 목차


### 1장. 파이썬답게 생각하기

1. [Better way 01. 사용 중인 파이썬의 버전을 알아두라][way1]
2. [Better way 02. PEP 8 스타일 가이드를 따르라][way2]
3. [Better way 03. bytes와 str의 차이를 알아두라][way3]
4. [Better way 04. C 스타일 형식 문자열 str.format과 쓰기보다는 f-string을 통한 인터폴레이션을 사용하라][way4]
5. [Better way 05. 복잡한 식을 쓰는 대신 도우미 함수를 작성하라][way5]
6. [Better way 06. 인덱스를 사용하는 대신 대입을 사용해 데이터를 언패킹하라][way6]
7. [Better way 07. map과 filter 대신에 리스트 컴프리헨션을 사용하자][way7]
8. [Better way 08. 리스트 컴프리헨션에서 표현식을 두 개 넘게 쓰지 말자][way8]
9. [Better way 09. 컴프리헨션이 클 때는 제너레이터 표현식을 고려하자][way9]
10. [Better way 10. range보다는 enumerate를 사용하자][way10]
<!--
11. [Better way 11. 이터레이터를 병렬로 처리하려면 zip을 사용하자][way11]
12. [Better way 12. for와 while 루프 뒤에는 else 블록을 쓰지 말자][way12]
13. [Better way 13. try/except/ese/finally에서 각 블록의 장점을 이용하자][way13]


### 2장. 함수

14. [Better way 14. None을 반환하기 보다 예외를 일으키자][way14]
15. [Better way 15. 클로저가 변수 스코프와 상호 작용하는 방법을 알자][way15]
16. [Better way 16. 리스트를 반환하는 대신 제너레이터를 고려하자][way16]
17. [Better way 17. 인수를 순회할 때는 방어적으로 하자][way17]
18. [Better way 18. 가변 위치 인수로 깔끔하게 보이게 하자][way18]
19. [Better way 19. 키워드 인수로 선택적인 동작을 제공하자][way19]
20. [Better way 20. 동적 기본 인수를 지정하려면 None과 docstring을 사용하자][way20]
21. [Better way 21. 키워드 전용 인수로 명료성을 강요하자][way21]


### 3장. 클래스와 상속

22. [Better way 22. 딕셔너리와 튜플보다는 헬퍼 클래스로 관리하자][way22]
23. [Better way 23. 인터페이스가 간단하면 클래스 대신 함수로 받자][way23]
24. [Better way 24. 객체를 범용으로 생성하려면 @classmethod 다형성을 이용하자][way24]
25. [Better way 25. super로 부모 클래스를 초기화하자][way25]
26. [Better way 26. 믹스인 유틸리티 클래스에만 다중 상속을 사용하자][way26]
27. [Better way 27. 공개 속성보다는 비공개 속성을 사용하자][way27]
28. [Better way 28. 커스텀 컨테이너 타입은 collection.abc의 클래스를 상속받게 만들자.][way28]


### 4장. 메타클래스와 속성

29. [Better way 29.  게터와 세터 메서드 대신에 일반 속성을 사용하자][way29]
30. [Better way 30.  속성을 리팩토링하는 대신 @property를 고려하자][way30]
31. [Better way 31.  재사용 가능한 @property 메서드에는 디스크럽터를 사용하자][way31]
32. [Better way 32.  지연 속성에는 \_\_getattr\_\_, \_\_getattribute\_\_, \_\_setattr\_\_을 사용하자][way32]
33. [Better way 33.  메타클래스로 서브클래스를 검증하자][way33]
34. [Better way 34.  메타클래스로 클래스의 존재를 등록하자][way34]
35. [Better way 35.  메타클래스로 클래스 속성에 주석을 달자][way35]


### 5장. 병행성과 병렬성

36. [Better way 36.  자식 프로세스를 관리하려면 subprocess를 사용하자][way36]
37. [Better way 37.  스레드를 블로킹 I/O용으로 사용하고 병렬화용으로는 사용하지 말자][way37]
38. [Better way 38.  스레드에서 데이터 경쟁을 막으려면 Lock을 사용하자][way38]
39. [Better way 39.  스레드 간의 작업을 조율하려면 Queue를 사용하자][way39]
40. [Better way 40.  많은 함수를 동시에 실행하려면 코루틴을 고려하자][way40]
41. [Better way 41.  진정한 병렬성을 실현하려면 concurrent.futures를 고려하자][way41]


### 6장. 내장 모듈

42. [Better way 42.  functions.wrap로 함수 데코레이터를 정의하자][way42]
43. [Better way 43.  재사용 가능한 try/finally 동작을 만들려면 contextlib와 with문을 고려하자][way43]
44. [Better way 44.  copyreg으로 pickle을 신뢰할 수 있게 만들자][way44]
45. [Better way 45.  지역 시간은 time이 아닌 datetime으로 표현하자][way45]
46. [Better way 46.  내장 알고리즘과 자료구조를 사용하자][way46]
47. [Better way 47.  정밀도가 중요할 때는 decimal을 사용하자][way47]
48. [Better way 48.  커뮤니티에서 만든 모듈을 어디서 찾아야 하는지 알아두자][way48]


### 7장. 협력

49. [Better way 49.  모든 함수, 클래스, 모듈에 docstring을 작성하자][way49]
50. [Better way 50.  모듈을 구성하고 안정적인 API를 제공하려면 패키지를 사용하자][way50]
51. [Better way 51.  루트 Exception을 정의해서 API로부터 호출자를 보호하자][way51]
52. [Better way 52.  순환 의존성을 없애는 방법을 알자][way52]
53. [Better way 53.  의존성을 분리하고 재현하려면 가상 환경을 사용하자][way53]


### 8장. 제품화
54. [Better way 54.  배포 환경을 구성하는 데는 모듈 스코프 코드를 고려하자][way54]
55. [Better way 55.  디버깅 출력용으로는 repr 문자열을 사용하자][way55]
56. [Better way 56.  unittest로 모든 것을 테스트하자][way56]
57. [Better way 57.  pdb를 이용한 대화식 디버깅을 고려하자][way57]
58. [Better way 58.  최적화하기 전에 프로파일하자][way58]
59. [Better way 59.  tracemalloc으로 메모리 사용 현황과 누수를 파악하자][way59] -->





[way1]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay01_KnowThyself.md
[way2]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay02_PythonStyleGuide.md
[way3]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay03_Bytes_Str_Unicode.md
[way4]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay04_f-string.md
[way5]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay05_UseHelpFunction.md
[way5]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay05_HowToSequenceSlice.md
[way6]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay06_Unpacking.md
[way7]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay07_useListComp.md
[way8]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay08_ListComprehension.md
[way9]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay09_UseGeneratorExpression.md
[way10]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay10_useEnumerate.md
[way11]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay11_UseZip.md
[way12]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay12_dontuse_else.md
[way13]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay13_use_tryetc.md
[way14]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay14_useexception.md
[way15]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay15_useClosure.md
[way16]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay16_generator.md
[way17]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay17_IterateDefensively.md
[way18]:https://github.com/SeungVictor/Effective-Python2nd_psblob/master/files/BetterWay18_PositionalArg.md
[way19]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay19_KeywordArg.md
[way20]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay20_DynamicDefaultArgument.md
[way21]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay21_ForceKeywordArgument.md
[way22]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay22_UseHelperClass.md
[way23]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay23_UseFuncForInterface.md
[way24]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay24_classmethod.md
[way25]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay25_InitializeSuperClassWithSuper.md
[way26]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay26_UseMixinClass.md
[way27]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay27_UsePrivateAttribute.md
[way28]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay28_CustomContainer_collections.abc.md
[way29]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay29_dontusegetter.md
[way30]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay30_Use@property_for_refactoring.md
[way31]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay31_UseDescriptorForReusablePropertyMethod.md
[way32]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay32_Use__getattr__and_etc_for_lazy_attributes.md
[way33]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay33_ValidateSubclassWithMetaclass.md
[way34]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay34_RegisterClassWithMetaclass.md
[way35]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay35_UseDocstringWithMetaclass.md
[way36]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay36_Usesubprocess.md
[way37]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay37_UseThreadForIO.md
[way38]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay38_UseLockForRaceConditionInThread.md
[way39]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay39_UseQueueToTuneUpTasksInThreads.md


[way42]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay42_Use_functoolswraps.md
[way43]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay43_UseContextlib.md
[way44]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay44_UsePickleWithCopyreg.md
[way45]:https://github.com/SeungVictor/Effective-Python2nd_psblob/master/files/BetterWay45_UseDatetimeForLocalTime.md
[way46]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay46_UseBuiltinAlgorithmsAndDataStructures.md
[way47]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay47_UseDecimalForPrecision.md
[way48]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay48_PypiModules.md


[way49]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay49_WriteDocstring.md
[way50]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay50_UsePackage.md
[way51]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay51_DefineRootException.md
[way52]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay52_HowToGetRidOfCircularDependency.md
[way53]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay53_UseVirtualEnvironment.md


[way54]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay54_ConsiderModuleScopeForDeployment.md
[way55]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay55_UseReprForDebug.md
[way56]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay56_UseUnittest.md
[way57]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay57_Use_pdb.md
[way58]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay58_ProfileBeforeOptimization.md
[way59]:https://github.com/SeungVictor/Effective-Python2nd_ps/blob/master/files/BetterWay59_UseTracemalloc.md

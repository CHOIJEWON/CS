# Memory

프로그램이 실행되기 위해서는 프로그램이 메모리에 로드(load)되어야 한다 프로그램이 메모리에 로드된 상태를

프로세스라고 하는데 그렇다면 프로세스를 알아보기전 `memory`가 도대체 무엇인지 알아보자

# | _Memory Structure_ |

메모리에 프로그램을 로드한다는 것은 코드가 메모리를 할당 받는다는 얘기이다

따라서 컴퓨터의 운영체제는 프로그램 실행을 위해 다양한 메모리 공간을 제공하고 각각의 메모리 공간은

상호작용하며 프로그램 실행에 이바지한다 프로그램이 운영체제로부터 할당받는 대표적인 메모리 공강은 다음과 같다

<center>

![memory](./img//memory_structure.png)

</center>

<br>

1. 코드(code) 영역: 실행할 프로그램의 소스 코드

2. 데이터(data) 영역: 전역 변수, static 변수

3. 스택(stack) 영역: **컴파일** 타임에 크기가 결정됨

4. 힙(heap) 영역: **런타임시** 크기가 결정됨(동적으로 할당)

> 컴파일 타임: 소스코드가 실행가능한 기계어코드로 변환되어 머신에서 실행가능한 프로그램이 되며, 이러한 편집과정을 컴파일 타임이라고 함
>
> 런타임: 컴파일 과정을 마친 프로그램이 사용자에 의해 실행되고 이러한 응용플그램이 동작되는 시점을 런타임이라고 칭함

<br>

# | _코드 영역_ |

메모리의 코드(code) 영역은 실행할 프로그램의 코드가 저장되는 영역으로 텍스트(code) 영역이라고도 부른다

CPU는 코드 영역에 저장된 명령어를 하나씩 가져가서 처리하게 됨

<br>

# | _데이터(data) 영역_ |

메모리의 데이터(data) 영역은 프로그램의 전역 변수와 정적(static) 변수가 저장되는 영역

데이터 영역은 프로그램의 시작과 함께 할당이 완료되며, 프로그램이 종료되면 소멸됨

<br>

# | _스택(stack) 영역_ |

메모리의 스택(stack) 영역은 함수의 호출과 관계되는 지역 변수와 매개 변수가 저장되는 영역이다

스택 영역은 함수의 호출과 함께 할당되며, 함수의 호출이 완료되면 소멸하게 된다

이렇게 스택 영역에 저장되는 함수의 호출 정보를 스택 프레임(stack frame)이라고 칭한다

스택 영역은 (LIFO, Last In First Out) 알고리즘으로 데이터를 인출한다

스택 영역은 메모리의 높은 주소에서 낮은 주소의 방향으로 할당한다

<br>

# | _힙(heap) 영역_ |

메모리의 힙(heap) 영역은 사용자가 직접 관리할 수 있는 '그리고 해야만 하는' 메모리 영역

힙 영역은 사용자에 의해 메모리 공간이 동적으로 할당되고 해제됨

힙 영역은 메모리의 낮은 주소에서 높은 주소의 방향으로 할당됨

Java와 마찬가지로 JavaScript또한 GC의 동작 범위에 해당하는 곳이다
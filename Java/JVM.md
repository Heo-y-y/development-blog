# JVM의 역할

## JVM이란?

**JVM**은 **Java Virtual Machine**의 줄임 말로, **스택 기반으로 동작**하며, **Java Byte Code를 OS에 맞게 해석** 해주는 역할을 하며 **가비지컬렉션을 통해 자동적인 메모리 관리**를 해준다.

### JVM의 중요성

- 동일한 기능을 하는 프로그램이라도 메모리 관리에 따라 성능이 좌우될 수 있다.
- 메모리가 관리 되지 않을 경우 속도 저하나 프로그램의 중단 및 시스템의 부하 및 장애 등으로 발생할 가능성이 있다.
- **한정된 메모리를 효율적으로 사용하여 최고의 성능**을 끌어낼 수 있다.

## JVM 구조 및 작동 원리

![스크린샷 2023-11-23 오후 11.22.21.png](https://github.com/Heo-y-y/development-blog/assets/112863029/47825837-12f4-44a5-a6c0-b7dca1103c91)

**동작 순서**

1. 프로그램이 실행되면 JVM은 OS로부터 프로그램이 필요로 하는 메모리를 할당 받고, JVM은 해당 메모리를 용도에 따라 여러 영역으로 나누어서 관리한다.
2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어들여 자바 바이트코드(.class)로 변환한다.
3. Class Loader를 통해 class 파일들을 JVM으로 로딩한다.
4. 로딩된 class 파일들은 Exceuction engine를 통해 해석된다.
5. 해석된 바이트코드는 Runtime Data Area에 배치되어 실질적인 수행이 이루어진다.
6. 이러한 과정 속에서 JVM은 필요에 따라 Thread Synchronization, GC 같은 관리 작업을 수행한다.

JVM의 구조는 크게 Class Loader, Runtime Data Area, Execution Engine, GC로 나뉘어져 있다.

- **Class Loader**: 클래스 파일을 Runtime Data Area의 메서드 영역으로 불러오는 역할
- **Execution Engine**: .class 파일과 같은 바이트코드를 실행 가능하도록 해석
- **GC**(**Garbage Collector**): 메모리 관리 기법 중 하나로, Heap 영역에 배치된 객체들을 관리하는 모듈
- **Runtime Data Area**: 런타임 시 클래스 데이터와 같은 메타 데이터와 실제 데이터가 저장된다.

간단하게 말하면, 프로그램을 수행하기 위해 OS로부터 할당 받은 메모리 영역을 의미하는데, Runtime Data Area에는 또 다시 Method Area, Heap, PC Registers, Java Stacks, Native Method Stacks로 나뉘어진다.

![스크린샷 2023-11-23 오후 11.38.22.png](https://github.com/Heo-y-y/development-blog/assets/112863029/41e35057-d9c5-4a9c-896f-ca3b477d33c3)

참고로 Java는 멀티 스레드 환경으로 모든 스레드는 Heap, Method Area를 공유한다.

- **PC Register**
    - JVM은 스택 기반의 가상 머신으로, CPU에 직접 접근하지 않고, Stack에서 주소를 뽑아서 가져온다. 가져오는 주소는 Register에 저장된다.
    - 따라서 현재 어떤 명령을 실행해야 할 지에 대한 기록을 담당한다.
- **JVM Stacks**
    - 호출된 메서드의 파라미터, 지역 변수, 리턴 값 및 연산 값 등이 저장되는 영역이다.
    - 프로그램 실행 시 임시로 할당되었다가 메서드를 빠져나가게 되면 소멸되는 특성의 데이터들이 저장되는 영역이다.
    - 메서드 호출 시마다 스택에 각각의 스택 프레임이 생성되고, 수행이 끝나면 스택 포인트에서 해당 프레임을 제거한다.
- **Native Method Stacks**
    - Java 이외의 언어에 제공되는 Method의 정보가 저장되는 공간이다.
    - Java Native Interface를 통해 바이트 코드로 저장된다.
    - Kernel이 자체적으로 Stack을 잡아 독자적으로 프로그램을 실행시키는 영역이다.
- **Heap**
    - GC(가비지 컬렉션)의 대상이 되는 영역이다.
    - 객체를 동적으로 생성하게 되면 인스턴스가 Heap 영역의 메모리에 할당된다.
    - 단, 레퍼런스 변수의 경우 Heap에 인스턴스가 저장되는 것이 아닌 포인터가 저장된다.
    
    Permanent Generation
    
    - 생성된 객체들의 주소 값이 저장된 공간이다.
    - Class Loader에 의해 Load되는 Class, Method 등에 대한 메타 정보가 저장되는 영역이다.
    - JVM에 의해 사용되고, Reflaction을 사용하여 동적으로 클래스가 로딩되는 경우에 사용된다.
    - Java 8부터는 Mata space라는 영역으로 대체되었다.
    
    Meta Space
    
    - 기존 Permanent Generation JVM에 의해 크기가 강제되었었다.
    - Native 메모리 영역으로 OS가 자동으로 크기를 조절한다.
    
    New / Young Generation
    
    - Eden: 객체들이 최초로 생성되는 공간
    - Suvivor 0/1: Eden에서 참조되는 객체들이 저장되는 공간
    - Eden 영역이 가득차면 첫 번째 GC가 발생하게 된다. Eden 영역에 있는 값들을 survivor 영역 중 한 곳에 복사하고, 나머지 영역에 있는 객체들을 삭체한다. → Minor GC
    
    Tenured Generation
    
    - New / Young Gerneration에서 일정 시간 이상 참조되고 있는 객체들이 저장되는 공간이다.
    - survivor 영역에 계속해서 살아남는 객체들을 이곳으로 옮긴다.
    - 모든 객체들을 검사하여 참조되지 않은 객체를 전부 삭제한다.
    - 시간이 오래 걸리고, GC를 실행하는 스레드를 제외하고는 전부 작업을 멈추고 GC가 완료되면 다시 실행된다. stop-the-world 라고도 한다. → Major GC
- **Method Area**
    - 클래스 정보를 처음 메모리에 올릴 때 초기화되는 대상을 저장하기 위한 영역이다.
    - Runtime Constant Pool이라는 별도의 관리 영역을 통해 상수 자료형을 저장하여 참조하고 중복을 막는다.
    - GC의 관리 대상에 포함된다.

**참고 자료**

- <https://junghyungil.tistory.com/94>
- <https://dev-coco.tistory.com/153>
- [https://velog.io/@pearl0725/JVM은-어떤-역할을-할까](https://velog.io/@pearl0725/JVM%EC%9D%80-%EC%96%B4%EB%96%A4-%EC%97%AD%ED%95%A0%EC%9D%84-%ED%95%A0%EA%B9%8C)

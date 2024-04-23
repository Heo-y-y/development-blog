# Java - assert

Java 1.4부터 assert 구문을 사용할 수 있다고 한다. assert 구문은 프로그램에 대한 가정을 테스트할 수 있는데, 프로그램의 오류를 감지하고 수정하는 방법을 제공한다.

즉, 코드의 논리적인 조건이 올바르게 설정되었는지 확인하는 용도로 사용된다. 주로 개발 및 테스팅 단계에서 유효성 검사나 내부 불변성을 확인하는데 유용하다.

### 사용법

1. **기본 형식**

```java
assert 조건;
```

조건이  false인 경우 AssertionError를 발생시킨다.

2. **메시지 포함 형식**

```java
assert 조건 : "에러 메시지";
```

조건이 false인 경우 주어진 에러 메시지와 함께 AssertionError를 발생시킨다.

### 예제

```java
class Assertion {
	public static void main(String args[]) {
		
		int value = -1;
		assert value >= 0: "음수입니다.";
		
		System.out.print("value: " + value);
	}
}
```

위 코드를 실행할 경우 아래와 같이 출력된다.

```java
Exception in thread "main" java.lang.AssertionError: 음수입니다.
```

### assert와 exception의 차이점

assert와 exception은 크게 아래와 같은 차이점을 가지고 있다.

1. **활성화 / 비활성화**

기본적으로 JVM에서 assert는 비활성 상태이다. 따라서 assert를 사용하기 위해서는 실행 시 -ea(enable assertions) 옵션을 통해 활성화해야 한다.

IntelliJ에서는 VM options에 -ea를 추가하여 활성화할 수 있다.

![스크린샷 2024-04-23 오후 3.08.49.png](https://github.com/Heo-y-y/development-blog/assets/112863029/549595e9-8f8f-4e75-8eee-3de1c0074541)

참고로 VM Options 입력창이 없는 경우 이미지 우측에 Modify options를 눌러 Add VM options를 체크하면 된다.

2. **Error / Exception**

![Untitled](https://github.com/Heo-y-y/development-blog/assets/112863029/d135a714-26ad-4117-84ce-f7d3ffb7ed22)

assert와 exception의 차이점은 assert는 `AssertionError`라는 Error를 발생시키지만, exception은 특정 예외 상황에 대응하는 `Exception`을 발생시킨다.

`Error`는 `Exception`과 본질적으로 다르다.

- 예외(Eception): 입력 값에 대한 처리가 불가능하거나, 프로그램 실행 중에 참조된 값이 잘못된 경우 등 정상적인 프로그램의 흐름을 어긋나는 것
- 에러(Error): `OutOfMemoryError`, `StackOverflowError`와 같이 발생하면 복구할 수 없는 심각한 오류
3. **프로그램 종류**

`Exception`은 적절한 처리(try-catch)를 통해 프로글매의 정상적인 흐름으로 복귀하도록 하여 애플리케이션이 강제로 종료가 되지 않지만, assert는 실패 시 `AssertionError`를 발생시키기 때문에 애플리케이션이 강제로 종료가 된다.

### 정리

`Exception`과 `Error`는 프로그램의 정상적인 흐름을 방해하는 문제를 나타내지만, 그 원인과 대응 방법은 다르다. 각각 특별한 사용 사례에 맞게 설계되어야 하며 개발자는 두 가지를 혼동하지 않고 적절한 상황에 사용하는 것이 좋다.

또한 실제 배포 환경에서는 `-ea` 옵션을 꺼서 assert로 인한 실제 서비스에서 예기치 않은 종료를 방지해야하는 것이 일반적이다.

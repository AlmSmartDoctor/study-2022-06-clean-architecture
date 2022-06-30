# 진행 자료
# 10장 - ISP: 인터페이스 분리 원칙
*특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다*

인터페이스 분리 원칙은 다음의 다이어그램으로부터 유래한다.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fblsa6C%2FbtqZUVLDy0z%2FyAW8iMv6jzCL9kZkCfdsEk%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fblsa6C%2FbtqZUVLDy0z%2FyAW8iMv6jzCL9kZkCfdsEk%2Fimg.png)

User1은 오직 op1을 User2는 op2 만을 User3는 op3만을 사용한다고 가정해보자. 이 떄 User1에서는 op2와 op3를 사용하지 않아도 두 메서드에 의존하므로, op2의 소스코드가 변경되면 User1도 다시 컴파일해서 배포해야 한다. 이를 아래 그림과 같이 인터페이스 단위로 분리해 해결할 수 있다.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbzEMJy%2FbtqZSuA1Dz0%2FXYH3xsm7jOizUUhZTqIJb1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbzEMJy%2FbtqZSuA1Dz0%2FXYH3xsm7jOizUUhZTqIJb1%2Fimg.png)

### ISP와 언어

- 정적 타입언어는 사용자가 import, use 또는 include 와 같은 타입 선언문을 사용하도록 강제한다.
- 이처럼 소스코드에 보함된 선언문으로 인해 의존성이 발생한다.
- 동적타입 언어에는 이러한 선언문이 존재하지 않는다.

### ISP와 아키텍처

S 시스템 구축에 참여하고 있는 아키텍트가 있다고 가정해보자.

- 아키텍트는 F라는 프레임워크를 시스템에 도입하고자 한다.
- F프레임워크 개발자는 D 데이터베이스를 반드시 사용하도록 만들었다고 가정해본다.
- D 내부에서 변경이 발생하면, F를 재배포해야한다. 이 경우 S까지 재배포해야 할 수도 있다.

### 결론

인터페이스를 분리해야하는 이유

**무언가에 의존하면 예상치도 못한 문제에 빠진다. 따라서 인터페이스로 결합도를 낮춘다.**

# 11장 - DIP: 의존성 역전 원칙
*프로그래머는 추상화에 의존해야지, 구체화에 의존하면 안된다.*

```
의존성 역전 원칙에서 말하는, 유연성이 극대화된 시스템이란 소스코드 의존성이 추상에 의존하며 구체에는 의존하지 않는 시스템이다.
```

자바를 예시로 들면, import 구문은 오직 인터페이스나 추상 클래스만 참조해야 한다는 뜻이다.


이런 식으로 구체 클래스인 (java.lang.String)은 참조하면 안되며
```java
import java.lang.String
...
```

이와 같이 인터페이스와 추상 클래스만 의존해야한다.

```java
import com.example.Interface1
import com.example.AbstractClass1
...
```

그러나 이 규칙은 비현실적이다. 애시당초 String 클래스와 같은 것들은 변경가능성이 매우 낮고, 있더라도 엄격하게 통제된다.

그러므로 변동성이 큰 구체적 요소에 대해 DIP를 적용해야 한다.

### 안정된 추상화

인터페이스의 변동을 낮추기 위해 다음과 같은 실천해보자.

- 변동성이 큰 구체 클래스를 참조하지 말라
- 변동성이 큰 구체 클래스로부터 파생하지 말라
- 구체 함수를 오버라이드 하지 말라

### 팩토리

바람직하지 않은 의존성(변동성이 큰 구체)을 처리하기 위해 자바에서는 보통 추상 팩토리를 사용한다. 

![https://velog.velcdn.com/images%2Fno-brand%2Fpost%2F36c62886-e975-4cfb-8ac2-ff71b26fdd2f%2F967BE606-5F64-4AC1-9232-5EA78A3ABD59.jpeg](https://velog.velcdn.com/images%2Fno-brand%2Fpost%2F36c62886-e975-4cfb-8ac2-ff71b26fdd2f%2F967BE606-5F64-4AC1-9232-5EA78A3ABD59.jpeg)

- 의존성은 추상적인 쪽으로 향하게 된다.
- 곡선은 시스템을 추상적인 컴포넌트와 구체적인 컴포넌트로 분리한다. 

**소스코드의 의존성은 제어흐름과는 반대로 역전이 된다.**
**그래서 이 원칙을 의존성 역전(Dependency Injection)이라고도 부른다.**

### 구체 컴포넌트

위의 그림에선 구체적인 의존성이 하나 존재한다. DIP에 위배되지만, DIP 위배를 모두 없애는건 불가능하다.

대다수의 시스템은 이러한 구체 컴포넌트를 최소한 하나는 포함해야하는데, 이러한 컴포넌트를 보통 메인(Main)이라 부른다.

### 결론

의존성을 반대로 바꾸는 이유

**변하지 않는 것(변동성이 적은 것)에 의존해야 수정하고 배포할일이 적어지기 때문**

# 12장 - 컴포넌트

소프트웨어 개발 초창기에는 메모리에 프로그램 위치를 직접 프로그래머가 제어했다.

당시 시절에는 한 번 프로그램 위치가 결정이 되면, 재배치가 불가능하였다. 

이 당시에는 라이브러리 함수에 접근하려면 모든 라이브러리 함수 소스 코드를 하나의 어플리케이션 코드 한에 직접 다 넣어야했다.

그러나 이 시절에는 메모리가 비싸고 자원이 한정적이어서 문제가 되었다. 소스코드를 메모리에 모두 적재할 수 없는 문제가 발생하였다.


# 질의응답

```text
지인:

대답: (PR에서 suggest로 커밋하기 편하게 질문 작성 후 이 구문은 지워주세요 ㅎㅎ)
```

```text
하진:

대답: (PR에서 suggest로 커밋하기 편하게 질문 작성 후 이 구문은 지워주세요 ㅎㅎ)
```

```text
진영:

대답: (PR에서 suggest로 커밋하기 편하게 질문 작성 후 이 구문은 지워주세요 ㅎㅎ)
```

```text
규훤:

대답: (PR에서 suggest로 커밋하기 편하게 질문 작성 후 이 구문은 지워주세요 ㅎㅎ)
```

```text
진호:

대답: (PR에서 suggest로 커밋하기 편하게 질문 작성 후 이 구문은 지워주세요 ㅎㅎ)
```

```text
승직:

대답: (PR에서 suggest로 커밋하기 편하게 질문 작성 후 이 구문은 지워주세요 ㅎㅎ)
```

```text
천규:

대답: (PR에서 suggest로 커밋하기 편하게 질문 작성 후 이 구문은 지워주세요 ㅎㅎ)
```

```text
성준:

대답: (PR에서 suggest로 커밋하기 편하게 질문 작성 후 이 구문은 지워주세요 ㅎㅎ)
```

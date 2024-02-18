## JUnit5

> Java를 위한 테스트 프레임워크

### JUnit5란?

**JUnit5** = JUnit Platform + JUnit Jupiter + JUnit Vintage
이전 버전의 JUnit과 다르게 3개의 서브 프로젝트에서 온 다른 모듈들을 통합한 것.

**구성 요소**

- **JUnit Platform** : JVM에서 테스트 프레임워크를 실행하기 위한 기반을 제공
- **JUnit Jupiter** : JUnit5에서 테스트와 확장을 작성하기 위한 프로그래밍 모델과 확장 모델의 조합
- **JUnit Vintage** : JUnit4와 이전 버전의 테스트를 실행하기 위한 `TestEngine` 제공

### JUnit5 테스트 코드 구조

JUnit5는 어노테이션을 기반으로 테스트 코드임을 나타내며, 테스트 코드는 반환형이 `void`여야함.

```java
/*
 * A first test case
 * https://junit.org/junit5/docs/current/user-guide/#writing-tests
 */
import static org.junit.jupiter.api.Assertions.assertEquals;

import example.util.Calculator;
import org.junit.jupiter.api.Test;

class MyFirstJUnitJupiterTests {
    private final Calculator calculator = new Calculator();

    @Test
    void addition() {
        assertEquals(2, calculator.add(1, 1));
    }
}
```

### JUnit5 Annotations

**JUnit Jupiter**는 테스트 설정과 프레임워크 확장을 위해 다음과 같은 어노테이션을 제공.

모든 핵심 어노테이션은 `org.junit.jupiter.api` 패키지, `junit-jupiter-api` 모듈에 속함.

| 주요 Annotation      | 설명                                                            |
| -------------------- | --------------------------------------------------------------- |
| `@Test`              | 메소드가 테스트 메소드임을 나타냄                               |
| `@ParameterizedTest` | 메소드가 매개변수화 테스트임을 나타냄                           |
| `@RepeatedTest`      | 메소드가 반복 테스트임을 나타냄                                 |
| `@DisplayName`       | 테스트 클래스 또는 메소드의 이름을 지정함                       |
| `@Nested`            | 해당 클래스가 중첩 클래스임을 나타냄                            |
| `@Disabled`          | 메소드가 테스트 메소드로서 동작하지 않도록 지정함               |
| `@BeforeEach`        | 메소드가 **각** 테스트 메소드 **실행되기 전**에 실행되도록 함   |
| `@AfterEach`         | 메소드가 **각** 테스트 메소드 **실행된 후**에 실행되도록 함     |
| `@BeforeAll`         | 메소드가 **모든** 테스트 메소드 **실행되기 전**에 실행되도록 함 |
| `@AfterAll`          | 메소드가 **모든** 테스트 메소드 **실행된 후**에 실행되도록 함   |

### JUnit5 Methods

대부분의 메소드는 `org.junit.jupiter.api.Assertions` 클래스에 정의되어 있음. 따라서 `Assertions` 클래스를 `static`으로 `import`하여 사용하는 것이 좋음.

| 주요 Method          | 설명                                                                  |
| -------------------- | --------------------------------------------------------------------- |
| `assertEquals`       | 두 **값**이 같은지 비교, 같다면 성공, 다르다면 실패                   |
| `assertNotEquals`    | 두 **값**이 다른지 비교, 다르다면 성공, 같다면 실패                   |
| `assertSame`         | 두 **객체**가 같은지 비교, 같다면 성공, 다르다면 실패                 |
| `assertNotSame`      | 두 **객체**가 다른지 비교, 다르다면 성공, 같다면 실패                 |
| `assertThrows`       | 특정 예외가 발생하는지 확인, 발생하면 성공, 발생하지 않으면 실패      |
| `assertDoesNotThrow` | 특정 예외가 발생하지 않는지 확인, 발생하지 않으면 성공, 발생하면 실패 |
| `assertAll`          | 여러 검증 코드를 모두 함께 실행하고 하나라도 실패하면 실패            |

### 주의 사항

- 테스트 메소드는 **무조건** `void`를 반환해야함
- 테스트 메소드는 한글 사용을 자제해야함
  - 일부 IDE, 라이브러리, 프레임워크와의 호환을 위함
  - 일관성 유지를 위함
- `@Nested`가 없으면 중첩 클래스로 인식하지 못하고 내부 테스트 메소드도 동작하지 않음
- 테스트 메소드는 `private`이어서는 안됨
- 테스트 메소드는 추상 메소드(`abstract`)이어서는 안됨
- `equals`를 재정의하지 않은 클래스는 `assertEquals`를 사용할 수 없음

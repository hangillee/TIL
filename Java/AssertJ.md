## AssertJ

> JUnit5를 더욱 편리하게 사용할 수 있도록 돕는 라이브러리

### AssertJ란?

AssertJ는 JUnit과 마찬가지로 여러 모듈들의 집합.

- **AssertJ Core**: JDK의 여러 자료형을 위한 `assertion`을 제공.
- **AssertJ Guava**: Google Guava의 자료형들을 위한 `assertion`을 제공.
- **AssertJ Joda Time**: Joda Time의 자료형들을 위한 `assertion`을 제공.
- **AssertJ Neo4J**: Neo4J의 자료형들을 위한 `assertion`을 제공.
- **AssertJ DB**: 관계형 DB의 자료형들을 위한 `assertion`을 제공.
- **AssertJ Swing**: Swing UI를 위한 간단하고 직관적인 테스트 API 제공.

이 중, 가장 가장 자주 사용되는 모듈은 **AssertJ Core**.

### AssertJ Core

AssertJ는 풍부한 `assertion`들과 유용한 오류 메시지를 제공하여 테스트 코드의 가독성을 높임.

다음은 간단한 AssertJ Core의 `assertion` 사용 예시. JUnit5를 대체한다기 보단 둘 다 활용하면서 선택지를 늘리고 더 편리한 방식을 제공한다고 생각. 다음은 공식 페이지에서 제공하는 다양한 `assertion`들의 예제.

```java
// https://assertj.github.io/doc/#assertj-core
// static import가 주로 사용됨
// 모든 assertion들의 entry point
import static org.assertj.core.api.Assertions.*;

// 기본 assertions
// JUnit5의 assertThat과 같은 역할을 함
// 그럼 왜 쓰느냐?
// AssertJ는 JUnit5와 크게 다르지 않으나 더 직관적인 메소드 이름을 활용
assertThat(frodo.getName()).isEqualTo("Frodo");
assertThat(frodo).isNotEqualTo(sauron);

// 문자열 검증 assertions 메소드 체이닝
// AssertJ는 Fluent API를 활용하기 때문에 체이닝이 가능
assertThat(frodo.getName()).startsWith("Fro")
                           .endsWith("do")
                           .isEqualToIgnoringCase("frodo");

// 컬렉션 검증 assertions (이것보다 더 많음)
// fellowshipOfTheRing = List<TolkienCharacter>
assertThat(fellowshipOfTheRing).hasSize(9)
                               .contains(frodo, sam)
                               .doesNotContain(sauron);

// as는 테스트를 묘사하기 위해 사용. 오류 메시지 직전에 출력됨.
assertThat(frodo.getAge()).as("check %s's age", frodo.getName()).isEqualTo(33);

// 예외 assertions. 표준 스타일.
assertThatThrownBy(() -> { throw new Exception("boom!"); }).hasMessage("boom!");
// BDD 스타일 (Behavior-Driven Development)
Throwable thrown = catchThrowable(() -> { throw new Exception("boom!"); });
assertThat(thrown).hasMessageContaining("boom");

// extracting을 활용한 컬렉션에 포함된 객체들의 특정 필드 검증
assertThat(fellowshipOfTheRing).extracting(TolkienCharacter::getName)
                               .doesNotContain("Sauron", "Elrond");

// extracting 튜플에 있는 여러 필드들을 한번에 검증
assertThat(fellowshipOfTheRing).extracting("name", "age", "race.name")
                               .contains(tuple("Boromir", 37, "Man"),
                                         tuple("Sam", 38, "Hobbit"),
                                         tuple("Legolas", 1000, "Elf"));

// assertion전 컬렉션 필터링
assertThat(fellowshipOfTheRing).filteredOn(character -> character.getName().contains("o"))
                               .containsOnly(aragorn, frodo, legolas, boromir);

// 필터링과 추출을 함께 사용
assertThat(fellowshipOfTheRing).filteredOn(character -> character.getName().contains("o"))
                               .containsOnly(aragorn, frodo, legolas, boromir)
                               .extracting(character -> character.getRace().getName())
                               .contains("Hobbit", "Elf", "Man");

// 더 많은 assertions: iterable, stream, array, map, dates, path, file, numbers, predicate, optional ...
```

> ❗️ **AssertJ는 Java 8 이상의 JDK가 요구됨**

### AssertJ Quick Start

> 의존성 추가: https://assertj.github.io/doc/#get-assertj-core

AssertJ를 사용하기 위해서 유일하게 필요한 클래스는 `Assertions` 뿐. 이 클래스가 모든 필요한 메소드들을 제공. `WithAssertions`라는 클래스를 대신 사용해도 같은 메소드를 제공함.

```java
import static org.assertj.core.api.Assertions.assertThat;

import org.junit.jupiter.api.Test;

public class SimpleAssertionsExample {
    @Test
    void a_few_simple_assertions() {
        assertThat("The Lord of the Rings").isNotNull()
                                        .startsWith("The")
                                        .contains("Lord")
                                        .endsWith("Rings");
  }
}
```

가장 간단한 `String` 검증 테스트.

### AssertJ Methods

AssertJ에서 제공하고 있는 많은 메소드들 중 자주 사용되는 메소드 정리.

**assertThat()**
JUnit5의 `assertThat()`과 같은 역할을 함. `assertEqauls()`, `assertNull()`과 같이 검증하고자 하는 상황마다 따로 메소드가 있었던 JUnit5와 다르게, `assertThat()` 메소드에 테스트하고자 하는 객체를 두고, 각 상황에 맞는 메소드를 체이닝을 통해 추가하여 검증함. 개인적으로 AssertJ의 방식이 더 직관적.

```java
// AssertJ example
assertThat(actual).isEqualTo(expected);

// JUnit5 example
assertEquals(expected, actual);
```

| 주요 Method    | 설명                                                                         |
| -------------- | ---------------------------------------------------------------------------- |
| `isEqualTo`    | `assertThat`의 값이 매개변수에 대입된 값과 **같은지 비교**                   |
| `isNotEqualTo` | `assertThat`의 값이 매개변수에 대입된 값과 **같지 않은지 비교**              |
| `isNull`       | `assertThat`의 객체가 **`null`인지 검증**                                    |
| `isNotNull`    | `assertThat`의 객체가 **`null`이 아닌지 검증**                               |
| `isSameAs`     | `assertThat`의 객체가 매개변수에 대입된 객체와 **같은지 비교**               |
| `isNotSameAs`  | `assertThat`의 객체가 매개변수에 대입된 객체와 **같지 않은지 비교**          |
| `contains`     | `assertThat`의 객체에 매개변수에 대입된 객체가 **포함하는지 비교**           |
| `startsWith`   | `assertThat`의 문자열이 매개변수에 대입된 문자열로 **시작하는지 비교**       |
| `endsWith`     | `assertThat`의 문자열이 매개변수에 대입된 문자열로 **끝나는지 비교**         |
| `matches`      | `assertThat`의 문자열이 매개변수에 대입된 **정규표현식과 일치하는지 검증**   |
| `size`         | `assertThat`의 **컬렉션**의 크기를 반환. 여러 메소드와의 체이닝에 활용       |
| `extracting`   | `assertThat`의 **컬렉션**에서 특정 필드를 추출하여 여러 메소드 체이닝에 활용 |

`assertThat`은 이것보다 훨씬 많은 기능을 제공. 혹시 이런 것도 있나?라는 생각이 든다면 공식 문서를 확인해 볼 것.

> https://www.javadoc.io/doc/org.assertj/assertj-core/latest/org/assertj/core/api/Assertions.html#assertThat(T)

---

**assertThatThrownBy()**
JUnit5의 `assertThrows()`와 같은 역할을 함. 예외를 발생시키는 코드를 검증하는 메소드. 매개변수에 예외가 발생하는 `lambda`식을 넣어주면 예외가 정상적으로 발생되는지 검증.

| 주요 Method            | 설명                                                              |
| ---------------------- | ----------------------------------------------------------------- |
| `isInstanceOf`         | 발생한 예외가 매개변수에 대입된 예외 클래스의 인스턴스인지 검증   |
| `hasMessage`           | 발생한 예외의 메시지가 매개변수에 대입된 값과 **일치하는지 비교** |
| `hasMessageContaining` | 발생한 예외의 메시지에 매개변수에 대입된 값이 **포함되는지 비교** |

---

**assertThatCode()**
`assertDoesNotThrow()`와 같은 역할을 함. 코드가 예외가 발생하지 않는지 검증하는 메소드. 매개변수에 예외가 발생하지 않을 것으로 기대되는 코드를 `lambda`식으로 넣어주면 예외가 발생하지 않는 것을 검증.

```java
assertThatCode(() -> {
    final var number = Integer.valueOf(0x80000000);
}).doesNotThrowAnyException();
```

| 주요 Method                | 설명                                               |
| -------------------------- | -------------------------------------------------- |
| `doesNotThrowAnyException` | `lambda` 내부의 코드가 예외가 발생하지 않는지 검증 |

### 주의 사항

- `as()`나 `overridingErrorMessage()`와 같이 `assertions`가 아닌 메소드 체이닝 과정에서 추가 정보를 제공하는 메소드는 마지막에 와서는 안됨. 즉, `isEqualTo()`나 `contains()`와 같은 `assertions` 메소드가 마지막에 와야 함.
- `assertThat()` 메소드에 조건문, `boolean` 반환 메소드를 활용해도 아무 것도 `assert`하지 않고 통과됨. `assertThat()` 메소드에는 검증할 대상이 들어가야 함.
  ```java
      assertThat(actual.equals(expected)); // ❌
      assertThat(actual).isEqualTo(expected); // ✅
      assertThat(actual.equals(expected)).isTrue(); // ✅
  ```

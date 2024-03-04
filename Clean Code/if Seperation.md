## `if` Seperation

> `if`문도 의미를 부여할 수 있다.

### 한 메소드는 하나의 기능만 가져야함.

좋은 코드를 작성하기 위해서는 메소드는 하나의 기능만 가져야함. 하나의 메소드가 여러 기능을 가지고 있으면, 유지보수하기 어려워짐. 좋은 코드의 가장 중요한 기준은 유지보수성과 확장성인 것을 생각하면, 메소드는 최대한 작게 유지해야함.

그런데, 특정 상태에 대한 검증 로직이나, 분기가 많은 코드의 경우, **`if`문이 하나의 메소드 안에 여러개가 존재**하게되는 일이 많음. 이때, 각 **`if`문 하나에 의미를 부여해 메소드로 분리**하는 것이 좋은 코드를 작성하는 방법.

이렇게 `if`문을 메소드로 분리하게 되면 해당 `if`문이 어떤 역할을 하고, 무엇을 검증하는지 알기 쉽게 함. 또한, 검증 조건이 변경되어도 다른 메소드는 건들지 않고 빠르게 코드를 수정할 수 있음.

### `if`문 분리 예시

매개변수로 전달 받은 이름이 올바른 값인지 검증하는 기능(메소드).

```java
// Bad case
private void validatName(final String name) {
    if (name == null) {
        throw new IllegalArgumentException("이름은 null일 수 없습니다.");
    }

    if (name.isEmpty()) {
        throw new IllegalArgumentException("이름은 빈 문자열일 수 없습니다.");
    }

    if (name.isBlank()) {
        throw new IllegalArgumentException("이름은 공백일 수 없습니다.");
    }

    if (name.contains(" ")) {
        throw new IllegalArgumentException("이름에 공백이 포함될 수 없습니다.");
    }

    if (name.length() < 2) {
        throw new IllegalArgumentException("이름은 2글자 이상이어야 합니다.");
    }
}
```

위의 나쁜 예시만 보더라도 한 메소드가 가지고 있는 기능이 너무 많은 것을 알 수 있음. 각 `if`문은 `name`이 `null`인지 확인하는 기능, 빈 문자열(`""`)인지 확인하는 기능, 공백이거나 공백을 포함하고 있는지 확인하는 기능, 그리고 길이가 2글자 이상인지 확인하는 기능까지, 분리해서 독립시킬 수 있는 기능들이 눈에 보임. 이렇게 코드에서 개선점을 찾았을 때, 상황에 맞춰 `TODO` 주석을 남겨두어 잊지 않도록 하거나 바로 개선해야함.

```java
// Good case
private void isNameNotNull(final String name) {
    if (name == null) {
        throw new IllegalArgumentException("이름은 null일 수 없습니다.");
    }
}

private void isNameNotEmpty(final String name) {
    if (name.isEmpty()) {
        throw new IllegalArgumentException("이름은 빈 문자열일 수 없습니다.");
    }
}

private void isNameNotBlank(final String name) {
    if (name.isBlank()) {
        throw new IllegalArgumentException("이름은 공백일 수 없습니다.");
    }
}

private void isNameNotContainBlank(final String name) {
    if (name.contains(" ")) {
        throw new IllegalArgumentException("이름에 공백이 포함될 수 없습니다.");
    }
}

private void isNameLengthOverLimit(final String name) {
    if (name.length() < 2) {
        throw new IllegalArgumentException("이름은 2글자 이상이어야 합니다.");
    }
}
```

한 메소드에 있었던 여러 `if`문들을 각각 메소드로 분리함. 이 과정을 통해 `if`문에 역할을 부여했고, 메소드의 이름만 읽어도 해당 `if`문이 무엇을 검증하는지 알 수 있게 됨. 이렇게 코드를 작성하면 요구사항 상의 검증 조건이 변경되어도 쉽게 코드를 수정할 수 있고, 다른 동료 개발자가 코드를 읽었을 때 빠르게 이해할 수 있음.

또한, 새로운 검증 로직이 추가되어도 메소드만 빠르게 추가하면 되기 때문에, 코드의 확장성도 높아짐. 읽기 쉽고, 이해하기 쉽고, 고치기 쉽고, 확장하기 쉬운 코드가 완성되었으니 유지보수성과 확장성이 모두 높은 좋은 코드라 할 수 있음.

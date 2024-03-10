# 강의 - TDD 소개 및 시연

[🔗 유튜브 - 세미나 공유 - TDD 소개 및 시연 by 최범균](https://www.youtube.com/watch?v=6Vt-wKPBbuc&list=PLwouWTPuIjUj_QqgXlFsqjUwyC0-5dZ_q&index=5)

[🔗 암호 검사기 TDD로 구현하기](https://github.com/Jaster25/tdd-practice)

<br>

## 요구사항

- 다음 규칙을 모두 충족하면 매우 강함이다.
  - 길이가 8글자 이상
  - 0부터 9 사이의 숫자를 포함
  - 대문자 포함
- 2개의 규칙을 충족하면 보통이다.
- 1개 이하의 규칙을 충족하면 약함이다.

<br>

## TDD 과정

### Step1.1 - 암호 검사기 null, empty 테스트 코드 작성

```java
@DisplayName("인자가 null인 경우 예외 발생")
@Test
void nullInput() {
    // given

    // when

    // then
    assertThrows(IllegalArgumentException.class,
            () -> new PasswordChecker().check(null));
}

@DisplayName("인자가 빈 값인 경우 예외 발생")
@Test
void emptyInput() {
    // given

    // when

    // then
    assertThrows(IllegalArgumentException.class,
            () -> new PasswordChecker().check(""));
}
```

### Step1.2 - 암호 검사기 클래스 생성

```java
public class PasswordChecker {

    public void check(String password) {
        throw new IllegalArgumentException();
    }
}
```

<br>

### Step2.1 - 암호 강도: 강함 테스트 코드 작성

```java
@DisplayName("모든 조건 충족 -> 암호 강도: 강함")
@Test
void meetAllRules() {
    // given

    // when
    PasswordStrength passwordStrength1 = passwordChecker.check("abcABC123");
    PasswordStrength passwordStrength2 = passwordChecker.check("1ab2cA3BC");

    // then
    assertEquals(PasswordStrength.STRONG, passwordStrength1);
    assertEquals(PasswordStrength.STRONG, passwordStrength2);
}
```

### Step2.2 - 암호 검사기 로직 추가 및 암호 강도 Enum 생성

```java
public class PasswordChecker {

    public PasswordStrength check(String password) {
        if (password == null || password.isEmpty()) {
            throw new IllegalArgumentException();
        }

        return PasswordStrength.STRONG;
    }
}
```

```java
public enum PasswordStrength {

    STRONG,
    NORMAL,
    WEAK
}
```

<br>

### Step3.1 - 암호 강도: 보통(길이가 8 미만) 테스트 코드 작성

```java
@DisplayName("길이가 8 미만, 다른 조건들은 충족 -> 암호 강도: 보통")
@Test
void meetDigitAndUppercaseRules() {
    // given

    // when
    PasswordStrength result1 = passwordChecker.check("aBC123");
    PasswordStrength result2 = passwordChecker.check("1B3C2");

    // then
    assertEquals(PasswordStrength.NORMAL, result1);
    assertEquals(PasswordStrength.NORMAL, result2);
}
```

### Step3.2 - 길이가 8미만 조건 구현

```java
boolean lengthRule = password.length() >= 8;
if (!lengthRule) {
    return PasswordStrength.NORMAL;
}
```

<br>

### Step4.1 - 암호 강도: 보통(대문자 없음) 테스트 코드 작성

```java
@DisplayName("대문자 없음, 다른 조건들은 충족 -> 암호 강도: 보통")
@Test
void meetDigitAndLengthRules() {
    // given

    // when
    PasswordStrength result1 = passwordChecker.check("abcd12345");
    PasswordStrength result2 = passwordChecker.check("12ab34cde");

    // then
    assertEquals(PasswordStrength.NORMAL, result1);
    assertEquals(PasswordStrength.NORMAL, result2);
}
```

### Step4.2 - 대문자 조건 구현

```java
boolean uppercaseRule = false;
for (char ch : password.toCharArray()) {
    if (Character.isUpperCase(ch)) {
        uppercaseRule = true;
        break;
    }
}
if (!uppercaseRule) {
    return PasswordStrength.NORMAL;
}
```

<br>

### Step - 리팩토링

- 조건 코드 메서드로 추출

<br>

### Step5.1 - 암호 강도: 보통(숫자 없음) 테스트 코드 작성

```java
@DisplayName("숫자 없음, 다른 조건들은 충족 -> 암호 강도: 보통")
@Test
void meetUppercaseAndLengthRules() {
    // given

    // when
    PasswordStrength result1 = passwordChecker.check("abcdABCD");
    PasswordStrength result2 = passwordChecker.check("aAbBcCdDeE");

    // then
    assertEquals(PasswordStrength.NORMAL, result1);
    assertEquals(PasswordStrength.NORMAL, result2);
}
}
```

### Step5.2 - 숫자 조건 구현

```java
boolean digitRule = containsDigit(password);
if (!digitRule) {
    return PasswordStrength.NORMAL;
}
```

```java
private boolean containsDigit(String password) {
    for (char ch : password.toCharArray()) {
        if (Character.isDigit(ch)) {
            return true;
        }
    }
    return false;
}
```

<br>

### Step6.1 - 암호 강도: 약함 테스트 코드 작성

1. 길이만 충족
2. 대문자만 충족
3. 숫자만 충족

### Step6.2 - 암호 강도: 약함 조건 구현

<br>

### Step - 리팩토링

<br>

### Step7.1 - TDD 완료 후 소스 위치 변경

- test -> main

<br>

## 📚 References

- [유튜브 - 세미나 공유 - TDD 소개 및 시연 by 최범균](https://www.youtube.com/watch?v=6Vt-wKPBbuc&list=PLwouWTPuIjUj_QqgXlFsqjUwyC0-5dZ_q&index=5)

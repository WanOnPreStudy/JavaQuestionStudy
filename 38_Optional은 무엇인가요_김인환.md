## Optional은 무엇인가요?

`Optional<T>`는 '`T`타입의 객체'를 감싸는 래퍼 클래스이다.

### Optional의 용도 (옵셔널 반환은 신중히 하라, 이펙티브 자바 아이템 55)

자바 8전에는 메서드가 특정 조건에서 값을 반환할 수 없을 때, 예외를 던지거나 null을 반환했다. 두 방법 모두 허점이 있다.

- 예외는 진짜 예외 상황에만 사용해야 한다. (이펙티브 자바 아이템 69)
  - 또, 예외 생성 시 스택 추적에 비용이 든다.
- null을 반환할 수 있는 메서드 호출 시, null 처리 코드를 추가해야 한다.
  - 만약, 무시하고 처리하지 않으면 나중에 원인과는 상관없는 곳에서 NPE가 발생할 수 있다.

`Optional<T>`는 null이 아닌 `T`타입 참조를 하나 담거나, 아무것도 담지 않을 수 있다. <br>
이를 이용해 보통은 `T`를 반환하지만 특정 조건에서는 아무것도 반환하지 않을 때, `T` 대신 `Optional<T>`를 반환하도록 선언하면 된다.


### Optional 객체의 생성
- of()
- ofNullable()

#### of() 사용 예시
```java
String str = "abc";
Optional<String> optVal = Optional.of(str);
Optional<String> optVal = Optional.of("abc");
Optional<String> optVal = Optional.of(new String("abc"));
```

#### ofNullable() 사용 예시
```java
Optional<String> optVal = Optional.of(null); // NullPointerException 발생
Optional<String> optVal = Optional.ofNullable(null);
```

#### Optional 객체 초기화
```java
Optional<String> optVal = null; // 바람직하지 않음
Optional<String> optVal = Optional.empty();
```

### Optional 값 가져오기
- isPresent()
- get()
- orElse()
- orElseGet()
- orElsThrow()

#### Optional 값 가져오기 예시
```java
if(Optional.ofNullable(str).isPresent()) {
    System.out.println(str);
}

Optional<String> optVal = Optional.of("abc");
String str1 = optVal.get(); // 저장된 값 반환, null이면 예외 발생
String str2 = optVal.orElse(""); // 저장된 값이 null일 때, ""반환
String str3 = optVal.orElseGet(String::new); // () -> new String()와 동일
String str4 = optVal.orElseThrow(NullPointerException::new); // null이면 지정한 예외 발생
```
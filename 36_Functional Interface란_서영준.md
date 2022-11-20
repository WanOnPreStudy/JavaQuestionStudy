# 일급 객체

함수형 프로그래밍에서 일급객체(First-class Object)란 다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체를 말합니다.

```java
function mul(num) {
  return num*num;
}

// func는 매개변수임, 이름은 아무거나 지정해도 상관없음
function mulNum(func, number) {
  return func(number);
}

let result = mulNum(mul, 3); // 9
```

**1. 변수나 데이터에 할당 할 수 있어야 한다.**

**2. 객체의 인자로 넘길 수 있어야 한다.**

**3. 객체의 리턴값으로 리턴 할 수 있어야 한다.**

즉, 함수나 클래스를 일반적인 데이터(string, number, boolean, array, object) 다루 듯이 다룰 수 있다는 것이죠. 일급 객체는 지연 연산이라는 특징을 활용해 콜백 패턴 같은 디자인 패턴의 구현이 가능합니다.

# 익명 클래스

자바에서는 함수를 일급 객체와 유사한 동작을 하기 위해, 단 하나의 메서드만을 가진 인터페이스, 혹은 클래스를 익명 클래스의 형태로 구현해 사용해 왔습니다.

```java
// 익명 클래스 생성
Runnable run = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello World!");
            }
        };

// 익명 클래스 주입
new Thread(run).start();

// 익명 클래스 생성과 동시에 주입
new Thread(new Runnable() {
   @Override
   public void run() { 
      System.out.println("Hello World!"); 
   }
}).start();
```

하지만 익명 클래스로 구현하기엔 코드의 길이가 너무 길어 부적합하고, **가독성을 떨어트린다는 단점**이 있습니다.

# 람다(Lambda Expression)

람다란 함수(메서드)를 간단한 ‘식(expression)’으로 표현하는 방법입니다.  **함수를 보다 단순하게 표현하는 방법**이라고 생각하시면 됩니다,

람다 함수는 이름을 가질 필요가 없기 때문에 익명 함수로서 활용 할 수 있습니다.

```java
// 기존의 방식
반환티입 메소드명 (매개변수, ...) {
	실행문
}

public String hello() {
    return "Hello World!";
}

// 람다 방식
(매개변수, ... ) -> { 실행문 ... }

() -> "Hello World!";
```

람다식을 통해 코드의 길이가 줄어들고 가독성이 좋아졌지만, 람다식을 인수로 주거나 인자로 받을 수 없기에 일급 객체로 활용하기에는 좀 버거워 보입니다.

### 장점

- 코드가 간결해 짐, 식에 개발자의 의도가 명확하게 드러나 가독성이 높아짐
- **지연 연산 수행 가능**
- **병렬 프로그래밍이 용이**하다.

### 단점

- 람다식을 활용하여 만든 **익명 함수는 재사용이 불가능** 하다.
- **디버깅이 어렵다.**
- 남발하면 비슷한 함수가 중복 생성되어 코드가 지저분해 진다.
- 재귀로 만들경우 부적합하다.

# 함수형 인터페이스

**단 하나의 추상 메서드를 가지는 인터페이스**입니다. 내부에 선언된 함수를 1급 객체처럼 다룰 수 있게 해주며, **@Functionallnterface 어노테이션을 통해 명시적으로 선언**할 수 있습니다.

함수형 인터페이스는 내부의 추상 메서드를 구현하는 구현체를 익명 클래스 혹은 **람다식으로 구현해 사용할 수 있습니다.**

```java
// 단 하나의 추상 메서드만을 가지는 함수형 인터페이스
interface MyFunction2 {
    abstract int max(int a, int b);
}

// @FunctionalInterface 어노테이션을 통해 명시적으로 선언한 함수형 인터페이스
@FunctionalInterface
interface MyFunction2 {
    int max(int a, int b);
}
```

> @FunctionalInterface 어노테이션으로 선언된 함수형 인터페이스에 두 개 이상의 메서드를 선언하면 오류를 발생시킵니다.
>

**중요한 것은 람다식으로 구현해서 사용할 수 있다는 부분**입니다.

```java
Runnable run = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello World!");
            }
        };

// 람다식을 변수에 저장
Runnable run = () -> System.out.println("Hello World!");

new Thread(run).start();
```

이제 람다식을 활용해 함수를 변수에 저장할 수도 있고,

# 추가로 공부하면 좋을 키워드

- 다양한 람다 표현식
- 람다식을 활용한 병렬 프로그래밍
- 자바에서 지원하는 java.util.function 인터페이스들
- 콜백 패턴
## Java 8에 추가된 기능

  
**2014년 발표된 자바8버전의 대표적인 추가기능은 아래와 같다.** 

1\. Lamda표현식(Lamda Expression) : 함수형 프로그래밍 시작

2\. Stream API : 데이터의 추상화

3\. java.time 패키지

4\. Interface의 deafault, static메서드

5.Optional<?>

### 1\. Lamda

람다는 메소드를 하나의 식으로 표현한것으로 **식별자 없이 실행할 수 있는 함수표현식**이다. **익명함수**라고 부르기도한다.

람다는 메소드의 매개변수로 전달될 수도 있고 메서드의 결과값으로 반환될수도 있다.

람다를 사용하는 중요목적은 **불필요한 코드를 줄여주고 작성된 코드의 가독성을 높여주기위해서이다.**

```
//정상적인 유형
() -> {}
() -> 1
() -> { return 1; }

(int x) -> x+1
(x) -> x+1
x -> x+1
(int x) -> { return x+1; }
x -> { return x+1; }

(int x, int y) -> x+y
(x, y) -> x+y
(x, y) -> { return x+y; }

(String lam) -> lam.length()
lam -> lam.length()
(Thread lamT) -> { lamT.start(); }
lamT -> { lamT.start(); }


//잘못된 유형 선언된 type과 선언되지 않은 type을 같이 사용 할 수 없다.
(x, int y) -> x+y
(x, final y) -> x+y  
```

#### **함수형인터페이스**

함수형인터페이스란 **Interface 하나에 추상메서드 하나만** 갖고있는 것으로, Interface 타입의  변수에  람다식을 할당하는 것이다.

기존의 자바에서 메서드는 클래스에 포함되어야하는데 람다식은 익명클래스의 객체로 이루어져있어서 익명클래스의 객체로 표현이 가능하다. 

즉 **추상클래스에 존재하는 하나의 메서드를 람다식으로 간편하게 사용할 수 있다는 것**이다.

그리고 함수형인터페이스라는 것을 구분하기 위해 @FunctionalInterface어노테이션을 붙인다.

Java.util.function 패키지의 표준 함수형 인터페이스를 사용하면 된다. 

```
Runnable runnable;
runnable = () -> System.out.println("Test");
runnable = () -> {
    System.out.println("Test");
};

runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("Test");
    }
};

Consumer<String> consumer;
consumer = a -> System.out.println(a);
consumer = (String a) -> System.out.println(a);
consumer = (a) -> {
    System.out.println(a);
};

consumer = new Consumer<String>() {
    @Override
    public void accept(String a) {
        System.out.println(a);
    }
};

Supplier<Double> supplier;
supplier = () -> Math.random();
supplier = () -> {
    return Math.random();
};

supplier = new Supplier<Double>() {
    @Override
    public Double get() {
        return Math.random();
    }
};

BiFunction<String, String, Integer> biFunction;
biFunction = (a, b) -> Integer.parseInt(a) * Integer.parseInt(b);
biFunction = (String a, String b) -> {
    int aValue = Integer.parseInt(a);
    int bValue = Integer.parseInt(b);

    return aValue * bValue;
};

biFunction = new BiFunction<String, String, Integer>() {
    @Override
    public Integer apply(String a, String b) {
        int aValue = Integer.parseInt(a);
        int bValue = Integer.parseInt(b);

        return aValue * bValue;
    }
};
```

#### **메서드 참조** 

람다식을 더 가독성 좋은 형태로 나타내는 것으로 하나의 메서드만 호출하는 경우 사용할 수 있다. 

:: 를 사용하여 코드를 작성한다.

```
//클래스명::메서드명
List<String> list = Arrays.asList("a", "b", "c");

list.stream().forEach(a -> System.out.println(a));
list.stream().forEach(System.out::println);

//변수명::메서드명
String target = "ab";

list.stream().filter(a -> target.contains(a));
list.stream().filter(target::contains);

//클래스명::new
List<Integer> list = Arrays.asList(1, 2, 3);

list.stream().map(a -> new ArrayList<String>(a));
list.stream().map(ArrayList::new); // 이 둘은 동일한 표현이다.
```

### **2. 스트림 API**

스트림 API는 데이터를 추상화하여 다룬다. 그렇기에 각자 다른방식으로 저장된 데이터를 읽고 쓸 때 공통된 방법을 제공한다. 즉 배열이나 컬렉션, 파일에 저장된 데이터까지도 모두 같은 방법으로 다룰 수 있다.  (컬렉션이나 배열과 같은 요소들의 집합에 대한 처리를 효과적으로 할 수 있게 돕는다.)

  
Collection Interface에 stream()메서드가  default method로 추가되었다. 그렇기에 Collection을 상속하는 모든 클래스

는 stream()로 Stream을 생성할 수 있다. 

이 Stream을 사용하면 아래와 같이 코드를 간결하게 나타낼 수 있으며 그 외에도 다양한 기능을 제공한다. 

```
for (String a : list) {
    System.out.println(a);
}

list.stream().forEach(a -> System.out.println(a));
```

```
List<String> list = Arrays.asList("abc", "bac", "ccc", "ccc");

// a를 포함한 문자열이 있는지의 여부를 반환
boolean isExist = list.stream().anyMatch(element -> element.contains("a")); 

// a로 시작하는 문자열만 필터링
Stream<String> stringStream = list.stream().filter(element -> element.startsWith("a"));

// 문자열의 길이로 이루어진 스트림으로 변환
Stream<Integer> integerStream = list.stream().map(element -> element.length());

// 리스트 내의 중복 제거
Stream<String> distinctStream = list.stream().distinct();

// 멀티쓰레드를 이용한 병렬 처리. 메서드 참조도 사용
list.parallelStream().forEach(System.out::println);
```

  
Stream에서 중요한 것은  **최종연산이 수행되기 전까지는 중간 연산이 수행되지 않는다**는 것이다. 

stream메서드에서 체인으로 연결된 최종 메서드가 호출될 때까지 중간에 체인된 다른 메서드는 실행되지 않는 것으로 이를 **'지연된 연산'**이라고 하며 연산이 실행될 때마다 새로운객체가 생성되는 것을 막아준다. 

### **3\. Java.time 패키지**

Calendar 클래스를 사용하여 날짜와 시간에 대한 정보를 얻을 수 있지만,

1\. Calendar인스턴스는 불변객체가 아니기 때문에 값이 수정될 수 있고

2\. 윤초(leap second)와 같은 특별상황이 고려되지 않았으며

3\. 월 을 나타낼 때 1월을 0으로 12우러을 11로 표현하는 불편함이 있었다.

그래서 Joda-Time이라는 라이브러리를 사용하며 Calendar의 불편사항을 개선했는데

8버전에서 Java.time 패키지를 사용하여 위 문제점을 해결했고 추가로 다양한 기능을 제공한다.

 LocalDate, LocalTime, LocalDateTime   
  

### **4. Interface의 default메서드와 static 메서드** 

기존에 Interface는 오직 public abstract method만 가질 수 있었다. 그렇기에 인터페이스에 메서드가 추가될 때마다 구현

한 모든 클래스는 매서드를 추가하고 구현해야했다. 

자바 8부터 default method가 생겨 이 디폴트 메서드는 모든 구현클래스가 오버라이딩하지 않아도 된다. 

그리고 Interface 내에 static method를 추가하는 것도 가능해졌고, 메서드 내용을 입력할 수 있게 되었다.  

```
public interface TestInterface {
    public String method();
    public int multiply(int a, int b);
    //deafault메서드
    default int add(int a, int b) {
        return a + b;
    }
    //static메서드
    public static int minus(int a, int b) {
        return a - b;
    }
}

public class TestClass implements TestInterface{
    @Override
    public String method() {
        return "method";
    }
    @Override
    public int multiply(int a, int b) {
        return a * b;
    }
}

public class Main {
    public static void main(String[] args) {
        TestInterface testInterface = new TestClass();
        
        testInterface.add(1, 2);
        TestInterface.minus(2, 1);
    }
}
```

## **5.Optinal<T>**

NullPointerException을 효과적으로 다룰 수 있는 Optinal<T>클래스가 추가되었다. 

옵셔널은 null이나 null이 아닌 값을 담을 수 있는 클래스로 옵셔널을 사용하면 DB를 조회하고 null인 경우 orElseThrow 메소드를 사용하여 에러를 던지거나 ifpresent 메소드로 분기 처리를 하는 등 null 처리를 세련되게 할 수 있게 된다.

기존에 null을 체크하기 위해서 if 문을 많이 사용했지만 Opitonal을 사용하면 Null을 핸들링 할 수 있게 된다.

filter() : 조건에 충족하면 값을 출력하고 같지 않으면 Optional.empty()를 반환하거나, orElse() 값을 반환한다.

```
// ABCD
Optional.of("ABCD").filter(v -> v.startsWith("AB")).orElse("Not AB");

// Not AB
Optional.of("XYZ").filter(v -> v.startsWith("AB")).orElse("Not AB");
```

map() : 값을 수정해서 반환한다.

```
// xyz -> 소문자로 변환
Optional.of("XYZ").map(String::toLowerCase).orElse("Not AB");
```

isPresent() : null인지 아닌지 조사하여 boolean을 반환한다.

```
Optional.of("TEST").isPresent(); // true
Optional.of("TEST").filter(v -> "Not Equals".equals(v)).isPresent(); // false
```

ifPresent() : 값이 있으면 값을 출력하고 그렇지 않으면 아무것도 출력하지 않는다. 

```
// TEST 출력
Optional.of("TEST").ifPresent(System.out::println);

// 아무것도 출력되지 않음
Optional.ofNullable(null).ifPresent(System.out::println);
```

orElseThrow() : 값을 조사하여 null이면 Exception 발생시킨다.

```
Optional.of("XYZ").filter(v -> v.startsWith("AB")).orElseThrow(NoSuchElementException::new);
```
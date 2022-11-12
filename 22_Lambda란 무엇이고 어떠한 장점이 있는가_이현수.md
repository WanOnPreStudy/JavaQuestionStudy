# 람다 표현식(lambda expression)이란?


람다 표현식(lambda expression)이란  "선언없이 실행가능한 함수"

자바 8부터 람다식(Lambda Expressions)을 지원하며, 간결한 코딩만큼 가독성 면에서 장점을 가진다.

 <br>
 

# 람다식 장단점
 

### 장점
1. 불필요한 코드를 제거하여 간결.

2. 코드가 간결하고 식에 개발자의 의도가 명확히 드러나므로 가독성 향상.

3. 함수를 만드는 과정없이 한번에 처리할 수 있기에 코딩시간이 단축.

4. 다중 cpu를 활용하는 형태로 구현되어 병렬 처리에 유리.

### 단점
1. 람다를 사용하면서 만드는 무명함수는 재사용이 불가능.

2. 디버깅 시 함수 콜 스택 추적이 다소 어려움.

3. 재귀로 만들어 완전탐색하는 경우, 느릴수 있다.

4. 한 클래스내에 많은 람다식을 사용하는 경우, 오히려 가독성이 떨어진다.


# 람다식 작성법
 

1. 자바에서는 화살표(->) 기호를 사용하여 매개변수부와 선언부를 나눕니다.

2. 람다식 사용을 위해서는, 함수형 인터페이스에 접근하여 사용됩니다. 함수형 인터페이스란 한개의 추상메소드를 가지는 인터페이스로, 동적인 함수구현후 사용하기 위해 정의합니다. 

```java
@FunctionalInterface	// @FunctionalInterface 어노테이션을 반드시 명시하여 정의함
interface Calc {	// 함수형 인터페이스의 선언

    public int min(int x, int y);

}
```

```java
public class LambdaExam1 {

public static void main(String[] args){

        Calc minNum = (x, y) -> x < y ? x : y; // 추상 메소드의 구현

        System.out.println(minNum.min(3, 4));  // 함수형 인터페이스의 사용

    }

}
```
람다식 사용을 위해 매번 함수형인터페이스를 정의하는건 말도 안되는 일이다. 오히려 간결하지 못하다.

자바에서 기본으로 제공되는 함수형 인터페이스를 알고 활용해야 가치가 있다.

https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html
 

3. 매개변수의 타입을 추론할 수 있는 경우에는 타입을 생략할 수 있습니다.

4. 매개변수 작성부에서  변수가 하나인 경우에는 괄호(())를 생략할 수 있습니다.

5. 함수의 몸체가 하나의 명령문만으로 이루어진 경우에는 중괄호({})를 생략할 수 있습니다. (이때 세미콜론(;)은 붙이지 않음)

6. 함수의 몸체에 return 문이 있는 경우에는 중괄호({})를 생략할 수 없습니다.

7. return 문 대신 표현식을 사용할 수 있으며, 이때 반환값은 표현식의 결괏값이 됩니다. (이때 세미콜론(;)은 붙이지 않음)

# 람다식 활용 예제(기본 함수형 인터페이스 (파라미터가 없거나 하나))

사실 제일 많이 활용되는건 sort를 위해 함수형 인터페이스 Comparator의 compare를 표현할 때이다.

<img width="663" alt="image" src="https://user-images.githubusercontent.com/65898555/200233549-06205cae-3fa5-4547-955b-778c05963dd1.png">

그 외에 상황별로 사용할 수 있는 기본적인 함수형 인터페이스는 다음과 같다.


### Comparator 인터페이스의 compare()

```java
// 람다식을 지원하지않던 java 1.8이전에 구현 코드
public String solution(int[] numbers){
    StringBuffer sb = new StringBuffer();  
        
    String[] str = new String[numbers.length];       
      for(int i=0;i<numbers.length; i++)
        str[i] = Integer.toString(numbers[i]);
            
    Arrays.sort(str, new Comparator<String>() {
      @Override
      public int compare(String o1, String o2) {  
        return -Integer.compare(Integer.parseInt(o1+o2),Integer.parseInt(o2+o1));  // 여기만 다름
        }
    });
        
    if(str[0].equals("0"))
      return "0";
    else {
      for(String s : str) {
        //System.out.println(s);
        sb.append(s);
      }
      return sb.toString();
    }
}
```
사용자 정렬을 구현하는 부분에서 메소드 오버라이딩을 람다식으로 간단히 진행할수 있다.

```java
public String solution(int[] numbers){
    StringBuffer sb = new StringBuffer();  
        
    String[] str = new String[numbers.length];       
    for(int i=0;i<numbers.length; i++)
      str[i] = Integer.toString(numbers[i]);
            
    // 핵심부분 lamda식으로 두개를 가져와 compareTo()로 비교 정렬
    Arrays.sort(str, (a,b)->{
      return -(a+b).compareTo(b+a);
    });

    if(str[0].equals("0"))
      return "0";
    else {
      for(String s : str) {
        //System.out.println(s);
        sb.append(s);
      }
      return sb.toString();
    }
}
```
Arrays. Sort시 두번째 파라미터는 Comparator 인터페이스의 구현이 들어가는 부분이다.

해당 부분에서 compare 메소드를 원래 아래 코드와 같이 함수 오버라이딩 해야하지만, 위에서와 같이 람다식을 쓰면 간단해진다.

### Runnable 인터페이스의 run() 

```java

import java.util.function.Consumer;

public class Lambda_Runnable {
	public static void main(String[] args) {
		
		//Runnable 객체 생성시 정의
		Runnable runnable = () -> System.out.println("1. Runnable run lambda 람다식 정의");
		runnable.run();
		
		Thread t1 = new Thread(runnable);
		t1.start();
		
		// Runnable 객체가 파라미터로 들어가는 부분에서 정의
		Thread t2 = new Thread(() -> System.out.println("2. Runnable run lambda 람다식 정의"));
		t2.start();
	}	
}
```
한번은 미리 Runnable의 객체 생성시 람다식으로 정의하였고, 한번은 Thread 객체 생성시 파라미터에 들어갈 Runnable 객체전달 부분에 람다식으로 정의하여 전달하였다.

이를 통해 Thread start()가 수행될때 불러지는 run을 재정의하여, 로그를 기록하는데 활용할 수 있다. 

### Supplier인터페이스의 get() 

매개변수가 없이, 파라미터를 반환하는 Supplier인터페이스는 정의된 값을 반환해주므로 Supplier이다.

```java
import java.util.function.*;

public class ETC {
	public static void main(String[] args) {
		
		// Supplier 
		Supplier<String> supplier = () -> "String";
		System.out.println(supplier.get()); // String
	}
}
```

### Consumer인터페이스의 accept( T ) 

매개변수는 있지만, 소비해버리고 반환하지 않는 인터페이스이다. 

```java
import java.util.function.*;

public class ETC {
	public static void main(String[] args) {			
		// Consumer
		Consumer<Integer> consumer = (num) -> System.out.println(num + 1);
		consumer.accept(1); // 2
	}
}
```

 

### Function인터페이스의 apply( T ) 

매개변수도 있고, 반환값도 존재하는 일반적인 적용 함수.

```java
import java.util.function.*;

public class ETC {
	public static void main(String[] args) {
		// Function
		Function<Integer, Integer> function = (num) -> num + 1;
		int result1 = function.apply(1);
		System.out.println(result1); // 2
        
       		// 메소드 참조 표현 - 아래에서 설명하겠음
		Function<String, Integer> function2 = String::length; 
		int len = function2.apply("Hello World");
		System.out.println(len);
	}
}
``` 

### Predicate 인터페이스의 test( Boolean )

매개변수도 있고, 반환값도 존재하지만, Function과 달리 Boolean 값을 반환.

```java
import java.util.function.*;

public class ETC {
	public static void main(String[] args) {
		// Predicate
		Predicate<Integer> predicate = (num) -> num > 0;
		boolean result2 = predicate.test(1);
		System.out.println(result2); // true
	}
}
```

<br>

# 람다식 활용 예제(파라미터가 두개인 함수형 인터페이스)

<img width="664" alt="image" src="https://user-images.githubusercontent.com/65898555/200234149-922e9752-1b6d-443d-951f-6325c4a9615f.png">

```java
// BiConsumer
BiConsumer<Integer, Integer> biConsumer = (num1, num2) -> System.out.println(num1 + num2);
biConsumer.accept(1, 2); // 3


// BiFunction
BiFunction<Integer, Integer, Integer> biFunction = (num1, num2) -> num1 + num2;
int result1 = biFunction.apply(1, 2);
System.out.println(result1); // 3


// BiPredicate
BiPredicate<Integer, Integer> biPredicate = (num1, num2) -> num1 > num2;
boolean result2 = biPredicate.test(1, 2);
System.out.println(result2); // false
```

# 람다식 활용 예제(파라미터 타입이 반환타입과 일치하는 인터페이스)

<img width="665" alt="image" src="https://user-images.githubusercontent.com/65898555/200234289-692b1d99-69bb-4632-b1f9-42645511041d.png">

```java
// UnaryOperator
UnaryOperator<Integer> unaryOperator = (num) -> num;
int result1 = unaryOperator.apply(1);
System.out.println(result1); // 1

// BinaryOperator
BinaryOperator<Integer> binaryOperator = (num1, num2) -> num1 + num2;
int result2 = binaryOperator.apply(1, 2);
System.out.println(result2); // 3
```

<br>

# 메소드 참조


메소드 참조란 함수형 인터페이스를 람다식이 아닌 일반 메소드를 참조시켜 선언하는 방법이다.

일반 메소드를 참조하기 위해서는 다음의 3가지 조건을 만족해야 한다.

```
함수형 인터페이스의 매개변수 타입 = 메소드의 매개변수 타입
함수형 인터페이스의 매개변수 개수 = 메소드의 매개변수 개수
함수형 인터페이스의 반환형 = 메소드의 반환형
```

참조가능한 메소드는 일반 메소드, Static 메소드, 생성자가 있으며 클래스이름 :: 메소드이름으로 참조할 수 있다. 이렇게 참조를 하면 함수형 인터페이스로 반환이 된다.

메소드 참조는 람다식 만큼이나 깔끔하고 가독성 좋은 코드를 작성하는데 도움을 준다.

```java
// 메소드 참조 비교 예제
DoubleUnaryOperator oper;

oper = (n) -> Math.abs(n); // 람다 표현식
System.out.println(oper.applyAsDouble(-5));

oper = Math::abs; // 메소드 참조
System.out.println(oper.applyAsDouble(-5));
```


```java
public class MethodRef {
	public static void main(String[] args) {
		
		// Consumer
		Consumer<Integer> consumer = System.out::println;
		consumer.accept(1); // 2
	
	
		// Function
		Function<Double, Double> function = Math::sqrt;
		System.out.println(function.apply(25.0)); // 2
	
	}
}
```

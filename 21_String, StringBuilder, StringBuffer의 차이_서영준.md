# String

### 불변성

```java
String str = "hello";
str = str + " world";
```

String은 **불변의 속성**을 가집니다.

위의 예제를 살펴보면, 기존의 hello에 world가 더해져 hello world가 되었다고 생각할 수 있지만, 사실은 String 클래스의 참조변수 str이 **“hello world”라는 값을 가지고 있는** 새로운 메모리 영역, 즉 **새로운 String 객체를 가리키게 되는 것** 입니다.

![String.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/658251c6-805e-46ad-a741-e93e0c2f1574/String.png)

- 변하지 않는 문자열을 자주 읽어 들이는 경우 좋은 성능을 기대할 수 있다.
- 변경이 빈번하게 발생하는 알고리즘에 String 클래스를 사용하면 힙 메모리에 Garbage가 많아져 애플리케이션 성능에 영향을 끼칠수 도 있다.

### String Constant Pool

String 객체에 값을 할당하는 방법은 두 가지가 있습니다.

```java
String str = "hello world";                 // 리터럴 값을 대입하는 방법
String str = new String("hello world");     // new 연산자를 사용하는 방법
```

흔히 **new 연산자를 사용해 String 객체를 생성하지 않는 것이 좋다**라는 말을 볼 수 있습니다.

그 이유는 String 객체의 값 할당 방식에 따라 저장 방식이 달라지기 때문입니다.

**리터럴 값을 통해 String 객체를 생성**할 경우 String 객체는 Heap 영역 내 **String Constant Pool에 저장**합니다. 만약 String Constant Pool에 존재하는 리터럴 값을 사용하게 될 경우 해당하는 **String 객체를 재사용**하게됩니다.

반면, **new 연산자를 통해 String 객체를 생성할 경우** 일반적인 객체와 동일하게 Heap 영역의 메모리에 객체를 생성합니다. 이미 **메모리에 같은 값이 존재하더라도 새로운 공간에 객체를 할당**하게 되는 것입니다.

![String Constant Pool.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9cc643b0-a1e4-4024-a331-5f7d3b9138a5/String_Constant_Pool.png)

- 리터럴로 생성한 String 객체는 String Constant Pool에 저장된다.
- 리터럴로 String을 생성할 때 Constant Pool에 같은 값이 존재하면 객체를 재사용한다.
- new 연산자를 통해 String 객체를 생성하면 Constant Pool에 값이 존재 하더라도 객체를 Heap영역 내 별도의 메모리에 객체를 생성한다.

```java
String str1 = "hello";
String str2 = "hello";
// 리터럴 값을 사용할 경우 true가 나옴
System.out.println(str1 == str2);       // true

String str3 = new String("world");
String str4 = new String("world");

// new 연산자를 사용할 경우 false가 나옴
System.out.println(str3 == str4);       // false
```

</br>

# StringBuilder VS StringBuffer

위에서 설명한 String은 불변성을 가진다고 했습니다. 때문에 변경이 빈번하게 발생하는 알고리즘에서 사용하기엔 알맞지 않다고 했었죠. String의 이러한 단점을 극복한 것이 String Builder와 String Buffer입니다.

**String Builder와 String Buffer는 둘 다 가변적인 특징**을 가집니다.

String과는 다르게 append(), delete() 메서드들을 통해 **동일 객체내에서 문자열을 변경**할 수 있죠,

따라서 변경이 자주 발생하더라도 Heap 메모리에 새로운 객체를 계속해서 생성하지 않기 때문에 성능의 부하를 줄일 수 있습니다.

```java
StringBuffer sb = new StringBuffer("hello");
sb.append("world");
```

![StringBuffer.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/664a6f8a-45fb-4b19-a87e-fdb6305da73d/StringBuffer.png)

기본적으로 StringBuilder와 StringBuffer는 둘 다 크기가 유연한 가변적인 특성을 지니고, 제공하는 메서드와 사용 방법도 모두 동일합니다.

하지만 동기화의 관점에서 두 클래스는 큰 차이점을 보이는데요.

**StringBuffer는 메서드에 synchronized 동기화 키워드를 사용해 멀티스레드 환경에서 안전성을 보장**합니다.

반대로 **StringBuilder는 동기화를 지원하지 않기 때문에 멀티스레드 환경에서 사용하는 것은 적합하지 않지만,** 싱글 스레드 환경에서의 성능은 StringBuffer보다 빠릅니다.

</br>

# 정리

### String

- **불변적인 특징**, 문자열 연산이 적은 알고리즘에서 사용
- 멀티스레드 환경에서 안전함

### StringBuffer

- **가변적인 특징**, 문자열 연산이 많은 알고리즘에서 사용
- **synchronized 키워드**를 통해 **멀티스레드 환경에서 안전함**

### StringBuilder

- **가변적인 특징**, 문자열 연산이 많은 알고리즘에서 사용
- 동기화를 지원하지 않지만, **싱글 스레드 환경에서 가장 좋은 성능**
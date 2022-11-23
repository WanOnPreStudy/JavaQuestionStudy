## **Method Reference**

Method Reference 는 Java8 버전에서 도입된 표현식으로, Lamda를 더 간단하게 표현하여 가독성을 높이고 코드 작성을 편리하게 만들어준다.

#### Method Reference  

ClassName::MethodeName

아래 코드로 메서드레퍼런스에 대해 이해해보자. 

**람다**를 사용하면 **메서드명, (), {}를 생략**할 수있다.

**메서드레퍼런스**를 사용하면 **메서드명, (), {} 를 생략**하고 **인자값** 또한 표시하지 않고 있다. 

그리고 클래스명::메서드명 식을 사용하여서 괄호를 사용하지 않고 메서드를 호출한다.

_메서드레퍼런드를 사용한 로직이 훨씬 간단하고 명료하다._ 

```
//Consumer 는 함수형인터페이스다. (하나의 메서드만 가진다.)
//람다
Consumer<String> printOut = sentence -> System.out.printls(sentence);
printOut.accept("안녕 나야 잘지내니 요즘날씨 많이춥지");

//메서드 레퍼런드
Consumer<String> printOut=System.out::println;
printOut.accept("안녕 나야 잘지내니 요즘날씨 많이춥지");
```

메서드레퍼런스는 패턴에 따라 세가지로 분류할 수 있다.

-   **Static Method Reference**
-   **Instance Method Reference**
-   **Constructor Method Reference**

### 1\. Static Method Reference

ClassName::StaticMethodName  
  

Static Method Reference는 Static Method를  Method Reference로 사용하는 것이다.

```
import java.util.function.Consumer;

public class Example {

    public static void main(String[] args) {

        Consumer<String> fCall = sentence -> Printer.printSomething(sentence);
        Consumer<String> sCall = Printer::printSomething;
        fCall.accept("안녕하세요");
        sCall.accept("반갑습니다");
    }

    public static class Printer {
        static void printSomething(String sentence) {
            System.out.println(sentence);
        }
    }
}
```

### 2\. Instance Method Reference

Object::instanceMethodName

Instance Method는 객체의 멤버 메서드를 Method Reference로 사용하는 것이다.

```
public class Open {
  public String hello(String name){
    return "hello "+name;
  }
}
public class Main{
  public static void main(String args[]){
    Open open = new Open(); //객체생성
    UnaryOperator<String> u = open::hello;
    System.out.println(u.apply("Hello")); 
  }
}
```

### 3\. Constructor Method Reference

ClassName::new

Constructor Method Reference는 생성자를 호출한다.

```
public class Constor {
    private String name;
    public Constor(){} //인자없는 생성자
    public Constor(String name){ //인자있는생성자
        this.name = name;
    }
}

public Main{
  public static void main(){
    //인자 없는 생성자 만들어짐
    //Supplier<Constor> constor1= ()-> new Constor();
    Supplier<Constor> constor1= Constor::new;
    constor1.get();

    //인자있는 생성자 만들어짐
    //Function<String, Constor> f = () -> new Constor();
    Function<String, Constor> f = Constor::new;
    Constor constor2 = f.apply("Hello");
    System.out.println(constor2.getName());
  }
}
```
# Annotation
<br/>
간단하게 정리하고 들어가자면

-   JDK 5에서 도입되었습니다.
-   자바 Annotation이란 주석이란 뜻으로 비즈니스 로직을 포함하지 않는 메타 데이터 입니다.
-   Annotation은 크게 Built-in Annotation(General Purpose Annotation, Meta Annotation), Custom Annotation으로 나눌 수 있습니다.
- Annotaion은 Annotation Processor에 의해 컴파일 시점에 처리됩니다


<br/>

## Annotation

**Annotation 란?**

Java의  Annotation은 JDK 5에서 도입되었습니다.

Annotation은 자바 소스코드에 추가할 수 있는 일종의 메타 데이터입니다

그러므로 애노테이션에는 비즈니스 로직이 들어가지 않습니다.  Annotation은 ' **@** '로 시작하며 일반적으로 클래스, 인터페이스, 메서드 변수, 파라미터 등에 사용됩니다.

크게 Built-in Annotation(General Purpose Annotation, Meta Annotation), Custom Annotation으로 나눌 수 있습니다.

<br/>
<br/>

## Annotation 역할

**코드 문법 에러 체크**

어노테이션은 컴파일러에게 코드 문법 에러를 체크하도록 정보를 제공합니다.

-   @Override 어노테이션이 메서드 앞에 명시된 경우, 이는 해당 메서드가 오버라이드 된 메서드임을 컴파일러에게 알려주는 것이다. 컴파일러는 @Override 어노테이션을 보고 그 메서드가 오버라이드 된 것인지 확인한다. 그리고 오버라이드 된 메서드가 아닌 경우 컴파일 에러를 발생시킨다.

<br/>

**런타임 시간에 특정 기능 실행**

어노테이션은 실행 시(런타임 시) 특정 기능을 실행하도록 정보를 제공합니다.

-   스프링 프로젝트 때 컨트롤러, 서비스 등 클래스의 역할을 정의하기 위해 @Controller, @Service와 같은 어노테이션을 사용한다

<br/>

**빌드나 배치 시 코드를 자동으로 생성**

어노테이션은 소프트웨어 개발 툴이 빌드나 배치 시 코드를 자동으로 생성할 수 있도록 정보를 제공합니다.

-   lombok 라이브러리를 예시로 들었을 때, @Getter, @Setter, @AllArgsConstructor와 같은 어노테이션들이다. 해당 어노테이션이 클래스에 붙어있다면 Getter, Setter 메서드와 생성자를 자동으로 생성해준다.

<br/>
<br/>

## Built-In Annotation

자바에서는 기본으로 제공해주는 애노테이션은 10개가 있으며 아래와 같이  범용 애노테이션과(General Purpose Annotation), 메타 애노테이션(Meta Annotation)으로 나눌 수 있습니다.

<center>
  <img
    src="https://media.geeksforgeeks.org/wp-content/uploads/20211110125455/JavaAnnotations.jpg"
    width="100%"
    height="100%"
  />
  https://www.geeksforgeeks.org/annotations-in-java/
</center>

<br/>

**General Purpose Annotation**

-   **@Override** : Superclass의 메서드를 재정의하고 싶을 때 이 주석을 사용하여 메서드를 재정의하고 있음을 컴파일러에 알려야 합니다. 따라서 상위 클래스 메서드가 제거되거나 변경되면 컴파일러에서 오류 메시지를 표시합니다.

-   **@Deprecated** : 메서드가 더 이상 사용되지 않는다는 사실을 컴파일러가 알기를 원할 때 이 주석을 사용해야 합니다. Java는 javadoc에서 이 메서드가 더 이상 사용되지 않는 이유와 사용할 대안이 무엇인지에 대한 정보를 제공해야 한다고 권장합니다.

-   **@SuppressWarnings** :예를 들어 자바 제네릭에서 원시 유형을 사용하는 것과 같이 컴파일러가 생성하는 특정 경고를 무시하도록 컴파일러에 지시하기 위한 것입니다. 보존 정책은 SOURCE이며 컴파일러에서 폐기됩니다.

-   **@FunctionalInterface** : 인터페이스가 기능적 인터페이스임을 나타내기 위해 Java 8에 도입되었습니다.

-   **@SafeVarargs** :주석이 달린 메서드 또는 생성자의 본문이 해당 varargs 매개 변수에 대해 잠재적으로 안전하지 않은 작업을 수행하지 않는다는 프로그래머 주장입니다.

<br/>

**Meta Annotation**

메타 애노테이션은 어노테이션을 위한 어노테이션으로 해당 어노테이션의 동작 대상, 스코프를 결정하는 어노테이션입니다. 주로 새로운 어노테이션을 정의할 때 사용됩니다.

-   **@Documented**: 애노테이션에 대한 정보가 javadoc 문서에 포함되도록 합니다. 유형 선언에 Documented로 애노테이션이 달린 경우 해당 주석은 주석이 달린 요소의 공개 API의 일부가 됩니다.

-   **@Target** : 애노테이션 유형을 적용할 수 있는 요소의 종류를 나타냅니다. 요소를 정해주지 않으면  모든 요소에서 애노테이션을 사용할 수 있습니다.
    -   TYPE – Type에만 적용됩니다. Type은 Java 클래스나 인터페이스, Enum 또는 Annotation일 수 있습니다.
    -   FIELD – Java 필드(객체, 인스턴스 또는 정적, 클래스 수준에서 선언됨)에만 적용됩니다.
    -   METHOD – 메서드에만 적용됩니다.
    -   PARAMETER – 메서드 정의의 메서드 매개변수에만 적용됩니다.
    -   CONSTRUCTOR – 클래스의 생성자에만 적용할 수 있습니다.
    -   LOCAL\_VARIABLE – 로컬 변수에만 적용할 수 있습니다. (메서드 또는 코드 블록 내에서 선언된 변수).
    -   ANNOTATION\_TYPE – 주석 유형에만 적용됩니다.
    -   PACKAGE – 패키지에만 적용됩니다.

-   **@Inherited**: 어노테이션을 자식 클래스에게도 붙이기 위해(상속) 사용하는 어노테이션으로 슈퍼클래스에 이 애노테이션을 붙이면 서브클래스에도 이 애노테이션을 붙인 것과 같습니다.  
    
-   **@Retention**: 애노테이션이 삭제되는 시기를 지정합니다. 가능한 값이 SOURCE, CLASS 및 RUNTIME인 RetentionPolicy 인수를 사용합니다.
    -   SOURCE: 주석은 컴파일 타임에 사용되며 런타임에 삭제됩니다.
    -   CLASS: 주석은 컴파일 타임에 클래스 파일에 저장되고 런타임에 폐기됩니다.
    -   RUNTIME: 주석은 런타임 시 유지됩니다.

-   **@Repeatable** : 어노테이션이 유형이 반복 가능함을 나타내는 데 사용됩니다.

<br/>
<br/>

## Custom Annotation

사용자가 개발의 편의를 위해 정의하는 어노테이션입니다.  
어노테이션은 특별한 종류의 인터페이스이며, 일반 인터페이스와 타입 구분을 위해 @를 앞에 붙여  Java 어노테이션 정의임을 Java 컴파일러에 알립니다.

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface LoginUser {

}
```

어노테이션은 메타데이터의 저장을 위한 Element를 가질 수 있습니다. 그리고 이 Element의 개수에 따라 Marker Annotation, Single-value Annotation, Full Annotation으로 나뉩니다.

**Maeker Annotation**

 - 멤버 변수가 없으며, 단순히 표식으로서 사용되는 Annotaion입니다.   
 - @AnnotationName

**Single-value Annotation**

 - 멤버로 단일 변수만 갖는 Annotaion. 단일 변수 밖에 없기 때문에 (값)만을 명시하여 데이터를 전달  
 - @AnnotationName(elementValue)

**Full Annotation**

 - 멤버로 둘 이상의 변수를 갖는 Annotaion으로, 데이터를 (값=쌍)의 형태로 전달  
 - @AnnotationName(element=value, element=value, ...)

<br/>
<br/>

## Annotation은 어떻게 처리되는가

위에서는 어노테이션은 무엇이고, 어떤 것들이 있는가에 대해서 알아봤다면 이제는 이 애노테이션이 어떻게 처리되는가에 대해서 알아보겠습니다. 애노테이션은 Annotation Processor에 의해 컴파일 시점에 처리됩니다

Annotation Processor는 실제로 javac(java compiler)의 일부이므로 모든 처리를 컴파일 시점에 하게 됩니다.


<details>
<summary>더보기</summary>
<div markdown="1">

모든 처리를 컴파일 타임에..? 여기서 조금 헷갈렸는데  
왜냐하면 의존성 주입 (ex. @Autowired, @Service) 같은 경우 런타임 시점에 이루어진다고 알고 있었기 때문입니다.  
<br/>
여기서는 Annotation Processor와 관계없이 리플렉션이 무엇인가를 알아야 하는데   
리플렉션이란 "구체적인 클래스 타입을 알지 못해도 그 클래스의 메서드, 타입, 변수들을 접근할 수 있도록 해주는 자바 API"로
<br/>
자바는 컴파일 시점에 타입을 결정하는 정적 언어이지만 리플렉션을 이용하면 런타임 시점에 동적으로 클래스의 정보를 가져올 수 있습니다.  이 클래스의 정보에 는 '어노테이션'에 대한 정보도 포함입니다.
<br/>   
리플렉션은 runtime 시점에 실행된다고 했는데,  
 @Retention(RetentionPolicy.RUNTIME) 설정을 하면 어노테이션이 runtime 시점에도 살아 있을 수 있기 때문에 
리플렉션의 getAnnotation(s), getDeclareAnnotaion(s) 등의 메서드를 통해 원하는 어노테이션이 있는지 확인하고 붙어 있다면 원하는 로직을 수행하는 것입니다  
<br/>
다시 한번 애노테이션은 주석일 뿐 비즈니스 로직이 아닙니다.  
<br/>
@Autowired에 @Retention(RetentionPolicy.RUNTIME) 설정인 것을 확인할 수 있습니다
<center>
  <img
    src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqVwCY%2FbtrRS5iLMEB%2FKmVT9W7ErUkpgMzYAbFxok%2Fimg.png"
    width="100%"
    height="100%"
  />
  
</center>

</div>
</details>

<br/>

Annotation Processor는 컴파일 단계에서 Annotation에 정의된 일렬의 프로세스를 동작하게 합니다. (특정 애노테이션 프로세서는 라운드를 돌면서 특정한 애노테이션이 붙어있는 소스코드를 참조해서 또 다른 소스코드를 만들 수 있습니다)
<br/>
밑에 이미지와 설명을 보면 이해하기 쉽습니다.

<center>
  <img
    src="https://miro.medium.com/max/750/1*JE0JEPdnwTvhYISlL62nEA.webp"
    width="100%"
    height="100%"
  />
  https://miro.medium.com/max/750/1*JE0JEPdnwTvhYISlL62nEA.webp
</center>

<br/>

-   Javac이 컴파일을 시작합니다. Javac은 ServiceLoader가 선택할 클래스 경로에 애노테이션 프로세서를 넣었기 때문에 다른 모든 애노테이션 프로세서를 이미 알고 있습니다.
-   Javac은 첫 번째 프로세싱 라운드에서 '.class 파일을 생성합니다(javac는 이미 '.java' 파일을 처리했으므로 사용된 주석을 이미 알고 있습니다) 각 애노테이션에 대한 애노테이션 프로세서에 제어권을 넘깁니다.
-   이제 애노테이션 프로세서가 작업을 시작합니다. 여기서 애노테이션 프로세서는 추가 '.java' 파일을 생성하거나 생성하지 않을 수 있습니다.
-   위 단계에서 모든 원본 '.java' 파일이 컴파일되었습니다. 그러나 애노테이션 프로세서가 애노테이션이 포함된 '.java' 클래스를 생성했다면 javac가 이를 감지하고 다른 프로세싱 라운드를 시작합니다. (round 2)
-   이러한 라운드는 어노테이션 프로세서가 더 이상 처리할 어노테이션이 없어질 때까지 반복합니다

애노테이션 프로세서의 동작원리는 기존 파일을 변경하는 것이 아니라, 새로운 파일을 생성하는 것을 알 수 있습니다

<br/>
<br/>

#### Reference

-   [https://medium.com/swlh/all-about-annotations-and-annotation-processors-4af47159f29d](https://medium.com/swlh/all-about-annotations-and-annotation-processors-4af47159f29d)
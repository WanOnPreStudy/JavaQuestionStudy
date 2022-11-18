**들아가기 전에 간단히**

-   java의 특징 중 하나는 OS에 독립적이다 라는 것입니다.
-   이것을 가능하게 해주는 것이 JVM인데 
-   바이트코드는 JVM에서 코드를 이해할 수 있도록  컴파일러에 의해 변환된 코드(.class)를 말합니다.

<br/>

## 자바 바이트 코드란 
---

**바이트 코드**

특정 하드웨어가 아닌 가상 컴퓨터(Virtual Machine)에서 돌아가는 실행 프로그램을 위한 이진 표현법으로 하드웨어가 아닌 소프트웨어에 의해 처리되기에 기계어보다 추상적입니다.

<br/>

**자바 바이트 코드(Java bytecode)**

-   개발자가 작성 한 코드(.java)가 컴파일러에 의해  JVM이 이해할 수 있는 언어로 변환된 자바 소스 코드(.class)를 의미합니다.
-   바이트코드의 각 명령어는 1바이트짜리 OpCode와 피연산자(Operand)로 이루어져 있습니다.
-   각 OpCode들은 1바이트의 바이트 번호로 표현됩니다. ( aload\_0 = 0x2a,  invokevirtual = 0xb6 등)
-   따라서, 자바 바이트코드 명령어 OpCode는 최대 256개라는 점을 알 수 있습니다.
-   JVM이 실행하는 코드를 굳이 자바 "바이트"코드라고 하는 이유일  것입니다.
-   자바 바이트 코드는 JVM만 설치되어 있으면, 어떤 운영체제에서라도 실행될 수 있습니다.

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FcpDQSN%2FbtrQVzjitSY%2FlfyDCc1i7Q6o36qfTY8Yf1%2Fimg.png)

<br/>

## 바이트 코드 확인하는 방법
---

### 바이트 코드 확인하기

**방법 1.**

intellij에서 프로젝트를 빌트 시킨 후에 바이트 코드를 확인하고 싶은 폴더를 열고 View > Show Bytecode를 누르면  
간단하게 바이트코드를 확인할 수 있습니다

<img
    src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FMOyby%2FbtrQIuvn8lO%2Fh9uYUARuV0MMfKUvLDQ7k0%2Fimg.png"
    width="40%"
    height="40%"
  />

**Bytecode.java**

```
public class Bytecode {
    public static void main(String[] args) {
        int a = 5;
        int b = 3;
        System.out.print(a + b);
    }
}
```

**Bytecode.java의 바이트코드**

```
// class version 55.0 (55)
// access flags 0x21
public class com/example/baseproject/Bytecode {

  // compiled from: Bytecode.java

  // access flags 0x1
  public <init>()V
   L0
    LINENUMBER 3 L0
    ALOAD 0
    INVOKESPECIAL java/lang/Object.<init> ()V
    RETURN
   L1
    LOCALVARIABLE this Lcom/example/baseproject/Bytecode; L0 L1 0
    MAXSTACK = 1
    MAXLOCALS = 1

  // access flags 0x9
  public static main([Ljava/lang/String;)V
    // parameter  args
   L0
    LINENUMBER 5 L0
    ICONST_5
    ISTORE 1
   L1
    LINENUMBER 6 L1
    ICONST_3
    ISTORE 2
   L2
    LINENUMBER 7 L2
    GETSTATIC java/lang/System.out : Ljava/io/PrintStream;
    ILOAD 1
    ILOAD 2
    IADD
    INVOKEVIRTUAL java/io/PrintStream.print (I)V
   L3
    LINENUMBER 8 L3
    RETURN
   L4
    LOCALVARIABLE args [Ljava/lang/String; L0 L4 0
    LOCALVARIABLE a I L1 L4 1
    LOCALVARIABLE b I L2 L4 2
    MAXSTACK = 3
    MAXLOCALS = 3
}
```

**방법 2.**

intellij에서 바이트 코드를 확인하고 싶은 파일의 .class 폴더를 터미널로 열고  

javap -c \[클래스파일이름\] 을 입력하면 아래와 같은 바이너리 코드를 확인할 수 있습니다.

또한 javap의 다른 옵션들을 이용하여 더 자세한 정보들을 볼 수도 있습니다. (javap는 바이너리인 바이트코드 .class 파일을 텍스트로 보여주는 일종의 역어셈블러 프로그램입니다)

**Bytecode.java의 바이트코드**

```
Compiled from "Bytecode.java"
public class com.example.baseproject.Bytecode {
  public com.example.baseproject.Bytecode();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: iconst_5
       1: istore_1
       2: iconst_3
       3: istore_2
       4: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
       7: iload_1
       8: iload_2
       9: iadd
      10: invokevirtual #3                  // Method java/io/PrintStream.print:(I)V
      13: return
}
```
<br/>

## 바이트 코드 동작 살펴보기
---

바이트 코드의 동작을 살펴보기 전에 바이너리 코드에서는 아래와 같이 각 타입들이 고유의 표현을 같습니다.

이것을 먼저 확인하고 보면 좀 더 이해에 도움이 될 것입니다. 

<br/>

| 접두사/접미사 | 피연산자 타입 |
| --- | --- |
| i | integer |
| I | long |
| s | sort |
| b | byte |
| c | charater |
| f | float |
| d | double |
| a | reference |

<br/>

타입의 표현도 알아봤으니 이제 위에서 확인한 바이트 코드를 조금 더 자세히 살펴보겠습니다.

```
public com.example.baseproject.Bytecode();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return
```

생성자 부분입니다. 생성자를 작성하기 않아도 컴파일러에서 생성자를 넣어주는 것을 알 수 있습니다.

-   aload\_0: 지역 변수 배열의 0번 인덱스 내용을 피연산자 스택에 추가합니다. 지역 변수 배열의 0번 인덱스는 언제나 this, 즉 현재 클래스 인스턴스에 대한 레퍼런스이다.
-   invokespecial: 생성자, private 메서드, 슈퍼 클래스의 메서드 호출

<br/>

```
public static void main(java.lang.String[]);
    Code:
       0: iconst_5
       1: istore_1
       2: iconst_3
       3: istore_2
       4: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
       7: iload_1
       8: iload_2
       9: iadd
      10: invokevirtual #3                  // Method java/io/PrintStream.print:(I)V
      13: return
```

main함수 내에서 동작하는 코드입니다. (코드 앞에 번호는 바이트 번호를 의미합니다 getstatic의 경우 2byte의 피연산자를 필요로 하기 때문에 바이트 번호가 4 -> 7로 넘어가는 것을 확인 할 수 있습니다.) 

-   iconst\_5 :  iconst로 5를 스택(Operand stack)에 push 하고
-   istore\_1: 다시 pop 해서 local variable array 인덱스 1에 저장 (istore #index: int 값을 변수 #index에 저장)
-   getstatic #2:  현재 클래스 상수 풀에서 2번 인덱스에 해당하는 클래스의 정적 필드 값 가져오기 (여기서는 java/lang/System.out:Ljava/io/PrintStream)
-   iload\_1:  local variable array 인덱스 1에서  int 값을 가져와서 다시 스택에 넣는다 (iload\_#index: #index에서 값을 가져옴)
-   iadd : int의 add연산 실행 
-   invokevirtual #3: 현재 클래스 상수 풀에서 3번 인덱스에 해당하는 메서드(여기서는 print)를 호출하고 결과를 스택에 넣습니다.
-   return : 메서드에서 void 반환

<br/>

**더 많은  OpCode는 [여기](https://en.wikipedia.org/wiki/List_of_Java_bytecode_instructions) 에서 확인할 수 있습니다.**

**바이너리 코드에 대한 더 자세한 정리는 [바이너리 코드(2)](https://jueun275.tistory.com/entry/Java-%EB%B0%94%EC%9D%B4%ED%8A%B8-%EC%BD%94%EB%93%9C2-%EB%B0%94%EC%9D%B4%ED%8A%B8-%EC%BD%94%EB%93%9C-%EC%98%88%EC%A0%9C) 에서 볼 수 있습니다.**


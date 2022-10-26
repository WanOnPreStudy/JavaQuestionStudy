# JVM의 용도와 정의

JVM에는 2가지의 기본적인 목적이 있습니다.

1. 자바 프로그램이 어느 기기, 또는 어느 운영체제 상에서도 실행될 수 있게 하는 것
2. 프로그램의 메모리를 관리하고 최적화 하는 것

</br>

Java Compiler에 의해 해석된 자바 바이트코드를 실행할 수 있는 환경을 제공해 줍니다. 이를 통해 자바 바이트 코드가 플랫폼에 독립적으로 실행될 수 있게합니다.

즉, JVM 덕분에 Java 애플리케이션은 OS에 상관없이 어디 서든 실행할 수 있는 것 입니다.

![JVM 목적](https://user-images.githubusercontent.com/90227655/197998495-79732e21-d71f-434e-9a9d-d197a8fce871.jpg)

</br>

# JVM 구성

### 1**. Class Loader**

클래스 로더는 말 그대로 Clsss파일을 불러와 로딩하는 역할을 합니다.

자바에서 소스를 작성하면 .java파일이 생성되고, .java 소스를 컴파일러가 컴파일하면 .class파일이 생성됩니다.

클래스 로더는 **.class 파일을 불러와 JVM내의 Runtime Data Area에 적재**합니다.

</br>

### 2****. Runtime Data Area****

자바 애플리케이션을 실행할 때 **JVM이 운영체제로부터 할당 받은 메모리 영역**입니다.

사용되는 데이터들을 적재하는 영역이며, 적재하는 데이터에 따라 다시 Method Area, Heap Area, Stack Area, PC Register, Method Stack으로 나누어 집니다.

</br>

### 3**. Execution Engine**

클래스 로더에 의해 메모리(Runtime Data Area의 Method Area)에 적재된 **.class 파일(바이트 코드)들을 기계어로 변경해 명령어 단위로 실행하는 역할**을 합니다. 한줄씩 수행하기 때문에 느리다는 단점이 있습니다.

</br>

### **4. Garbage Collector**

가비지 컬렉터(GC, Garbage Collector)는 더 이상 사용하지 않는 메모리를 자동으로 회수합니다. Heap 메모리 영역에 적재(공유 메모리)된 객체들의 생존 여부를 판단해서 **더 이상 참조되지 않거나 null인 객체의 메모리를 해제**하는 역할을 하며 해당 역할을 언제 수행할 지는 알 수 없습니다. GC역할을 수행하는 스레드를 제외한 나머지 모든 스레드들은 일시정지상태가 됩니다.

</br>

# ****자바 실행 과정****

![자바 실행 과정](https://user-images.githubusercontent.com/90227655/197998591-f50c93fb-a6ef-4d4c-a19f-a29de5ac217f.png)


1. 자바 프로그램이 실행되면 JVM에 메모리가 할당됩니다.
2. Java Compiler(javac.exe)가 Java source(.java)파일을 바이트 코드로 바꿔 .class 파일(바이트 코드)로 만들어줍니다.
3. Class Loader가 만들어진 .class 파일을 JVM내의 할당된 메모리(Runtime Data Area)에 적제합니다.
4. Execution Engine이 배치된 바이트 코드를 해석하고 명령어 단위로 실행합니다.
5. Garbage Collector가 어플리케이션의 메모리를 관리합니다.(더 이상 참조되지 않거나, null인 객체의 메모리 해제)
6. 위의 과정이 반복되며 코드가 실행됩니다.

</br>

# ****Runtime Data Area 구조****

![Runtime Data Area 구조](https://user-images.githubusercontent.com/90227655/197998672-fefde983-ab4d-4131-8571-d4eb967bc99b.png)

- Method Area : **클래스, 인터페이스에 대한 런타임 상수풀**, 메서드, 필드, 생성자, 타입 정보, static 변수, static 메서드, 바이트 코드등을 보관합니다. 전체 thread에서 공유할 수 있고, 시작시에 생성되어 종료될 때 까지 존재한다. 따라서 전역변수를 너무 많이 쓰게 되면 메모리 초과 위험이 있습니다.

</br>

> **Runtime constant poll**
> 
> **메서드와 필드, 데이터들의 래퍼런스 값을 가지고 있습니다.** 중복되는 정보가 필요하게 되면 런타임 상수풀에서 꺼내서 사용합니다.

</br>

- Heap Area : **래퍼런스 타입을 갖는 객체(인스턴스), 배열 등이 동적으로 생성되서 저장되는 곳**입니다. new 연산자를 통해 heap영역에 객체를 생성해서 메모리를 부여받고 stack영역에 참조변수에 실제 객체의 래퍼런스 값을 리턴받습니다. 이 래퍼런스 값을 통해서만 객체를 다룰 수 있습니다.

</br>

![Thread 메모리 구조](https://user-images.githubusercontent.com/90227655/197998696-ed4625ad-0af2-425b-909e-46b280f60d2d.PNG)

- Stack Area : **각 스레드 시작 시에 할당되고 종료될 때 해제**됩니다. 각 스레드는 **메서드 호출 시 마다 각각의 스택 프레임이 생성**됩니다. 그리고 메서드 안에서 사용되는 값들을 저장하고, 호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장합니다. 마지막으로, 메서드 수행이 끝나면 프레임별로 삭제합니다.

</br>

> **Stack Frame**
>
> - 스택 프레임은 메서드가 호출될 때마다 새로 생겨 스택에 push 됩니다.
>
> - 스택 프레임은 **Local Variables Array, Operand Stack, Frame Data**를 갖습니다.
>
> - Frame Data는 **Constant Pool 참조 정보**, **이전 스택 프레임에 대한 정보**, **현재 메서드가 속한 클래스/객체에 대한 참조 정보** 등을 갖습니다.

</br>

- PC Register : 현재 실행 중인 JVM 주소를 저장합니다.

</br>

- Native Method Stack Area : 자바 외 언어로 작성된 네이티브 코드를 위한 메모리 stack입니다. Java Native Interface를 통해 호출되는 C/C++ 등의 코드를 수행하기 위한 스택입니다.

</br>

# 바이트 코드 동작 예제

```java
public class Test {
		public void test() {
        double a = 1.0;
        double c;

        c = a + 6.0;

        double d = sum(c);
    }

    public double sum(double e) {
        double b = 8.0;
        return e + b + 6.0;
    }
}
```

</br>

위 코드를 컴파일 한 후 바이트 코드를 확인한 결과 입니다.

![바이트 코드 예제](https://user-images.githubusercontent.com/90227655/197998745-c40cc35c-6260-4280-b274-39cc62088c9a.png)

</br>

바이트 코드를 스택 프레임으로 그려보면 아래와 같이 되어 있겠죠?

![바이트 코드 예제 Stack Frame](https://user-images.githubusercontent.com/90227655/197998797-9620922b-7551-4470-9ef8-833a02b3b863.PNG)

</br>

**test() 메서드**

| 바이트 코드 | 작동 | 자바 코드 |
| --- | --- | --- |
| 0: dconst_1 | 상수 1을 오퍼 랜드 스택(Operand Stack)에 push | 1.0 |
| 1: dstore_1 | pop 하여 지역 변수 배열(Local Variables Array) 1번 인덱스(double a)에 저장 | double a = 1.0 |
| 2: dload_1 | 지역 변수 배열 1번 인덱스의 값을 오퍼랜드 스택에 push | 1.0 |
| 3: ldc2_w #2 | 런타임 상수 풀(Runtime constant poll)의 2번 인덱스의 값을 오퍼랜드 스택에 push | 6.0 |
| 6: dadd | 오퍼랜드 스택의 두 값을 더하라. | 1.0 + 6.0 |
| 7: dstore_3 | 지역 변수 배열 3번 인덱스(double c)에 저장하라. | c = 7.0 |
| 9: dload_3 | 지역 변수 배열 3번 인덱스(double c)에 저장된 값을 오퍼랜드 스택에 push | 7.0 |
| 10: invokevirtual #4 | 런타임 상수 풀의 4번 인덱스에 저장된 메서드(sum(double))의 래퍼런스를 호출 | sum(c) |
| 13: dstore_5 | 지역 변수 배열 5번 인덱스에 저장 | d = sum(c) |

</br>

**sum() 메서드**

| 바이트 코드 | 작동 | 자바 코드 |
| --- | --- | --- |
| 0: ldc2_w #5 | 런타임 상수 풀의 5번 인덱스의 값을 오퍼랜드 스택에 push | 8.0 |
| 3: dstore_3 | pop 하여 지역 변수 배열 3번 인덱스에 저장 | double b = 8.0 |
| 4: dload_1 | 지역 변수 배열 1번 인덱스의 값을 오퍼랜드 스택에 push | 7.0 |
| 5: dload_3 | 지역 변수 배열 3번 인덱스의 값을 오퍼랜드 스택에 push | 8.0 |
| 6: dadd | 오퍼랜드 스택의 두 값을 더하라. | e + b |
| 7: ldc2_w | 런타임 상수 풀의 2번 인덱스의 값을 오퍼랜드 스택에 push | 6.0 |
| 10: dadd | 오퍼랜드 스택의 두 값을 더하라. | e + b + 6.0 |
| 11: dreturn | 이전 스택 프레임으로 리턴 | return e + b + 6.0 |

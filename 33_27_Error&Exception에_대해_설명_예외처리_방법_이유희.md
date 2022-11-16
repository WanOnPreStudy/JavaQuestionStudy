# 발표 주제

---

- Error, Exception 에 대해 설명
- 예외처리 방법

### 오류 정의

---

- 프로그램이 실행 중 어떤 원인에 의해서 **오작동**을 하거나 **비정상적으로 종료**되는 경우가 있다.
  이 결과를 초래하는 원인을 프로그램 에러 또는 오류라고 한다.
- 발생시점에 따라서 컴파일 에러, 런터임 에러로 나눌 수 있다.

### 자바에서의 오류 정의

---

### Error

- **수습할 수 없는** 심각한 문제로 프로그램 코드로 처리 할 수 없다.

ex) `OutOfMemory`, `StackOverFlow`

### Exception

- 발생하더라도 **수습될 수 있는** 비교적 덜 심각한 것

ex) `ArithmeticException`

### Exceptions 계층

---

자바에서는 실행 시 발생할 수 있는 오류를 클래스로 정의하였다.

![Untitled](https://static.javatpoint.com/core/images/hierarchy-of-exception-handling.png)

- 모든 예외의 최고 조상은 Exception 클래스이다.
    1. RuntimeException과 자손들을 제외한 자손 클래스  
       a.**주로 외부의 영향**으로 발생할 수 있는 것들로 프로그램의 사용자들의 동작에 의해서 발생하는 경우가 많음

           `FileNotFoundException` : 사용자가 잘못 입력한 파일명으로 인한 예외 발생

        b. 예외 처리를 강제함
        
        → 컴파일러가 예외처리를 확인하지 않는 `Checked 예외`
        
    3. RuntimeException 클래스와 그 자손들
        1. 주로 **프로그래머의 실수**에 의해서 발생될 수 있는 예외들
        
        ```java
        Integer[] arr = new Integer[3];
        int num = arr[5]; --> IndexOutOfBoundsException 발생!
        ```
        
        b. 프로그래머의 실수로 발생하는 것들이기 때문에 예외처리를 강제하지 않는다. 
        
        → 컴파일러가 예외처리를 확인하지 않는 `Unchecked 예외`
        
    4. 사용자 정의 예외
        1. 프로그래머가 새로운 예외 클래스를 정의하여 사용할 수 있다. 
        2. 주로 checked 예외로 작성하는 경우가 많았지만, 요즘은 예외처리를 선택적으로 할 수 있도록 RuntimeException을 상속받아서 작성하는 쪽으로 바뀌어 가고 있다.

### 에러 처리

---

예외 처리는 예기치 못한 예외 발생을 **대비하기 위해 코드를 작성**하는 것이다.
이를 통해 프로그램의 비정상적 종료를 막고, 정상적인 실행상태를 유지할 수 있도록 한다.

자바에서 제공하는 에러처리 예약어는

- try
- catch
- finally
- throw
- throws

이 있다.
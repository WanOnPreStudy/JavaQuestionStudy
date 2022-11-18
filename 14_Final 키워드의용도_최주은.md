<br/>
간단하게 정리하고 들어가자면…

-   **final 키워드**는 해당 선언이 최종 상태이고 **수정할 수 없음**을 의미합니다.
-   그렇기 때문에 변경하면 안 되는 것을 지정할 때 사용합니다.
-   클래스, 메서드, 변수 선언 시에 사용할 수 있습니다.

<br/>

## final 클래스

---
클래스에 final을 사용하면 그 클래스는 최종 상태가 되어 더 이상 상속이 불가능합니다.

아래 코드를 보면 상속 시 에러가 발생합니다.

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FbHhn3X%2FbtrQfencoIF%2FJIH1dW7D11LrhCX9fkySB1%2Fimg.png)


<br/>

## final 메소드

---

메서드에 final을 사용하면 상속받은 클래스에서 final 메서드를 Override(재정의)할 수 없습니다.

아래 코드를 보면 Override시 에러가 발생합니다.

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2F6EOpw%2FbtrQeTQ9I2F%2FgmbBHyCe4nY6oerP1akNfK%2Fimg.png)


<br/>

## fianl 변수

---

**변수에 final을 사용하면 이 변수는 수정될 수 없습니다**. 수정될 수 없기 때문에 초기화 값은 필수적입니다.
​
초기화가 되지 않은 final 변수가 있다면 컴파일 에러가 발생합니다.
​
아래 코드처럼 final 변수의 값을 변경하려고 하면 에러가 발생합니다.

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FYqw78%2FbtrQcNj7Mka%2FdD83sMhPUcTR7WWyXt3Ct0%2Fimg.png)


<br/>

## Final 참조형(reference type)

---

바로 위에서 변수에 final을 사용하면 수정될 수 없다고 했는데 여기서 조금만 더  들어가 보면, **수정할 수 없다는 범위는 그 변수의 값에 한정**합니다. 다시 말해서 다른 객체를 참조할 때(참조 타입의 경우) **참조하는 객체의 내부의 값은 변경할 수 있다는** 의미입니다.

> 참조 타입은, 객체(Object), 배열(Array), Integer/Long과 같은 Wrapper Class가 해당합니다.


이유는 자바 메모리 구조 때문인데

**기본 자료형** **지역변수**는 데이터의 값이  **Stack 영역**에 저장되고 (int, double, byte, boolean 등)

**참조형**(Reference Type) 변수는 **Heap 영역**에 데이터가 저장되고 Stack영역에는 주소 값만 저장됩니다.

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2Fc7tTQR%2FbtrQkFEREVm%2FXNolVfinQy4CKCmteh7pX0%2Fimg.png)

**final 키워드를 변수에 사용하면 그 영역에만 변경할 수 없다는 영향을 미칩니다**. 그렇기 때문에 주소 값(가리키는 객체)은 변경할 수 없지만 **가리키고 있는 객체의 내부의 값은 final의 영향밖에 있기 때문에 변경이 가능**합니다.

아래 코드를 보면 가리키는 객체를 변경하지 못하지만, 객체 내부의 값은 변경할 수 있다는 사실을 알 수 있습니다.


<center>
  <img
    src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2Fo64w3%2FbtrQgFF8BpH%2FSiPDWCgsPPQGKRKOhcu1N1%2Fimg.png"
    width="80%"
    height="80%"
  />
</center>
setAge()를 통해 내부 값을 변경 가능하지만, new Person()으로 값 재할당 불가

<br/>

<center>
  <img
    src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FejvZ9u%2FbtrQgMrPoUC%2FXkprS1hgPIt509DFDbj1ok%2Fimg.png"
    width="80%"
    height="80%"
  />
</center>
배열 내부 원소의 값은 변경할 수 있지만, 다른 배열 객체로는 변경 불가

<br/>
<br/>

## **정리**
-   final 키워드를 클래스에 사용하면 상속을 제한한다.
-   메서드에 사용하면 오버라이드를 제한한다.
-   변수에 사용할 경우 해당 값의 변경을 제한한다
-   함조형 변수에 사용할 경우 가리키는 객체는 바꿀 수 없지만, 객체 내부 값은 final 영향 밖이기 때문에 변경이 가능하다

<br/>

## Reference
-   [https://www.geeksforgeeks.org/final-keyword-in-java/](https://www.geeksforgeeks.org/final-keyword-in-java/)
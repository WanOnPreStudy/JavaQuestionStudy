Call by Reference와 Call by Value의 차이에 대해 설명하시오. + 자바에서 사용하는 방식은?

## 값의 전달 방식

- Call by Reference :
    - 함수의 인자를 전달 시 주소를 전달
    - 데이터 값을 복사해서 함수의 인자로 넘길때마다 메모리 공간을 할당해야 하는 문제는 해결되지만 원본값이 변경될 위험이있다.
    - 함수 안에서 인자값이 변경되면, argument로 전달되는 객체값도 변경
    
    ```java
    class Test{
    	
    	int value;
    	
    	Test(int value) {
    		this.value = value;
    	}
    	
    }
    
    public class Main {
    	
    	public static void main(String[] args) {
    		Test a = new Test(10);
    		Test b = new Test(20);
    
    		changeByValue(a,b);
    
    		System.out.println("a : " + a.value);
    		System.out.println("b : " + b.value);
    	}
    	
    	static void changeByValue(Test a1, Test b1) {
    		a1.value = 100;
    		b1.value = 200;
    	}
    }
    ```
    

```java
a : 100
b : 200
```

- Call by Value : 값에 의한 호출
    - 함수의 인자를 전달 시 값을 전달
    - 함수 호출 시 전달되는 변수의 값을 복사해서 함수의 인자로 전달
    - 함수 호출 시 함수의 인자의 데이터 값을 복사해서 넘길때마다 메모리 공간을 할당해야 한다.
    - 복사된인자는 함수안에서 지역적으로 사용하므로 local variable 처럼 사용
    
    만약 함수 A에서 B로 int 변수를 전달한다고 했을 때, 넘겨받은 B에서 어떤 행동을 하던지
    
    변수에는 변함이 없습니다.
    
    ```java
    public class SwapTest {
        public static void swap(int a) {
            a = 10;
        }
     
        public static void main(String[] args) {
            int a = 1;
            System.out.println("a : " + a);
            swap(a);
            System.out.println("a : " + a);
            
        }
    }
    ```
    

```java
a : 1
a : 1
```

자바에서 사용하는 방식은 Call by Value 이지만 reference type(참조 자료형)을 파라미터로 넘길때는 객체의 주소값을 복사하여 사용한다.

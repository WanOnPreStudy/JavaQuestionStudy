# 접근제어자(Access Modifier)

----

### 제어자(Modifier)

> **클래스와 클래스 멤버의 선언 시 사용하여 부가적인 의미를 부여하는 키워드**를 의미한다.

### 

### 접근 제어자란

> 객체 지향에서 정보 은닉(data hiding)이란 사용자가 굳이 알 필요가 없는 정보는 사용자로부터 숨겨야 한다는 개념이다.
> 
> 자바에서는 이러한 정보 은닉을 위해 **접근 제어자(access modifier)라는 기능을 제공**한다.
> 
> 접근 제어자를 사용하면 클래스 외부에서의 직접적인 접근을 허용하지 않는 멤버를 설정하여 **정보 은닉을 구체화**할 수 있습니다.

### 접근 제어자의 종류

> Public : 변수, 메소드는 어떤 클래스에서라도 접근이 가능하다.
> 
> Private : 변수, 메소드는 해당 클래스에서만 접근이 가능하다.
> 
> Protected : 변수, 메소드는 동일 패키지의 클래스 또는 해당 클래스를 상속받은 다른 패키지의 클래스에서만 접근이 가능하다.
> 
> default : 변수, 메소드는 default 접근 제어자가 되어 해당 패키지 내에서만 접근이 가능하다. **별도의 예약어는없다**
> 
> ```null
> 접근범위 : public > protected > default > private
> ```

#### 

#### Private

![](/Users/byeonsungmun/Library/Application%20Support/marktext/images/2022-10-26-19-48-50-image.png)

#### Public

![](/Users/byeonsungmun/Library/Application%20Support/marktext/images/2022-10-26-19-51-37-image.png)

##### Default

![](/Users/byeonsungmun/Library/Application%20Support/marktext/images/2022-10-26-19-52-15-image.png)

##### Protected

![](/Users/byeonsungmun/Library/Application%20Support/marktext/images/2022-10-26-19-52-45-image.png)

### 정리

![](/Users/byeonsungmun/Library/Application%20Support/marktext/images/2022-10-26-19-18-56-image.png)

- 접근제어를 사용하는 이유는 **보안**때문이다.

- public 접근 제어자를 사용하면 다른 클래스에서도 접근해서 값을 변경하는 등 예기치 않는 상황을 방지하기위해서다.

## 좋은 객체지향의 설계의 5가지 원칙

### SRP : 단일 책임 원칙 (Single Resposibility Priciple)

- 하나의 클래스는 하나의 책임을 가진다
    - 하지만 하나의 책임의 기준이 애매함

      ⇒ 변경이 있을 때 파급효과가 적으면 단일 책임 원칙을 잘 따르는 것


### ⭐OCP : 개방-폐쇄 원칙(Open/closed Priciple)

- 소프트웨어 요소는 확장에는 열려있으나 변경에는 닫혀있다.
- 다형성은 잘지켜지고 있음

    ```java
    public class MemberService {
    	//jdbcrepository로 변경해야하는 상황이라면 코드의 변경이 발생. 
    	//private MemberRepository memberRepository = new MemberRepository();
    	private MemberRepository memberRepository = new JdbcMemberRepository();
    }
    ```


클라이언트(MemberService)가 구현 클래스(new JdbcMemberRepository)를 직접 선택하게되는 상황발생 → ocp원칙 만족하지 못함.

객체를 생성하고, 연관관계를 맺어주는 별도의 조립장치가 필요 = spring container, DI, IoC필요

### LSP: 리스코프 치환 원칙(Liskov substitution Priciple)

- 인터페이스에서 정의한 기능을 무조건 따라야 함
    - 예)자동차 인터페이스 악셀기능은 앞으로 간다는 기능으로 정의됨
    - car interface를 구현한 테슬라 구현체가 엑셀 메서드 실행 시 뒤로가면 리스코프 치환 원칙을 위배하게됨


### ISP : 인터페이스 분리 원칙

- 특정 클라이언트를 위한 인터페이스 여러개가 하나의 덩치가 큰 인터페이스보다 낫다.
- 자동차 인터페이스 → 운전, 정비 인터페이스로 나눔
- 운전자, 정비사 클라이언트가 정비 인터페이스에서 변경이 일어나면 운전자 구현체에 영향주지 않음

### ⭐DIP : 의존관계 역전 원칙(Dependency Inversion Principle)

클라이언트가 추상화(인터페이스)에 의존해야함

상위모듈은 하위 모듈의 구현에 의존해서는 안된다. 하위 모듈이 상위 모듈에 정의한 추상타입에 의존해야한다.

예제)

추상화에 의존하지 않은 경우

예시로 결제수단 서비스를 호출하는 컨트롤러이다. 결제 서비스 호출 시 PaymentController는 ShinhanCardPaymentService에 강하게 의존한다.

동일한 로직의 다른 카드사의 호출이 필요할 경우 아래 코드는 확장성이 떨어지게 된다.

```java
class PaymentController {
    @RequestMapping(value = "/dip/anti/payment", method = RequestMethod.POST)
    public void pay(@RequestBody ShinhanCardDto.PaymentRequest req){
        shinhanCardPaymentService.pay(req);
    }   
}
class ShinhanCardPaymentService {
    public void pay(ShinhanCardDto.PaymentRequest req) {
        shinhanCardApi.pay(req);
    }   
}
```

컴파일 단계에서는 PaymentController는 CardPaymentService를 바라보지만 런타임에서는 getType으로 얻은 객체  CardPaymentService의 구현체 ShinhanCardPaymentService를 바라보게된다.

```java
class PaymentController {
    @RequestMapping(value = "/payment", method = RequestMethod.POST)
    public void pay(@RequestBody CardPaymentDto.PaymentRequest req) {
        final CardPaymentService cardPaymentService = cardPaymentFactory.getType(req.getType());
        cardPaymentService.pay(req);
    }
}

public interface CardPaymentService {
    void pay(CardPaymentDto.PaymentRequest req);
}

public class ShinhanCardPaymentService implements CardPaymentService {
    @Override
    public void pay(CardPaymentDto.PaymentRequest req) {
        shinhanCardApi.pay(req);
    }
}
```

---

다형성만으로 클라이언트 코드의 변경을 막을 수 없다.

다형성만으로 OCP, DIP 원칙을 지킬 수 없다.

### IoC(Inversion of Control, 제어의 역전)

코드의 흐름을 제어하는 주체가 바뀌는것, 객체 자체가 수행하던 행동(객체 생성, 객체의 생명 주기 관리, 메소드 수행등)들을 제3자가 수행

→ 프레임워크와 라이브러리의 차이를 묻는 질문에 IoC관점에서 설명이 가능

라이브러리는 나의 코드가 이용하지만, 프레임워크는 나의 코드를 실행하고 제어권을 갖는다.

### DI(Dependency Injection, 의존성 주입)

오브젝트를 직접 생성이 아닌 외부에서 주입받는 방식

---

참고

[Spring 예제로 보는 SOLID DIP - Yun Blog | 기술 블로그](https://cheese10yun.github.io/spring-solid-dip/)

[https://vagabond95.me/posts/about-ioc-dip-di/](https://vagabond95.me/posts/about-ioc-dip-di/)
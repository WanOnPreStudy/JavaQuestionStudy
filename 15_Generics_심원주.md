# Generics
다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일 시 타입체크 해주는 기능
-> 타입 안정성을 제공
-> 타입체크와 형변환을 생략해 코드가 간결해짐

* 용어
	`Class Box<T> {}`
	Box<T> 	: 제네릭 클래스
	T		: 타입변수 또는 타입 매개변수(T는 타입 문자)
	Box		: raw type

	**타입변수**는 T가 아닌 다른 것을 사용할 수 있다
	ArratList<E>, Map<K,V> 처럼 상황에 맞게 의미있는 문자를 선택하는 것이 좋고
	기호의 종류만 다를 뿐 모두’ 임의의 참조형 타입’을 의미한다

	`Box<String> b = new Box<String>();`
	**제네릭 타입 호출** : 타입 매개변수에 타입을 지정하는것
	지정된 타입(String)을 **매개변수화된 타입**이라 함



제네릭 타입은 클래스와 메서드에 선언할 수 있다
# 제네릭 클래스
```
class Box<T>{
	T item;

	void setItem(T item) { this.item = item; }
	T getItem() { return item; } 
}
```

* 제네릭 클래스의 객체 생성 시 객체별로 다른 타입을 지정하는것은 적절하지만 **static 멤버에 타입변수 T를 사용할 수는 없다**. 
<- static멤버는 타입변수에 지정된 타입과 관련없이 동일해야하기 때문.
 T는 인스턴스변수로 간주되며 static멤버는 인스턴스변수를 참조할 수 없다.
`class Box<T>{
	static T item;
	static int compare(T t1, T t2){…} 
}`

* 제네릭 타입의 배열을 생성할 수 없다
New 연산자는 컴파일 시점에 타입이 무엇인지 정확히 알아야한다.
-> new 연산자 대신 newInstance()와 같이 동적으로 객체를 생성하는 메소드로 배열을 생성하거나 Object 배열을 생성하고 복사한 다음 T[]로 형변환하는 방법을 사용할 수 있다
* Box<T>의 객체를 생성할 때는 참조변수와 생성자에 대입된 타입이 일치해야한다
`Box<Apple> appleBox = new Box<Apple>()`
* 상속관계에서도 일치하지않으면 에러가 발생한다
(두 타입이 상속관계에 있고 대입된 타입이 같은것은 괜찮다)
`Box<Apple> appleBox = new FruitBox<Apple>()`
* JDK1.7 부터는 추정이 가능한 경우 타입을 생략할 수 있다
아래 두 문장은 동일
`Box<Apple> appleBox = new Box<Apple>();`
`Box<Apple> appleBox = new Box<>();`
* 생성된 box<T> 객체에 객체를 추가할 때 대입된 타입과 다른 타입의 객체는 추가할 수 없다
`Box<Apple> appleBox = new Box<Apple>();`
`appleBox.add(new Apple()); ` -> ok
`appleBox.add(new Grape()); ` -> error
* 단,  자손들은 메서드의 매개변수가 될 수 있다
`Box<Fruit> fruitBox = new Box<Fruit>();`
`fruitBox.add(new Fruit()); ` -> ok
`fruitBox.add(new Grape()); ` -> ok

## 제한된 제네릭클래스
타입 매개변수에 지정 타입 종류를 제한하는 방법으로 자손 타입으로만 지정할 수 있다
`class FruitBox<**T extends Fruit**>{
	ArrayList<T> list = new ArrayList<T>();
}`

## 와일드카드
어떠한 타입도 될 수 있으며 상한과 하한을 제한할 수도 있음
<**? extend T**>	:상한제한. T와 그 자손들만 가능
<**? super T**>		:하한제한. T와 그 조상들만 가능
<**?**>				: 제한 없음. <? extends Object> 와 동일



# 제네릭 메서드
메서드의 선언부에 제네릭 타입이 선언된 메서드. **한 개의 로직으로 여러 타입의 인자와 리턴 타입을 가실 수 있는 메소드**로 타입변수의 사용 범위가 메소드 내로 한정된다.

* 제네릭 메소드 사용 방법(변환 예시)
 Static Juice makeJuice(FruitBox<**? Extends Fruit**> box){ …}  
->
```
Static <T extends Fruit> Juice makeJuice(FruitBox<T> box){
	...
}
```

* 메서드 내에서만 지역적으로 사용되기때문에 static과 함께 사용할 수 있다.
* 메서드를 호출할 때는 타입변수에 대입해야한다
`FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();`
* 대부분 컴파일러가 타입을 추정할 수 있지만 대입된 타입을 생략할 수 없을때는 참조변수나 클래스 이름을 생략할 수 없다
 `System.out.println(<Fruit>makeJuice(fruitBox))` -> error
 `System.out.println(this.<Fruit>makeJuice(fruitBox))` -> ok
 `System.out.println(Juicer.<Fruit>makeJuice(fruitBox))` -> ok

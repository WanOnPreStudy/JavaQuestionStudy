Autoboxing Unboxing

Autoboxing : 컴파일러가 기본형타입을 참조타입(Wrapper Class)으로 자동변환하는 것
Unboxing   : 컴파일러가 참조타입(Wrapper Class)을 기본형타입으로 자동변환하는것 

int -> Integer
char -> Character
boolean -> Boolean
short -> Short
...

자바에서는 데이터타입을 크게 두가지로 나눌 수 있다.
1. primitive Type 기본형 : boolean, char, int, short, long, float, double 실제 데이터값을 저장하는 타입
2. Reference Type 참조형 : 객체(Object)의 주소를 저장. 메모리주소로 객체를 참조하는타입

primitive Type
특징 : 가볍다. Stack 메모리에 데이터 저장. Null 포함 불가.제너릭타입에서 사용불가

Reference Type : 객체 
특징 : 무겁다. Heap 메모리에 데이터 저장. 주소만 Stack에 저장. Null포함 가능. 제너릭타입에서 사용가능


여기서 primitiveType을 객체화 시켜서 Reference Type으로 만든 것이  Wrapper Class이다. 



자바컴파일러가 Autoboxing하는 경우
기본형이 Wrapper Type을 변수로 받는 메서드의 파라미터를 통과할 때  ex) int i=10; autoB(i); Integer autoB(Integer num){ return num;}
기본형이 Wrapper Type의 변수로 할당될 때  ex) int i=10; Integer in= i;


자바컴파일러가 Unboxing하는경우
Wrapper Type이 기본형을 변수로 받는 메서드의 파라미터를 통과할 때  ex) Character c='a'; unB(c);  char unB(char ch){ return ch; }
Wrapper Type이 기본형의 변수로 할당될 때  ex) Character c='a'; char ch=c;



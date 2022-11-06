# 동일성

동일성은 동일하다는 뜻으로 두 개의 객체가 완전히 같은 경우를 의미한다.

여기서 완전히 같다는 뜻은 두 객체가 사실상 하나의 객체로 봐도 무방하며, 주소 값이 같기 때문에 두 변수가 같은 객체를 가리키게 된다.

# 동등성

동등성은 동등하다는 뜻으로 두 개의 객체가 같은 정보를 갖고 있는 경우를 의미한다. 

동등성은 변수가 참조하고 있는 객체의 주소가 서로 다르더라도 내용만 같으면 두 변수는 동등하다고 이야기할 수 있다. 

동일하면 동등하지만, 동등하다고 동일한 것은 아니다. 그리고 해당 변수가 동등한지 equals 연산자를 통해 판별할 수 있다.

 

# ==과 equals() 공통점

비교한 값을 boolean type으로 반환

# ==과 equals() 형태의 차이

equals()는 메서드

==은 비교를 위한 연산자

# ==과 equals() 주소값 비교와 내용 비교

equals 메소드는 비교하고자 하는 대상의 내용 자체를 비교

== 연산자는 비교하고자 하는 대상의 주소값을 비교.

단순하게 말하면 == 연산자는 int,boolean과 같은 primitive type에 대해서는 값을 비교한다. reference type에 대해서는 주소값을 비교한다.

# ==과 equals() 예시

```java
//primitive type
int a = 10;
int b = 10;
int c = 20;

System.out.println(a==b);    //true
System.out.println(a==c);    //false
```
== 연산자는 int,boolean과 같은 primitive type에 대해서는 값을 비교

사실 primitive type도 Constant Pool에 있는 특정 상수를 참조하는 것이기 때문에 결국 주소값을 비교하는 것으로 볼 수 있다.
같은 상수를 참조하면 주소값이 같으니 결국 같은 값이면 동일하다고 판단할 수 있다.

```java
//reference type - Integer
Integer i1 = new Integer(10);
Integer i2 = new Integer(10);

System.out.println(i1 == i2); // false
System.out.println(i1.equals(i2)); // true

Integer i3 = Integer.valueOf(10); // Integer i3 = 10;
Integer i4 = Integer.valueOf(10);

System.out.println(i3 == i4); // true

Integer i5 = Integer.valueOf(1000);
Integer i6 = Integer.valueOf(1000);
        
System.out.println(i5 == i6); // false
```
== 연산자는 reference type에 대해서는 주소값을 비교

equals 연산자는 내용일 비교

Integer 클래스는 내부에서 integer 사용을 위해 IntegerCache를 관리합니다. 
이 캐시의 기본 범위는 -128 ~ 127이며, Integer.valueOf() 메소드는 캐시 범위에 해당하는 objects를 리턴합니다. 
그렇기에, i3와 i4 둘다, 같은 object 를 가리키게 되므로, i3==i4는 true

```java
//reference type - String
String text1 = "sample";
String text2 = text1;
String A = "Java"; // 리터럴(literal)       		주소값 : 1000 (예시 - 실제주소는 다름)
String B = "Java"; // 리터럴(literal)			주소값 : 1000
String C = new String("Java"); // new 연산자	주소값 : 2000
String D = new String("Java"); // new 연산자	주소값 : 3000


System.out.println(text1==text2);            //true
System.out.println(text1.equals(text2));    //true
System.out.println(A==B);            //true
System.out.println(A.equals(B));    //true
System.out.println(C==D);    // false
System.out.println(C.equals(D));    //true
System.out.println(A==C);    // false
System.out.println(A.equals(C));    //true
```
```java
public boolean equals(Object anObject) {
      if (this == anObject) {
          return true;
      }
      if (anObject instanceof String) {
          String anotherString = (String) anObject;
          int n = value.length;
          if (n == anotherString.value.length) {
              char v1[] = value;
              char v2[] = anotherString.value;
              int i = 0;
              while (n-- != 0) {
                  if (v1[i] != v2[i])
                          return false;
                  i++;
              }
              return true;
          }
      }
      return false;
}
```
== 연산자는 reference type에 대해서는 주소값을 비교

equals 연산자는 내용일 비교

```java
class Test1{
    int x;
    String y;
    public Test1(int x, String y){
        this.x=x;
        this.y=y;
    }
    public void setX(int x){this.x=x;}
    public void setY(String y){this.y=y;}
}

Test1 test1_1 = new Test1(1,"1");
Test1 test1_2 = new Test1(1,"1");

System.out.println(test1_1==test1_2); // false
System.out.println(test1_1.equals(test1_2)); // false
```
사용자 정의 class type의 경우

== 연산자는 주소값을 비교

equals 연산자는 주소값을 비교

```java
class Test2{
    int x;
    String y;
    public Test2(int x, String y){
        this.x=x;
        this.y=y;
    }
    public void setX(int x){this.x=x;}
    public void setY(String y){this.y=y;}
    @Override
    public boolean equals(Object obj){
        if(obj instanceof Test2){
            Test2 test2 = (Test2) obj;
            if(x==test2.x && y.equals(test2.y)){
                return true;
            } else{
                return false;
            }
        }
        else{
            return false;
        }
    }
}

Test2 test2_1 = new Test2(1,"1");
Test2 test2_2 = new Test2(1,"1");

System.out.println(test2_1==test2_2); // false
System.out.println(test2_1.equals(test2_2)); // true
```
equals 연산자를 재정의할 수 있음. String에 구현한것처럼 사용자 정의 class를 사용자가 오버라이딩해서 정의해야함.


```java
class Person{
    long id;

    public boolean equals(Object obj){
        if(obj instanceof Person){
            return id == ((Person)obj).id;
        }
        else{
            return false;
        }
    }
    Person(long id){
        this.id = id;
    }
}

Person p1 = new Person(21710736L);
Person p2 = new Person(21710736L);

System.out.println(p1 == p2); // false
System.out.println(p1.equals(p2)); // true
```
# equals 메서드 재정의

```java
@Override
public boolean equals(Object o){
  if(this==0) return true; // 자기 자신과 비교하면 true;
  if(o==null || getClass()!=o.getClass()) return false; // null이거나 다른 클래스이면 false
  Person person = (Person) o;
  return id=person.id && Objects.equals(name, person.name); // 속성 값 비교
}
```
equals는 최고 조상 Object 객체의 메서드이기 때문에 오버라이딩을 통해 재정의.


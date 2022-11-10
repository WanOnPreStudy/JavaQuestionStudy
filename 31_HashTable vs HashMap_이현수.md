```java
// HashMap
Map<Integer, String> map = new HashMap<>();
map.put(1, "one");
map.put(2, "two");

System.out.println(map.get(1)); // one
System.out.println(map.get(2)); // two

// Hashtable
Map<Integer, String> table = new Hashtable<>();
table.put(1, "one");
table.put(2, "two");

System.out.println(table.get(1)); // one
System.out.println(table.get(2)); // two
```


- HashMap과 Hashtable 모두 map 자료구조.

- Hashtable는 자바에서 해시 테이블을 구현한 클래스 중 가장 오래되었습니다. 그리고 두 번째로 구현한 클래스는 HashMap 클래스. 

- 일반적으로 사용법과 특징이 거의 동일. 예를들면 key - value 형태이고 key는 중복될 수 없고, value는 중복될 수 있다는 특징.

- 두개 모두 separate chaining 방식

# Hashtable

Hashtable 클래스는 컬렉션 프레임웍이 만들어지기 이전부터 존재하던 것이기 때문에 컬렉션 프레임워의 명명법을 따르지 않습니다.

Vector나 Hashtable과 같은 기존의 컬렉션 클래스들은 호환을 위해, 설계를 변경해서 남겨두었지만 가능하면 사용하지 않는 것이 좋습니다. 
대신 ArrayList와 HashMap을 사용하는 것이 좋습니다.

---

Hashtable은 동기화를 지원하여, thread-safe하다.

멀티스레드 환경에서 사용하기 좋은 자료구조. HashMap에 비해 느리다. (다른 스레드가 block되고 unblock 되는 대기 시간을 기다리기 때문)

```java
// get method
public synchronized V get(Object key) {
    // ... 중략 ...
}
    
// put method
public synchronized V put(K key, V value) {
    // ... 중략 ...
}
```
IDE를 이용해 구현부를 보면 synchronized 키워드가 붙어있다.

```java
Map<Integer, String> table = new Hashtable<>();
table.put(null, "null");
// Exception in thread "main" java.lang.NullPointerException
//	at java.util.Hashtable.put(Hashtable.java:465)
//	at Main.main(Main.java:10)
```
또, Hashtable은 key값이나 value값에 null이 들어갈 수 없다.

키가 같은 값을 두번 넣게 되면 초기 값을 유지.

Hashtable은 같은 경우 Enumeration(not fail-fast Enumeration)을 반환.
```
Enumeration과 Iterator는 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스.

Enumeration은 컬렉션 프레임워크 이전에 사용되던 인터페이스로 Iterator의 사용을 권장.
```

# HashMap

HashMap은 동기화를 지원하지 않는다.

단일스레드 환경에서 사용하기 좋은 자료구조

```java
// get method
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}
    
// put method
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```
IDE의 구현부를 보면, 아래 Hashtable과는 다르게 synchronized 키워드가 없음

```java
Map<Integer, String> map = new HashMap<>();
map.put(null, "null");

System.out.println(map.get(null)); // null
```
또, HashMap은 key 값이나 value 값에 null이 들어갈 수 있음

키가 같은 값을 두번 넣게 되면 두번째 값으로 덮어버림.

HashMap은 저장된 요소들의 순회를 위해 Fail-Fast Iterator를 반환. HashMap은 Enumeration을 제공하지 않습니다.

```
Enumeration과 Iterator는 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스.

Enumeration은 컬렉션 프레임워크 이전에 사용되던 인터페이스로 Iterator의 사용을 권장.
```

# HashMap과 Hashtable 클래스의 차이점

![image](https://user-images.githubusercontent.com/65898555/201068757-0f22f6ad-d026-4a6b-89f6-dab6162796ac.png)

HashMap은 보조해시를 사용하기 때문에 보조 해시 함수를 사용하지 않는 Hashtable에 비하여 해시 충돌(hash collision)이 덜 발생할 수 있어 상대적으로 성능상 이점이 있다.

최근까지 Hashtable은 구현에 거의 변화가 없지만, HashMap은 현재까지도 지속적으로 개선되고 있다.




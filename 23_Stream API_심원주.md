# Stream API
데이터를 다루는 방식으로
컬렉션이나 배열에 데이터 담고 for문과 iterator 이용해 코드로 작성할 때
	1. 가독성 떨어지고 재사용성 떨어지고
	2. 데이터 소스마다 다른 방식으로 다뤄야 한다는(Collection.sort(), Arrays.sort()) 단점이 있었음

-> 스트림
데이터소스를 추상화해서 재사용성 높아짐
배열, 컬렉션, 파일에 저장된 데이터까지 같은 방식으로 다룰 수 있음
```
String[] strArr = {"aa","A","c"}
Stream<String> strStream1= Arrays.stream(strArr);

List<String> strList = Arrays.asList(strArr);
Stream<String> strStream2= strList.stream();

```


# 스트림의 특징
* 데이터소스의 변경이 없다
`List<String> sortedList = strStream2.sorted().collect(Collectors.toList());`
Iterator처럼 일회용이며 한번 사용하면 닫혀서 사용할 수 없고 필요하다면 다시 생성해야한다

* 작업을 내부 반복으로 처리해 간결하다
반복문을 메서드의 내부에 숨길 수 있다. 
```
for(String str:strList)
	System.out.println(str);
```
-> `stream.forEach(System.out::println);`

* 다양한 연산을 제공해 복잡한 연산을 간단히 처리할 수 있다
	* 중간 연산: 연산결과가 스트림. 연속해서 사용 가능
	* 최종 연산: 스트림의 요소를 소모하며 연산을 수행하므로 한번만 가능

# 사용 방법
```
public List<Member> findAdultAsName(List<Member> members, int count) {
  return members.stream()    // 멤버 리스트에서 스트림을 얻는다.
  .filter(member -> member.isAdult())    // 중간 연산
  .limit(count)    // 중간 연산
  .map(Member::getName)    // 중간 연산
  .collect(Collectors.toList());    // 최종 연산, 스트림을 원하는 컬렉션 자료구조로 변경한다.
  }
```

## 생성하기
* 배열, 컬렉션, 임의의 수 등 다양한 소스로부터 스트림을 생성할 수 있다

```
public static void main(String[] args) {
  List<String> member = new ArrayList<>();
  Stream<String> stream = member.stream();
  }
//list를 대상으로 한 스트림
```
* 빈 스트림을 생성
Stream.empty()
* 스트림을 연결

## 중간연산
* 자르기
	* Stream<T> skip(long n): 처음 n개의 요소 건너뛴다
	* Stream<T> limit(long maxSize): 스트림의 요소를 maxSize개로 제한한다
* 걸러내기
	* Stream<T> filter(Predicate<? Super T> predicate): 중복요소 제거
	* Stream<T> distinct(): 주어진 조건에 맞지않는 요소 제거
* 정렬
	* Stream<T> sorted(Comparator<? Super T> comparator): comparator지졍하지 않으면 기본 정렬기준으로 정렬
* 변환
	* map(Function<? super T, ? extends R> mapper): 원하는 요소만 뽑아내거나 특정 형태로 변환해야 할 때 사용
	* flatmap(): 배열을 하나의 스트림으로 생성
* 조회
	* peek(): 중간 연산 사이에 연산이 올바르게 되고있는지 확인할 때 사용

## 최종연산
연산 후에는 스트림이 닫히게 되며 값은 단일 값, 배열, 컬렉션이 될 수 있다.
* void forEach(Consumer<? Super T> action): 스트림의 요소를 출력
	`.forEach(System.out::println);`
* 조건 검사
allMatch(), anyMatch(), noneMatch(), findFirst(), findAny()
* 통계
counting(), maxBy(), minBy()
* 컬렉션과 배열로 변환
toList(),  toSet(), toCollection(), toMap(), toArray()
* collect(): 스트림의 요소를 수집하는 연산으로 수집 방법을 정의
* 그 외 문자열 결합, 그룹화, 분할 등

그 외 메소드들은 공식문서에서 확인 가능
[Stream (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)

- - - -
참고자료
[StreamAPI 나도 한 번 써보자!](https://tecoble.techcourse.co.kr/post/2021-05-23-stream-api-basic/)
[Java 스트림(stream) - 최종 연산](https://velog.io/@max9106/Java-%EC%8A%A4%ED%8A%B8%EB%A6%BCstream-%EC%B5%9C%EC%A2%85-%EC%97%B0%EC%82%B0)

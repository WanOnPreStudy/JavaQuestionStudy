# 직렬화(Serialization)

- 자바 시스템 내부에서 사용되는 Object 또는 Data를 외부의 자바 시스템에서도 사용할 수 있도록 byte 형태로 데이터를 변환하는 기술
- JVM(Java Virtual Machine)의 메모리에 상주 되어 있는 즉, **런타임 시 메모리에 적재되어 있는 객체 데이터를 바이트 형태로 변환하는 기술**

# 역직렬화(Deserialize)

- byte로 변환된 Data를 원래대로 Object나 Data로 변환하는 기술을 역직렬화(Deserialize)라고 부릅니다.
- 직렬화된 바이트 형태의 데이터를 객체로 변환해서 JVM으로 상주시키는 형태

# 직렬화 예제

`java.io.Serializable` 인터페이스를 상속받은 객체는 직렬화할 수 있습니다.

```java
public class SerializableObject implements Serializable {
    private String name;
    private int age;
    private String gender;

    public SerializableObject(
            String name,
            int age,
            String gender
    ) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    public String toString() {
        return String.format("SerializableObject{name='%s', age='%d', gender='%s'}", name, age, gender);
    }
}
```

`java.io.ObjectOutputStream` 클래스를 사용하여 직렬화를 진행합니다.

```java
// 직렬화
public static String encodingObjectToByteArray(SerializableObject object) {
    byte[] serializeObject = new byte[0];

    try(ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
        try(ObjectOutputStream oos = new ObjectOutputStream(baos)) {
            oos.writeObject(object);
            // serializedObject -> 직렬화된 object 객체
            serializeObject = baos.toByteArray();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

		// 바이트 배열로 생성된 직렬화 데이터를 base64로 Encoding
    String base64 = Base64.getEncoder().encodeToString(serializeObject);
    System.out.println(base64);
    return base64;
}
```

**결과 확인**

![Serializable](https://user-images.githubusercontent.com/90227655/203539748-8e29d749-2473-4f66-8ae5-b7e0b9467c12.PNG)

# 역직렬화 예제

역직렬화는 `java.io.ObjectInputStream` 클래스를 사용하여 진행합니다. 또한 직렬화한 객체는 다른 자바 시스템에서 역직렬화 가능합니다.

```java
// 역직렬화
public static void decodingObjectFromByteArray(String base64) {
    // base64로 변환된 바이트 배열 Decoding
    byte[] serializeObject = Base64.getDecoder().decode(base64);

    try(ByteArrayInputStream bais = new ByteArrayInputStream(serializeObject)) {
        try(ObjectInputStream ois = new ObjectInputStream(bais)) {
            Object object = ois.readObject();
            SerializableObject serializableObject = (SerializableObject) object;
            System.out.println(serializableObject);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

**결과 확인**

![Deserializable](https://user-images.githubusercontent.com/90227655/203539774-7c0973fa-8723-467d-ae33-5a9d55f3cc29.PNG)

만약 역직렬화 시 대상이 되는 객체에 변경이 있으면 `java.io.InvalidClassException` 예외를 발생시킵니다. 때문에 역직렬화 대상이 되는 객체는 반드시 **serialVersionUID를 일치 시켜 같은 버전임을 명시**해야 합니다. (없는 경우, 클래스의 기본 해쉬값을 사용)

```java
public class SerializableObject implements Serializable {

		// serialVersionUID 버전을 명시
    private static final long serialVersionUID = 1L;

    private String name;
    private int age;
    private String gender;
    private String email;      // <- 속성 추가, 변경이 생겨도 역직렬화 가능

    public SerializableObject(
            String name,
            int age,
            String gender
    ) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    public String toString() {
        return String.format("SerializableObject{name='%s', age='%d', gender='%s'}", name, age, gender);
    }
}
```

일반적으로 serialVersionUID는 프로그램의 버전을 명시한다고 하네요.

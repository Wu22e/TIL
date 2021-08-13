# Dependency

`build.gradle`

```
repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.6.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine'
}
```

'org.junit.jupiter:junit-jupiter-api:5.6.0'와 'org.junit.jupiter:junit-jupiter-engine' 라이브러리가 의존성 설치가 되도록 build.gradle에 등록되어 있다. 이 의존성 등록은 mavenCentral이라는 리포지토리에서 위 두 라이브러리에 대한 자료를 찾아서 .jar 파일을 같이 빌드해 준다.



이 라이브러리들은 mavenCentral에서 검색해서 의존성 등록에 추가해 주면 된다. (https://search.maven.org/)

예를들어 javafaker라는 라이브러리를 등록하고 싶다면?

![image](https://blog.kakaocdn.net/dn/LY9UT/btrbh3NJE5k/NLVGskGkhIIqJ951vpfcR0/img.png)

![image](https://blog.kakaocdn.net/dn/bdHYAg/btrbvxTPShw/jr0AKmKdkfN3LZgJTUVVy0/img.png)

```java
repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.6.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine'
    implementation 'com.github.javafaker:javafaker:1.0.2' // 이렇게 등록 가능
}
```

gradle에 해당하는 의존성을 `build.gradle`에 등록하면 된다.

mavenCentral에서 javafaker의 1.0.2버전에 해당되는 라이브러리를 다운받아서 우리가 사용할 수 있도록 프로젝트에 내장해 준다.

```java
// build tool인 gradle이 파일을 등록해 줬기 때문에 
// 우리가 등록할 필요없이 그냥 import해서 갖다 쓰기만 하면 된다.
import com.github.javafaker.Faker;

public class Main {
    public static void main(String[] args) {
        Faker faker = new Faker();

        String title = faker.name().title();
        System.out.println(title);
    }
}
```

👉result

```
Lead Response Supervisor
```

실행할때마다 이름은 다른 값이 나오게 된다. faker는 가짜 데이터를 만들어내는 기능을 제공해 준다.

https://github.com/DiUS/java-faker 여기서 어떤 값을 만들어 낼 수 있는지 확인할 수 있다.



faker를 stream과 활용하여 랜덤 값을 출력할 수 있다.

```java
public class Main {
    public static void main(String[] args) {
        Faker faker = new Faker();
        // randomDigit은 한자리 숫자 (0~9)를 반환함
        // randomDigitNotZero() 는 0을 제외한 1~9를 반환함
        Stream.generate(()-> faker.number().randomDigit()) 
                .limit(10)
                .forEach(System.out::println);

    }
}
```

👉result

```
9
8
0
3
0
2
5
4
0
7
```



전에 배웠던 내용과 결합하여 배열로 만들 수도 있다.

```java
public class Main {
    public static void main(String[] args) {
        Faker faker = new Faker();

        Integer[] nums = Stream.generate(() -> faker.number().randomDigitNotZero())
                .distinct()
                .limit(3)
                .toArray(Integer[]::new);

        System.out.println(Arrays.toString(nums));

    }
}
```

👉result

```
[4, 9, 6]
```

위처럼 1~9중 랜덤한 3개의 값을 중복 없이 뽑아서 배열로 만들 수 있다.

이렇게 faker를 이용하면 java util의 random을 조합해서 사용하지 않고 쉽게 랜덤 값을 만들어 내서 사용할 수 있다.



또다른 라이브러리로 lombok이 있다. 이 롬복은 자바프로젝트할때 굉장히 많이 사용하는 라이브러리중 하나이다.(https://projectlombok.org/)

![image](https://blog.kakaocdn.net/dn/bWhCgt/btrbuJmEIMc/HxIMkzvm1KGGXbkNh8Hn9K/img.png)

![image](https://blog.kakaocdn.net/dn/dgULXb/btrbuBifwWh/2D3hcsTRhua7fODkKaJ0Fk/img.png)

해당 의존성을 build.gradle에 붙여 넣어주면 된다.

```java
repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.6.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine'
    
    // JavaFaker
    implementation 'com.github.javafaker:javafaker:1.0.2'
    
    // Lombok
    compileOnly 'org.projectlombok:lombok:1.18.20'
    annotationProcessor 'org.projectlombok:lombok:1.18.20'
    testCompileOnly 'org.projectlombok:lombok:1.18.20'
    testAnnotationProcessor 'org.projectlombok:lombok:1.18.20'
}
```



롬복은 굉장히 다양한 기능을 제공한다.

롬복 라이브러리를 설치하는 것 뿐만아니라 인텔리제이에서 롬복 플러그인을 설치해야 기능을 사용할 수 있다.

```java
public class User {
    private int age;
    private String name;
    
    public User(int age, String name) {
        this.age = age;
        this.name = name;
    }
}
```

원래 우리는 User 클래스를 위와 같이 만들었고, 만약 위 코드에서 생성자를 따로 작성하지 않았다면(오버로딩 하지 않았다면) 디폴트 생성자로 new 키워드로 만들 수 있고, 위 코드처럼 파라미터를 추가해서 오버로딩 하여 new 키워드로 객체를 생성할 수 있다.

근데 롬복을 통해서 아래와 같이 코드를 작성하면

```java
import lombok.AllArgsConstructor

@AllArgsConstructor
public class User {
    private int age;
    private String name;
}
```

롬복의 AllArgsConstructor을 import하고 @AllArgsConstructor 애노테이션을 달아주면 생성자를 만들어 주지 않아도 모든 파라미터를 가진 생성자를 사용할 수 있게 해준다. (모든 파라미터를 가진 생성자만 만들어 주므로 디폴트 생성자는 따로 만들어 줘야 한다. 이때는 @NoArgsConstructor 애노테이션을 추가해주면 된다.)

이전에 객체를 print하면 객체의 주소가 나와서 직접 toString을 오버라이딩하여 문자열로 표현하여 작성하였다. 그러나 롬복이 아주 편하게 아래와 같이 애노테이션을 제공해준다.

```java
import lombok.AllArgsConstructor
import lombok.ToString

@AllArgsConstructor
@ToString
public class User {
    private int age;
    private String name;
}
```

```java
public class Main {
    User user = new User(1, "a");
    System.out.println(user);
}
```

👉result

```
User(age=1, name=a)
```



뿐만아니라 데이터 클래스를 만들게 되면 equals랑 hashCode도 같이 오버라이딩 해줘야 하는데 이 또한 롬복이 제공해준다.

```java
import lombok.AllArgsConstructor
import lombok.ToString    
import lombok.EqualsAndHashcode

@AllArgsConstructor
@ToString
@EqualsAndHashcode
public class User {
    private int age;
    private String name;
}
```

```java
public class Main {
    User user = new User(1, "a");
    User user2 = new User(1, "a");
    System.out.println(user.equals(user2));
}
```

👉result

```
true
```

equals를 구현하지 않았지만 롬복에 의해 코드 구현을 해주게되고 (참조변수가 아닌 멤버변수의 값을 비교하는 코드로 오버라이딩) 따라서 결과는 값이 같기 떄문에 true가 나오게 된다.



getter, setter 또한 @Getter, @Setter 애노테이션을 통해 getAge, setAge(), getName(), setName() 메소드를 사용할 수 있게 된다.



이렇게 우리가 데이터 클래스를 만들게 되면

```java
@AllArgsConstructor
@NoArgsConstructor
@ToString
@EqualsAndHashCode
@Getter
@Setter
```

이것들을 항상 쓰게 된다. 그래서 이걸 하나로 퉁쳐서

```java
@Data
```

하나만 써주면 위 애노테이션 6개가 모두 적용된다.
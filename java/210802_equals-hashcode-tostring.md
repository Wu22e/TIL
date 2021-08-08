# 자바 Object 클래스의 메소드 (equals, hashCode, toString)

Object 클래스는 모든 클래스의 최고 조상이므로 모든 클래스는 Object클래스 메소드를 사용할 수 있다. 

그중에서 가장 많이쓰이는 메소드인 equals, hashCode, toString를 정리해 보았다.

## 1. equals

아래 코드는 Object 클래스에 정의 되어 있는 equals 메서드의 실제 내용이다.

```java
public boolean equals(Object obj) {
    return (this == obj) // 주소 비교
}
```

즉, 객체 자신(this)과 주어진 객체(obj)의 주소값을 비교한다.

```java
public class Main {
    public static void main(String[] args) {
        Value v1 = new Value(10);
        Value v2 = new Value(10);
        // v1과 v2 멤버변수는 10으로 같지만 서로 다른 인스턴스이므로
        // v1과 v2의 주소값은 다르다.
        
        // 주소값이 다르므로 false
        System.out.println(v1.equals(v2)); 
    }
}

class Value {
    int value;
    
    Value(int value) {
        this.value = value;
    }
}
```

👉result

```
false
```

만약 위 코드에서 주소값이 아닌 Value 클래스의 멤버변수인 value값으로 비교하려면 equals 메소드를 오버라이딩 해야한다.

```java
public class Main {
    public static void main(String[] args) {
        Value v1 = new Value(10);
        Value v2 = new Value(10);

        System.out.println(v1.equals(v2)); 
    }
}

class Value {
    int value;
    
    Value(int value) {
        this.value = value;
    }
    
    public boolean equals(Object obj) {
        if(obj!=null && obj instanceof Value) 
            return this.value == ((Value)obj).value;
        else
            return false;
    }
}
```

👉result

```
true
```

equals를 오버라이딩 할때 주의할 점은 매개변수 타입이 Object 이므로 

멤버변수를 비교하려면 Value 타입으로 형변환해야한다. 

참조변수의 형변환 전에 반드시 instanceof를 통해 Value 타입이 맞는지 먼저 확인해야 그다음 멤버변수를 조회할 수 있기 때문이다! 

그래서 타입이 Value가 아니면 값을 비교할 필요도 없이 false를 반환한다.



String, Date, File, wrapper클래스(Integer, Double 등) 클래스는 이러한 equals의 오버라이딩을 통해 주소값 비교가 아닌 내용을 비교하도록 되어있음.

하지만 의외로 StringBuffer 클래스는 오버라이딩 되어있지 않다. 그러나 toString 메소드는 오버라이딩 되어있으므로 StringBuffer의 내용 비교를 할때는 다음과 같이 작성한다.

```java
StringBuffer strbuf1 = new StringBuffer("a");
StringBuffer strbuf2 = new StringBuffer("a");

System.out.println(strbuf1.equals(strbuf2)); // false

String str1 = strbuf1.toString();
String str2 = strbuf2.toString();

System.out.println(str1.equals(str2)); // true
```

👉result

```
false
true
```

StringBuffer 인스턴스에 toString()을 호출하면 담고 있는 문자열을 String으로 반환하기 때문에 위와같이 비교할 수 있다.

## 2. hashCode

hashCode 메소드는 해싱(hasing)기법에 사용되는 해시함수(hash function)를 구현한 것이다. 

해시 함수는 찾고자 하는 값을 입력하면 그 값이 저장된 위치를 알려주는 해시코드(hashcode)를 반환하게 된다. 여기서 해시코드란 정수값인데, 자바의 Object 클래스에서 hashCode 메소드는 객체의 주소값으로 해시코드를 만들어 반환한다.

> 참고 : 해시코드가 4byte이기 때문에 32bit JVM에서는 서로 다른 두객체는 해시코드가 같을 수가 없었다. 하지만 64bit JVM에서는 8byte의 주소값에 대한 해시코드 값을 4byte의 해시코드로 만들기 때문에 해시코드가 중복될 수 있음.

결국 hashCode는 객체의 지문이라고도 불리기 때문에, hashCode를 이용하여 인스턴스 변수 값으로 객체의 같고 다름을 판단해야 한다면 equals메서드 뿐만 아니라 hashCode 메서드도 오버라이딩 해줘야 한다.

보통 오버라이딩은 다음과 같이 한다.

```java
public int hashCode() {
    return Objects.hash(멤버변수1, 멤버변수2, ...);
}
```

이렇게 되면 멤버변수 1, 2 .. 값이 같을때 같은 hashCode를 반환하게 된다.



아래 코드는 Object 클래스에 정의 되어 있는 hashCode 메서드의 실제 내용이다.

```java
public class Obejct {
    // ... 내용 생략
    public native int hashCode(); // 구현 내용 X
}
```

native라는 키워드가 사용되었는데, 이를 **네이티브 메서드** 라고한다. 네이티브 메서드란 OS의 메서드로 C언어로 작성되어 있다. 이미 OS에서 작성된 메서드를 사용하기 때문에 구현 내용이 없다.

> 참고 : 자바와 자바가 아닌 언어가 서로 호출할 수 있게 해주는 기술을 JNI 라고 한다.
>
> 위키 백과에서는 다음과 같이 설명하고 있다.
>
> JNI(Java Native Interface)는 JVM 위에서 실행되고 있는 자바 코드가 하드웨어나 운영체제 플랫폼에 종속된 프로그램이나 C, C++, 어셈블리 같은 다른 언어들로 작성된 라이브러리들을 호출하거나 반대로 호출되는 것을 가능하게 하는 프로그래밍 프레임워크이다.

String 클래스에는 hashCode가 오버라이딩 되어있기 때문에, 문자열 내용이 같으면 동일한 해시코드를 반환하도록 한다.

## 3. toString

객체를 문자열로 변환하기 위한 메서드이다.

기본적으로 객체는 인스턴스 변수(멤버 변수)의 집합이기 때문에 객체를 문자열로 변환한다는 것은 멤버 변수의 값을 문자열로 변환한다는 것과 같다.

아래 코드는 Object 클래스에 정의 되어 있는 toString 메서드의 실제 내용이다.

```java
public String toString() {
    return getClass().getName()+"@"+Integer.toHexString(hashCode());
}
```

getClass는 객체 자신의 클래스 정보를 담고있는 Class 인스턴스를 반환한다. 

(getClass 역시 Object 클래스에 정의된 메소드 이므로 인스턴스 없이 사용 가능)

그래서 클래스이름+ @(at, 위치) + 16진수 해시코드를 얻게 된다.

```java
class Card {
    String kind;
    int number;
    
    Card() {
        this("SPADE", 1);
    }
    Card(String kind, int number) {
        this.kind = kind;
        this.number = number;
    }
}

public class Main {
    public static void main(String[] args) {
        Card c1 = new Card();
        Card c2 = new Card();
        
        System.out.println(c1.toString());
        System.out.println(c2.toString());
    }
}
```

👉result

```
Card@73a28541
Card@6f75e721
```

위와 같이 호출하지 않고 멤버 변수 내용을 호출하고 싶으면 오버라이딩을 하여 멤버변수 값을 적절히 문자열로 만들어서 반환하면 된다.

```java
class Card {
    String kind;
    int number;
    
    Card() {
        this("SPADE", 1);
    }
    Card(String kind, int number) {
        this.kind = kind;
        this.number = number;
    }
    
    public String toString() {
        return "kind : " + kind + ", number : " + number; 
    }
}

public class Main {
    public static void main(String[] args) {
        Card c1 = new Card();
        Card c2 = new Card("HEART", 2);
        
        System.out.println(c1.toString());
        System.out.println(c2.toString());
    }
}
```

👉result

```
kind : SPADE, number : 1
kind : HEART, number : 2
```


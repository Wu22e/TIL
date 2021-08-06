# 디폴트 메서드 란?

현재 인터페이스는 굉장히 많은 형태로 사용되고 있다.

그러다보니 인터페이스의 단점들이 드러나기 시작하였고 그것을 해결하기위해 나온것이 디폴트 메서드(default method) 이다.



우선 default method의 역할을 살펴보자.

디폴트 메소드를 사용하면 인터페이스가 구현체(메소드의 구현부)를 가질 수 있게 된다.

```java
interface MyInterface { // 추상 메소드로만 이뤄진 클래스 == 인터페이스
    void method1(); // 구현이 없다 : 추상 메소드

    default void sayHello() { // 구현이 있다 (자바 8 부터 이런 구현 메소드를 인터페이스가 가질 수 있다) 대신 default 키워드를 써줘야 함
        System.out.println("Hello World");
    }
}

public class Main implements MyInterface{
    public static void main(String[] args) {
        new Main().sayHello();
    }

    @Override
    public void sayHello() {
        System.out.println("Bye World");
    }

    @Override
    public void method1() {
        throw new RuntimeException();
    }
}
```

MyInterface라는 인터페이스에 sayHello 디폴트 메소드로 선언하여 인터페이스 안에서 sayHello 메소드를 구현하고 있다. 이게 바로 디폴트 메소드의 역할이다.



그럼 이 디폴트 메소드는 도대체 왜 등장하게 된 것일까?

아래 예시를 살펴보자.

`MyInterface.java`

```java
interface MyInterface {
    void method1();
    void method2();
    void method3();
    void method4();
    void method5();
    void method6();
    void method7();
}
```

`MyInterfaceAdapter.java`

```java
public class MyInterfaceAdapter implements MyInterface{
    @Override
    public void method1() {}

    @Override
    public void method2() {}

    @Override
    public void method3() {}

    @Override
    public void method4() {}

    @Override
    public void method5() {}

    @Override
    public void method6() {}

    @Override
    public void method7() {}
}
```

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        new Service().method1();
    }
}

class Service extends MyInterfaceAdapter {

    @Override
    public void method1() {
        System.out.println("Hello World");
    }
}
```



만약 인터페이스에 메소드가 엄청 많은데, 내가 쓰고 싶은 몇개의 메소드만을 오버라이딩해서 쓰고 싶은 상황이 생기게 된다. (위 그림에서 MyInterface 메소드 7개를 선언하였는데, 인터페이스의 메소드가 많다고 가정하고 만든 메소드이다) 

그렇게 되면 이 Myinterface를 implements하는 측에서 모든 메소드를 구현하고 사용해야 하므로, 내가 쓰고 싶은 몇개 뿐만 아니라 쓰지 않을 메소드까지 구현해야 한다.

이때 MyInterfaceAdapter라는 클래스를 만들어서 MyInterface 인터페이스를 implements 하고 빈 메소드를 만들어 놓기만 하면 된다. 그리고 이 MyInterfaceAdapter를 사용할 클래스(위 코드에서는 Service 객체)에서 extends 하면 내가 쓰지않을 메소드를 구현할 필요 없이 필요로하는 메소드만 오버라이드하여 구현할 수 있게 된다. (Adapter의미 처럼 중간자 역할을 수행한다.)

이렇게 하면 굉장히 문제를 잘 해결한 듯 보이지만 역시나 불편한 점이 존재한다.

```java
class Service extends A implements MyInterface {}
```

이와 같이 Service클래스가 이미 A를 상속하고 있다면? 

자바는 다중 상속을 지원하지 않기 때문에 어쩔 수 없이 MyInterface를 다시 implements해야하고, 또 다시 모든 메소드를 구현해야 할 수 밖에 없게 된다.

**이때 디폴트 메소드의 진가를 발휘하게 된다.**

그냥 Myinterface 의 모든 메소드를 디폴트 메소드로 바꾸면 모든 문제를 해결할 수 있고, 더이상 Adapter 객체를 따로 만들 필요가 없어지게 된다.

`MyInterface.java`

```java
interface MyInterface {
    default void method1() {};
    default void method2() {};
    default void method3() {};
    default void method4() {};
    default void method5() {};
    default void method6() {};
    default void method7() {};
}
```

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        new Service().method1();
    }
}

class Service extends A implements MyInterface { 
    // MyInterface의 모든 메소드가 디폴트 메소드 이므로 내가 사용하고자하는 메소드만 오버라이드해서 사용 가능!
    @Override
    public void method1() {
        System.out.println("Hello World");
    }
}
```



디폴트 메소드의 활용 예를 살펴보자.

`Ability.java`

```java
// interface에 static 메소드도 구현가능 (인터페이스 이름으로 접근 가능)
public interface Ability {
    static void sayHello() {
        System.out.println("Hello World");
    }
}

interface Flyable {
    default void fly(){
        System.out.println("FLY");
    }
}

interface Swimmable {
    default void swim() {
        System.out.println("SWIM");
    }
}

interface Walkable {
    default void walk() {
        System.out.println("WALK");
    }
}
```

여기서 Ability 인터페이스를 보면 static 메소드를 구현한 것을 확인할 수 있는데, 즉 이 Ability는 sayHello라는 메소드를 제공해주게 되고 메소드가 한개있으므로 그냥 함수로 볼 수 있다. 그래서 **Ability 인터페이스는 sayHello라는 함수를 제공해준다** 라고 말할 수 있다.

우리가 말하는 메소드는 클래스 안에 종속된(구현된) 함수라고 표현을 한다. 

> 근데 자바 8 부터는 function이라고 구분해서 부르는 것들이 생겨나기 시작함 ex) functional interface

`Main.java`

```java
// default 메소드를 이용하면 아래에서 Duck, Swan 클래스에서
// 구현해 줄 필요가 없으므로 훨씬 깔끔하게 코드를 짤 수 있다.
class Duck implements Swimmable, Walkable {}

// Swan이 swim이 가능하다면 그냥 implements로 하나 넣어주기만 하면 아무런 구현 없이 기능이 확장됨
class Swan implements Flyable, Walkable,Swimmable {}

public class Main {
    public static void main(String[] args) {
        new Duck().swim();
        new Duck().walk();
        new Swan().fly();
        new Swan().swim();
        Ability.sayHello(); // static 메소드를 인터페이스 이름으로 접근
    }
}
```

👉result

```
SWIM
WALK
FLY
SWIM
Hello World
```

이처럼 디폴트 메소드로 미리 구현해놓고 Duck이나 Swan에 어울리는 인터페이스를 implements하여 기능을 추가할 수 있다.
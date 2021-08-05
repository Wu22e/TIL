# 인터페이스란?

자바에서 인터페이스는 추상클래스처럼 추상 메서드를 갖는다. 추상 클래스와 차이점은 멤버 변수를 구성원으로 가질 수 없고 메서드는 구현되어 있지 않아야 한다.

보통 인터페이스를 밑그림만 그려진 '기본 설계도' 라고 한다.

그럼 도대체 이러한 특성을 가지는 인터페이스를 왜 사용하는 걸까?

1. **구현을 강제 시킨다.**

```java
interface Login {
    void login();
}

public class KakaoLogin implements Login{
    @Override 
    public void login() {
        System.out.println("카카오로 로그인 합니다.");
    }
}
```

위 코드와 같이 Login을 implements하게 될 경우 Login의 메서드인 login을 반드시 KakaoLogin에서 구현해 줘야한다.

2. **다형성을 제공한다.**

```java
interface Login {
    void login();
}

class KakaoLogin implements Login{
    @Override 
    public void login() {
        System.out.println("카카오로 로그인 합니다.");
    }
}

public class Main {
    public static void main(String[] args) {
        Login user = new KakaoLogin();
        user.login();
    }
}
```

위의 main 메서드에서 KakaoLogin을 인스턴스로 생성하였지만 참조변수 타입을 Login으로 설정할 수 있다. 이렇게 되면 Login 인터페이스에 있는 메서드만 사용할 수 있게 된다.

3. **객체간의 결합도를 낮춰 준다.**

결국 1, 2로 인해 객체간의 결합도를 낮출 수 있게 된다.

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        UserService s = new UserService(new KakaoLogin());
        s.login();
    }
}
```

`KakaoLogin.java`

```java
public class KakaoLogin implements Login{
    @Override
    public void login() {
        System.out.println("카카오로 로그인 합니다.");
    }
}
```

`NaverLogin.java`

```java
public class NaverLogin implements Login{
    @Override
    public void login() {
        System.out.println("네이버로 로그인 합니다.");
    }
}
```

`Login.java`

```java
interface Login {
    void login();
}
```

`UserService.java`

```java
package com.programmers.java.poly;

public class UserService implements Login{

    // 로그인에 의존한다.
    private Login login;

    // 의존성을 외부에 맡긴다면, 의존도를 낮춘다.
    // 결합성 : UserService가 KakaoLogin이나 NaverLogin같은 구상체와 결합하게 되면 결합성이 강하다.
    // 반면 UserService가 추상체인 Login과 결합하게 되면 결합도가 낮아졌다고 얘기한다.
    // 아래 생성자를 통해 의존성을 외부로 부터 전달 받는다면 => 의존성을 주입 받았다고 표현한다.
    // 이것을 의존성 주입, Dependency Injection, DI 라고 한다.
    public UserService(Login login) {
        this.login = login;
    }

    @Override
    public void login() {
         login.login();
    }
}

```

UserService 클래스 입장에서는 멤버변수인 login에 구현체가 들어와야 메서드를 수행할 수 있다. (이를 로그인에 의존한다고 말한다.)

만약 멤버 변수에서 바로 인스턴스를 생성(예를들어 카카오 로그인)한다면 이 UserService는 카카오 로그인밖에 수행하지 못하게 된다.

이를 UserService 내부에서 처리하는게 아니라 외부에서 처리하도록 만든다. 외부에서 UserService 생성자를 통해 멤버변수인 login에 구상체를 결합시켜 주게 된다 (외부에서 결합시켜 주므로 이를 의존성 주입이라고 한다). 이때 카카오 로그인으로 할지, 네이버 로그인으로할지 결정만 해주면 된다.  

![image](https://user-images.githubusercontent.com/52458039/128135309-6e5f1d63-decb-48ee-a0c1-6f9093998878.png)

객체 관계를 다음과 같이 표현할 수 있고, Login 인터페이스가 실제 구상체의 **껍데기 역할**을 수행하게 되면서 결합도를 낮춰주는 효과를 가져다 준다.

이러한 인터페이스의 기본 효과로 인해 좋은 객체지향의 설계 5가지 원칙중 하나인 DIP(Dependency Inversion Principle)를 만족시킬 수 있게 된다. 즉, 쉽게 구상체를 갈아끼우며 필요에 따라 사용할 수 있게 된다.

이것이 인터페이스를 사용하는 이유이다.
# Optional이란?

NPE (Null Pointer Exception)은 가장 많이 발생하는 에러중 하나이다.

자바에서는 거의 모든것이 참조형이므로, 이는 다른말로 거의 모든것이 null이 될 수잇다.

그래서 항상 null을 확인할 필요가 있다.

이렇게 되면 굉장히 피곤해지게 되는데, 이거를 보완하기위해

이제 null을 쓰지 않기로 서로 약속하기로 한다. 

근데 문법적으로 null을 없앨 수는 없고 (요즘은 null을 사용하면 error를 사용하기도 함) 그래서 notnull이나 nullable같은 애노테이션을 사용해서 null인지 아닌지 알아내서 문제를 막기도 한다.

어쨋든 개발자들끼리 null을 사용하지 않겠다고 서로 약속하고 프로그래밍을 하게 되는데, (계약 기반 프로그래밍)

null을 쓰지 않는 방법으로 여러가지가 있다.

**(1) EMPTY 객체를 사용하는 방법**

EMPTY 객체를 null 대신 사용한다.

```java
public class Main {
    public static void main(String[] args) {
        User user = new User(27, "김형우"); 
        // 이 user는 레퍼런스 타입이므로 User user = null; 이렇게 null을 넣을 수도 있다. 이 null 이 들어간 채로 print가 실행되면 nullpointerexception이 뜨게 된다.
        System.out.println(user);
    }
}
```

여기서 user에는 null이 아닌 무언가가 반드시 들어가 있도록 서로 약속한다. 즉, user에 뭔가 값이 항상 있도록 넣어준다. 근데 user에 초기값으로 넣어줄게 없는거 같은데..? 라고 생각할 수 있다.

그렇다고

```java
User user = new User(0, ""); 
```

초기값을 0살, 이름 없음 으로 만들게 된다면? 근데 저 0살 이름 없음이 우리가 사용하는 실제 데이터에도 존재할 수 있을지도 모르기 때문에 이렇게 임의의 값으로 초기값을 사용하기에는 표현이 애매해지게 된다. 그래서 이때 EMPTY 값을 사용한다.

`User.java`

```java
public class User {
    public static final User EMPTY = new User(0, ""); // static final로 고정된 EMPTY값(0살, 이름 없음)을 하나 만들어 놓는다.
    
    private int age;
    private String name;

    public User(int age, String name) {
        this.age = age;
        this.name = name;
    }

    public boolean isOver19() {
        if(this == EMPTY) return false; // 이런식으로 null 처리 가능
        return age >= 19;
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}
```

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        User user = User.EMPTY; // 이런식으로 초기값을 가지게 하려면 EMPTY를 사용한다. 이 EMPTY는 한번 만들어 진 얘고 또 final한 얘기 때문에 다른 곳에서 getUser로 값을 얻어 왔어도 이전에는 이 값이 null인지 아닌지 확인했는데, null을 안쓰기로 했으므로 EMPTY인지 아닌지를 확인하면 된다. 이러면 적어도 NPE는 발생하지 않는다.
        User user 2 = getUser();
        if(user2 == User.EMPTY) // 원래는 user2 == null로 확인했음.
        System.out.println(user);
    }
    
    private static User getUser() {
        return User.EMPTY;
    }
}
```



**(2) Optional 사용**

Optional은 null이 될수도있고 안될수도 있는 값들에 대해서 wrapping 해준다.

아까 User user = User.EMPTY; 로 값이 없다고 표현을 했는데, 오히려 실제로 null이라는 값을 써야하는 경우라던지, 아니면 다른곳(외부)에 결과를 전달해야 하는데 소스코드 상으로 `null인지 아닌지 확인해야된다`혹은 `EMPTY 값으로 확인해야 된다` 라는 말을 표현해서 전달해 줘야한다. 근데 EMPTY가 있다는것은 그것을 아는 사람들 끼리만 통용이 되는 거다. 

그러한 내용을 전달받지 못한 사람은 그것을 활용하거나 그것에 대한 방어코드를 사용할 수 없게 된다.



그래서 아예 USER user 이 변수는 null이 될 수도 있으니 null 체크를 해줘야 한다고 표현을 바꿔 버린다.

```java
public class Main2 {
    public static void main(String[] args) {

        Optional<User> optionalUser = Optional.empty(); // 현재는 운반체 안이 null임. 
        optionalUser = Optional.of(new User(1, "2")); // 어떤 값이 있을 때는 Optional.of(new ~) 로 값을 만들어서 채워 넣어주면 된다. 
        
//         optionalUser.isEmpty(); : 값이 없으면(== null) true
//        optionalUser.isPresent(); : 값이 있으면(!= null) true
                
    }
}
```

Optional은 User라는 데이터를 wrapping해서 운반해주는 운반체이다. 운반체 안에 들어간 값이 User고 이 User가 실제로 값이 있을 수도 있고 없을 수도 있다. 그래서 실제 User가 null일때 null을 return하는게 아니라 null이 담긴 Optional을 return한다. (바구니에 담아서 준다.) 바구니가 비었으면 null이라고 생각하면 되고 바구니가 채워져 있으면 그 안의 데이터를 꺼내서 사용하면 된다.

그래서 위 isEmpty()와 isPresent에 의해 다음과 같이 코드 패턴이 이루어진다.

```java
if(optionalUser.isPresent()) {
            // do 1
} else {
            // do 2
}

if(optionalUser.isEmpty()) {
            // do 1
} else {
            // do 2
}
```

이런 표현이 자주 등장하게 되서, 아예 메소드로 제공해준다.

```java
optionalUser.ifPresent(user -> {
            // do 1
}); // 얘는 else가 없다.

optionalUser.ifPresentOrElse(user -> {
            // do 1
}, () -> {
            // do 2    
});
```

이런 방식으로 값이 있을때만 코드를 수행하도록 강제할 수 있다.

이렇게 Optional 방식을 이용하면 EMPTY로 인해 발생하는 문맥전달을 생각할 필요 없이 Optional로 인해 null이 있을수도 없을수도 있다는 정보를 포함하여 전달해주기 때문에 그것을 받아서 사용하는 입장에서 어떻게 취급해야하는지 명확하게 알 수 있게 된다.

리턴값이 User 면 이거를 null 값을 확인해줘야되나 말아야하나? 고민할 수 있지만 Optional<User>를 반환하면 아! Present를 확인해야 겠구나! 라고 명확하게 알 수 있다.

즉 Optional이라는 타입을 이용해서 **값이 존재하는지 안하는지에 대한 불확실성**과, 그것을 **어떻게 처리해야하는지 방법**까지 모두 알려주게 된다.

> @NotNull 같은것도 표준 자바에서 제공해주는 것은 아니지만 프레임워크에서 제공해주므로 적극적으로 사용하면 좋다.
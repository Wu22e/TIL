# 함수형 인터페이스, 익명클래스, 람다 표현식이란?

자바 8에서 `함수`라는 단위가 계속 언급되고 있다.

거기서 `함수형 인터페이스` 라는 키워드가 등장하게 된다. 우선 이 함수형 인터페이스가 무엇인지 살펴보자.

```java
// 특별히 함수형 인터페이스라는 걸 알려주기 위한 애노테이션을 달아주기도 함
@FunctionalInterface
public interface MyRunnable {
    void run(); // 추상 메소드가 하나밖에 없는 메소드 == 함수형 인터페이스
}

// 얘는 추상 메소드가 2개이므로 함수형 인터페이스가 될 수 없다.
interface MyRunnable2 {
    void run1();
    void run2();
}

// 얘도 함수형 인터페이스다. 왜냐면 '추상' 메소드는 하나이기 때문이다.
// default, static method가 있는 것은 함수형 인터페이스의 결정과 상관 없다.
@FunctionalInterface
interface MyMap {
    void map();
    default void sayHello() {
        System.out.println("Hello World");
    }
    static void sayBye() {
        System.out.println("Bye World");
    }
}
```

즉 함수형 인터페이스는 default, static 메소드와 상관없이 추상메소드 딱 1개 만 가지고 있어야 함수형 인터페이스라고 할 수 있다.

> FunctionalInterface에 있는 추상메소드를 함수라고 부른다.



그럼 함수형 인터페이스를 왜 사용할까? 

함수형 인터페이스를 살펴보기전에 **익명 클래스**와 **람다 표현식**을 이해해야 한다.

우선 인터페이스로 인해 불편한 점을 살펴보자.

`MyRunnable.java`

```java
@FunctionalInterface
public interface MyRunnable {
    void run();
}
```

`MySupply.java`

```java
public interface MySupply {
    String supply();
}
```

`Main.java`

```java
package com.programmers.java.func;

class Greeting implements MySupply {

    @Override
    public String supply() {
        return "Hello World";
    }
}

class SayHello implements MyRunnable {

    @Override
    public void run() {
        System.out.println(new Greeting().supply());
    }
}

public class Main {
    public static void main(String[] args) {
        new SayHello().run();
    }
}

// 여기서 이 sayHello 클래스가 만들어진 이유는 MyRunnable은 인터페이스기 때문에
// 인스턴스를 new 키워드로 생성할 수 없기 때문에 sayHello 클래스가 있어야 비로소
// 인터페이스의 메소드를 구현하여 호출할 수 있게 된다.
```

MyRunnable과 MySupply 인터페이스를 구현할 임시 클래스를 만들어야 만 new 키워드를 이용한 인스턴스 생성이 가능하고 그제서야 인터페이스의 메소드를 사용할 수 있게 된다. 이로 인해 임시적인 클래스가 계속 늘어나게 되서 귀찮아진다..

그래서 그냥 인터페이스지만 new 키워드를 이용하여 사용할 수 없을까? 생각하게 된 것이 바로 익명 클래스의 등장 배경이다.



```java
public class Main {
    public static void main(String[] args) {
        
        
        //new class XXX implements MySupply {
        //    @Override
        //    public String supply() {
        //        return "Hello World";
        //    }
        //}.supply();
        // 원래는 이렇게 class를 만들어서 추상메소드를 구현하여 new 키워드를 이용했음.
    }
}
```



```java
public class Main {
    public static void main(String[] args) {
        
        // 인터페이스를 new 함으로써 임시 클래스 생성 안해도 됨
        // 이름 없는 클래스를 생성 => 익명 클래스
        // 굳이 class 생성없이 사용 가능하게 함 (어짜피 class 만드는건 뻔하니까)
        new MySupply() {
            @Override
            public String supply() {
                return "Hello World";
            }
        }.supply();
    }
}
```

이와 같이 익명 클래스를 만드는 아이디어를 통해 인터페이스를 new 키워드로 생성할 수 있다.

아래는 동작 코드를 작성한 것이다.

```java
public class Main {
    public static void main(String[] args) {
        
        MyRunnable r = new MyRunnable() {
            @Override
            public void run() {
                MySupply s = new MySupply() {
                    @Override
                    public String supply() {
                        return "Hello Hello"; 
                    }
                };
                System.out.println(s.supply());
            }
        };
        r.run();
    }
}
```

👉result

```
Hello World
```



익명 클래스를 만들 수 있다면, 익명 메소드도 만들 수 있지 않을까? 생각하게 됨.

```java
public class Main2 {
    public static void main(String[] args) {
        // 인스턴스가 만들어지면 구현부가 필요하니까 정의와 함께 구현부를 바로 넣어줌
        MyRunnable r1 = new MyRunnable() {
            // Override 하는 부분부터 뒤쪽은 너무 뻔하게 등장해야 하는 것들임.
            @Override
            public void run() {
                System.out.println("Hello");
            }
        };
        // 위 코드는 '익명 클래스'를 사용한 거다.
        
        // 위 코드를 아래와 같이 간단하게 만들 수 있다. (익명 메소드를 사용한 것임)
        // 익명 메소드를 사용해서 표현하는 방법 : 람다 표현식
        MyRunnable r2 = () -> {
            System.out.println("Hello");
        }
        
        // MyRunnable r2 = () -> System.out.println("Hello"); 이렇게 한줄 밖에 없으면 {} 중괄호 까지도 생략 가능. 
        // MyRunnable r2 = () -> (return) "Hello"; 이처럼 만약 MyRunnable 에 있는 추상 메서드의 반환 타입이 있으면 return을 해줘야하는데, 이 또한 한줄이면 중괄호를 생략했듯이 return 또한 생략할 수 있다.
        
    }
}
```

여기서 MyRunnable은 함수형 인터페이스 이므로 어짜피 구현해야할 추상 메서드가 1개인 것은 뻔하다.

현재 위 코드는 r1, r2 동일한데, @Override, public, void, run 이 모든것들은 무조건 나와야 하는 것들이기 때문에 생략이 가능하다. (이게 가능한 이유는 추상메소드가 1개인 함수형 인터페이스 이기 때문에 가능) 

생략하고 나면 ()과 {}이 남게 된다.

() : 인자 받는 부분 

 {} : 구현 몸덩어리 가 남게 되는데 이들은 구분할 필요가 있으므로 -> 화살표를 통해 구분해 준다.

여기서 r2처럼 구현 된 것을 **람다 표현식** 이라고 한다.



만약 MyRunnable2 라는 인터페이스가 있고, 추상메서드가 void run1(), void run2(), 총 2개 있다고 해보자.

그럼 이 람다 표현식을 사용할 수 없다.

```java
MyRunnable2 r3 = () -> {};
```

여기서 run1 메소드를 이용하여 인스턴스를 만든것인지 run2 메소드를 이용하여 인스턴스를 만든 것인지 알 방법이 없기 때문이다.

> 즉, 함수형 인터페이스의 특성으로 인해 익명클래스, 익명메소드의 뻔한 부분을 생략시킬 수 있게되고, 최종적으로 생략한 형태를 람다표현식이라고 부르게 되는 것이다. ( 이 흐름을 이해하는게 굉장히 중요하다. )



앞서 익명 클래스로 작성한 코드를 아래와 같이 확 줄일 수 있다.

```java
public class Main2 {
    public static void main(String[] args) {
        MyRunnable r = () -> {
            MySupply s = () -> "Hello Hello";
            System.out.println(s.supply())''
        };
        r.run();
    }
}
```

👉result

```
Hello Hello
```

**결국 코드를 깔끔하게 작성하기 위해 함수형 인터페이스가 필요했고, 함수형 인터페이스의 특성으로 인해 익명클래스와 익명메소드의 뻔한 코드를 삭제하고 람다표현식으로 깔끔하게 표현할 수 있게 된 것이다**

---

## 람다 표현식 연습

`MyConsumer.java`

```java
@FunctionalInterface
public interface MyConsumer {
    void consume(int i);
}
```

`MyMapper.java`

```java
@FunctionalInterface
public interface MyMapper {
    int map(String s);
}
```

`MyRunnable.java`

```java
@FunctionalInterface
public interface MyRunnable {
    void run();
}
```

`MySupplier.java`

```java
@FunctionalInterface
public interface MySupplier {
    String supply();
}
```

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        MySupplier s = () -> "Hello World";

        MyMapper m = (str) -> str.length();
        //MyMapper m = String::length; // 메소드 레퍼런스

        MyConsumer c = (i) -> System.out.println(i);
        //MyConsumer c = System.out::println; // 메소드 레퍼런스
        MyRunnable r = () -> c.consume(m.map(s.supply()));

        r.run();
    }
}
```

여기서 인텔리제이가 노란줄을 보여주는데, 이때 alt + intsert를 이용하면

메소드 레퍼런스가 적용된다.

메소드 레퍼런스는 입력 받은 값이 **변경 없이** 바로 사용할 때 사용 가능하다.



이 메소드 레퍼런스를 쓰는 이유는 들어오는 인풋 값이 변경없이 바로 사용되는데, 코드를 공유하는 상황이 오면 **작성자**가 인풋 값을 변경없이 바로 사용하는 것을 **강조**했다고 내포할 수 있는 의미에서 메소드 레퍼런스를 사용하게 된다. (입력 값을 변경하지 말라는 표현 방식이다.)



위 예제에서 MySupplier가 현재는 String을 반환하는데, 만약 다른 형으로 타입을 반환하고 싶다면? 그때마다 매번 새로운 인스턴스를 만들어야 할까?

이때 사용하는것이 **제네릭** 이다.

제네릭을 사용하여 아래와 같이 코드를 변경할 수 있다.

(제네릭은 참조형 타입만 사용가능)

`MyConsumer.java`

```java
@FunctionalInterface
public interface MyConsumer<T> {
    void consume(T t);
}
```

`MyMapper.java`

```java
@FunctionalInterface
public interface MyMapper<IN, OUT> {
    OUT map(IN s);
}
```

`MySupplier.java`

```java
@FunctionalInterface
public interface MySupplier<T> {
    T supply(); // 제네릭을 이용하여 T라는 타입을 사용
}
```

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        MySupplier<String> s = () -> "Hello World";

        MyMapper<String, Integer> m = String::length;
       
        // 제네릭을 이용하면 아래와 같이 타입을 변경하여 메서드를 바꿀 수 있다.
        MyMapper<Integer, Integer> m2 = i -> i*i;
        MyMapper<Integer, String> m3 = Integer::toHexString;

        MyConsumer<String> c = System.out::println;
        
        MyRunnable r = () -> 
            c.consume(
                		m3.map(
                        		m2.map(
                                		m.map(
                                        		s.supply()
                                		)
                        		)
                		) 
        		);
            
        r.run();

    }
}
```

👉result

```
79
```

Hello World 길이가 11이고 이를 제곱한 121, 그리고 이를 16진수로 바꾸었으므로

결과는 79(16) 이 된다.

---

### 람다 표현식(함수형 인터페이스)을 이용한 또다른 예



위에서 우리가 MyConsumer, MyMapper, MyRunnable, MySupplier같은 함수형 인터페이스를 만들어 봤는데, 자바 8에서 이를 미리 만들어 두었음.

java 1.8 이상부터 java.util.function 안에 있는 인터페이스들이 함수형 인터페이스로 되어 있다.

```java
public class Main2 {
    public static void main(String[] args) {
        // 여기가 호스트다.
        new Main2().loop(10, System.out::println);
    }

    void loop(int n, MyConsumer<Integer> consumer) {
        for(int i = 0; i< n; i++) {
            // 반복문을 돌 건데, i를 입력으로 줘서 뭔가를 동작 시켜야 함 == 입력은 있고, 출력은 따로 없어도 된다.
            // 즉 MyConsumer의 메서드를 사용하면 됨.
            consumer.consume(i);
        }
    }
}

```

👉result

```
0
1
2
3
4
5
6
7
8
9
```



이 코드에서 loop 메소드가 만약 인자로 n만 받고 n까지의 합을 더하는 로직을 짜게 된다면 그냥 이 loop 메소드가 `기능`을 수행하게 된다.

근데 이 loop 함수안에서 자체적인 `기능`을 수행하지 않고 수행해야 할 것을 외부로 부터 전달받게 하면 좋다.

구체적인 사항에 대해서 내가 처리하지 않고, 뒷쪽으로 넘겨서 최종적으로 호스트가 그 결정을 하게 만드는 것이다. 

결국, 이 loop는 '나 (loop)는 루프(for)를 도는 것에만 관심이 있고(루프를 도는 작업만 처리할 거고) 그 외에 다른 작업은, 다른쪽으로 넘겨줄 테니까, 나를 호출하는 쪽에서 정하라고 호스트 코드쪽으로 넘긴다.

이렇게 하면 loop 라는 메소드가 본연의 기능을 만족하면서도, 호스트 코드는 자신이 원하는 인자를 넣어서 원하는 것을 얻을 수 있다. (이게 앞에서 설명한 DI와 같은 원리)

즉, 구체적으로 어떤 일을 할지는 나를 호출하는 사람이 안다. 이런 경우에 functional interface를 사용할 수 있음.



또 다른 함수형 인터페이스 예를 보자.

```java
public class Main2 {
    public static void main(String[] args) {
        // 여기가 호스트다.
        new Main2().filteredNumbers(30,
                i -> i%2 == 0,
                System.out::println
        );
    }
    
    void filteredNumbers (int max, Predicate<Integer> p, Consumer<Integer> c) {
        for (int i=0; i<max; i++) {
            if(p.test(i)) c.accept(i);
        }
    }
}
```

👉result

```
0
2
4
6
8
10
12
14
16
18
20
22
24
26
28
```

filteredNumbers는 홀수를 거른다 던가, 짝수를 거른다던가 하는 기능이 없지만 그런 기능을 인자로 함수형 인터페이스를 받아서 수행할 수 있게 된다. (호출하는 측에서, 즉 외부에서 그 기능을 수행하게 된다) 이런 상황에서 함수형 인터페이스를 사용한다!
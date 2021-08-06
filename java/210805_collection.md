# Collection이란?

자바에서 Collection 프레임워크를 제공하는데, 여기서 Collection의 뜻은 **데이터의 묶음** 이다.

Collection 은 인터페이스(추상체)이다. 

컬렉션의 구상체로  List가 있고, List의 구상체로 LinkedList, ArrayList, Vector, Stack이 있다.

마찬가지로 컬렉션의 구상체로 Set이 있고 Set의 구상체로 HashSet이 있다.

```java
LinkedList<Integer> list1 = new LinkedList<>();
List<Integer> list2 = new LinkedList<>();  // LinkedList는 List 추상체를 구현한 것이므로 List 인터페이스로 표현할 수 있다. (다형성)
List<Integer> list3 = new ArrayList<>(); // ArrayList는 List 추상체를 구현한 것이므로 List 인터페이스로 표현할 수 있다. (다형성)
```

이 list는 보통 다음과 같이 데이터를 넣고 순회한다.

```java
public class Main {
    public static void main(String[] args) {
        List<Integer> list = new LinkedList<>();
        
        list.add(1);
        list.add(2);
        list.add(3);
        
        // 데이터 순회
        for (int i=0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
    }
}
```

👉result

```
1
2
3
```

데이터 순회를 for문 말고 forEach로 할 수도 있다.

`MyCollection.java`

```java
public class MyCollection<T> {
    private List<T> list;

    public MyCollection(List<T> list) {
        this.list = list;
    }
    // 전에 배웠던 loop 처럼 어떤 작업을 할지 외부에서 결정해 줌.
    public void foreach(Consumer<T> consumer) {
        for(int i=0; i<list.size(); i++) {
            T data = list.get(i);
            consumer.accept(data);
        }
    }
}
```

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        new MyCollection<>(Arrays.asList(1, 2, 3, 4, 5)) // Arrays의 asList로 List를 만들 수 있음.
                .foreach(System.out::println);
    }
}
```

👉result

```
1
2
3
4
5
```

MyCollection의 list는 제네릭으로 만들었으므로 List가 어떤 형태여도 가능하다. 

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        new MyCollection<>(Arrays.asList("A", "B", "C", "D", "E")) // 타입 바꿔도 상관 없음 (Generics) , 근데 배열 원소 타입이 모두 같아야 할 필요는 없는듯.. 왜?
                .foreach(System.out::println);
    }
}
```

그럼 저 타입을 다른 타입으로 바꿔주는 map 함수를 만들어 보자.

`MyCollection.java`

```java
public class MyCollection<T> {
    private List<T> list;

    public MyCollection(List<T> list) {
        this.list = list;
    }

    // 데이터의 변경 (a타입에서 b타입으로 데이터를 mapping 한다!)
    // 즉, 멤버변수 list가 T타입인데, U타입으로 바꿔서 u타입으로 모아진 MyCollection을 리턴
    public <U> MyCollection<U> map(Function<T, U> function) { // 인자로 T타입으로 부터 U타입을 만들어 낼 수 있는 Function을 인자로 받는다.
                                                              // 이 MyCollection은 T타입 밖에 모르는데, 여기서 새롭게 U타입이 나왔다. 이건 이 메소드에서만
                                                              // 쓰라는 의미로, 메서드 이름 앞에 <U>를 붙여줘야 한다.
        List<U> newList = new ArrayList<>();
        foreach(d -> newList.add(function.apply(d)));
        return new MyCollection<>(newList);
    }

    public void foreach(Consumer<T> consumer) {
        for(int i=0; i<list.size(); i++) {
            T data = list.get(i);
            consumer.accept(data);
        }
    }
}

```

이렇게 map 함수를 만들고

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        MyCollection<String> c1 = new MyCollection<>(Arrays.asList("A", "BA", "CDS", "DASD", "QWERS"));

        MyCollection<Integer> c2 = c1.map(String::length); // c1 배열의 원소를 Integer로 바꿔줌 (map 함수에 의해 타입을 T에서 U로 바꿈)

        c1.foreach(System.out::println);
        c2.foreach(System.out::println);

    }
}
```

👉result

```
A
B
C
D
E
1
2
3
4
5
```

이렇게 map함수가 잘 작동하여 리스트 원소의 타입이 바뀐 것을 확인할 수 있다.

위 코드를 따로 변수를 둘필요 없이 바로 출력해 줄 수도 있다.

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        new MyCollection<>(Arrays.asList("A", "BA", "CDS", "DASD", "QWERS"))
                .map(String::length)
                .foreach(System.out::println);
    }
}
```

👉result

```
1
2
3
4
5
```



filter 함수도 만들 수 있다. 

`MyCollection.java`

```java
public class MyCollection<T> {
    private List<T> list;

    public MyCollection(List<T> list) {
        this.list = list;
    }

    public <U> MyCollection<U> map(Function<T, U> function) {  
        List<U> newList = new ArrayList<>();
        foreach(d -> newList.add(function.apply(d)));
        return new MyCollection<>(newList);
    }


    // 필터링 하는 기능 ( 데이터 타입이 바뀌는 건 아님 )
    public MyCollection<T> filter(Predicate<T> predicate) {
        List<T> newList = new ArrayList<>();
        foreach(d -> {
            if(predicate.test(d)) newList.add(d);
        });
        return new MyCollection<>(newList);
    }

    public void foreach(Consumer<T> consumer) {
        for(int i=0; i<list.size(); i++) {
            T data = list.get(i);
            consumer.accept(data);
        }
    }
}

```

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        // method chaining
        new MyCollection<>(Arrays.asList("A", "BA", "CDS", "DASD", "QWERS"))
                .map(String::length)
                .filter(i -> i % 2 == 0) // 짝수만 출력
                .foreach(System.out::println);
    }
}
```

👉result

```
2
4
```



지금까지 MyCollection에다가 map, filter, foreach 메서드를 구현하여 List의 데이터를 처리했다.

이때 리턴 타입이 MyCollection이 되도록하여 서로 .으로 연결될 수 있도록 하였다. (이를 method chaining 이라 함) 이는 collection이 데이터의 묶음이기 떄문에 return을 collection으로 하는 이유도 있다.



count 메소드도 만들 수 있다.

`MyCollection.java`

```java
public class MyCollection<T> {
    private List<T> list;

    public MyCollection(List<T> list) {
        this.list = list;
    }

    public <U> MyCollection<U> map(Function<T, U> function) {  
        List<U> newList = new ArrayList<>();
        foreach(d -> newList.add(function.apply(d)));
        return new MyCollection<>(newList);
    }


    public MyCollection<T> filter(Predicate<T> predicate) {
        List<T> newList = new ArrayList<>();
        foreach(d -> {
            if(predicate.test(d)) newList.add(d);
        });
        return new MyCollection<>(newList);
    }

    public void foreach(Consumer<T> consumer) {
        for(int i=0; i<list.size(); i++) {
            T data = list.get(i);
            consumer.accept(data);
        }
    }
    
    // 멤버변수 size 
    public int size() {
        return list.size();
    }
}
```

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        int n = new MyCollection<>(Arrays.asList("A", "BA", "CDS", "DASD", "QWERS"))
                .map(String::length)
                .filter(i -> i % 2 == 0)
                .size();

        System.out.println(n);
    }
}
```

👉result

```
2 ( 2, 4 총 2개)
```

즉 MyCollection 에서 함수형 인터페이스를 활용하여 고정된 형태의 기능을 하는 것이 아니라 **사용하는 측에서 기능을 정의 할 수 있는** 범용적인 형태의 Collection을 만들 수 있다.

이렇게 **여러개의 데이터를 통합적으로 묶어서 관리하는 것**을 `Collection` 이라고 한다.

---

collection의 또다른 예

`User.java`

```java
public class User {
    private int age;
    private String name;

    public User(int age, String name) {
        this.age = age;
        this.name = name;
    }

    public int getAge() {
        return age;
    }
}
```

`Main2.java`

```java
public class Main2 {
    public static void main(String[] args) {
        new MyCollection<User>(
                Arrays.asList(
                        new User(15, "AAA"),
                        new User(16, "BBB"),
                        new User(17, "CCC"),
                        new User(18, "DDD"),
                        new User(19, "EEE"),
                        new User(20, "FFF"),
                        new User(21, "GGG"),
                        new User(22, "HHH"),
                        new User(23, "III")
                )
        )
                .filter(u -> u.getAge() >= 19)
                .foreach(System.out::println);
    }
}
```

👉result

```
com.programmers.java.User@3b6eb2ec
com.programmers.java.User@1e643faf
com.programmers.java.User@6e8dacdf
com.programmers.java.User@7a79be86
com.programmers.java.User@34ce8af7
```

User 객체를 바로 반환하면 주소값이 반환된다 (hashCode) 그래서 유저 이름이 나오도록 map 함수를 이용하여 바꿔준다.



`Main2.java`

```java
public class Main2 {
    public static void main(String[] args) {
        new MyCollection<User>(
                Arrays.asList(
                        new User(15, "AAA"),
                        new User(16, "BBB"),
                        new User(17, "CCC"),
                        new User(18, "DDD"),
                        new User(19, "EEE"),
                        new User(20, "FFF"),
                        new User(21, "GGG"),
                        new User(22, "HHH"),
                        new User(23, "III")
                )
        )
                .filter(u -> u.getAge() >= 19)
                .map(User::getName)
                .foreach(System.out::println);
    }
}
```

👉result

```
EEE
FFF
GGG
HHH
III
```

19세 이상인 사람들 이름만 필터링 해서 출력 할 수 있다.

따라서 배열의 원소로 String, Integer 뿐만아니라 custom 객체또한 원소로 사용할 수 있고 그에 해당되는 멤버 변수로 필터링(filter) 및 출력 타입을 변경할 수 있다(map)



근데 위 방식도 좋은 방식은 아닌게, 

```java
.filter(u -> u.getAge() >= 19)
```

이 부분에서 결국 User 객체의 age를 바로 접근하는 거나 마찬가지의 효과를 가지게된다.

따라서 이때는 아예 조건에 해당되는 메소드를 따로 만들어 주는게 좋다. 

마찬가지로 

```java
.map(User::getName)
```

여기서도 이름을 가져오는게 아니라 toString을 오버라이드 해서 객체에서 해당 값을 출력하게 만들어 줘야 한다.

`User.java`

```java
public class User {
    private int age;
    private String name;

    public User(int age, String name) {
        this.age = age;
        this.name = name;
    }

    public boolean isOver19() { // 이렇게 하면 나이에 대한 정보는 User만 알고 있고, age 정보를 밖에 꺼내서 알려주는게 아니라 내가 스스로 이 함수내에서 판단해서 true인지 false인지만 알려주는 거다.
        return age >= 19;
    }
    
    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}
```

`Main2.java`

```java
public class Main2 {
    public static void main(String[] args) {
        new MyCollection<User>(
                Arrays.asList(
                        new User(15, "AAA"),
                        new User(16, "BBB"),
                        new User(17, "CCC"),
                        new User(18, "DDD"),
                        new User(19, "EEE"),
                        new User(20, "FFF"),
                        new User(21, "GGG"),
                        new User(22, "HHH"),
                        new User(23, "III")
                )
        )
                .filter(User::isOver19)
                .foreach(System.out::println);
        // 이렇게 age, name을 밖으로 꺼내지 않고 동작을 수행할 수 있다.
    }
}
```

👉result

```
EEE (19)
FFF (20)
GGG (21)
HHH (22)
III (23)
```



이번에는 다른 방식으로 데이터를 순회해 보자.

Iterator라는 방식인데, 여러 데이터의 묶음을 풀어서 하나씩 처리할 수 있는 수단을 제공해 준다.

Iterator는 배열의 원소를 하나씩 쪼개서 하나만 관심을 가지고 처리하는 방식이다.

`Main.java`

```java
public class Main {

    public static void main(String[] args) {
        List<String> list = Arrays.asList("A", "AB", "ABC", "ABCD", "ABCDE");
        // 이 데이터 덩어리를 iterator 방식으로 픽업할 수 있다.

        //Iterator라는 타입이 있음
        Iterator<String> iter = list.iterator();

        // 이 iter로 부터 데이터를 꺼내오려면 .next()를 해야함
        // next로 데이터를 배열에서 하나씩 떼온다.
        System.out.println(iter.next());
        System.out.println(iter.next());
        System.out.println(iter.next());
        System.out.println(iter.next());
        System.out.println(iter.next());
//        System.out.println(iter.next());
        // 배열을 넘어서서 next를 하면 NoSuchElementException 발생

    }
}

```

👉result

```
A
AB
ABC
ABCD
ABCDE
```



Iterator의 특징 : next()를 통해서 다음 데이터를 조회할 수 있음.

but, 역으로 움직일 수 없다. (이전 데이터 조회 불가능)



또 next()를 계속해서 데이터 갯수를 초과하면 NoSuchElementException이 발생하므로

데이터가 있는지 확인해 줘야 한다.

```java
if(iter.hasNext()) iter.next(); // 데이터가 있으면 그때 호출해라.
```

이 키워드를 이용하여 데이터 전체를 다 순회하는 코드를 아래와 같이 작성할 수 있다.

```java
public class Main {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("A", "AB", "ABC", " ABCD", "ABCDE");
        Iterator<String> iter = list.iterator();
        
        while(iter.hasNext()) {
            System.out.println(iter.next());
        }
    }
}
```

👉result

```
A
AB
ABC
ABCD
ABCDE
```



이 Iterator를 아까 만들었던 MyCollection에 추가해 보자.

`MyIterator.java`

```java
public interface MyIterator<T> {
    boolean hasNext();
    T next();
}
```

`MyCollection.java`

```java
public class MyCollection<T> {
    private List<T> list;

    public MyCollection(List<T> list) {
        this.list = list;
    }

    public <U> MyCollection<U> map(Function<T, U> function) { 
        List<U> newList = new ArrayList<>();
        foreach(d -> newList.add(function.apply(d)));
        return new MyCollection<>(newList);
    }

    public MyCollection<T> filter(Predicate<T> predicate) {
        List<T> newList = new ArrayList<>();
        foreach(d -> {
            if(predicate.test(d)) newList.add(d);
        });
        return new MyCollection<>(newList);
    }

    public int size() {
        return list.size();
    }

    public void foreach(Consumer<T> consumer) {
        for(int i=0; i<list.size(); i++) {
            T data = list.get(i);
            consumer.accept(data);
        }
    }
	  
    
    // MyIterator를 구현함. 
    public MyIterator<T> iterator() {
        return new MyIterator<T>() {
            private int index = 0;

            @Override
            public boolean hasNext() {
                return index < list.size();
            }

            @Override
            public T next() {
                return list.get(index++);
            }
        };
    }
}

```

`Main.java`

```java
public class Main {
    public static void main(String[] args) {
        MyIterator<String> iter =
            new MyCollection<String>(Arrays.asList("A", "AB", "ABC", "ABCD", "ABCDE"))
                    .iterator();
        while(iter.hasNext()) {
            System.out.println(iter.next());
        }
    }
}
```

👉result

```
A
AB
ABC
ABCD
ABCDE
```

아까처럼 글자수도 바꾸고, 그게 홀수 일때만 모아서 출력하고 싶다면?

```java
public class Main {
    public static void main(String[] args) {
        MyIterator<String> iter =
            new MyCollection<String>(Arrays.asList("A", "AB", "ABC", "ABCD", "ABCDE"))
                    .iterator();
        while(iter.hasNext()) {
            String s = iter.next();
            int len = s.length();
            if(len % 2 == 0) continue;
            System.out.println(iter.next());
        }
    }
}
```

👉result

```
A
ABC
ABCDE
```

즉, 데이터를 하나씩 꺼내오는 건 성공했고, 하나씩 취급할 수는 있지만, 하나씩 꺼내온 것에 대해서는 

```java
while(iter.hasNext()) {
            String s = iter.next();
            int len = s.length();
            if(len % 2 == 0) continue;
            System.out.println(iter.next());
}
```

이런식으로 처리를 해야 한다.

MyCollection에서는 데이터 덩어리를 가지고 map, filter, foreach를 했는데,

iterator를 이용해서 데이터를 하나씩 떨어뜨려서 만들어냈고, 그 데이터 한 건에 대해서는 map, filter, foreach를 하지는 못한다. (iterator는 거기까지 제공해 주지 못한다.)



그럼 map, filter, foreach, reduce 처럼, 함수형 인터페이스를 사용하여 데이터를 처리할 수 없을까 생각함. (고차함수를 사용할 수 없을까?)

그래서 나온 방식이 **Stream**이다. 

아까 iterator도 데이터를 하나씩 떨어 뜨려놔서, 전체 묶음중 어디 위치에 있고, 끝이 어딘 지모르고, 현재 주어진 데이터 값만 가지고 끝인지 아닌지 판단했어야 했다. 

stream도 데이터의 연속상에서 제공되는 데이터 하나만 취급되고 그렇게 전달된 데이터 하나가 전체 연속된 데이터 상에서 어느 위치에 있는지 언제끝나는지, 전체 몇개인지 알 수는 없다.

그래서 이 stream도 연속된 데이터 중의 하나이다.

이미 우리는 stream을 많이 사용했다. System.in / System.out , 이게 다 스트림이다.



결국은 OutputStream이다.



```java
InputStream in = System.in; // 인풋 스트림
OutputStream out = System.out; // 아웃풋 스트림
```

System.in 도 계속 입력되는 연속된 데이터 들의 흐름이다는 의미로 System.in 이라 하고

System.out도 우리가 출력하는 값들이 연속적으로 출력 되니까 끊어지지 않는 데이터의 흐름을 본다.



stream을 활용한 데이터 활용예는 다음 포스팅에 정리하였다.


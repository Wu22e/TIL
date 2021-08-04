# StringBuffer와 StringBuilder의 차이?

StringBuffer와 StringBuilder의 차이를 보기전에 먼저 String 객체의 특성과 StringBuffer객체의 특성의 차이점을 먼저 살펴보아야 한다.

자바에서는 문자열을 String 객체로 많이 처리 한다.

특히, 문자열 리터럴의 경우 프로그램 실행시 자동으로 생성이 되는데, 이는 Constant Pool(상수 저장소)에 저장된다.

이때, 문자열들을 서로 결합하기위에 += 연산을 하게 되는데, String 객체는 불변(immutable) 하기 때문에 += 연산을 수행할 때마다 constant pool에 새로운 문자열 객체가 생겨나게 된다. 

![image](https://user-images.githubusercontent.com/52458039/128130102-d5806f7f-4ea4-491d-a7e7-7265eade919d.png)

이렇게 되면 복잡한 문자열을 계산할때 쓸때없는 문자열 객체가 계속 쌓이게 되므로 비효율 적이다.

이를 해결하기 위한 것이 **StringBuffer** 객체이다.

StringBuffer 객체는 문자열을 변경할 수 있다(mutable). 내부적으로 문자열 편집을 위한 버퍼(buffer)를 가지고 있으며, StringBuffer를 생성할 때 그 크기를 지정해 줄 수 있다. 또 StringBuffer가 제공하는 메소드를 통해 버퍼의 문자열을 수정함으로써 불필요한 문자열 객체가 생성되지 않고 버퍼 하나로 작업을 처리할 수 있게 된다.

---

**이제 StringBuffer와 StringBuilder의 차이를 살펴보자.**

StringBuffer와 StringBuilder는 완전히 똑같은 기능으로 작성되어 있다. 하지만 StringBuffer는 멀티쓰레드에 안전하도록 (thread-safe) 동기화 되어있지만, StringBuilder는 그렇지 않다.

여기서 스레드의 개념을 살펴보자.

싱글 쓰레드 : 한번에 1개의 작업을 수행

멀티 쓰레드 : 한번에 n개의 작업을 수행 

예를들어 카카오톡을 하며 메시지를 보내면서 파일을 전송하는 경우 멀티스레드라고 볼 수있다. 

이러한 쓰레드는 하나의 일꾼으로 생각할 수 있는데, 멀티쓰레드의 경우 일꾼이 여러명이므로 일을 동시에 처리할 수 있다. 하지만 이 과정에서 동시에 일을 처리하다 보니 데이터를 공유하게 되는 상황이 발생한다. 이를 막아주는 방법이 바로 동기화 이다. (동기화를 이용한 멀티 스레드 기법은 다음에 정리하도록 하자.)

여기서 StringBuffer는 멀티쓰레드에 안전하도록 동기화되어있고, StringBuilder는 그렇지 않다. 

따라서 우리가 멀티쓰레드로 프로그램을 작성하지 않는다면 StringBuffer의 동기화는 불필요하게 성능만 떨어뜨리게 된다. 따라서 멀티쓰레드로 프로그램을 작성하지 않는다면 StringBuilder를 사용하는게 낫다.

> 근데 대부분의 경우 StringBuffer도 충분히 성능이 좋기 때문에 성능향상이 반드시 필요한 경우를 제외하고는 굳이 기존 코더에서 StringBuffer를 StringBuilder로 바꿀 필요는 없다.


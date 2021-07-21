# 구구단 만들기



## 최종 요구사항 1

- 사용자가 입력한 값에 따라 크기가 다른 구구단을 계산해 출력한다.
- 예를 들어 사용자가 8을 입력하면 팔팔단, 19를 입력하면 십구십구단(2 * 1에서 19 * 19)을 계산해 출력한다.



#### 소스 코드

```java
import java.util.Scanner;

public class GugudanMain {
	public static void main(String[] args) {
		System.out.printf("숫자를 입력해 주세요 : ");
		Scanner scanner = new Scanner(System.in);
		int number = scanner.nextInt();
		
		for(int i=2; i <= number; i++) {
			int[] result = Gugudan.calculate(i);
			Gugudan.print(result);
		}		
	}
}

public class Gugudan {
	
	public static int[] calculate(int times) {
		int[] result = new int[times];
		
		for(int i=0; i < result.length; i++) {
			result[i] = times * (i + 1);
		}
		
		return result;
	}
	
	public static void print(int[] result) {
		int times = result.length;
		for(int i=1; i <= result.length; i++) {
			System.out.printf("%d * %d = %d%n", times, i, times*i);
		}
	}
}

```

- 입력 수에 대한 예외처리는 진행하지 않음.

👉실행 결과

![image](https://user-images.githubusercontent.com/52458039/126437739-2eaac906-6651-44dd-b3b6-e8c614908195.png)

## 최종 요구사항 2

- 사용자가 입력한 값에 따라 크기가 다른 구구단을 계산해 출력한다.
- 예를 들어 사용자가 "8,7"과 같은 문자열을 입력하면 팔칠단을 구현한다. 팔칠단은 2 * 1 ... 2 * 7, 3 * 1 ... 3 * 7, ... , 8 * 1 ... 8 * 7 까지 구현하는 것을 의미한다.

##### 힌트

- 사용자가 입력한 값을 문자열 (String)로 받는다.

```
Scanner scanner = new Scanner(System.in);
String inputValue = scanner.nextLine();
```

- 사용자가 입력한 값을 문자열(String)로 받는다.

```
String[] splitedValue = inputValue.split(",");
```

- 쉼표를 기준으로 분리한 문자열을 숫자로 변환한다.

```
int first = Integer.parseInt(splitedValue[0]);
int second = Integer.parseInt(splitedValue[1]);
```

- 2중으로 반복문(for 또는 while)을 실행해 곱셈 결과를 화면에 출력한다.



#### 소스 코드

```java
import java.util.Scanner;

public class GugudanMain {
	public static void main(String[] args) {
		System.out.printf("숫자를 입력해 주세요 : ");
		
		Scanner scanner = new Scanner(System.in);
		String inputValue = scanner.nextLine();
		
		String[] splitedValue = inputValue.split(",");
		
		int first = Integer.parseInt(splitedValue[0]);
		int second = Integer.parseInt(splitedValue[1]);
		
		for(int i = 2; i <= first; i++) {
			for(int j=1; j <= second; j++) {
				System.out.printf("%d * %d = %d%n", i, j, i*j);
			}
		}
		
	}
}
```

- 입력 수에 대한 예외처리는 진행하지 않음.

👉실행 결과

![image](https://user-images.githubusercontent.com/52458039/126439215-a3fb294f-cfb9-4f88-80f9-9449c8e781d6.png)
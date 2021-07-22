# 캘린더 만들기



## 요구사항 

- 년, 월을 입력하면 해당 년, 월의 달력을 출력한다.
- 1일의 요일을 입력받아서 그에 맞게 출력한다.
- 달력의 모양은 다음과 같은 형태로 출력한다.

```
SU MO TU WE TH FR SA
--------------------
 1  2  3  4  5  6  7
 8  9 10 11 12 13 14
15 16 17 18 19 20 21
22 23 24 25 26 27 28
29 30 31
```

- 프롬프트로 값을 입력받는다.



#### 소스 코드

```java
public class Calendar {
	
	private static final int[] MAX_DAYS = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
	private static final int[] LEAP_MAX_DAYS = { 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
	
	public boolean isLeapYear(int year) {
		if (year%4 == 0 && (year%100 != 0 || year%400 == 0)) {
			return true;
		} else {
			return false;
		}
	}
	
	public int getMaxDaysOfMonth(int year, int month) {
		if (isLeapYear(year)) {
			return LEAP_MAX_DAYS[month - 1];
		} else {
			return MAX_DAYS[month - 1];
		}
	}
	
	public void printCalendar(int year, int month, int weekday) {
		System.out.printf("    <<%4d년%3d월>>%n", year, month);
		System.out.println(" SU MO TU WE TH FR SA");
		System.out.println("---------------------");
		
		int maxDay = getMaxDaysOfMonth(year, month);
		int count = 7 - weekday;
		int delim = (count < 7) ? count : 0; // 일요일 일때 줄바꿈 해주기 위함
		
		//print blank space
		for(int i = 0; i < weekday; i++) {
			System.out.print("   ");
		}
		
		//print first line
		for(int i = 1; i <= count; i++) {
			System.out.printf("%3d", i);
		}
		System.out.println();
		
		//print from second line to last line
		for(int i = count+1; i <= maxDay; i++) {
			System.out.printf("%3d", i);
			if (i%7 == delim) {
				System.out.println();
			}
		}
		
		System.out.println();
		System.out.println();
	}
	
	public static void main(String[] args) {
		// 셀 실행
		Prompt p = new Prompt();
		p.runPrompt();
	}
	
	
}

package calendar;

import java.util.Scanner;

public class Prompt {
	
	/**
	 * 
	 * @param week 요일명
	 * @return 0 ~ 6 (0 = Sunday, 6 = Saturday)
	 */
	public int parseDay(String week) {
		switch (week) {
			case "su": return 0;
			case "mo": return 1;
			case "tu": return 2;
			case "we": return 3;
			case "th": return 4;
			case "fr": return 5;
			case "sa": return 6;
			default: return 0; 
		}
	}
	
	public void runPrompt() {
		Scanner scanner = new Scanner(System.in);
		Calendar cal = new Calendar();
		
		int month = 1;
		int year = 2017;
		int weekday = 0;
		
		while(true) {
			System.out.println("년도를 입력하세요.(exit: -1)");
			System.out.print("YEAR> ");
			year = scanner.nextInt();
			if (year == -1) {
				break;
			}
			
			System.out.println("달을 입력하세요.");
			System.out.print("MONTH> ");
			month = scanner.nextInt();
			if (month > 12 || month < 1) {
				System.out.println("잘못된 입력입니다.");
				continue;
			}
			
			System.out.println("첫째 날의 요일을 입력하세요(su, mo, tu, we, th, fr, sa).");
			String str_weekday = scanner.next();
			weekday = parseDay(str_weekday);
			
			cal.printCalendar(year, month, weekday);
		}
		
		System.out.println("Bye~");
		scanner.close();
	}
}

```

- 입력한 년이 윤년인지 확인하여 일수를 맞춰줌
- 첫번째 요일을 입력 받으면 요일별 숫자를 매칭하여 달력에 출력할때 그에 맞게 파싱하여 출력해줌
- 잘못된 입력에 대해서는 다시 입력하도록 while문을 이용함.
- 년을 입력받을때 -1을 입력하면 프로그램이 종료됨.
- Scanner 클래스 사용후 resoure leak를 막기 위해 scanner.close()를 해줌.

👉실행 결과

![image](https://user-images.githubusercontent.com/52458039/126593558-10442f99-8430-4e31-8271-8c0580b64094.png)

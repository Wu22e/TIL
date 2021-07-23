# 캘린더 만들기



## 요구사항 

- 년, 월을 입력하면 해당 년, 월의 달력을 출력한다.
- 1일의 요일을 입력받지 않아야 한다.



#### 소스 코드

```java
import java.util.Scanner;

public class Calendar {
	
	private static final int[] MAX_DAYS = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
	private static final int[] LEAP_MAX_DAYS = {0, 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
	
	public boolean isLeapYear(int year) {
		if (year%4 == 0 && (year%100 != 0 || year%400 == 0)) {
			return true;
		} else {
			return false;
		}
	}
	
	public int getMaxDaysOfMonth(int year, int month) {
		if (isLeapYear(year)) {
			return LEAP_MAX_DAYS[month];
		} else {
			return MAX_DAYS[month];
		}
	}
	
	public void printCalendar(int year, int month) {
		System.out.printf("    <<%d년 %d월>>%n", year, month);
		System.out.println(" SU MO TU WE TH FR SA");
		System.out.println("---------------------");
		
		//get weekday automatically
		int weekday = getWeekDay(year, month, 1);
		
		//print blank space
		for(int i = 0; i < weekday; i++) {
			System.out.print("   ");
		}
		
		int maxDay = getMaxDaysOfMonth(year, month);
		int count = 7 - weekday;
		int delim = (count < 7) ? count : 0; // 일요일 일때 줄바꿈 해주기 위함
		
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
	
	private int getWeekDay(int year, int month, int day) {
		 
		// 컴퓨터 하드웨어에서 시간을 1970년 1월 1일 (목요일)부터 시작하므로 그날짜를 기준으로 카운팅
		int syear = 1970;
		final int STANDARD_WEEKDAY = 4; // 1970.Jan.1st = Thursday
		
		int count = 0;
		
		for(int i=syear; i<year; i++) {
			int delta = isLeapYear(i) ? 366 : 365;
			count += delta;
		}
		
		for(int i=1; i < month; i++) {
			int delta = getMaxDaysOfMonth(year,i);
			count += delta;
		}
		
		count += day-1;
		int weekday = (count + STANDARD_WEEKDAY) % 7;
		return weekday;
	}
	
	public static void main(String[] args) {
		// 셀 실행
		Prompt p = new Prompt();
		p.runPrompt();
	}
	
	
}



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
		
		int month = 0;
		int year = 0;
		
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
            
			cal.printCalendar(year, month);
		}
		
		System.out.println("Bye~");
		scanner.close();
	}
}

```

특징: 365일을 7로 나눈 나머지가 1이므로 동일월의 1일이 전년의 동일월 1일의 요일보다 하루 늦게 나오게 된다.

ex) 1970년 1월 1일 : 목요일 -> 1971년 1월 1일: 금요일

단, 윤년이 끼어있을 경우는 이틀 늦게 나온다.

1972년 1월 1일 : 토요일 -> 1973년 1월 1일 : 월요일

이를 이용하여 getWeekDay 메서드를 추가로 작성하였음.



👉실행 결과

![image](https://user-images.githubusercontent.com/52458039/126738007-d12eb9b3-c245-4748-a34f-0f915551d197.png)

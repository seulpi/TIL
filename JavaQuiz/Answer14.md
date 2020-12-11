# 1. 상속을 UML로 표기해 보세요
 ## UML : 그림으로  된 표기법  ex) 다이어그램 
![Untitled Diagram](https://user-images.githubusercontent.com/74290204/101778982-fd123100-3b37-11eb-99af-0a58fbb017f3.png)

 <br>

 # 2. 부모클래스와 자식클래스의 다른 용어들은?
## - 부모클래스 = 상위클래스 = Super 클래스 
## - 자식클래스 = 하위클래스 = Sub 클래스
 >> extends ; 상속 키워드
<br>

# 3. super 키워드와 this 키워드의 차이는 무엇인가요?
>> super키워드는 상속에서의 생성자 호출 (부모의 생성자, 변수, 함수 호출 )<br> this 키워드는 상속이 아닌 자기자신의 멤버 의미
```java
// this() / this. / super()

- this() : 클래스 안에서 또 다른 생성자를 호출할 때
- this. : 동일한 이름의 변수가 사용될 경우
          클래스 영역에서 정의한 변수를 식별하기 위해 사용 (자기자신의 변수접근)
- super() : 상속한 부모클래스에 있는 변수 호출
- super. :  상속한 부모클래스에 있는 함수 호출 (super 생략가능)
```

# 4. 단일 상속과 다중상속 이란?
- 단일상속 : 하나의 부모의 하나의 자손 / 자손의 자손 
- 다중상속 : 하나의 자손의 여러 부모 
>> 자바에선 '다중상속' 금지 / C언어에서는 가능

# 5. 다음 코드와 같이 과목과 점수가 짝을 이루도록 2개의 배열을 작성하라
```java
String course[] = {"Java", "C++", "HTML5", "컴퓨터구조", "안드로이드"};
int score[]  = {95, 88, 76, 62, 55};
그리고 다음 예시와 같이 과목 이름을 입력받아 점수를 출력하는 프로그램을 작성하라. 
"그만"을 입력받으면 종료한다(Java는 인덱스 0에 있으므로 score[0]을 출력)
===============================================================
과목 이름 >> Jaba
없는 과목입니다.
과목 이름 >> Java
Java의 점수는 95
과목 이름 >> 안드로이드
안드로이드의 점수는 55
과목 이름 >> 그만

===============================================================
[Hint] 문자열을 비교하기 위해서는 String 클래스의 equals()메소드를 이용해야 한다.

String name;
if(course[i].equals(name)) {
    int n = score[i];
    ...
}
```

```java
>> Answer

// SubjectNum class
import java.util.Scanner;

public class SubjectNum {
	
	private String course[] = {"Java", "C++", "HTML5", "컴퓨터 구조", "안드로이드"};
	private int score[]  = {95, 88, 76, 62, 55};
	private String subject;
	
	Scanner sc = new Scanner(System.in);
	
	public  void scPrint() {
		
			System.out.println("과목이름>> ");
			subject = sc.next();

			if (subject.equals("Jaba")) {
				System.out.println("없는 과목입니다.");
			}
		}
	
	public void subjectOut() {
		for (int i = 0; i < course.length; i++) {
			if (course[i].equals(subject)) {
				System.out.println(course[i] + "의 점수는 " + score[i]);
				return;
			}
		}
	}
	
	public void subjerctReturn () {
		while(true) {
			
			this.scPrint();
			this.subjectOut();
			
			System.out.println("계속 과목을 입력하시겠습니까?");
			
			String answer;
			answer =  sc.next();
			if(answer.equals("그만") || answer.equalsIgnoreCase("stop")) {
				System.out.println("프로그램을 종료합니다");
				break;	
			}
		
		}
	}
}


// Main Class
public static void main(String[] args) {

	SubjectNum subjectNum = new SubjectNum();
		
	subjectNum.scPrint();
	subjectNum.subjectOut();
	subjectNum.subjerctReturn();
}
```
# 6. 아래의 프로그래밍을 하시오
```java
다음은 2차원 상의 한 점을 표현하는 Point 클래스이다.

class Point {
   private int x, y;
   public Point(int x, int y) { this.x = x; this.y = y; }
   public int getX() { return x; }
   public int getY() { return y; }
   protected void move(int x, int y) { this.x =x; this.y = y; }
}

Point를 상속받아 색을 가진 점을 나타내는 ColorPoint 클래스를 작성하라.
다음 main() 메소드를 포함하고 실행 결과와 같이 출력되게 하라.

public static void main(String[] args) {
   ColorPoint cp = new ColorPoint(5, 5, "YELLOW");
   cp.setXY(10, 20);
   cp.setColor("RED");
   String str = cp.toString();
   System.out.println(str+"입니다. ");
}
=======================
RED색의 (10,20)의 점입니다. 
```
```java
>> Answer

// Point를 상속받은 ColorPoint
class Point {
	 private int x, y;
	   public Point(int x, int y) { 
		   this.x = x; this.y = y;
		   }
	   public int getX() { 
		   return x; 
		   }
	   public int getY() { 
		   return y;
		   }
	   protected void move(int x, int y) { 
		   this.x =x; 
		   this.y = y; 
		   }
}

class ColorPoint extends Point {
	
	private String color;
	
	public ColorPoint(int x, int y, String color) {
		super(x, y);
		this.color = color;
	}
	
	public void setXY(int x, int y) {
		super.getX(); // 왜 super(getX(), getY()) 안되지?
		super.getY();
	}
	
	public String setColor(String color) {
		this.color = color;
		return color;
	}
	
	public String toString() {
		
		return color + "색의 (" + super.getX() + "," + super.getY() + ")의 점";
	}
}
```

# 7. 아래의 프로그래밍을 하시오
```java
Point를 상속받아 색을 가진 점을 나타내는 ColorPoint 클래스를 작성하라. 
다음 main() 메소드를 포함하고 실행 결과와 같이 출력되게 하라.

public static void main(String[] args) {
   ColorPoint zeroPoint = new ColorPoint(); // (0,0) 위치의 BLACK 색 점
   System.out.println(zeroPoint.toString() + "입니다.");
   ColorPoint cp = new ColorPoint(10, 10); // (10,10) 위치의 BLACK 색 점
   cp.setXY(5,5);
   cp.setColor("RED");
   System.out.println(cp.toString()+"입니다.");
}
=========================
BLACK색의 (0,0) 점입니다.
RED색의 (5,5) 점입니다.
```
```java
>> Answer

class Point {
	 private int x, y;
	   public Point(int x, int y) { 
		   this.x = x; this.y = y;
		   }
	   public int getX() { 
		   return x; 
		   }
	   public int getY() { 
		   return y;
		   }
	   protected void move(int x, int y) { 
		   this.x =x; 
		   this.y = y; 
		   }
}

class ColorPoint extends Point {
	
	private String color;

	ColorPoint() {
		super(0, 0);
		color = "BLACK";
	}
	
	ColorPoint(int x, int y) {
		super(x, y);
	}
	
	public ColorPoint(int x, int y, String color) {
		super(x, y);
		this.color = color;
	}

	public void setXY(int x, int y) {
		super.getX();  
		super.getY();
	}
	
	public String setColor(String color) {
		this.color = color;
		return color;
	}
	
	public String toString() {
		
		return color + "색의 (" + super.getX() + "," + super.getY() + ")의 점";
	}
}
```

# 8. 아래의 프로그래밍을 하시오
```java
Point를 상속받아 3차원의 점을 나타내는 Point3D 클래스를 작성하라. 다음 main() 메소드를 포함하고 실행 결과와 같이 출력되게 하라.

public static void main(String[] args) {
   Point3D p = new Point3D(1,2,3); // 1,2,3은 각각 x, y, z축의 값.
   System.out.println(p.toString()+"입니다.");
   p.moveUp(); // z 축으로 위쪽 이동
   System.out.println(p.toString()+"입니다.");
   p.moveDown(); // z 축으로 아래쪽 이동
   p.move(10, 10); // x, y 축으로 이동
   System.out.println(p.toString()+"입니다.");
   p.move(100,  200, 300); // x, y, z축으로 이동
   System.out.println(p.toString()+"입니다.");
}
=======================================================
(1,2,3) 의 점입니다.
(1,2,4) 의 점입니다.
(10,10,3) 의 점입니다.
(100,200,300) 의 점입니다.
```
```java
>>Answer


class Point {
	 private int x, y;
	   public Point(int x, int y) { 
		   this.x = x; this.y = y;
		   }
	   public int getX() { 
		   return x; 
		   }
	   public int getY() { 
		   return y;
		   }
	   protected void move(int x, int y) { 
		   this.x =x; 
		   this.y = y; 
		   }
}



class Point3D extends Point {
	
	private int z;
	
	public Point3D(int x, int y, int z) {
		super(x, y);
		this.z = z;
		
	}

	public void moveUp() {
		z++;
	}
	
	public void moveDown() {
		z--;
	}
	
	public void move(int x, int y) {
		super.move(x, y);
	}
	
	public void move(int x, int y, int z) {
		super.move(x, y);
		this.z = z;
	}
	
	public String toString() {
		return "(" + super.getX() + "," + super.getY() + "," + z + ")의 점 ";
	}
}
```
# 8. 배열을 이용하여 간단한 극장 예약 시스템을 작성하여 보자
## ! 로직을 공유했는데도 못품 ㅠㅠㅠ...
```java

아주 작은 극장이라서 좌석이 10개 밖에 되지 않는다.
사용자가 예약을 하려고 하면 먼저 좌석 배치표를 보여준다.
즉, 예약이 끝난 좌석은 1로, 예약이 되지 않은 좌석은 0으로 나타낸다.
==================================================================
출력
--------------------
0 1 2 3 4 5 6 7 8 9
--------------------
0 0 0 0 0 0 0 0 0 0

몇번째 좌석을 예약 하시겠습니까? 2
--------------------
0 1 2 3 4 5 6 7 8 9
--------------------
0 0 1 0 0 0 0 0 0 0
```

```Answer
public static void main(String[] args) {
	int[] seatNum = new int[10];
	int[] seatNum2 = new int[10];
	int selectNum;
		
	System.out.println("--------------------");
		
	for(int i = 0; i <seatNum.length; i++) {
		seatNum[i] = i;
		System.out.print(seatNum[i] + " ");
	}System.out.println();
	
	for(int i = 0; i < seatNum2.length; i++) {
		seatNum2[i] = 0;
		System.out.print(seatNum2[i] + " ");
	}System.out.println();
	System.out.println("--------------------");	
		
	System.out.println("몇 번 째 좌석을 예약하시겠습니까?");
	Scanner sc = new Scanner(System.in);
	selectNum = sc.nextInt();
		
	for(int i = 0; i <seatNum.length; i++) {
		if(selectNum == seatNum[i]) {
			seatNum2[i] = 1;
		} else {
			seatNum2[i] = 0;
		}
	}

	for(int i = 0; i <seatNum.length; i++) {
		seatNum[i] = i;
		System.out.print(seatNum[i] + " ");
		}System.out.println();
		
	for(int i = 0; i < seatNum2.length; i++) {
		System.out.print(seatNum2[i] + " ");
	}System.out.println();
	System.out.println("--------------------");	
}

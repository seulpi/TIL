# 1. 아래의 접근제한자에 대하여 설명하시오

```java
- private
- protected
- default
- public
```
1. public : **다른 패키지**에서의 클래스 접근 가능 (어디서나 접근 ok)
2. protected : **같은 패키지**와 **상속관계**에서 접근 가능
3. default : **같은 패키지**에서 접근 가능
4. private : **클래스 내부**에서만 변수나 함수의 접근이 가능 

    ☞ public과 default만 클래스에 키워드 사용이 가능하고 함수나 변수에는 접근제한자 4가지 모두 사용 가능하다
    ```java
    pubilc class Helloworld() {  
        private int num = 0;
     }

        // default는 아무것도 선언되지 않는것을 말한다 
        String person;
    ```

<br>

# 2. 지역변수에 접근제한자를 붙이지 않는 이유는?

- 지역변수는 어차피 지역이라는 영역을 벗어나면 어떠한 영향을 받지 않기 때문에 굳이 접근제한자를 붙일 필요가 없다

<br>

# 3. 캡슐화에 대하여 설명하시오
캡슐화는 프로그램을 잘 짜는 것 중 하나 <br>
캡슐화는 클래스를 만들 때 클래스의 함수와 변수를 하나로 묶어서 사용자가 쉽게 사용할 수 있게 만드는 것 <br>
예로 하나의 알약만 먹어도 여러가지의 기능이 있는것처럼 사용자가 간략하게 하나의 함수만 사용해도 기능들이 돌아가게 만드는것이 포인트 <br>
보기에 좋은 떡이 먹기에도 좋은 것 

<br>

# 4. 랜덤 숫자 맞추기 게임을 짜시오
```java
>> Game class

import java.util.Scanner;

public class Game {
	
	private int count = 10;
	private int num;
	private int value = 44;
	
	public Game() {
		
	}
	
	public int getCount() {
		return count;
	}
	public void setCount(int count) {
		this.count = count;
	}
	public int getNum() {
		return num;
	}
	public void setNum(int num) {
		this.num = num;
	}
	public int getValue() {
		return value;
	}
	public void setValue(int value) {
		this.value = value;
	}

	public void gameRun() {
		if (num > value) {
			count--;
			System.out.println("down");
			System.out.println("남은 횟수 : " + count);
		} else if (num < value) {
			count--;
			System.out.println("up");
			System.out.println("남은 횟수 : " + count);
		} else {
			count--;
			System.out.println("답은 " + value + "입니다");
			System.out.println("축하합니다" + count + "회 만에 맞추셨습니다");
		}
	}	
}
==========================================================

>> Main class 

import java.util.Scanner;

public class GameMain {

	public static void main(String[] args) {
		
		Game game = new Game();
		
		while(true) {
	
			int num;
			
			Scanner sc = new Scanner(System.in);
			System.out.println("Game Start!");
			num = sc.nextInt();
			
			game.setNum(num);
			game.gameRun
			();
		
			if(num != game.getValue()) 
				continue;
			else 
				break;
		}
	}
}
```
<br>

# 5. static 변수의 다른 용어 3가지를 말해 보시오
클래스변수, 공유변수, 정적변수

<br>

# 6. 자바의 메모리 영역을 3가지로 나누고, 해당 영역에 들어가는 정보를 말하여 보시오
JAVA의 메모리 공간은 크게 Method Area / Calltack / Heap 이 있다 <br>
Method Area : JVM이 class를 메모리에 올릴 때 class 내부에 main함수를 먼저 스캔하고 static으로 선언된 변수나 함수를 **먼저** 올린다 
Calltack : main함수가 여기에서 호출되어서 쌓이고 인스턴스 변수의 공간이 여기 생성된다
Heap : 객체와 변수&함수들이 여기에 저장된다

**▶ 단, Method Area에 올라간 static으로 선언된 변수나 함수는 두번 다시 메모리에 올리지 않는다(메모리에 한번만 올린다)** <br>

![static 메모리에 올리는 구조](https://user-images.githubusercontent.com/74290204/101135246-e60ea300-364e-11eb-81c2-8da53070a81e.PNG)

<br>

# 7. static 변수의 접근 방법은?
static변수는 1. 클래스명으로 접근하거나 2. 객체명으로 접근가능하다 <br> 접근 방법은 2가지이지만 클래스명으로 직접 접근하는건 메모리 측면에서 볼 때 좋은 방법이 아니기 때문에 함수로 접근하는 것이 훨씬 좋다 <br> 
(*static이 클래스명으로 접근가능하기 때문에 클래스변수라고도 불리는것)
- static 변수는 선언하자마자 값을 넣는게 바람직하다

<br>

# 8. 클래스 변수의 활용의 예를 드시오

클래스변수는 변하지 않고 사용해야되고 그 값을 계속 공유해야되는 상황에서 활용된다 <br>
예를들어 1. PI값을 구한다던가 2. 사이즈가 같은 여러개의 카드의 객체를 생성한다던가 3. 프로그램의 전체개수를 count한다던가 하는 것들이 있다
```java
static final double Math.PI;
```


<br>

# 9. 스태틱 함수에 인스턴스 변수가 올수 없는 이유는?
static은 메모리 내부에서 Method Area에 먼저 공간을 잡는데 인스턴스 변수는 Heap이라는 공간에 저장된다 <br>
따라서 메모리가 올라가는 공간이 다르기 때문에 static함수안에는 인스턴스 변수가 올 수 없다 <br>

<br>

# 10. 아래의 프로그램을 작성 하시오
```java

- int 타입의 x, y, width, height 필드: 사각형을 구성하는 점과 크기 정보
- x, y, width, height 값을 매개변수로 받아 필드를 초기화하는 생성자
- int square() : 사각형 넓이 리턴
- void show() : 사각형의 좌표와 넓이를 화면에 출력
- boolean contatins(Rectangle r) : 매개변수로 받은 r이 현 사각형 안에 있으면 true 리턴
- main() 메소드의 코드와 실행 결과는 다음과 같다
public static void main(String[] args) {
   Rectangle r = new Rectangle(2, 2, 8, 7);
   Rectangle s = new Rectangle(5, 5, 6, 6);
   Rectangle t = new Rectangle(1, 1, 10, 10);
   
   r.show();
   System.out.println("s의 면적은 "+s.square());
   if(t.contains(r)) System.out.println("t는 r을 포함합니다.");
   if(t.contains(s)) System.out.println("t는 s를 포함합니다.");
}
(2,2)에서 크기가 8x7인 사각형
s의 면적은 36
t는 r을 포함합니다.
```

```java
// Answer 이거 답 아닌것같음ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ....

public class Rectangle {
	
	private int x,y,width,height;
	
	Rectangle(int x, int y, int width, int height) {
		this.x = x;
		this.y = y;
		this.width = width;
		this.height = height;
	}
	
	public int square() { 
		
		return width*height;
	}
	
	public void show() {
		System.out.println("(" + x + "," + y + ")에서 크기가 " + width + " X "+ height + "인 사각형");
	}

	public boolean contains(Rectangle r) {
		return true;
	}
}
```

---
# ▶  java basic08 정리 참고

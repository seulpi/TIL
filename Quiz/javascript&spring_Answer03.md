# 1. 아래를 설명하시오
```
-DI
-IoC
-IoC 컨테이너
```

## @ DI(Dependency Injection) : 의존성 주입
   - 예를 들어 컴퓨터는 Cpu가 없으면 안되는데 이러한 관계를 두고 **의존성관계**라고 한다(부모 자식간의 관계가 아님)
   - 주입이란 객체 생성를 생성하고 나서 **값을 받는것**을 말한다
   - 객체를 생성할때는 3가지의 방식이 존재한다
   1. new를 통한 객체 생성
   2. 생성자 함수를 통한 객체 생성
   3. getter&setter함수를 통한 객체 생성 
   ### ▶  1번은 다이렉트로 값 주입, 2-3번은 값을 외부에서 받아야하기 때문에 **의존성 주입**이라고 하는데 **이게 바로 DI**다 

## @ IOC(Inversion Of Contol) 
   - 객체 안에 객체를 생성할때 **흐름**이라는 것이 존재하는데 이 흐름이 **반대로 실행되는 것**을 말한다
   ```java
   예를 들어 컴퓨터라는 객체를 생성한다고 해보자

   //객체 실행의 흐름
   Computer → Cpu → Chip  

   //값을 주입받는 흐름 
   Chip → Cpu → Computer
   ```

## @ IOC 컨테이너 = Spring
### : IOC를 관리하는 컨테이너 (IOC를 조립하고 생성한다)

## ▶ Spring_basic01(DI, IOC,IOC컨테이너) 
- https://github.com/seulpi/TIL/blob/main/Basic/Spring/Spring_basic01(DI%2C%20IOC%2CIOC%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88).md
<br>

# 2. JS로 시간이 초단위로 갱신되는 페이지를 만드시오
```jsp
// 새로고침 페이지
<script type="text/javascript">
		var pageSetTimeStop = setTimeout(location.reload();
		}, 10000); // 10초마다 새로고침
	
	</script>
``` 

## ▶ 문제의도: 전자시계 만들어라 (초단위로 나오는)

```html
<!DOCTYPE html>
<html>

   <head>
      <title>Javascript</title>
      <script>
      
      	setInterval(function(){
      		var timer = new Date();
      		var h = timer.getHours();
      		var m = timer.getMinutes();
      		var s = timer.getSeconds();
      		document.getElementById('clock').innerHTML = h + ":" + m + ":" +s;
      	})
         
      </script>
   
   </head>
   
   <body>
   	<hi id="clock">
   
   </body>

</html>
```


# 3. js에서의 객체 생성 방법은? 
## var 변수명 = {key: value}  
```html
<script type="text/javascript">
    var car = {
        name: Kona;
        color : blue;
    }
</script>
```
### - js에서는 함수안에서도 객체 생성이 가능
```html
<script type="text/javascript">
    function car(name, blue) {
        var carObj = {
            name: name;
            color: color;
        };
        return carObj;
    };
</script>
```

# 4. 아래를 자바 스크립트로 작성하시오
```
-변수 radius
-get set 함수 작성
-프롬프트로 숫자 입력값 받음
-set 함수를 radius 값 세팅
-객체 생성후 getArea() 함수 호출하면 원넓이 출력
```
```javascript
function Circle() {
			var radius = 0;
			
			this.getRadius = function() {
				return this.radius;
			}
			
			this.setRadius = function(radius) {
				this.radius = radius;
			};
			
			this.getArea = function() {
				return this.radius *this.radius * 3.14; // this를 붙여줘야 값이 나오는데 자바스크립트에서는 내부변수에 접근할때 this붙여줄것
			};
		};
		
		
		var circle = new Circle();
		var radius = prompt("반지름값을 입력하세요", "value");
		circle.setRadius(Number(radius));
		document.write("원의 넓이는 : " + circle.getArea());
```
<br>

# 5. 위와 같은 방식으로 가위바위보 게임을 짜시오(수정필요) 
```
- 텍스트로만 나오게 하면됨
- DOM 객체를 배우면 이미지도 바꿔 보도록 합시다
```
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>가위 바위 보 게임</title>
</head>
<body>
	<script type="text/javascript">
		function RandomGame() { //여기서 유저로 받아가지고 비교?
			var user = "";
			var computer = 0;
			var strCom ="";	

			this.getUser = function() {
				return this.user;
			}
			
			this.setUser = function(user) {
				this.user = user;
			} 
			
			this.getComputer = function() {
				return this.computer;
			}
			
			this.setComputer = function(computer) {
				this.computer = computer;
				
			};
			
			this.getStrCom = function() {
				return this.strCom;
			}
			
			this.setStrCom = function(strCom) {
				this.strCom = strCom;
				
			};
			
			this.play = function comeResult(getComputer) {
	  			if (getComputer == 1) {
	  				this.strCom = "가위";
	  			} else if (getComputer == 2) {
	  				this.strCom = "바위";
	  			} else {
	  				this.strCom = "보";
	  			};

	  		}
			
		};
		
  		var game = new RandomGame();
  		var user = prompt("가위바위보를 입력하세요", "ex. 가위")
  		game.setUser(user);
  		game.setComputer(Math.floor(Math.random()*3 + 1));
  		game.play;
  		
		switch (game.getUser()) {

		case "가위":
			if (game.getStrCom()== "가위") {
				document.write("무승부입니다");
			} else if (game.getStrCom()== "바위") {
				document.write("컴퓨터 WIN!");
			} else if (game.getStrCom()== "보") {
				document.write("유저 WIN!");
			} else {
				document.write("다시입력하세요");
			}
			break;

		case "바위":
			if (game.getStrCom()== "바위") {
				document.write("무승부입니다");
			} else if (game.getStrCom() == "보") {
				document.write("컴퓨터 WIN!");
			} else if (game.getStrCom() == "가위") {
				document.write("유저 WIN!");
			} else {
				document.write("다시입력하세요");
			}
			break;

		case "보":
			if (game.getStrCom() == "보") {
				document.write("무승부입니다");
			} else if (game.getStrCom() == "가위") {
				document.write("컴퓨터 WIN!");
			} else if (game.getStrCom()== "바위") {
				document.write("유저 WIN!");
			} else {
				document.write("다시입력하세요");
			}
			break;

		}
	</script>
</body>
</html>
```

### ▶ 출력
![가위바위보](https://user-images.githubusercontent.com/74290204/105151636-878f7c80-5b49-11eb-874c-67c7782d4048.PNG)

# 6. annotation 방식으로 하여 객체 생성후 사각형과 삼각형 넓이를 구하시오
## 6-1 삼각형넓이
```java
//Circle.java

package spring_rec_cir;

public class Circle {
	
	private double radius;

	public Circle() {
		
	}
	
	public Circle(double radius) {
		this.radius = radius;
	}
	
	
	public double getRadius() {
		return radius;
	}

	public void setRadius(double radius) {
		this.radius = radius;
	}
	
	public double getArea() {
		return radius*radius*Math.PI;
	}
}
```
```java
//Application.java

package spring_rec_cir;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Appilcation {

	@Bean
	public Circle circle() {
		Circle circle1 = new Circle(3.15);

		return circle1;
	}	
}
```
```java
//Main.java

package spring_rec_cir;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {

	public static void main(String[] args) {
		
		AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(Application.class);
		
		Circle circle = context.getBean("circle", Circle.class);
		System.out.println("원의 넓이는: " + circle.getArea());

	}
}
```

### ▶ 출력
![6-1출력](https://user-images.githubusercontent.com/74290204/105140627-5445f100-5b3b-11eb-996a-6a5e9f55bdc2.PNG)
<br>

## 6-2 사각형넓이
```java
//Rectangle.java

package spring_rec_cir;

public class Rectangle {
	
	private double width;
	private double height;
	
	public Rectangle() {
		
	}
	
	public Rectangle(double width, double height) {
		this.width = width;
		this.height = height;
	}
	public double getWidth() {
		return width;
	}
	public void setWidth(double width) {
		this.width = width;
	}
	public double getHeight() {
		return height;
	}
	public void setHeight(double height) {
		this.height = height;
	}
	
	public double getArea() {
		return width*height;
	}

}
```
```java
//Application.java
package spring_rec_cir;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Appilcation {
	
	@Bean
	public Rectangle rectangle() {
		Rectangle rec = new Rectangle(2.3, 3.4);
		return rec;
	}
}
```
```java
//Main.java

package spring_rec_cir;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {

	public static void main(String[] args) {
		
		AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(Application.class);
		
		Rectangle rec = context.getBean("rectangle", Rectangle.class);
		System.out.println("원의 넓이는: " + rec.getArea());

	}
}
```

### ▶ 출력
![6-2출력](https://user-images.githubusercontent.com/74290204/105141362-61171480-5b3c-11eb-9f59-a6b70a2c98ce.PNG)
<br>

# 7. 금일 배운 Pencil의 예처럼 아래를 인터 페이스를 구현하여, 원, 삼각형, 사각형의 넓이를 설정파일 에서 바꾸면 각각의 넓이가  구하여 지도록 하시오.
```java
interface IShape{
	double getArea();
}
```

## 7-1 원넓이
```java
// Area.java
package spring_rec_cir;

public interface Area {
	public void area();

}
```
```java
//CircleArea.java

package spring_rec_cir;

public class CircleArea implements Area {

	@Override
	public void area() {
		
		Circle circle = new Circle(2.4);
		System.out.println("원의 넓이는 : " + circle.getArea());

	}

}
```
```java
// Circle.java

package spring_rec_cir;

public class Circle {
	
	private double radius;

	public Circle() {
		
	}
	
	public Circle(double radius) {
		this.radius = radius;
	}
	
	
	public double getRadius() {
		return radius;
	}

	public void setRadius(double radius) {
		this.radius = radius;
	}
	
	public double getArea() {
		return radius*radius*Math.PI;
	}
}
```
```java
//Main.java

package spring_rec_cir;

import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class Main {

	public static void main(String[] args) {
		String configLocation = "classpath:applicationCTX.xml";
		AbstractApplicationContext ctx = new GenericXmlApplicationContext(configLocation);
		
		Area area = ctx.getBean("area", Area.class);
										//↑ 이 클래스는 인터페이스로 맞춰줘야함, 자식들은 xml에서 컨트롤
		area.area();
		ctx.close();

	}
}
```
```xml
<!--applicationCTX.xml-->

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!-- 여기 위에까지는 ctrl c + ctrl v -->

<bean id="area" class="spring_rec_cir.CircleArea"></bean>
	
 </beans>
```

### ▶ 출력
![7-1](https://user-images.githubusercontent.com/74290204/105145545-421b8100-5b42-11eb-922c-0fd9e6c27bc2.PNG)
<br>

## 7-2 삼각형넓이
```java
//TriangleArea.java
package spring_rec_cir;

public class TriangleArea implements Area {

	@Override
	public void area() {
		Triangle triangle = new Triangle(2.5, 6.5);
		System.out.println("삼각형의 넓이는: " + triangle.getArea());
	}

}
```
```java
//Triangle.java

package spring_rec_cir;

public class Triangle {
	
	private double width;
	private double height;
	
	public Triangle() {
		
	}
	
	public Triangle(double width, double height) {
		this.width = width;
		this.height = height;
	}

	public double getWidth() {
		return width;
	}

	public void setWidth(double width) {
		this.width = width;
	}

	public double getHeight() {
		return height;
	}

	public void setHeight(double height) {
		this.height = height;
	}
	
	public double getArea() {
		return (width * height)/2 ;
	}

}
```
```xml
<!-- applicationCTX.xml -->

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<bean id="area" class="spring_rec_cir.TriangleArea"></bean>

 </beans>
```

### ▶ 출력
![7-2](https://user-images.githubusercontent.com/74290204/105145483-257f4900-5b42-11eb-8120-72b14f01b0c2.PNG)
<br>

## 7-3 사각형넓이
```java
//RectangleArea.java

package spring_rec_cir;

public class RectangleArea implements Area {

	@Override
	public void area() {
		Rectangle rectangle = new Rectangle(5.4, 6.5);
		System.out.println("사각형의 넓이는 : " + rectangle.getArea());

	}
}
```
```java
//Rectangle.java

package spring_rec_cir;

public class Rectangle {
	
	private double width;
	private double height;
	
	public Rectangle() {
		
	}
	
	public Rectangle(double width, double height) {
		this.width = width;
		this.height = height;
	}
	public double getWidth() {
		return width;
	}
	public void setWidth(double width) {
		this.width = width;
	}
	public double getHeight() {
		return height;
	}
	public void setHeight(double height) {
		this.height = height;
	}
	
	public double getArea() {
		return width*height;
	}

}
```
```xml
<!-- applicationCTX.xml -->

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<bean id="area" class="spring_rec_cir.RectangleArea"></bean>

 </beans>
```

### ▶ 출력
![7-3](https://user-images.githubusercontent.com/74290204/105145351-f79a0480-5b41-11eb-9229-2eabe2c58771.PNG)


# 8.스프링 미리 공부 구현 해야될 내용-미리준비해 놓읍시다.(한마디로 외워 제낍시다)
## 스프링 게시판(오라클 + 마이바티스),스프링 시큐리티, 소셜로그인(OAuth2)-카카오,네이버 먼저, 결재구현(아임포트)

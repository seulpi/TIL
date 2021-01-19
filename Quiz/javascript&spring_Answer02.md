# 1. 클로져란 무엇인가?
: **함수 안에 함수가 존재하는 형태**로써 외부 함수의 지역변수에 대해 내부함수가 접근가능하고 <br>
외부함수는 외부함수의 지역변수를 사용하는 내부함수가 소멸될때까지 소멸되지 않는 특성을 지닌다
- 외부함수의 데이터 멤버들은 범위가 전체적으로 사용됨을 의미 (외부함수내에서)

## ▶ 한마디로 **클로져 = 함수 in 함수** 
<br>

# 2. js를 이용하여, 구구단중 홀수단만 나오게 하시오
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>	
	<script src="jsAnswer.js" type="text/javascript">
		
	
	</script>

</body>
</html>
```
```java script
for(var i = 2; i <10; i++) {
	for(var j = 1; j <10; j++) {
		if(i % 2 == 0) {
			continue;
		}else {
			console.log(i + " * " + j + " = " + (i*j));
		}
	}
}
```
<br>

# 3. 부서별로 sal의 최소 값을 구하는데, 30번 부서의 sal 최소값 보다 큰것을 구하시오
```sql
select min(emp.sal) from emp, dept where emp.deptno = dept.deptno group by dept.dname having min(emp.sal) >
(select min(sal) from emp, dept where emp.deptno = dept.deptno and dept.deptno = 30);
```
<br>

# 4. 삼각형및 사각형의 넓이를 구하는 프로그래밍을 IoC 컨테이너를 이용하여 프로그래밍 하시오
```java
//RectangleMain.java
package com.javalec.ex;

import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class RectangleMain {

	public static void main(String[] args) {
		String configLocation = "classpath:applicationCTX2.xml";
		
		AbstractApplicationContext ctx = new GenericXmlApplicationContext(configLocation);
		
		Rectangle rec = ctx.getBean("rec", Rectangle.class);
		System.out.println("사각형의 넓이: " + rec.area());
		ctx.close();
	}
}
```
```java
//Rectangle.java

package com.javalec.ex;

public class Rectangle {
	
	private double width;
	private double height;
	
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
	
	public double area() {
		return width * height;  
	}

}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!-- 여기 위에까지는 ctrl c + ctrl v -->

  <bean id="rec" class="com.javalec.ex.Rectangle">
    <property name="width">
      <value>3.5</value>
    </property>
    <property name="height">
      <value>2.5</value>
    </property>
  
  </bean>

</beans>
```


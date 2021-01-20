# Spring 
## Spring이란 자바 플랫폼을 위한 오픈소스 어플리케이션 프레임워크 
- 프레임워크 : 특정한 목적에 맞게 프로그래밍을 쉽게 하기 위한 약속(캡슐화)


# ★ 면접에서 꼭 묻는 DI, IOC, IOC컨테이너 ★

# @ DI (Dependency Injection) : 의존성 주입 
- 스프링이 다른 프레임워크와 차별화되어 제공하는 의존 관계 주입 기능을 말한다 <br>
▶ 객체를 직접 생성하는 게 아니라 외부에서 생성한 후 주입 시켜주는 방식

### - 객체 생성 방법 3가지가 존재
1. new를 통한 객체 생성
```java
class Computer {
	Cpu cpu = new IntelCpu();
	MatherBoard board;
	...
	...	
}
// 객체에 값을 다이렉트로 넣는 방법
```

2. 생성자를 통한 객체 생성 
```java
class Computer {
	Cpu cpu;
	MatherBoard board;
	...
	...
	public Computer(Cpu cpu) {
		this cpu = cpu;
	}	
}
```

3. set함수를 통한 객체 생성
```java
public setCpu(Cpu cpu) {
		this.cpu = cpu;
}
``` 

### ☞ new를 통한 객체생성의 값을 다이렉트로 할당하는 방법과 달리 <br> 2, 3(생성자, set함수를 통한 객체생성)은 값을 다른 클래스에서 값을 넣어줄 수 밖에 없음 (외부에서 주입) 
### ▶ 여기서 <span style= "color:red">'주입'의 개념이 Injection </sapn>
### ▶ Dependency : 서로 없으면 안되는 관계
```
예를 들어 컴퓨터는 CPU가 없으면 안됨 → 이러한 관계를 두고 '의존성'이라고 함 (부모와 자식관계가 아님)
```

## @ DI 활용 
```java
//Student.java
package edu.bit.ex;

public class Student {
	
	private String name;
	private String age;
	private String gradeNum;
	private String classNum;
	
	
	public Student() {
		
	}
	
	public Student(String name, String age, String gradeNum, String classNum) {
		
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getAge() {
		return age;
	}
	public void setAge(String age) {
		this.age = age;
	}
	public String getGradeNum() {
		return gradeNum;
	}
	public void setGradeNum(String gradeNum) {
		this.gradeNum = gradeNum;
	}
	public String getClassNum() {
		return classNum;
	}
	public void setClassNum(String classNum) {
		this.classNum = classNum;
	}
	

}
----------------------------------------
//StudentInfo.java

package edu.bit.ex;

public class StudentInfo {

	private Student student;

	public StudentInfo(Student student) {
		this.student = student;
		//StudentInfo는 Student라는 객체를 생성하고 값을 주입시킨다
	}

	public void getStudentInfo() {
		if (student != null) {
			System.out.println("이름 : " + student.getName());
			System.out.println("나이 : " + student.getAge());
			System.out.println("점수 : " + student.getGradeNum());
			System.out.println("학번 : " + student.getClassNum());
			System.out.println("======================");
		}
	}

	public void setStudent(Student student) {
		this.student = student;
	}
}
----------------------------------------
//StudentMain.java

package edu.bit.ex;

import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class StudentMain {

	public static void main(String[] args) {

		String configLocatioin = "classpath:applicationCTX.xml"; //src에 넣으면 루트가 되는것

		AbstractApplicationContext ctx = new GenericXmlApplicationContext(configLocatioin);
		/* driver 다운로드 받아서 라이브러리 추가했듯이 이 친구들고 그렇게 해줘야함 
		→ pom.xml <build> 위에 <properties> ~ <dependency>부분 복사 붙여넣기 → 저장
		→ pom.xml 우클릭 → Maven → update project → Force update~ 체크 → ok */
		StudentInfo studentInfo = ctx.getBean("studentInfo", StudentInfo.class);
		studentInfo.getStudentInfo();

		Student student2 = ctx.getBean("student2", Student.class);
		studentInfo.setStudent(student2);
		studentInfo.getStudentInfo();

		ctx.close();

	}

}
```
### - tring configLocatioin = "classpath:applicationCTX.xml"에서 "classpath:applicationCTX.xml" 경로
![캡처](https://user-images.githubusercontent.com/74290204/105135949-2b6e2d80-5b34-11eb-8df9-9d05a6db39b4.PNG)

```xml
<!--pom.xml-->

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!-- 여기 위에까지는 ctrl c + ctrl v -->

<bean id="student1" class="edu.bit.ex.Student">
    <constructor-arg> <!-- Student클래스의 생성자 호출 -->
      <value>홍길동</value>
    </constructor-arg>
    <constructor-arg>
      <value>10살</value>
    </constructor-arg>
    <constructor-arg>
      <value>3학년</value>
    </constructor-arg>
    <constructor-arg>
      <value>20번</value>
    </constructor-arg>
  </bean>

	<!-- 위에 student1과 같은 방법인데 조금더 간결하게 표현한 방법 -->
  <bean id="student2" class="edu.bit.ex.Student">
    <constructor-arg value="홍길동" />
    <constructor-arg value="9살" />
    <constructor-arg value="2학년" />
    <constructor-arg value="10번" />
  </bean>

  <bean id="studentInfo" class="edu.bit.ex.StudentInfo">
    <constructor-arg>
      <ref bean="student1" /> <!-- 참조형을 주입할때는 ref -->
    </constructor-arg>
  </bean>
```

```java
//PencilMain.java

package edu.bit.ex;

import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class StudentMain {

	public static void main(String[] args) {

		String configLocatioin = "classpath:applicationCTX.xml"; //src에 넣으면 루트가 되는것

		AbstractApplicationContext ctx = new GenericXmlApplicationContext(configLocatioin);
		/* driver 다운로드 받아서 라이브러리 추가했듯이 이 친구들고 그렇게 해줘야함 
		→ pom.xml <build> 위에 <properties> ~ <dependency>부분 복사 붙여넣기 → 저장
		→ pom.xml 우클릭 → Maven → update project → Force update~ 체크 → ok */
		Pencil pencil = ctx.getBean("pencil", Pencil.class);
		pencil.use();

		ctx.close();

	}

}

----------------------------------------------------------
// Pencil Interface

package edu.bit.ex;

public interface Pencil {
	void use();
}

----------------------------------------------------------
//Pencil4B.java

package edu.bit.ex;

public class Pencil4B implements Pencil {

	@Override
	public void use() {
		System.out.println("4B 굵기로 쓰입니다.");

	}

}

----------------------------------------------------------
//Pencil6B.java

package edu.bit.ex;

public class Pencil6B implements Pencil {

	@Override
	public void use() {
		System.out.println("6B 굵기로 쓰입니다.");

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

<bean id="pencil" class="edu.bit.ex.Pencil4B"></bean>
<!-- 여기서 6B로 가고싶으면 만든 클래스명만 바꿔주면 가능
이게 바로 DI를 활용하는 이유 : 소스코드의 수정없이 xml파일만을 수정해서 내용을 수정할 수 있다
(Polymortism 적용) Main → Pencil pencil = ctx.getBean("pencil", Pencil.class);-->

 </beans>
```


## @ DI 설정 방법
### 1. 자바를 이용한 DI설정 방법 
- Bean어노테이션을 이용하는 방법
- 장점 : **디버깅이 가능하다** (xml은 디버깅안되서 실행시킨후 크롬에서 확인해야하는데.,)
```java
//Student.java

package edu.bit.ex;

import java.util.ArrayList;

import org.springframework.context.annotation.Bean;
package edu.bit.ex;

import java.util.ArrayList;

public class Student {
	
	private String name;
	private int age;
	 private ArrayList<String> hobbys;
	  private double height;
	  private double weight;

	  public Student(String name, int age, ArrayList<String> hobbys) {
	    this.name = name;
	    this.age = age;
	    this.hobbys = hobbys;
	  }

	  public void setName(String name) {
	    this.name = name;
	  }

	  public void setAge(int age) {
	    this.age = age;
	  }

	  public void setHobbys(ArrayList<String> hobbys) {
	    this.hobbys = hobbys;
	  }

	  public void setHeight(double height) {
	    this.height = height;
	  }

	  public void setWeight(double weight) {
	    this.weight = weight;
	  }

	  public String getName() {
	    return name;
	  }

	  public int getAge() {
	    return age;
	  }

	  public ArrayList<String> getHobbys() {
	    return hobbys;
	  }

	  public double getHeight() {
	    return height;
	  }

	  public double getWeight() {
	    return weight;
	  }
	}

-----------------------------
// AplicationConfig.java

import org.springframework.context.annotation.Configuration;

@Configuration
public class ApplicationConfig {
	
	@Bean// Bean어노테이션은 명시해주면 알아서 안에 있는 내용을 객체로 만들어주고 함수명을 변수명으로 가져가서 사용한다
	public Student student1() {
		ArrayList<String> hobbys = new ArrayList<>();
		hobbys.add("수영");
		hobbys.add("요리");
		
		Student student = new Student("홍길동", 20, hobbys);
		student.setHeight(180);
		student.setWeight(80);
		
		return student;
	}
	
	@Bean 
	public Student student2() {
		ArrayList<String> hobbys = new ArrayList<>();
		hobbys.add("독서");
		hobbys.add("음악감상");
		
		Student student = new Student("홍길순", 18, hobbys);
		student.setHeight(170);
		student.setWeight(55);
		
		return student;
	}
	
	

}

-------------------------
// StudentMain.java

package edu.bit.ex;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class StudentMain {

	public static void main(String[] args) {
		AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(ApplicationConfig.class);
		/* Annotation을 활용하면 xml을 사용하지 않아도 됨, 단 ApplicationConfig.class는 직접 구현해줘야하고
		ApplicationConfig.class안에 @Config라고 명시해줘야한다*/

	    Student student1 = ctx.getBean("student1", Student.class);
				// 함수명을 변수명으로 가져온것 
	    System.out.println("이름 : " + student1.getName());
	    System.out.println("나이 : " + student1.getAge());
	    System.out.println("취미 : " + student1.getHobbys());
	    System.out.println("신장 : " + student1.getHeight());
	    System.out.println("몸무게 : " + student1.getWeight());

	    Student student2 = ctx.getBean("student2", Student.class);
	    System.out.println("이름 : " + student2.getName());
	    System.out.println("나이 : " + student2.getAge());
	    System.out.println("취미 : " + student2.getHobbys());
	    System.out.println("신장 : " + student2.getHeight());
	    System.out.println("몸무게 : " + student2.getWeight());

	    ctx.close();

	}
}
```

---

# - IOC (Inversion Of Control) 
- DI에서 유래(기반)
```java
* Computer, CPU, Chip 객체가 만들어져있다고 가정

Computer computer = new Computer();

// 객체 생성 순서(흐름의 순서)
Computer → CPU → Chip '완제품' (반드시 만들어져야 있어야 호출이 가능하기 때문) 
-------------------------------------------------------------------------------

Chip chip = new Chip();
Cpu cpu = new Cpu(chip);
Computer computer = new Computert(cpu);

// 주입받아야할 때 순서 
Chip → CPU → Computer '조립품'

computer.setCpu(new Intel()); 이게 가능함(부분 부분 다른 조립품으로 수정이 가능)
```
## ▶ 위 코드를 보면, 흐름의 순서와 주입의 흐름순서가 **정반대** 그래서 **Inversion Of Control** 이라고 표현
### [여기서 더 좋은 코드는?] 
- 2번째 조립품의 코드가 더 좋은 코드
    - why? 1번째 코드는 한번 완성되면 중간에 수정이 불가능하지만 **2번째 코드는 완성되도 중간에 수정이 가능하기 때문**<br> (제조사 바뀌면 제조사만 바꿀 수 있는 그런 코드!!)

<br>

# - IOC 컨테이너
### - IOC를 조립하고 생성하는 컨테이너 = Spring(객체들의 놀이터)
- xml 문서로 만들 수 있음 → 설정 파일에서 값을 바꿀 수 있음 
- pojo : bean해서 getter&settter로 이루어진것을 말함 

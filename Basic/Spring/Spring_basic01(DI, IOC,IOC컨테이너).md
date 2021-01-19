# Spring 
## Spring이란 자바 플랫폼을 위한 오픈소스 어플리케이션 프레임워크 
- 프레임워크 : 특정한 목적에 맞게 프로그래밍을 쉽게 하기 위한 약속(캡슐화)


# ★ 면접에서 꼭 묻는 DI, IOC, IOC컨테이너 ★

## @ DI (Dependency Injection) : 의존성 주입 
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
### 여기서 <span style= "color:red">'주입'의 개념이 Injection </sapn>
### Dependency : 서로 없으면 안되는 관계
```
예를 들어 컴퓨터는 CPU가 없으면 안됨 → 이러한 관계를 두고 '의존성'이라고 함 (부모와 자식관계가 아님)
```

---

## @ IOC (Inversion Of Control) 
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

## @ IOC 컨테이너
### - IOC를 조립하고 생성하는 컨테이너 = Spring(객체들의 놀이터)
- xml 문서로 만들 수 있음 → 설정 파일에서 값을 바꿀 수 있음 
- pojo : bean해서 getter&settter로 이루어진것을 말함 

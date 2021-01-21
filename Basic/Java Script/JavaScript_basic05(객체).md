# ★ 객체 
## - 객체 생성 방법 ①
>> var 변수명 = { key : value}
- 접근 → 변수명.key / this는 내부변수 접근 (자바와 비슷)
```javascript
var objCar = {
	witdth : "3m",
	height : "2m",
	cc : "2000cc",
	energy : 100,
	speed : function(power) {
	return this.energy * power;
	}
};
		
console.log("objCar.speed : " + objCar.speed(2));
```
```javascript
var objCar = {
	witdth : "3m",
	height : "2m",
	cc : "2000cc",
	energy : 100,
	speed : function(power) {
		return this.energy * power;
	}
};
		
var print = "";
	for(var key in objCar) {
		print += key + " : " + objCar[key] + "\n";
}
		
console.log(print);
```

## - 객체 생성 방법 ②
>> 함수 안에 객체 생성 
```javascript
function createCar(name, color, speed) {
			var carObj = {
				name : name,  // name을 외부에서 값을 받아서 key- name에 대입
				color : color,
				speed : speed,
				run : function() {
					return this.speed + "km/h";
				}
			};
			return carObj;
		};
		
		var sorento = createCar("Sorento", "Gray", 220);
		console.log("sorento.name : " + sorento.name);
		console.log("sorent.run : " + sorento.run());
===========================================================================
▶
sorento.name : Sorento 
sorento.run : 220km/h
```

## - 객체 생성 방법 ③
>> 생성자를 이용한 객체 생성(for 초기화)
- 함수 형태로 this를 줌으로써 멤버변수 key를 만들고 함수인자를 받아 대입한다 
    - getter&setter 함수 만드는 객체 생성
- **생성자 함수명의 첫글자는 대문자로 사용**
- 함수를 대입했을때와 생성자를 대입했을때의 **타입이 달라짐**
    - **함수를 변수**에 대입하면 **0타입: 함수** / **생성자를 변수**에 대입하면 **타입:객체** 
```javascript
function BMICalculator (height, weight) {
			this.height = height;
			this.weight = weight;
			this.bmi = function() {
				return (this.weight / (this.height * this.height)).toFixed(2)
			};
		}
		
		var myBMI = new BMICalculator(1.9, 90);
		console.lof("myBMI.bmi : " + myBMI.bmi());


→ getter& setter함수로 
function BMICalculator () {
			var height = 0;
			var weight = 0;
			
			this.bmi = function() {
				return (this.weight / (this.height * this.height)).toFixed(2);
				//Number.prototype.toFixed{};'
			};
			
			this.getHeight = function() {
				return this.height;
			};
			
			this.setHeight = function(height) {
				if(!isNaN(height)) {
					this.height = height;
				}else {
					console.log("height is NaN(Not a Number);");
				};
 			};
 			
 			this.getWeight = function() {
 				return this.weight;
 			};
 			
 			this.setWeight = function(weight) {
 				if(!isNaN(weight)) {
 					this.weight = weight;
 				}else {
 					console.log("weight is NaN(Not a Number);");
 				};
 			} ;
		}
		
		var myBMI = new BMICalculator();
		myBMI.setHeight(1.9);
		myBMI.setWeight(90);
		console.log("myBMI.bmi : " + myBMI.bmi());
```

## @ Ptorotype
- prototype이란 **공통으로 공유된 공간에 하나만 사용**(JS만의 특징)
- [배경] : 생성자를 만들었을 때 생성자안의 함수(기능)들은 계속 new해서 메모리에 올릴 필요가 있을까?(메모리 낭비같음) <br> → 한번만 메모리에 올려놓고 계속 사용하면 되지 않을까? → 그런 발상으로 나온 게 *Prototype*
- prototype은 모든 객체에 존재하며 메모리에 한번만 올린다 (static과 비슷하다고 생각하면된다)


## @ 속성 추가&삭제 
- 추가 : '.'
- 삭제 : delete
```javascript
var objName = {
			name : "Lee sun",
			nation : "Korea",
			gender : "men",
			character : "good"
		};
		
		var print = "";
		for(var key in objName) {
			print += key + " : " + objName[key] +"\n"
		}
		console.log(print);
		//.을 이용한 내용 추가
		objName.talent = "fencing";
		print = "";
		for(var key in objName) {
			print += key + " : " + objName[key] + "\n"
		}
		console.log(print);
		//delete를 이용한 내용 삭제
		delete objName.talent;
		print = "";
		for(var key in objName) {
			print += key + " : " + objName[key] + "\n"
		}
		console.log(print);
```

## @ in, with 
- in : key 존재 유뮤 확인
- with : 객체접근을 간소화 
    - with(objName) { }으로 묶어주면 변수에 접근할 때 변수명을 적어주지 않아도 사용이 가능하다
```javascript
var objName = {
	name : "Lee sun",
	nation : "Korea",
	gender : "men",
	character : "good"
};
		
with(objName) {
	console.log(name);
	console.log(nation);
	console.log(gender);
	console.log(character);
}
```

# 내장 객체 
## 1. String : 자바와 가지고 있는 함수와 거의 유사하게 사용
```javascript
var setEn = "ABC";
var setKo = new String("가나다");
String.prototype.temp = 123;
```
- constructor() : constuctor를 리턴
    - In JavaScript, the constructor property returns the constructor function for an object
```javascript
string.constructor
```
- charAt()
- function String() { [native code] }
- replace()
- subString()
- split()
- slice(start, end) : start지점에서 start지점을 포함한 문자를 end까지만 남기고 문자 없애고 출력하는 함수 
    - index : 0부터 시작
```javascript
var strEn = "ABCDEFG";
var strKor = new String("가나다라마바사아");
String.prototype.temp = 123;
		
console.log("strEn : " + strEn);
console.log("strKor : " + strKor);
		
console.log("strEn.length : " + strEn.length);
console.log("strKor.length : " + strKor.length);
		
console.log("strEn.constuctor : " + strEn.costructor);
console.log("strKor.constructor : " + strKor.constructor);
		
console.log("strEn.temp : " + strEn.temp);
console.log("strKor.temp : " + strKor.temp);
		
console.log("strEn.length : " + strEn.length);
console.log("strKor.length : " + strKor.length);
		
console.log("chatAt() : " + strEn.charAt(3));
console.log("indexOf() : " + strEn.indexOf("E"));
console.log("lastIndexOf() : " + strEn.lastIndexOf("E"));
console.log("concat() : " + strEn.concat("abcde"));
console.log("replace() : " + strEn.replace("A", "a"));
console.log("split() : " + strEn.split("C"));
console.log("slice() : " + strEn.slice(2, 4));
// 앞에서 2번째 인덱스의 값에서 그 문자를 포함해서 4개까지만 문자를 남기고 다 잘라버리는 함수 

console.log("substring() : " + strEn.substr(1, 4));
		
var lc = strEn.toLocaleLowerCase();
console.log("lc : " + lc);
		
var up = lc.toUpperCase();
console.log("up : " + up);
```

## 2. Math 
- PI
- abs() : 절대값
- max(), min()
- pow() : n승 (xⁿ)
- floor(소수점잘라버리기)
- round(반올림)
- ceil() : 가까운 정수로 return, round랑은 다름
- sqrt(제곱근)

## 3. Date : 생성자를 통해 생성 
>> var objDate = new Date();

![date](https://user-images.githubusercontent.com/74290204/105261518-1eece200-5bd3-11eb-8858-b72d3cdfda8d.PNG)

```javascript
var objDate1 = new Date(); // 현재시간을 기준으로 실시간 날짜와 요일과 시간
var objDate2 = new Date(2010, 10, 10); // 2010년 10월 10일과 요일
var objDate3 = new Date(2010, 10, 10, 10, 10, 10, 100); // 2010년 10월 10일 10시 10분 10초와 요일, 100은 뭐지?
		
console.log(objDate1);
console.log(objDate2);
console.log(objDate3);
		
console.log("objDate1.getYear : " + objDate1.getYear()); // local 시간에 맞는 현재 년도 return 
console.log("objDate1.getYear : " + (objDate1.getYear() + 1900)); //1900년도 기준 년도 return
console.log("objDate1.getFullYear : " + objDate1.getFullYear()); // 1000~9999 사이의 년도를 4자리로 return
console.log("objDate1.getMonth : " + objDate1.getMonth());
console.log("objDate1.getDate : " + objDate1.getDate()); // 날짜
console.log("objDate1.getDay : " + objDate1.getDay());
```

## 4. Array 
- sort : 정렬
    - String도 정렬 가능 (사전 순서대로 정렬한다)
```javascript
//sort 함수로 오름차순,내림차순으로 정렬 가능
var varArr = new Array(1, 6, 4, 3, 5, 2);

// 오름차순		
console.log("varArr.sort(a-b)_오름차순 : " + varArr.sort(function(a, b) {
	return a - b
}))

//내림차순
console.log("varArr.sort(b-a)_내림차순 : " + varArr.sort(function(a, b) {
	return b - a			
}))
```

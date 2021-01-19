# [용어] table에서 하나의 행을 recode 또는 row라고도 함 

# 문자 관련 함수 
### 1. upper : 전부 대문자로 바꿔주는 함수 
```sql
select 'welcome to oracle' "적용 전", upper('welcome to oracle') 'upper적용 후" from dual;
```

### 2. lower : 전부 소문자로 바꿔주는 함수 
```sql
select 'WELCOME TO ORACLE' "적용 전", lower('WELCOME TO ORACLE') "lower적용 후" from dual;
```

### 3. initcap : 해당 문자의 **첫 글자만 대문자로** 바꿔주는 함수
```sql 
select 'WELCOME TO ORACLE' "적용 전", initcap('WELCOME TO ORACLE') "적용 후" from dual;
```

### 4. length : 문자의 길이 
```sql
select length('ORACLE') "적용 전", length('오라클') from dual;
```

### 5. lengthb : b = byte 
- 한국어는 3byte, 영어는 1byte로 계산해서 값을 출력 
```sql
select lenghtb('ORACLE') "영어", lengthb('오라클') "한글" from dual;
========================================================================
▶ 6     9

[tip] 행에 대해 " " 이름 설정을 따로 하지않으면 ex) lengthb('오라클') 
→ 테이블 이름이 lengthb('오라클') 로 출력 , 첫 레코드의 값으로 출력
```

### 6. instr : 문자의 해당 문자가 몇번째에 있는지 알려주는 함수 **(특정 문자의 위치)**
```sql
select instr('Welcome to Oracle', 'o') from dual;
========================================================================
▶ 5
```

### 7. substr : 문자열에서 저장된 문자 위치를 기준으로 지정하는 숫자만큼 문자 출력
- 오라클에서는 인덱스가 0부터 시작되는 게 아니라 1부터 시작
```sql
select substr('Welcome to Oracle', '3', '4') from dual;
========================================================================
▶ lcom : 3번째에서 문자4개 출력 (공백도 count!)
```
```sql
select ename, 19 || substr(hiredate, 1, 2)년도, substr(hiredate, 4, 2)달 from emp;
```
<details><summary>출력</summary>
 
 ![1](https://user-images.githubusercontent.com/74290204/103961210-543b0e00-5197-11eb-99da-1e4dd5bb6bc1.PNG)
</details>

```sql 
--9월에 입사한 사원을 출력하는 쿼리문 
select ename from emp where subtr(hiredate, 4, 2) ='09';

※  '/' 도 위치 count! 
```

### 8. lpad : left-pad
```sql 
select lpad('oracle', 20, '#') from dual;
=======================================================================
▶ ##############oracle

-- : 20개의 공간을 마련한 후 oracle 문자로 공간을 채우고 oracle 왼쪽으로 나머지 공간을 #으로 채워라 
```

### 9. rpad : right-pad
```sql 
select rpad('oracle', 20, '#') from dual;
=======================================================================
▶ oracle##############

-- : 20개의 공간을 마련한 후 oracle 문자로 공간을 채우고 oracle 오른쪽으로 나머지 공간을 #으로 채워라 
```

### 10. trim : 잘라내는 함수(앞뒤에 공백을 제거한다)
```sql 
select trim(' ORACLE ') from dual;
=======================================================================
▶ ORACLE

select trim('aaaORACLEaaa') from dual;
=======================================================================
▶ ORACLE
```

# 날짜 관련 함수 
### 1. sysdate : 기본적인 날짜 함수 
```sql
select sysdate from dual;
```
- sysdate **+** 숫자 : 오늘을 기준으로 숫자만큼의 **과거 날짜** 출력 
- sysdate **-** 숫자 : 오늘을 기준으로 숫자만큼의 **미래 날짜** 출력 
```sql
select sysdate-1 어제, sysdate 오늘, sysdate+1 내일 from dual;
```

### 2. month_between(sysdate, 원하는 항목) : sysdate와 원하는 항목사이의 개월수를 구한다 
```sql 
-- 각 직원들이 근무한 개월 수를 구하는 쿼리문
select ename, sysdate, hiredate, month_between(sysdate, hiredate) 근무개월수 from emp;

-- 뒤에 소수점 날리고 싶을 때 
select ename, sysdate, hiredate, trunc(month_between(sysdate, hiredate)) 근무개월수 from emp;
```

### 3. add_months(원하는 항목, 숫자) : 개월 수를 더해서 출력
```sql 
select ename, hiredate, add_months(hiredate, 4) from emp;
```

### 4. next_day(sysdate, day) : 해당 날짜부터 시작하여 명시된 요일을 만나면 해당되는 날짜를 반환하는 함수 
```sql
--현재를 기준으로 가장 가까운 수요일을 찾는 쿼리문
select sysdate, next_day(sysdate, '수요일') from emp;
```

### 5. last_day : 해당 달의 마지막 날짜를 변환
```sql
select hiredate, last_day(hiredate) from emp;
```

# 형 변환 함수 : 숫자, 문자 ,날짜의 데이터형을 다른 데이터형으로 변환하는 함수

### 1. TO_NUMBER(number로~) : 숫자형으로 바꾸는 함수
```sql
select to_number('20,000', '99,999') to_number('10,000', '99,999') from dual;
-- '99,999'는 ,의 단위를 명시해주는 것(1000단위)

select '20,000'-'10,000' from dual -- 논리적 오류
☞ '10,000' 과 '20,000'은 문자형이기 때문에 연산을 하려면 문자형을 숫자형으로 변환한 다음에 연산을 실행해야한다 
(NUMBER형으로 바꿔야하는 이유는 연산때문!)
```

### 2. TO_DATE(date로~) : 문자형을 날짜형으로 변환하는 함수 
- 날짜형은 *세기, 연도, 월, 일, 시간, 분, 초*와 같은 날짜와 시간에 대한 정보를 저장한다 
- 기본 날짜 형식은 'YY/MM/DD' 형식으로 '년/월/일'
- 문자를 날짜로 바꾸는 이유는 TO_NUMBER와 마차간지로 연산을 위해서다
```sql
-- 며칠 지났는지 출력하는 쿼리문 
select trunc(sysdate-TO_DATE('2016/01/01', 'YYYY/MM/DD')) from dual;
``` 

### 3. TO_CHAR(char로~) : 날짜 or 숫자를 문자형으로 변환하는 함수 
```sql 
select sysdate, TO_CHAR(sysdate, 'yyyy-mm-dd') from dual;
▶ 'yyyyAAmmBBdd' : error 문자 중간에 삽입x (기호중에서도 되는게 있고 안되는 게 있음 → 해보면서 check해야함) 

-- 사원들의 입사일을 출력하되, 요일까지 함께 출력하는 쿼리문
select ename, hiredate, to_char(hiredate, 'yyyy/mm/dd day') from dual;

▶ 'day'를 사용하면 요일 출력 

-- 현재 날짜와 시간을 출력하는 쿼리문 
select to_char(sysdate, 'yyyy/mm/dd, hh24:mi:ss') from dual;

▶ hh24= 24hour / mi= minute / ss= second

-- 각 지역별 통화 기호를 앞에 붙이고 천 단위마다 콤마를 붙여서 출력하는 쿼리문 (예: ￦1,230,000)
select ename, sal, to_char(sal, 'L999,999') from emp;

▶ L = local / OS 기준으로 출력(한국이면 한국통화: ￦원 미국이면 미국통화로: $)
```

# 기타 함수 
### @ NVL 함수: null을 다른 값으로 변환하는 함수 
- null을 0 또는 다른 값으로 변환하기 위해서 사용하는 함수 
- 컬럼형이 숫자형이면 숫자형으로만 값 변경 가능 / 문자형이면 문자형으로 
```sql
select ename, sal, nvl(comm, 0), job from emp order by job;
```

#### Q. null을 공백으로 처리하고 싶다면?
1. 숫자형에서의 null을 문자형으로 바꿔서 출력(TO_CHAR)
```sql 
select ename, sal, nvl(to_char(comm), 'a'), job from emp;
```
2. 소프트웨어 프로그램으로 null처리

### @ DECODE 함수 : Oracle에서 DECODE함수는 **Switch-Case문과 같은 기능**
- 여러가지 경우에 대해서 선택할 수 있도록 하는 기능을 제공(선택을 위한 함수)
```sql
--사원 부서 번호를 이름으로 설정하는 쿼리문
select deptno, decode(deptno,10,'A',20,'B', 'default') from emp order by deptno;

▶ 부서 번호가 10이면 A , 20이면 B, 나머지 default

select deptno, decode(deptno, 10, 'ACCOUNTING', 20, 'RESEARCH', 30, 'SALES', 40, 'OPERATIONS') as dname from emp;
```

### @ CASE 함수 : 조건에 따라 서로 다른 처리가 가능한 함수 
- if else문과 유사한 구조
- 여러가지 경우에서 하나를 선택하는 함수 
```sql
select ename, deptno, 
case when deptno=10 then 'ACCOUNTING'
     when deptno=20 then 'RESEARCH'
     when deptno=30 then 'OPERATIONS' end as dname from emp;
-- 한줄로 써도되는데 보기 편하게 하려고 이렇게 적음~,~
```

#### decode함수와의 차이점!
☞ decode함수는 비교연산자 '=' 에 대해서만 적용! case함수는 다양한 비교연산자(<> , <= 등)가 가능하다

### @ ★ GROUP 함수 : 하나 이상의 컬럼을 그룹으로 묶어 연산해서 하나의 결과를 나타내는 함수
1. sum : 합계
```sql 
-- 사원들의 총 급여의 합계
select sum(sal) from emp;
```

2. count : 조건에 만족하는 행의 갯수를 반환 
- null 값에 대해서는 갯수를 count하지 않음 
```sql
-- emp안에 총 행(레코드)의 갯수
select count(*) from emp;

-- comm 받는 사원의 수 
select count(comm) from emp;

-- 중복 제거해서 담당업무의 갯수 구하기
select count(distinct job) from emp;
```

3. avg : 평균
```sql
-- 급여 평균
select avg(sal) from emp;
```

4. min, max : 최소값, 최댓값
```sql
-- 급여 최소, 최댓값
select min(sal), max(sal) from emp;
```

### @ ★ GROUP BY : 중복 제거(distinct와 역할은 동일)
- group을 지어 중복을 제거하는 함수(칼럼을 기준으로 동작함)
- group by를 하면 group 함수를 같이 사용할 수 있음 (이 목적으로 group by 를 많이 씀)
- group을 지어줄 기준에 대해서는 어떤 칼럼값을 기준으로 적용할지 기술해야함!!
```sql
select * from emp group by deptno; --에러! (기준을 맞춰서 명시해줘야함)
select deptno from emp group by deptno;

-- 부서별 평균 급여 
select deptno, avg(sal) from emp group by deptno;

-- 부서별 평균 급여와 총 급여 
select deptno, avg(sal), sum(sal) from emp group by deptno;

-- 여러개의 group by 쿼리문
select deptno, ename, avg(sal) from emp group by deptno; -- 에러! why? ename을 group by로 정해주지 않았기 때문
→ [수정] select deptno, ename, avg(sal) from emp group by deptno, ename;

▶ 수정해서 에러는 나지 않지만 의도대로 그룹이 정리되지 않는다
why? 여러개의 칼럼에 대해서 group by 를 사용할때는 더 큰 범위에 따라 그룹이 지어지기 때문 
(ename은 deptno보다 그룹지어질 갯수가 더 많음, 이름은 다 다르기 때문에)

-- 부서별 사원의 수와 커미션을 받는 사원의 수 
select deptno, count(*), count(comm) from emp group by deptno;
```

### @ having : group by에 대한 조건절
```sql
-- 부서의 급여 최댓값과 최소값을 구하되 최대 급여가 2900이상인 부서만 출력하는 쿼리문
select deptno, min(sal), max(sal) from emp group by deptno having max(sal) >= 2900;
```

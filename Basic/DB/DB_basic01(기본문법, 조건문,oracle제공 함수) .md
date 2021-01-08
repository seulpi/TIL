# Oracle DB : 관계형 DB의 대표적인 기업 

# DB(Data Base) : 서로 관련있는 data들의 모임
## @ RDBMS(Relation Data Base Management System)
- Relation : key를 통해 data들이 관계를 맺는 것을 의미 
- 관계를 맺는 data들을 관리하는 시스템 *RDBMS*

### 1. 기본 문법
```sql
--dept에 있는 모든 데이터들을 선택해서 출력
select * from dept;
```
![select](https://user-images.githubusercontent.com/74290204/103838893-9437bd80-50d1-11eb-834a-ead5ce0c3cdf.PNG)

```sql
--dept에 있는 dname만 출력
select dnmae from dept;
```
![select2](https://user-images.githubusercontent.com/74290204/103839752-75d2c180-50d3-11eb-8a79-508dcfb73aca.PNG)

### 2. 조건문
#### - where 
>> select * from emp where 조건; 
<br> → emp 안에서 출력하고 싶은 항목에 조건에 해당하는 것을 다 출력해라

```sql
-- 연봉 3000이하인 사람 출력
select * from emp where sal >= 3000;
```
![select3](https://user-images.githubusercontent.com/74290204/103840443-ecbc8a00-50d4-11eb-8b73-02d59b7fe435.PNG)

#### - 부정 NOT (조건문과 다를 경우)
1. < >
    ```sql
    --10번 부서가 아닌 사원 출력
    select * from emp whewe deptno <> 10;
    ```
2. !=
3. ^=

#### - AND , OR , NOT, BETWEEN~AND, IN 
1. AND : &&
    ```sql
    --job이 MANAGE이고 10번 부서인 사원 출력
    select * from emp where job = 'MANAGE' and deptno = 10;
    ```
    
2. OR : || 
    ```sql
    --job이 MANAGE이거나 10번 부서인 사원 출력
    select * from emp where job = 'MANAGE' or deptno = 10;
    ```
    - sql은 대소문자를 가리지 않지만 ' '안에서는 구별 
    
3. NOT : 조건문 아닌 경우에 사용
    ```sql
    --10번 부서가 아닌 사원 출력
    select * from emp where not deptno = 10;
    ```

4. BETTWEEN~AND : 조건문을 어느 범위안에서 줄 때 사용
    ```sql 
    --연봉 2000과 3000사이의 사원 출력
    select * from emp where sal between 2000 and 3000;
    ```

5. IN : 조건이 3개 이상일 때 사용
    ```sql
    --comm이 300 혹은 500 혹은 1400인 사원 출력
    select * from emp comm in(300, 500, 1400);

    --comm이 300 혹은 500 혹은 1400인 아닌 사원 출력
    select * from emp comm not in(300, 500, 1400);
    ```

![select4](https://user-images.githubusercontent.com/74290204/103840450-ededb700-50d4-11eb-9159-605b5f98f491.PNG)

#### - like : ~인 것 같은
```sql 
--이름이 F로 시작하는 사람 출력
select * from emp where ename like 'F%';

▶ % 는 *랑 같은 것 (F% : F로 시작하는 모든 사람)

--위치 상관없이 이름에 A가 들어가는 사람 출력
select * from emp where ename like '%A%';

--이름이 N으로 끝나는 사람 출력
select * from emp where ename like '%N';

--이름 두번째 자리에 A가 들어가는 사람 출력 
select * from emp where ename like '_A%';

--이름에 세번째 자리에 A가 들어가는 사람 출력 
select * from emp where ename like '__A%';

▶ % 와 _ 의 차이점 : 
%는 몇개의 문자가 오든 상관이 없지만 _는 단 한 문자에 대해서만 와일드 카드 역할을 한다
```

### 3. 주석  : --

### 4. Data Type : 데이터 타입은 크게 3가지로 나눈다
- NUMBER
- DATE : ' / / ' or ' . . ' 표기
```sql
'1982/01/01' or '1982.01.01'
```
- VARCHAR  : 문자, ' ' 로 표기(char 타입)
```sql
'안녕DB'
```

### 5. NULL 
: Oracle 에서는 Columnd에 null 값이 저장되는 것을 허용한다 
>> Column : 관계형 데이터베이스 테이블에서 특정한 단순 자료형의 일련의 데이터값과 테이블에서의 각 열을 말한다
- sql에서의 null은 **값을 저장하지 않는 것**을 말한다 (공백)

```sql 
select * from emp where comm = null;
→ 논리적 오류! 
why? null은 알 수 없는 값이고 null이 저장되어 있는 경우에는 연산자로 판단할 수 없기 때문에 오류라는 것

--null인 사람을 출력하고 싶다면?
select * from emp where comm is null;

--null이 아닌 사람을 출력하고 싶다면?
select * from emp where comm is not null;
select * from emp where not comm is null;
```

### 6. 정렬 : ORDER BY 
- 오름차순: default / asc 사용
```sql
select * from emp order by hiredate asc;
```

- 내림차순 : desc 사용 
```sql
select * from emp order by hiredate desc;
```

### 7. 중복 제거 : DISTINCT
```sql
select distinct deptno from emp;
```

### 8. 별칭(항목 이름 바꾸기) : AS
```sql
select ename as "이름" from emp;
select ename as 이름 from emp;
```
![별칭2](https://user-images.githubusercontent.com/74290204/103846731-20ea7780-50e2-11eb-9d4c-1db30e999702.PNG)

![별칭](https://user-images.githubusercontent.com/74290204/103846728-1e881d80-50e2-11eb-9cbe-2c5a222f7817.PNG)

```sql
select ename, sal*12 + nvl(comm, 0) "연봉" from emp;
``` 
![별칭3](https://user-images.githubusercontent.com/74290204/103846753-2cd63980-50e2-11eb-8fec-2d92cb592b7a.PNG)


### 9. 연결점 : || 'is a ||
>> ename || 'is a || job : 기존의 컬럼내에 문자열을 추가

```sql
select ename || 'is a' || job "연결점의 예" from emp;
```
![중복](https://user-images.githubusercontent.com/74290204/103847959-b129bc00-50e4-11eb-868e-8a1ebdf65b32.PNG)


### 10. 듀얼 (Dual) 
: 산술 연산 결과를 한 줄로 얻기 위해서 Oracle에서 제공하는 테이블 → dummy 테이블이라고도 함 
- 간단한 연산을 할 때 활용 
```sql 
select 24*60 from dual;
```
![듀얼](https://user-images.githubusercontent.com/74290204/103848528-f3073200-50e5-11eb-9959-477dc9285e7e.PNG)

```sql
select sysdate from dual; 
--sysdate: 날짜 
```
![듀얼2](https://user-images.githubusercontent.com/74290204/103848533-f4385f00-50e5-11eb-96fa-d17a9d3ca244.PNG)

## @ Oracle에서 제공하는 기본 함수 

### 1. abs : 절대값 만들어주는 함수
```sql
select -10, abs(-10) from dual;
```
![절대](https://user-images.githubusercontent.com/74290204/103848518-eb478d80-50e5-11eb-9cc2-d6f97655c04d.PNG)

### 2. floor : 소수점 버리는 함수 
```sql 
select 34.532, floor(34.532) from dual;
```

### 3. round : 반올림하는 함수 
```sql
select 34.532, round(34.532) from dual;

-- ▶ 35로 출력!
```
#### - round는 특정 자릿수에서 반올림 가능 
```sql
select 34.53267, round(34.53267 2) from dual;
-- ▶ 34. 53 출력! 소수점 2번째 자리에서 반올림

select 34.53267, round(34.53267 -1) from dual;
-- ▶ 30 출력! 1의 자리에서 반올림 

select 34.53267, round(34.53267 -2) from dual;
-- ▶ 0 출력! 10의 자리에서 반올림
```

### 4. trunc : 잘라내는 함수(버리는 함수)
```sql
select trunc(34.5678, 2), trunc(34.5678, -1), trunc(34.5678), trunc(34.5678, 0)from dual;
```
![trunc](https://user-images.githubusercontent.com/74290204/103848918-d7505b80-50e6-11eb-9cad-6c1c5ed8292f.PNG)

### 5. mod : 나머지를 나타내는 함수
```sql 
select mod(27, 2), mod(27, 5), mod(27, 7) from dual;
```
![mod](https://user-images.githubusercontent.com/74290204/103848915-d6b7c500-50e6-11eb-8b0f-81590325bd83.PNG)






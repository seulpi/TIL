# - 데이터 딕션너리 (정처기용)
- DB 자원을 효율적으로 관리하기 위한 다양한 정보를 저장하는 시스템 테이블
- 논리적, 물리적 데이터 베이스 설계시에 사용되는 용어들(Entity, columm)의 의미를 정의해놓은 문서
- 사용자는 필요한 정보 탐색용(select)로만 사용
```
[저장되는 내용들]
- 오라클의 사용자 정보
- 오라클 권한과 롤 정보
- 데이터베이스 스키마 객체(TABLE, VIEW, INDEX ..etc)
- 무결성 제약조선에 관한 정보
- 데이버 베이스의 구조 정보
- 오라틀 테이터베이스의 함수와 프로지저 및 트리거에 대한 정보
- 기타 일반적인 DATABASE 정보
```
```sql
-- 모든 테이블 검색
select * from all_all_tables 
```

# - SUB QUERY : select문 안에 select 
- 컬럼끼리 연산 가능
```sql
--사원들의 평균 급여보다 더 많은 급여를 받는 사원 출력하는 쿼리문
select ename, sal from emp where sal > (select avg(sal) from emp);

-- SMITH가 속한 부서의 이름
select dname from emp, dept where emp.deptno = dept.deptno
and emp.ename='SMITH';

select dname from dept where deptno = (select deptno from emp where ename='SMITH');

--연봉 3000이상 받는 사원이 소속된 부서와 동일부서에 근무하는 사원들의 정보를 출력하는 쿼리문
select ename, sal, deptno from emp where deptno in(select distinct deptno from emp where sal >= 3000);
```

## @ 단일행 쿼리 : '=' 로 한개로 답이 오는 것
## @ 다중행 쿼리 : 'in'으로 한개 이상으로 답이 오는 것 
- any : 조건이 일치하면 true인것만 출력
>> any (조건) - 조건중에 한개라도 부등호에 맞게되면 출력, 범위를 따지는 것
```sql
--부서 번호가 30번인 사원들의 급여 중 가장 낮은 값(950)보다 높은 급여를 받는 전체 사원의 이름, 급여를 출력하는 쿼리문
select ename, sal from emp where sal > (select min(sal) from emp where deptno=30);
select ename, sal from emp where sal > any (select sal from emp where deptno=30);
→ 30번에 해당하는 min값(최소값)을 뽑고 그 값에 대해서 sal비교하는거라서 전체 다 나와도 됨(뽑아내는건 30번 부서랑 관련 없음)
```

- all : 모든 값이 일치하면 true(한개라도 조건이 만족하지 않으면 출력하지 않음)
```sql
-- 각 부서의 최대 급여를 받는 사원의 부서코드, 이름, 급여를 출력하는데 부서코드 순으로 오름차순 정렬하여 출력하는 쿼리
select deptno, ename, sal from emp where sal in (select max(sal) from emp group by deptno) order by deptno asc;
▶ order by depno asc를 ( ) 안에 같이 넣었더니 에러남 결과문을 뽑고 정렬하는건 밖으로 빼서 하자
'='으로 하면 에러남

select deptno, enmae, sal from emp e1 where sal = (select max(sal) from emp e2 where e1.deptno =e2.deptno);

-- 이름의 T가 들어가는 사람이 속해있는 부서의 이름과 사원부서를 출력하는 쿼리문
select empno, ename from emp where deptno in (select deptno from emp where ename like '%T%') ;

-- manager가 KING인 사람의 모든 사원의 이름과 급여를 출력하는 쿼리문 
select ename, sal from emp where mgr = (select empno from emp where ename='KING');
```

- escape : ""(따옴표)를 쿼리문에 포함시켜 출력하는 방법 
```sql
1. '\'hello\'' 
2. ''hello'' 
```

# @ 데이터 무결성 : 기본키 제약조건에 걸리는 것 자체를 말함
-  데이터의 정확성과 일관성을 유지하고 보증하는 것
>> [pk확인하는 방법] 테이블 → 우클릭 → 편집 → pk 확인 가능 
- 쿼리문을 사용하지 않고 자동으로 pk설정이 가능하다

## - 제약 조건 

### 1. NOT NULL : NULL을 허용하지 않음
- Null값 입력금지, 해당 컬럼의 값 입력 필수

### 2. UNIQUE : 중복된 값을 허용하지 않고 항상 유일한 값을 갖도록 함
- Map의 key같은 존재 

### 3. PRIMARY KEY : NULL을 허용하지 않고 중복된값도 허용하지않음   
- *NOT NULL + UNIQUE*
- 테이블에 저장된 record 데이터를 고유하게 식별하기 위한 기본키
- 하나의 테이블에 하나의 기본키만 제약 가능

### 4. ★Foreign Key : 참조되는 테이블의 컬럼의 값이 존재하면 허용 
- 테이블 관계를 정의
- 다른쪽 테이블에 있는 컬럼을 갖고 오는 것(각각 다른 테이블을 서로 연결하는 데 사용되는 키)
- Key를 갖고오게 되면 테이블이 관계를 갖게 됨(=Relation이 생기면 부모자식간의 관계가 형성) 
>> 자식: 부모에 있는 Key를 가지고 오는 쪽 <br>
부모: Key를 갖고있는 쪽, 어디서 Key를 가지고 오지 않는다
```sql
-- 관계를 갖게되면 관계가 맺어지는 유형 3가지
* 1은 무조건 부모쪽
1. 1 : 1 
	▶ 1개에 대해서 1개
2. 1 : N
	▶ 1개에 대해서 여러개를 사용
	DEPT DEPTNO ='10' EMP DEPTNO='10' emp에서 여러명에 부서 번호 할당 (n은 세발낙지 모양기호)
	ex) 철수가 여러 수업을 수강한다
3. N : N 
	▶ 1개에 대해서 2개 테이블 이상인 여러개를 사용 
	(각각의 테이블이 1개의 테이블을 중점으로 각각 1 : N 관계를 형성)
	ex) 학원에서 여러개의 수업을 제공하고 학생은 여러개의 수업을 들을 수 있다
```

![forignKey](https://user-images.githubusercontent.com/74290204/104687827-4828f080-5743-11eb-93ea-6c3aa4dbd453.PNG)


### 5. CHECK : 저장 가능한 데이터 값의 범위나 조건을 지정하여 설정한 값만을 허용
- 입력할 수 있는 값의 범위를 제한

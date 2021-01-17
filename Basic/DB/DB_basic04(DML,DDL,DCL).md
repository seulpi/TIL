# DML : 데이터에 관한 명령어 
## - DEPT : 복사 
```sql
create table dept02 as select * from dept where 1=0;
-- dept의 테이블 구조를 dept02라는 이름으로 1개 복사
▶ 'where 1=0' 은 내용 복사 아닌 테이블 구조를 뜻함
 
create table emp01 as select * from emp;
-- emp의 내용 전체를 emp01이라는 이름으로 복사
▶ 내용까지 전체 복사 
```


## - INSERT : 추가
```sql
insert into dept02(deptno, dname, loc) values(10, 'ACCOUNTING', 'NEW YORK');
commit;
▶ 변경된 내용을 모두 영구 저장한다(커밋하기전에는 메모리에 있다가 commit을 하게 되면 그때 파일로 저장한다)
select * from dept02;

insert into dept02 values(20, 'RESEARCH', 'DALLAS');
-- 순서를 정해주지 않으면 적은 순대로 입력되서 저장
```

## - UPDATE : 수정
>> update set 
```sql
-- 모든 사원의 부서번호를 30으로 수정하는 쿼리문
update emp01 set deptno=30;

-- 사원의 급여를 10%인상하는 쿼리문 
update emp01 set sal = sal *1.1;

-- 모든 입사일을 오늘로 수정하는 쿼리문
update emp01 set hiredate = sysdate;

-- job 컬럼값이 MANAGER인 경우, 급여를 10% 인상하는 쿼리문
update emp01 set sal = sal * 1.1 where job = 'MANAGER';

--부서 번호가 10번인 사원의 부서 번호를 40번으로 수정하는 쿼리문
update emp01 set deptno=40 where deptno=10;
 
--1982년에 입사한 사원의 입사일을 오늘로수정하는 쿼리문
update emp01 set hiredate=sysdate where hiredate between '1982/01/01' and '1982/12/31';
update emp01 set hiredate=sysdate where substr(hiredate, 1, 2) = '82';

--SMITH 사원의 부서번호는 20번으로 job 컬럼값은 MANAGER로 한꺼번에 수정하는 쿼리문
update emp01 set deptno=20, job='MANAGER' where ename='SMITH';
```

## - DROP : TABLE 삭제
```sql
drop table emp01;
commit;
```

## - DELETE : DATA 삭제
```sql
delete from dept01;

--생성한 dept01부서 테이블의 30번 부서를 삭제하는 쿼리문
delete from dept01 where deptno=30;
```

# DDL(Data Definition Language) : 테이블 자체에 대한 명령어
- 테이블 구조 자체를 생성, 변경, 삭제

## @ CHAR(size) : 고정 길이 문자 
- size = 정해진 길이 
- 고정되어 있기 때문에 검색할 때 빠름

## @ VARCHAR2(size) : 가변 길이 문자
- size = 최대 크기
- 실제 입력된 문자열의 길이만큼 저장 영역을 차지
- 단점은 변하는 길이기 때문에 **검색할때 느림**

## @ NUMBER : 40자리 까지 숫자(양수만)
- 소수점이나 부호는 길이에 포함되지 X
### - NUMBER(w) : 최대 38자리까지
### - NUMBER(w, d) : 최대 38자리까지
- w는 전체 길이, d는 소수점 이하 자릿수 (소수점은 자릿수에 포함되지X)
```sql
number(5, 2);
-- 최대 정수자리 3자리, 소수자리 2자리를 입력받을 수 있는 숫자형 데이터 형식
```

## @ DATE
## @ LONG : 잘안씀, 문자형 데이터 타입
```sql
create table emp01 (
    empno number(4),
    ename varchar(20),
    sal number(7, 2)
);

desc empo1; 
-- 제대로 생성했는지 확인하는 방법 
```

### - ALTER : table의 update (추가)
>> alter table add
```sql
-- job 컬럼 추가 
alter table emp01 add(job varchar2(9));
```

### - ALTER MODIFY : table의 수정
>> alter table modify
```sql
-- 1. 해당 데이터의 자료가 있는 경우(이미 컬럼이 존재하는 경우)
alter table emp01 modify(job varchar2(30));

-- 2. 해당 데이터의 자료가 없는 경우(컬럼이 없는 경우)

```

### - ALTER DROP : 컬럼 삭제 
>> alter table drop
```sql
alter table drop column;
alter table emp01 drop column job;
```

### - DROP TABLE : 기존 TABLE 제거 (TABLE 전체 삭제)
```sql
drop table emp01; 
```

### ▶ DB는 저장 목적이 아니라 HISTORY 저장 목적 (=기존에 저장된 데이터를 구조화시키는 게 목적임) <br> 따라서, 실무에서는 테스트가 아닌 이상 **※꼭꼭 백업했는지 확인하고 drop을 실행해야한다 (3번이상 묻기)**


### - TRUNCATE : 데이터 내용 삭제 (RECORD 전체를 삭제)
```sql
drop table emp01; 
```

### ▶ *DROP, DELETE, TRUNCATE 의 차이점*
- delete : 내용 삭제 <br>
 데이터는 사라지지만 저장공간은 사라지지않는다 / **삭제후 되돌리기 가능(rollback OK)**
- truncate : 내용, 저장공간 삭제<br> 
(테이블을 살아있으나 용량이 줄어들고 **삭제후 되돌릴 수 없음**) 
- drop : **테이블 자체를 삭제** , 되돌리기 불가능


![delete,drop,trunk차이](https://user-images.githubusercontent.com/74290204/104752989-b9e05900-579a-11eb-9edc-c1f2adb7a286.png)

# DCL : 접근 관련 명령어(권한)

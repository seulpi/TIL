# JOIN
- 컬럼의 2개이상 테이블을 합쳐 결과를 나타낼때 사용

## @ Cartesian product : 테이블을 연결했을 때 나올 수 있는 모든 경우의 수를 나타낸다 
```sql
select * from emp, dept;
```

## @ Equi Join : 동등 join (공통적인 컬럼이 존재)
- join의 대상이 되는 테이블에서 공통적으로 존재하는 **컬럼의 값이 일치되는 행을 연결**하여 결과를 생성하는 join
```sql
--사원정보를 출력할때, 각 사원이 소속된 부서의 상세정보를 출력하기 위해 두개의 테이블을 조인하는 쿼리문
select * from emp, deptno where emp.deptno = dept.deaptno;

--이름이 KING인 사람의 부서명 출력하는 쿼리문
select emp.enmae, dept.dname from emp, dept where emp.deptno = dept.deptno and emp.ename = 'KING';

select dept.dname from emp, dept where emp.ename = 'KING' 
→ 잘못된 코딩 why? 중복가려내는 조건문이 없기 때문에 모든 경우의 수를 다 출력해냄
```

## @ Non-Equi Join : 비등가 조인
- **'='가 아닌 다른 조건으로 연산자를 수행**하는 join , 예를 들어 부등호/ between ~ and, is null, is not null, in 등등 
```sql
--각 사원의 급여가 몇 등급인지 살펴보는 쿼리문
1) select ename, sal, grade from emp, salgrade where emp.sal >= salgrade.losal and emp.sal <= salgrade.j=hisal;

2) select ename, sal, grade from emp, salgrade where emp.sal between salgrade.losal and slagrade.hisal;
```

## @ Self Join : 자기 자신을 join 
- 동일한 테이블을 연결하는 방식
```sql
--각 사원의 상사이름을　출력하는　쿼리문　
select E.ename, M.ename from emp E, emp M where E.mgr = M.empno;
```

## INNER JOIN  (교집합)
## OUTER JOIN : join 조건에 만족하지 않는 행도 나타내는 방법
- 값이 없더라도 컬럼이 있기 때문에 그 컬럼을 빠뜨리지 않고 출력하기 위한 방법
- 한쪽 테이블에 해당하는 데이터가 존재 but 다른쪽 테이블에는 데이터가 존재하지 않을 경우 <br> 
→ 데이터가 출력되지 않는다 (INNER JOIN이 아닌 부분들) 따라서 그러한 문제를 해결하기 위해 나온 방법
### 따라서 **OUTER JOIN = "교집합 포함한 자기자신의 영역까지"**
>> [@문법] 포함할 영역에 ' + ' 추가
```sql 
select e.ename || '의 매니저는 ' || m.dname || '입니다.'
from emp e, emp m where e.mgr = m.empno(+);
```


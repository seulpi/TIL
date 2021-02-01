# 1. emp list 페이징을 구현하시오


# 2. 오라클 11g 이하에서의 페이징 처리 방법(DB)은?
- rownum을 활용해 pasing 처리 
- select가 3중으로 처리되서 '3중세트'라고도 함
```sql
select rownum rn, a.* from emp a; 
-- a.라고 표시를 해주지않으면 어디에서 all인지 모르기 때문에 에러가난다 (어디에서 컬럼을 가져올지 꼭 명시해줘야함)
```
<br>

# 3. rownum 에 대하여 설명하시오
- 해당 컬럼들에 대한 **행번호를 부여**해주는 요소 
- 오라클 전용 (mySql같은 경우는 limit를 사용해 페이징처리를 한다)

## @ (*)rownum의 처리 순서
```sql
[sql 처리 순서]

1. from , where절 처리 

>> rownum처리 (select 처리되기 전 where절의 조건까지 확인 후 번호를 할당한다) 

2. select 처리 
3. group by 조건 적용
4. having 적용
5. order by 적용
```


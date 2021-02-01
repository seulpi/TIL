# 1. Spring에서의 정적 리소스 처리 방법에 대해 설명하시오

# 2. 아래와 같이 페이징을 구현하시오
![q](https://user-images.githubusercontent.com/74290204/106420093-44180500-649d-11eb-947a-197d410f8284.PNG)

```sql
--아래는 테스트 항목을 넣기 위한 문장입니다
begin for i in 1..1000 loop 
insert into mvc_board(bId, bName, bTitle, bContent, bHit, bGroup, bStep, bIndent) 
values (mvc_board_seq.nextval, 'test', '테스트', '테스트', 0, mvc_board_seq.currval, 0, 0);
end loop;
end;

commit;

select count(*) from mvc_board;
```

# EXERD : DB 설계 툴
## @ 설치
- help - install new software -> work with : https://exerd.com/update/ 

![exerd설치](https://user-images.githubusercontent.com/74290204/107177762-f60f7e00-6a15-11eb-8c8b-9db3a5ece153.PNG)

![exerd설치2](https://user-images.githubusercontent.com/74290204/107177763-f7d94180-6a15-11eb-8b9e-5b6c1891d3c7.PNG)


## @ 파일 생성 : 우클릭 - new - other - exerd

![exerd생성](https://user-images.githubusercontent.com/74290204/107323363-79e66a80-6ae9-11eb-9c67-0673b4492a27.PNG)

![exerd생성1](https://user-images.githubusercontent.com/74290204/107323366-7b179780-6ae9-11eb-930d-51299dda4cb0.PNG)

![exerd생성11](https://user-images.githubusercontent.com/74290204/107323367-7bb02e00-6ae9-11eb-9ae3-6e2ea32163dc.PNG)

```
1. 테이블 선택 후 space바 → 속성

- 논리이름: 논리적설계 
- 물리이름 : 오라클에 들어가는 이름 (테이블명은 소문자로~)

2. 우클릭 - 새로만들기 - 컬럼
- pk일 경우에는 id로 지정 (다른 쪽 fk 들어갈때는 테이블명도 포함해서 기재)

3. 더블 클릭 하면 N.N로 변경가능(NotNull)
- 프로젝트할때는 scott .tiger로 하면안되고 계정하나 생성해서 할 것!

```

![pk설정](https://user-images.githubusercontent.com/74290204/107323277-56bbbb00-6ae9-11eb-9084-46ad55ae7ec5.PNG)


## @ 식별&비식별 

- 점선 표시선 : 비식별
- 점선 X : 식별

![점선](https://user-images.githubusercontent.com/74290204/107323304-63401380-6ae9-11eb-8bad-64578bc5ecb9.PNG)

```
- 회원아이디가 게시판에서 기본키로 들어갈때 : 식별
- 회원아이디가 게시판에서 일반 컬럼으로들어갈때(주키가X) : 비식별
```

![식별 비식별](https://user-images.githubusercontent.com/74290204/107323325-6b984e80-6ae9-11eb-8054-f77472bf66d8.PNG)

▶ 글번호로 식별하기 때문에 회원아이디는 일반 fk로 들어가면된다 

## @ 리버스&포워딩 엔지니어링 

- 리버스 엔지니어링 : DB에있는 테이블 가져오기
- 포워딩 엔지니어링 : 테이블 DB에 넣기
- jdbc 드라이버 : C:\oraclexe\app\oracle\product\11.2.0\server\jdbc\lib 에서 ojdbc6_g.jar

![exerd포워딩1](https://user-images.githubusercontent.com/74290204/107323223-3a1f8300-6ae9-11eb-8137-f951d377e158.PNG)

![exerd포워딩2](https://user-images.githubusercontent.com/74290204/107323229-3be94680-6ae9-11eb-886a-e0c5d2347c67.PNG)

![exerd포워딩3](https://user-images.githubusercontent.com/74290204/107323230-3c81dd00-6ae9-11eb-8935-6f22608d8a8d.PNG)

![exerd포워딩4](https://user-images.githubusercontent.com/74290204/107323232-3c81dd00-6ae9-11eb-94b6-3eca1c88d176.PNG)

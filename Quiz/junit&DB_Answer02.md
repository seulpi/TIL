# 1. 마이바티스 사용 4가지 방법에 대하여 설명하시오.
# 2. ajax+json으로 list를 뿌리시오.
# 3. 아래를 설계하시오 
```
1.  DVD를 대여하기 위해서는 회원 가입을 해야 한다. 회원 가입 시에는 회원에 대한 이름, 주
   민번호, 전화번호, 핸드폰번호, 이메일, 우편번호, 주소, 등록일 등을 기록해 둔다. 가입자는 
   관심 장르를 설정하여 신 프로 입고 시 이메일을 발송한다.
2.  DVD에 대한 정보는 영화제목, 제작사, 주연배우, 감독, 영화출시일 등의 상세 정보를 관리
   해야 하며 각 DVD에 대한 파손 여부와 대여 여부 등의 상태정보도 관리되어야 한다.
3.  DVD는 장르로 구분해서 체계적으로 관리된다.
4.  대여는 신 프로의 경우에는 대여일이 기본 1일이며 구 프로인 경우에는 2일이 기본 대여일
    이며 대여료는 신 프로의 경우에는 2000원이며 구 프로인 경우에는 대여료가 1000원이다.
5.  미납회원 관리와 함께 연체되었을 경우 하루에 500원 연체료가 누적된다. DVD가 파손되
    거나 망실된 경우 해당 회원은 DVD 가격을 변상해야 한다. 
6.  회원이 DVD를 대여할 경우 대여 1회당 1점씩의 포인트 점수를 부여하고 포인트 점수가 
    10점이 되면 무료로 DVD 하나를 대여할 수 있도록 포인트제를 운영하고자 한다.
7.  DVD 대여점 운영자는 금전상의 관리를 위해서 일별, 월별 매출액과 DVD가 대여된 횟수를 
    알고 싶어 한다.
```

## 초기설계
![KakaoTalk_20210204_162017809](https://user-images.githubusercontent.com/74290204/106882567-40e17b00-6722-11eb-9b08-2c4e808ae298.jpg)
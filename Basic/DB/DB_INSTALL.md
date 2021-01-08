# DB Oracle 설치 
 - 경로 바꾸지 않고 그대로 설치
 - 비밀번호는 잊어버리지 않는 것으로 설정(학원에선 1234로 통일함)

 ![DBsqldeveloper](https://user-images.githubusercontent.com/74290204/103732648-2933ab80-502b-11eb-95e5-4416614ece57.PNG)

 ![jdk_설치경로](https://user-images.githubusercontent.com/74290204/103732651-2a64d880-502b-11eb-92be-12c7cd302886.PNG)

# SqleDeveloper 설치

1. +를  선택해서 Name:system / 사용자이름:system / 비밀번호1234(처음 실행할 때 작성했던 비밀번호) <br> 
→ 비밀번호 저장 클릭, 테스트로 성공 뜨면 접속이나 저장

2. 워크시트에 *@C:\oraclexe\app\oracle\product\11.2.0\server\rdbms\admin\scott.sql* 입력 <br> 
→ 실행(CONNECT 스크립트 명령으로 생성된 접속이 해제되었습니다.출력)  → system-다른사용자-SCOTT 있는지 확인

3. SCOTT확인하면 워크시트에 *@C:\oraclexe\app\oracle\product\11.2.0\server\rdbms\admin\scott.sql* delete <br> 
→ *alter user scott identified  by  tiger;*  입력하고 실행 [스크립트 출력: User SCOTT이(가) 변경되었습니다.]

4. system워크시트 창 *alter user  scott identified  by  tiger;*  입력 후 실행 → 다시 + <br> 
→ Name:scott / 사용자이름:scott / tiger [*alter user  scott identified  by  tiger;*는 scott을 tiger로 비밀번호로 설정한것]  <br>
→ 테스트: 성공  → 접속

5. scott 창에 *select * from emp;* 쳐서 잘 도는지 확인

☞ 설치하는 영상 

# SqleDeveloper와 이클립스에서 불러오기
- 컴퓨터 / 로컬디스크(C:) / oraclexe / app / oracle / product / 11.2.0 / server /  jdbc / lib
![경로](https://user-images.githubusercontent.com/74290204/103959948-6bc4c780-5194-11eb-8834-95fffc5ba601.PNG)

- 복사한 파일을 이클립스에 프로젝트에 붙여넣기
![붙여넣을 경로](https://user-images.githubusercontent.com/74290204/103959981-8139f180-5194-11eb-98b2-a5692c5312a8.PNG)

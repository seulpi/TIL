# 변수와 데이터타입(형)
* 변수 : 변하는 수 
* 변수에는 정수와 실수가 존재
    * 실수에는 오차가 존재
    ```java 
    double num1, num2;
    double result;
    num1 = 1.0000001;
    num2 = 2.0000001;
    result = num1 + num2;
    System.out.println(result); 

    3.0000001999999997
    ```
* 아스키코드(American Standard Code for Information Interchange) 
    * 아스키코드란 CPU가 문자를 그대로 읽을 수 없기 때문에 문자에 번호를 매겨 '약속'한 표준코드 / 문자와 숫자의 일대일 매칭 <br> ex) 'A' - 65 '1' - 39
    * blank , 기호 , 숫자 모두 문자에 속한다
    * 문자  → 숫자 : 인코딩 <br> 숫자 → 문자 : 디코딩 

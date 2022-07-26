# Properties 
### : HashMap의 구버전이 HashTable을 상속받아 구현한 것으로 HashTable보다 단순화해 구현한 컬렉션 클래스 <br>
-> HashTable(Object, Object) - Properties(String, String)
-> 주로 환경설정과 관련된 속성을 저장하는데 사용<br>
-> 데이터를 파일로부터 읽고 쓰는 편리한 기능을 제공(간단한 입출력에서 사용하면 코드를 간결하게 사용이 가능)
### @함수
- Preoperties() : Properties 객체 생성
- getProperty(String key) : 지정된 key의 값(value)을 반환
- setProperty(String key, String value) : 지정된 key와 값을 저장 / 이미 존재하는 키 -> 새로운 value로 변경
- list(PrintStream out) : 지정된 PrintStream에 저장된 목록 출력
- list(PrintWrite out) : 지정된 PrintWrite에 저장된 목록 출력
- load(InputStream inStream) : 지정된 InputStram으로부터 목록을 읽어서 저장
- load(Reader reader) : 지정된 Reader로부터 목록을 읽어서 저장
- loadFromXML(InputStream in) : 지정된 InputStream으로부터 XML 문서를 읽고 XML 문서에 저장된 목록을 읽어서 담는다 (load & store)
- propertyNames : 목록의 모든 key기 담긴 Enumeration을 반환
- store(Writer writer, String comments) : 저장된 목록을 지정된 Writer에 출력(저장)한다. comments는 목록에 대한 주석으로 저장 
- storeToXML(OutputStream os, string comment) : 저장된 목록을 지정된 출력스트림에 XML문서로 출력(저장), comment는 목록에 대한 설명(주석)으로 저장
- stringPropertyNamse : Propertyies에 저장되어 있는 모든 키를 Set에 담아 반환

```java
Properties prop = new Properties();
```

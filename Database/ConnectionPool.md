## 🥕 DB Connection

- DB를 사용하기 위해 DB와 애플리케이션 간 통신을 할 수 있는 수단
- DB Connection은 Database Driver와 Database 연결 정보를 담은 URL이 필요함
- JAVA의 DB Connection은 JDBC를 주로 이용하는데, URL 타입을 사용함

![image](https://github.com/sengmin14/CS-Study/assets/140876841/584e151f-8193-4baa-9b53-8f961a265720)

### DB Connection 구조
- 2Tier - 클라이언트로서의 자바 프로그램(JSP)이 직접 데이터베이스 서버로 접근하여 데이터를 엑섹스하는 구조
- 3Tier - 자바 프로그램과 데이터베이스 서버 중간에 미들웨어 층을 두어, 그 미들웨어 층에서 비즈니스 로직 구현부터 트랜잭션 처리, 리소스 관리 등을 전부 맡기는 구조이다.

### JDBC
- 자바 언어로 다양한 종류의 관계형 데이터베이스에 접속하고 SQL문을 수행하여 처리하고자 할 때 사용되는 표준 SQL 인터페이스 API이다.
- 원라래면 DB마다 연결 방식과 통신 규격이 따로 있기 때문에 프로그램을 DB와 연결한다면, 해당 DB와 관련된 기술적 내용을 배우고 DB가 번결될 시 많은 변경 사항이 존재한다.
- 하지만 각 DBMS에 맞는 JDBC를 받아주게 되면 쉽게 DBMS를 변결할 수 있게 된다.
- 즉 DBMS의 종류에 상관 없이 하나의 JDBC API를 사용해서 데이터베이스 작업을 처리할 수 있게 된다.
- 자바 애플리케이션에서 데이터베이스에 접근하기 위해서는 JDBC API를 이용해서 데이터베이스에 접근하고, JDBC API는 JDBC드라이버를 거쳐 데이터베이스와 통신을 한다.
  
![image](https://github.com/sengmin14/CS-Study/assets/140876841/cf83fb5e-3d2e-4e8c-ab8b-b9895c94c798)

### JDBC 드라이버
- 자바 프로그램의 요청을 DBMS가 이해할 수 있는 프로토콜로 변환해주는 클라이언트 사이드 어댑터이다.
- 각각의 DBMS는 자신에게 알맞은 JDBC드라이버를 제공한다.

### JDBC 실행과정
![image](https://github.com/sengmin14/CS-Study/assets/140876841/6f70ecd8-940d-4036-85fe-c46fe79d6aaa)

- DB 벤더에 맞는 드라이버 로드
- DB 서버의 IP, ID, PW등을 DriveManager 클래스의 getConnection()메소드를 사용하여 Connection객체 생성
- Connection에서 PreparedStatement 객체를 받음
- executeQuery를 수행하고 ResultSet 객체를 받아 데이터를 처리
- 사용하였던 ResultSet, PreparedStatement, Connection을 close
























# 🥕 DBCP(DataBase Connection Pool)
```
웹 컨테이너(WAS)가 실행되면서 DB와 미리 connection(연결)을 해놓은 객체들을 pool에 저장해두었다가,
클라이언트 요청이 오면 connection을 빌려주고, 처리가 끝나면 실행된 상태로 다시 connection을 반납받아 pool에 저장하는 방식
```
## DBCP를 사용하는 이유
- 자바에서는 DB에 직접 연결해서 처리하는 경우(JDBC) 드라이버(Driver)를 로드 하고 컨넥션(connection) 객체를 받아와야 한다.
- 그러면 매번 사용자가 요청을 할 떄마다 드라이버를 로드하고 컨넥션 객체를 생성하여 연결하고 종료하기 때문에 비효율적이다.
- 이런 문제를 해결하기 위해서 DBCP를 통해 이미 연결하는 작업을 pool에 있기 때문에 그것을 재사용하는 것이다.

![image](https://github.com/sengmin14/CS-Study/assets/140876841/4d790283-dc38-4098-b5f0-924c9d139ca8)

- 웹 애플리케이션에서는 HTTP 요청에 따라 Thread를 생성하게 되고 대부분의 비지니스로직은 DB 서버로 부터 데이터를 얻게 된다.
- 만약 위와 같이 모든 요청에 대해 DB접속을 위한 Driver를 로드하고 Connection 객체를 생성하여 연결한다면 물리적으로 DB 서버에 지속적으로 접근해야 될 것이다.
- DBCP를 이용하면 객체를 생성하는 부분에 대한 비용과 대기 시간을 줄일 수 있다.

``` java
// JDBC Connection + psmt + rs 설정 예제
// DriverManager
Connection conn = null;
PreparedStatement  pstmt = null;
ResultSet rs = null;

try {
    sql = "SELECT * FROM T_BOARD"

    // 1. 드라이버 연결 DB 커넥션 객체를 얻음
    connection = DriverManager.getConnection(DBURL, DBUSER, DBPASSWORD);

    // 2. 쿼리 수행을 위한 PreparedStatement 객체 생성
    pstmt = conn.createStatement();

    // 3. executeQuery: 쿼리 실행 후
    // ResultSet: DB 레코드 ResultSet에 객체에 담김
    rs = pstmt.executeQuery(sql);
    } catch (Exception e) {
    	log.error("e 발생", e.getMessage())
    } finally {
        conn.close();
        pstmt.close();
        rs.close();
    }
}
```

## 커넥션 풀이 필요한 이유
![image](https://github.com/sengmin14/CS-Study/assets/140876841/74e4201d-b4c0-4fb5-9247-f1c86fa7bd68)

### 데이터베이스 컨넥션을 획득할 때는 일반적으로 다음과 같은 복잡한 과정을 거친다.
```
1. 애플리케이션 로직은 DB 드라이버를 통해 커넥션을 조회한다.
2. DB 드라이버는 DB와 TCP/IP 커넥션을 연결한다. 물론 이 과정에서 3 way handshake 같은 TCP/IP
연결을 위한 네트워크 동작이 발생한다.
3. DB 드라이버는 TCP/IP 커넥션이 연결되면 ID, PW와 기타 부가정보를 DB에 전달한다.
4. DB는 ID, PW를 통해 내부 인증을 완료하고, 내부에 DB 세션을 생성한다.
5. DB는 커넥션 생성이 완료되었다는 응답을 보낸다.
6. DB 드라이버는 커넥션 객체를 생성해서 클라이언트에 반환한다.
```

- 이렇게 커넥션을 새로 만드는 것은 과정도 복잡하고 시간도 많이 많이 소모되는 일이다.
- DB는 물론이고 애플리케이션 서버에서도 TCP/IP 커넥션을 새로 생성하기 위한 리소스를 매번 사용해야 한다.
- 진짜 문제는 고객이 애플리케이션을 사용할 때, SQL을 실행하는 시간 뿐만 아니라 커넥션을 새로 만드는 시간이 추가되기 때문에 결과적으로 응답 속도에 영향을 준다. 이것은 사용자에게 좋지 않은 경험을 줄 수 있다.

## 커넥션 풀(DBCP) 과정
1) 웹 컨테이너(WAS)가 실행되면서 connection 객체를 미리 pool에 생성해 둔다.
2) HTTP 요청에 따라 pool에서 connection객체를 가져다 쓰고 반환한다.
3) 이와 같은 방식으로 물리적인 데이터베이스 connection(연결) 부하를 줄이고 연결을 관리 한다.
4) pool에 미리 connection이 생성되어 있기 때문에 connection을 생성하는 데 드는 요청마다 연결 시간이 소비되지 않는다.
5) connection을 계속해서 재사용하기 때문에 생성되는 커넥션 수를 제한적으로 설정함

![image](https://github.com/sengmin14/CS-Study/assets/140876841/0799bd3f-3fd9-4ca1-bd75-c798b278ffa3)

### 스프링 부트의 커넥션을 획득하는 방법을 추상화 -> 인터페이스
![image](https://github.com/sengmin14/CS-Study/assets/140876841/cf86b84f-dde1-436a-9cc2-3dbec61ff658)

- 자바에서는 이런 문제를 해결하기 위해 javax.sql.DataSource 라는 인터페이스를 제공한다.
- DataSource는 커넥션을 획득하는 방법을 추상화 하는 인터페이스이다.
- 이 인터페이스의 핵심 기능은 커넥션 조회 하나이다. (다른 일부 기능도 있지만 크게 중요하지 않다.)

### DataSource
``` java
public interface DataSource {
Connection getConnection() throws SQLException;
}
```

```
정리)
대부분의 커넥션 풀은 DataSource 인터페이스를 이미 구현해두었다. 따라서 개발자는 DBCP2 커넥션 풀 ,
HikariCP 커넥션 풀 의 코드를 직접 의존하는 것이 아니라 DataSource 인터페이스에만 의존하도록 애플
리케이션 로직을 작성하면 된다.
```

### 동시 접속자가 많으면?
- 동시 접속 할 경우 pool에서 미리 생성 된 connection을 제공하고 없을 경우는 사용자는 connection이 반환될 때까지 번호순대로 대기상태로 기다린다.

- 여기서 WAS에서 커넥션 풀을 크게 설정하면 메모리 소모가 큰 대신 많은 사용자가 대기시간이 줄어들고, 반대로 커넥션 풀을 적게 설정하면 그 만큼 대기시간이 길어진다.


### DBCP의 종류
- 커넥션 풀에는 Apache의 Commons DBCP와 Tomcat-JDBC, BoneCP, HikariCP 등이 있는데, 우리는 이미 스프링 웹 프로젝트에서 자신도 모르게 사용하고 있을 수 있다.
- Commons DBCP, HikariCP 같은 DBCP가 있는데, 스프링 같은 경우 스프링부트2.0부터 기본적으로 HikariCP 를 사용한다.
- 참고) WAS가 실행 될 때 애플리케이션에서는 Connection Pool 라이브러리를 통해 Connection Pool 구현체를 사용할 수가 있는데, Apache의 Commons DBCP가 오픈소스 라이브러리로 제공되고 있다.

### WAS의 Thread 수와 Connection Pool 수의 관계
- WAS에서 설정해야 하는 값이 굉장히 많지만, 그 중 가장 성능에 많은 영향을 주는 부분은 Thread와 Connection Pool의 개수 이다.
- 이들 값은 직접적으로 메모리와 관련이 있기 때문에, 많이 사용하면 할 수록 메모리를 많이 점유하게 된다. 그렇다고 반대로 메모리를 위해 적게 지정한다면, 서버에서는 많은 요청을 처리하지 못하고 대기 할 수 밖에 없다.

### DB Connection Pool 관리 어떻게 하면 좋을까?
- 가장 좋은 방법은 애플리케이션을 실제 운영할 시스템 환경에서 성능 테스트를 진행하는 것이다.
- 성능 테스트를 진행하면서 지금까지 살펴본 DBCP에 대한 원리와 설정을 위한 값들을 복기하면서 시스템 환경에 최적화된 값을 찾아 내는 것이 좋겠다.

```
실제 운영중인 서비스에서 DBCP 값이 200에 가까운 수치가 설정되어 있어, 문제가 발생된 경우를 보았다.
무엇보다, WAS Thread 수는 DB Connection Pool의 갯수보다 더 적게 설정 되어 있었는데, 이러한 점을 효율적이지 못하다.
```


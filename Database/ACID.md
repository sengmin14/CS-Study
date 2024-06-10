## **트랜잭션이란?**

- 단일한 논리적인 작업 단위(a single logical unit if work)
- 논리적인 이유로 여러 SQL문들을 단일 작업으로 묶어서 나눠질 수 없게 만든 것이 transaction이다.
- transaction의 SQL문들 중에 일부만 실패해도 DB에 반영되는 일은 일어나지 않는다.
    
    ```sql
    START TRANSACTION // AutoCommit이 자동 비활성화 된다.
        -- 이 블록안의 명령어들은 마치 하나의 명령어 처럼 처리됨
        -- 성공하던지, 다 실패하던지 둘중 하나가 됨.
        A의 계좌로부터 인출;
        B의 계좌로 입금;
    COMMIT // 지금까지 작업한 내용을 DB에 영구적으로 저장 // transaction 종료
    // ROLLBACK // 지금까지 작업들을 모두 취소하고 transaction 이전 상태로 되돌린다.// 종료
    ```
    

## **트랜잭션 ACID란?**

### **✔️ 개념**

- 트랜잭션이 **어떤 속성을 지녀야 하는지** 나타내는 핵심!!!
- 트랜잭션은 ACID라 규칙을 모두 보장해 [데이터베이스 트랜잭션](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4_%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98)이 안전하게 수행되어야 한다.
- 원자성(**A**tomicity), 일관성(**C**onsistency), 격리성(**I**solation), 지속성(**D**urability)의 앞글자를 딴 규칙

!https://blog.kakaocdn.net/dn/bWKdQQ/btsC4cCwruw/vGu7670Gi9BnF6xLs2K5Yk/img.png

이미지 출처:&nbsp;https://programmingsharing.com/acid-properties-of-database-transactions-60b5804c4991

---

### **✔️ 원자성(Atomicity)**

- ALL OR NOTHING
- 트랜잭션은 **`모두 실행`**하거나 **`모두 실행하지 않음`**의 **두 가지 상태만**

트랜잭션은 논리적으로 쪼개질 수 없는 작업 단위이기 때문에 내부의 SQL문들이 모두 성공하고나, 중간에 어떤 하나의 SQL문이라도 실패하면, 지금까지 작업을 모두 취소해 이전 상태로 롤백해야 한다.

즉, **일부만 성공하는 상태는 존재해서는 안된다.**

성공 시 커밋했을 때 DB에 영구적으로 저장하는 것과 실패 시 롤백했을 때 이전 상태로 되돌리는 것은 DBMS가 담당하는 부분이다.

개발자는 **언제 커밋하고 언제 롤백할 지에 대한 기준을 세우고 코드를 작성**해야 한다.

### **✔️ 일관성(Consistency)**

- 트랜잭션 이전과 이후에 **데이터베이스는 항상 consistent한 상태여야 한다**는 규칙이다.
    - 트랜잭션은 DB 상태를 **consistent** 상태에서 또 다른 **consistent** 상태로 가야 한다.

만약에, constraints, trigger 등을 통해서 DB에 정의된 규칙을 트랜잭션이 위반했다면 롤백해야 한다.

예를 들어, 계좌에 잔액을 업데이트하는 트랜잭션 결과로 잔액이 마이너스로 가버렸다면 그냥 롤백해야 한다.

트랜잭션이 DB에 정의된 규칙을 위반했는지는 **DBMS가 커밋 전에 확인하고 알려준다.**

그렇다고 DBMS를 맹신하기보다는, ***애플리케이션 관점에서 트랜잭션이 consistent하게 동작하는지는 개발자도 챙겨야 한다.***

### **✔️ 고립성(Isolation)**

- 여러 트랜잭션이 동시에 실행될 때에도, 혼자 실행되는 것처럼 동작하게 만들어야 한다.
    - 즉, 동시에 실행되는 여러 트랜잭션은 서로 영향을 주지 않고 **독립적으로 실행되는 것처럼 보여야** 한다.
- DBMS는 여러 종류의 isolation level을 제공하고, 개발자는 이중에서 어떤 레벨로 트랜잭션을 동작시킬지 설정할 수 있다.
- concurrency control의 목표는 isolation이다.

!https://blog.kakaocdn.net/dn/MxomQ/btsC4syoea4/UMXeENATkejK5ZFrFxpBcK/img.png

출처:&nbsp;https://www.youtube.com/watch?v=sLJ8ypeHGlM&list=PLcXyemr8ZeoREWGhhZi5FZs6cvymjIBVe&index=15

여러 트랜잭션이 동시에 실행되었을 때, 결과가 이상해지는 문제가 발생할 수 있다.

### **✔️ 영속성(Durability)**

- 커밋된 트랜잭션의 결과는 데이터베이스에 **영구적으로 저장**되어야 한다.
    - '영구적 저장' == '비휘발성 메모리(HDD, SSD 등)에 저장'
- DB System에 전원이 꺼진다던가 뿌서진다던가 등등 문제가 생겨도 커밋된 결과는 계속 데이터베이스에 남아있어야 한다.
- 개발자가 할 수 있는 건 별로 없고, durability는 DBMS가 보장하니깐 믿고 간다.

# 🥕 Replication
```
두 개 이상의 DBMS 이용하여 Master/Slave구조를 활용하여 DB의 부하를 분산 시키는 기술
```
![image](https://github.com/user-attachments/assets/f1cafa70-51ea-4ed2-a211-acdbcb3d77ab)

- Replication은 Master DB에는 Insert, Update, Delete 작업을 수행
- Slave DB에는 Select 작업을 수행
- MySQL은 Replication을 바이너리 로그를 통하여 Replication이 이루어진다.
```
Select만을 분리하는 이유는 Select의 작업 시간이 많이 걸리기 때문
Table Full Scan을 하는 동안 다른 작업을 하지 못하게 되니 병목 현상 발생
```

## 바이너리 로그
- MySQL 서버에서 Create, Alter, Drop과 같은 작업을 수행하면 MySQL은 그 변화된 이벤트를 기록
- 변경사항들에 대한 정보를 담고 있는 이진 파일을 바이너리 로그 파일이라고 한다.
- 이때 show나 select와 같은 조회 문법은 제외

## Replication 동작 원리
- MySQL의 Replication은 기본적으로 비동기 복제 방식을 사용
- Master노드에서 변경되는 데이터에 대한 이력을 로그에 기록
- Replication Master Thread가 비동기적으로 이를 읽어서 Slave쪽으로 전송

![image](https://github.com/user-attachments/assets/18836d5d-eea2-4bc1-8e94-979ed23f7855)

### 순서
1. Commit 발생
2. Connection Thread에서 스토리지 엔진에게 해당 트랜잭션에 대한 Prepare(Commit 준비)를 수행
3. Commit을 수행하기 전에 먼저 Binary Log에 변경사항을 기록
4. 스토리지 엔진에게 트랜잭션 Commit을 수행하도록 한다.
5. Master Thread는 시간에 구애받지 않고 (비동기적으로) Binary Log를 읽어서 Slave로 전송
6. Slave의 I/O Thread는 Master로부터 수신된 변경 데이터를 Relay Log에 기록
7. Slave의 SQL Thread는 Relay Log에 기록된 변경 데이터를 읽어서 Slave의 스토리지 엔진에 적용

<br>

## Replication의 장점
### select 성능 향상
- read작업은 자원을 많이 소비한다.
- Replication을 구성하면 N개의 Slave를 가질 수 있기 때문에 Read에 대한 부하가 그만큼 분산 된다.

### 데이터 백업
- Master의 내용을 복제하기 때문에 데이터베이스를 지원도 Slave중 하나를 Master로 활용하면 되기에 백업 용도로 사용 가능

<br>

## Replication의 단점
### 데이터 정확성 보장
- Slave는 Master의 복사본을 사용하기 때문에 완벽하다고 할 수 없음
- 예를 들어 Slave가 Master의 쿼리 처리량을 따라가지 못하면 데이터 정합성이 보장되지 않는다.

### Binary Log File 관리
- Mater는 Binary Log가 무분별하게 쌓이는 것을 막기위해 데이터 보관 주기를 설정한다.
- 하지만 Master는 Slave까지 관리 하지 않기 때문에 Master에서 Binary Log를 삭제한다 해도 Slave의 Binary Log는 남아있다.

### Fail Over 불가
- Master에서 Error가 발생 했을 경우 Slave로 Failover하는 기능을 지원하지 않음
- Slave역시 Master와 Log 위치가 다르다면 관리자가 작업을 해야 한다.

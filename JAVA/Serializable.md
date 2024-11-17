# 🥕 직렬화 & 역직렬화
- 직렬화(serialize)란 자바 언어에서 사용되는 Object 또는 Data를 다른 컴퓨터의 자바 시스템에도 사용 할 수 있도록 바이트 스트림(stream of bytes) 형태로 연속적인(serial) 데이터로 변환하는 포맷 변환 기술
- 역직렬화(Deserialize)는 바이트로 변환된 데이터를 원래대로 자바 시스템의 Object 또는 Data로 변환하는 기술
- JVM의 힙(heap) 혹은 스택(stack) 메모리에 상주하고 있는 객체 데이터를 직렬화를 통해 바이트 형태로 변환하여 데이터베이스나 파일과 같은 외부 저장소에 저장해두고, 다른 컴퓨터에서 이 파일을 가져와 역직렬화를 통해 자바 객체로 변환해서 JVM 메모리에 적재한다.


```
[바이트 스트림]
스트림은 클라이언트나 서버 간에 출발지 목적지로 입출력하기 위한 데이터가 흐르는 통로를 말한다.
자바의 스트림의 기본 단위는 바이트로 두고 있어 전송을 위해 최소 단위인 바이트 스트림으로 변환하여 처리한다.
```
![image](https://github.com/user-attachments/assets/57f6e3ad-bab7-42c4-89eb-03b552faac17)
https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%A7%81%EB%A0%AC%ED%99%94Serializable-%EC%99%84%EB%B2%BD-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0

# 🥕 CPU
- CPU는 메모리에 저장된 명령어를 읽어 들이고, 해석하고 실행하는 장치이다.
- CPU 내부에는 ALU, 제어장치, 레지스터가 있다.

## ALU
- ALU는 레지스터로부터 피연산자를 받아들이고, 제어장치로부터 제어신호를 받아들인다.
- 계산된 결과(숫자 or 문자 or 주소...)는 레지스터로 보낸다.
- 연산 결과에 대한 부가 정보인 flag를 플래그 레지스터로 내보낸다.

![image](https://github.com/user-attachments/assets/25aeb480-5575-4cea-a7ec-07f113cb71e5)

### flag의 종류
![image](https://github.com/user-attachments/assets/5ec50fd3-756a-4385-9e58-2fc41807d4d6)
![image](https://github.com/user-attachments/assets/2ebbee60-0ad0-4f27-91b7-2d7a6b3701f8)

## 제어장치
- 제어 신호를 발생시키고 명령어를 해석하는 장치

### 제어 장치의 기본 기능
- CPU에 접속된 장치들에 대한 데이터 이동 순서를 조정
- 명령어를 해독
- CPU 내 데이터 흐름을 제어
- 외부 명령을 받아 일련의 제어 신호를 생성
- CPU에 포함된 많은 실행 장치(예를 들어 ALU, 데이터 버퍼, 레지스터)를 제어
- 명령어 인출, 명령어 해독, 명령어 실행 등을 순서에 맞추어 처리

### 제어장치가 내보내는 대상
- CPU 내부에 전달
  - 제어신호 (to 레지스터) : e.g. 이동해라, 저장해라, 레지스터가 어떤 행동을 해라 등을 지시
  - 제어신호 (to ALU) : e.g. 수행할 연산을 지시
- CPU 외부에 전달
  - 제어신호 (to 메모리) : e.g. 메모리를 읽어라, 써라 등을 지시
  - 제어신호 (to 입출력장치) : e.g. 입출력 장치를 읽어라, 써라, 테스트해라 등 지시
![image](https://github.com/user-attachments/assets/9bf9df5a-88bd-47de-949f-d43497f3eb08)

### 클럭
- 제어장치의 동작 타이밍 기준이 되는 신호로서, 하나의 클록 펄스마다 하나의 마이크로  연산 혹은 마이크로 연산의 집합 수행
![image](https://github.com/user-attachments/assets/d839508a-2cf8-4a25-bd11-ab4ce074100b)

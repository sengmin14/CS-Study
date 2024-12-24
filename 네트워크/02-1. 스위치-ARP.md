# 🥕 스위치
- 2계층의 대표적인 장비로 MAC주소 기반 통신
- 허브의 단점을 보완
- 라우팅 기능이 있는 스위치를 L3스위치라고도 부른다.

## 동작 방식
- 목적지 주소를 MA C주소 테이블에서 확인하여 연결된 포트로 프레임 전달
- Learning : 출발지 주소가 MAC 주소 테이블에 없으면 해당 주소를 저장
- Flooding(Broadcasting) : 목적지 주소가 MAC 주소 테이블에 없으면 전체 포트로 전달(목적지를 모르니까)
- Forwarding(Unicating) : 목적지 주소가 MAC 주소 테이블에 있으면 해당 포트로 전달
- Filtering(Collision Domain) : 출발지와 목적지가 같은 네트워크 영역이면 다른 네트워크로 전달하지 않음
- Aging : MAC 주소 테이블의 각 주고는 일정 시간 이후에 삭제

### Learning
- 4개의 PC는 스위치에 각 포트에 연결되고 프레임이 스위치에 전달
- 스위치는 해당 포트로 유입된 프레임을 보고 MAC 주소를 테이블에 저장
![image](https://github.com/user-attachments/assets/6bc46b4f-c27d-4616-8796-50c30e56991a)

### Flooding
- PC1은 목적지 aa:bb:cc:dd:ee:05 주소로 프레임 전달
- 스위치는 해당 주소가 MAC Table에 없어서 전체 포트로 전달
![image](https://github.com/user-attachments/assets/452df017-143f-42ba-bf6b-1f3904acafb0)

### Fowarding
- PC1은 목적지 aa:bb:cc:dd:ee:05 주소로 프레임 전달
- 스위치는 해당 주소가 MAC Table에 존재하므로 해당 프레임을 PC5로 전달
![image](https://github.com/user-attachments/assets/5f4bfe96-773e-44dd-8f08-2e55cda9b9d3)

### Filtering
- PC1은 목적지 aa:bb:cc:dd:ee:02 주소로 프레임 전달
- 스위치는 해당 주소가 동일 네트워크 영역임을 확인하여 다른 포트로 전달하지 않음
- 필터링은 각 포트별 Collision Domain을 나누어 효율적 통신이 가능하다.
![image](https://github.com/user-attachments/assets/469cd19f-6555-4645-bdc7-ce9509a1f552)

### Aging
- 스위치의 MAC 주소 테이블은 시간이 지나면 삭제
- 테이블 저장 공간을 효율적으로 사용하기 위함
- 해당 포트에 연결된 PC가 다른 포트로 옮겨지는 경우도 삭제
- 기본 300초 (Cisco 기준) 저장, 프레임이 시작되면 다시 카운트
![image](https://github.com/user-attachments/assets/8f5b130a-f26d-441c-954b-f265ab61ba44)


## 정리
![image](https://github.com/user-attachments/assets/15f2f4d1-476d-4bd2-8e55-26955cf6639f)



# ARP
- IP 주소를 통해서 MAC 주소를 알려주는 프로토콜
- 컴퓨터 A가 커퓨터 B에게 IP 통신을 시도하고 통신을 수행하기 위해, 목저기 MAC주소를 알아야 한다.
- 목적지 IP에 해당되는 MAC 주소를 알려주는 역할을 ARP가 해준다.
![image](https://github.com/user-attachments/assets/c3cde531-39a8-4f22-a307-d8471b10735b)

## 동작 방식
![image](https://github.com/user-attachments/assets/3e92fd02-60f3-4260-a595-c7f9a335c23d)
- PC1은 동일 네트워크 대역인 목적지 IP 172.20.10.9로 패킷 전송을 시도, 목저기 MAC 주소를 알기 위해 자신의 ARP Cache Table을 확인
- ARP Cache Table에 있으면 패킷 전송, 없으면 ARP Request(Broadcasting)전송
- IP 172.20.10.9에서 목적지 MAC 주소를 ARP Reply로 전달
- 목저기 MAC 주소는 ARP Cache Table에 저장되고 패킷 전송


### 1단계
![image](https://github.com/user-attachments/assets/00d66025-34bc-4fe7-86e8-ed6ee2358412)
```
1. PCI은 목적지 IP 172.20.10.9로 패킷 전송을 시도하기 위해 우선 ARP Cache Table 학인
```
![image](https://github.com/user-attachments/assets/d498eda8-37a2-4af3-abc1-5a3171a7a3c4)
```
172.20.10.9는 없음

ARP Request 수행
```



### 2단계
![image](https://github.com/user-attachments/assets/6996be6a-e736-43a0-9f02-9150744fccf6)
```
2. PC1은 목적지 IP 172.20.10.9에 대한 ARP Request 전송 - Broadcasting
```
![image](https://github.com/user-attachments/assets/0535757b-56ae-49f8-aa4e-d994f28d1b8c)



### 3단계
![image](https://github.com/user-attachments/assets/f11704eb-859f-4f00-8df7-7f8c9bf576d5)
```
3. IP 172.20.10.9에서 목적지 MAC 주소를 ARP Reply로 전달
```
![image](https://github.com/user-attachments/assets/5fee3364-9c13-42bb-8684-846e6bf0b252)



### 4단계
![image](https://github.com/user-attachments/assets/ffde19ea-342b-43af-98d1-8feaab7ac735)
```
4. 목적지 MAC주소는 ARP Cache Table에 저장되고 패킷 전송
```
![image](https://github.com/user-attachments/assets/0a73733a-638f-4a45-90a7-af20c10fc2dc)

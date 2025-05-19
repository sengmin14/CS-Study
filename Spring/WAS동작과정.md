# 🥕 WAS 동작 과정


## 정적 웹 페이지(Static Web Page)
![Image](https://github.com/user-attachments/assets/87674e6d-0b9d-46de-a19b-2e27d5ac0ec7)

- ```정적 웹 페이지```의 경우 서버(Web Server)에 미리 저장된 파일(HTML, css, JS, 이미지 등)을 불러와 구성하는 페이지이다.
- 서버에 저장된 데이터가 변경되지 않는 한 사용자의 고정된 웹 페이지를 보게 된다.

#### 정적 웹 페이지의 장, 단점
- 장점
  - Request에대한 데이터만 전송하고 추가적인 작업이 없으므로 빠르다.
  - Web Server만 구축해서 비용이 적게 든다.
- 단점
  - 저장된 데이터만 보여줄 수 있어 서비스가 한정적이다.
  - 삽입, 수정, 삭제 등의 작업이 모두 수동적이므로 관리가 힘들다.

## 동적 웹 페이지(Dynamic Web Page)
![Image](https://github.com/user-attachments/assets/67206bd9-e855-471d-9ffe-1c7ca2b09e37)

- ```동적 웹 페이지```의 경우 서버(Web Server)에 있는 데이터들을 사용자의 Request에 따라 가공 처리한 후 구성하는 페이지이다.
- 사용자의 Request에 따라서 데이터를 가공한 후 사용자는 달라지는 웹 페이지를 보게 된다.

#### 동적 웹 페이지의 장, 단점
- 장점
  - 사용자의 Request에 따라 동적으로 페이지를 생성, 제공하므로 서비스가 다양하다.
  - 삽입, 수정, 삭제 등의 작업의 관리가 쉽다.
- 단점
  - 사용자에게 웹 페이지를 구성해주기 전, 처리하는 비즈니스 로직이 있어 상대적으로 느리다.
  - Web Server외에 비즈니스 로직을 처리할 Web Application Sever가 필요하여 추가적인 비용이 든다.


## Web Server
- 웹 서버는 정적 컨텐츠(HTML, CSS, JS, 이미지 등)을 사용자에게 제공하는 서버
- 소프트웨어 관점으로 웹 브라우저에서 Client(사용자)로 부터 HTTP Request를 받은 후 HTML, CSS, JS 등과 같은 정적 컨텐츠들을 제공해주는 프로그램이다.
- 하드웨어 관점으로는 위 소프트웨어적 관점에서 본 프로그램을 탑재한 컴퓨터 시스템이다.
- Client의 요청에서 가장 앞에서 요청에 대한 데이터를 만들어 응답해준다.

### Web Server의 기능
- Http 프로토콜을 기반으로, 클라이언트의 요청을 서비스하는 기능을 담당한다.
- 요청에 맞게 두가지 기능 중 선택해서 제공해야 한다.
  - 정적 컨테츠 제공
    - WAS를 거치지 않고 바로 자원 제공
  - 동적 컨텐츠 제공을 위한 요청 전달
    - 클라이언트 요청을 WAS에 보내고, WAS에서 처리한 결과를 클라이언트에게 전달

## Web Application Server
- 웹 어플리케이션 서버는 흔히 WAS, 컨테이너, 웹 컨테이너, 서블릿 컨테이너로 블리우며 DB 조회 및 다양한 로직 처리 요구시 동적인 컨텐츠를 제공하기 위해 만들어진 애플리케이션 서버로, 동적 컨텐츠를 제공하는 서버
  - 컨테이너란 JSP, Servlet을 실행시킬 수 있는 소프트웨어, 즉 WAS는 JSP, Servlet 구동 환경을 제공해준다.

- WAS는 WS와 웹 컨테이너가 결합한 형태라고 볼 수 있다. 웹 서버의 기능들을 구조적으로 분리하여 처리하는 역할을 한다.
  - 주로 DB 서버와 함께 사용되며, 보안, 스레드 처리, 분산 트랜잭션 등 분산 환경에서 사용된다.

- 웹 컨테이너는 클라이언트 요청에 내부 로직을 통해 결과를 처리하고 동적 컨텐츠를 생성해 클라이언트에게 응답 해주는 역할을 수행한다.

### WAS의 기능
- 프로그램 실행 환경 및 DB 접속 기능 제공
- 여러 트랜잭션 관리 기능
- 업무를 처리하는 비즈니스 로직 수행

### WAS 동작 과정
![Image](https://github.com/user-attachments/assets/ad5b4c1a-0b91-420f-9765-93c33ec3a01d)
1. 웹 서버로 부터 요청이 들어오면 제일 먼저 컨테이너가 이를 알맞게 처리한다.
2. 컨테이너는 배포 서술자(web.xml)를 참조하여 해당 서블릿에 대한 스레드를 생성하고 요청(HttpServletRequest) 및 응답(HttpServletResponse) 객체를 생성하여 전달한다.
3. 컨테이너가 서블릿을 호출한다
4. 호출된 서블릿의 작업을 담당하게 된 미리 생성된 스레드는 요청에 따라 doGet()또는 doPost()를 호출한다.
5. 호출된 doGet()또는 doPost()메소드는 생성된 동적 페이지를 Response 객체에 실어서 컨테이너에 전달한다.
6. 컨테이너는 전달받은 Response 객체를 HttpResponse 형태로 전환하여 웹서버에 전달하고 생성되었던 스레드를 종료하고 요청 및 응답 객체를 소멸시킨다.


### Architecture
![Image](https://github.com/user-attachments/assets/1ca494f5-b1a3-4fd2-91ee-33845291c58b)

1. Web Server는 웹 브라우저 클라이언트로부터 HTTP 요청을 받는다.
2. Web Server는 클라이언트의 요청(Request)을 WAS에 보낸다.
3. WAS는 관련된 Servlet을 메모리에 올린다.
4. WAS는 web.xml을 참조하여 해당 Servlet에 대한 Thread를 생성한다. (Thread Pool 이용)
5. HttpServletRequest와 HttpServletResponse 객체를 생성하여 Servlet에 전달한다.
6. 5-1. Thread는 Servlet의 service() 메서드를 호출한다.
7. 5-2. service() 메서드는 요청에 맞게 doGet() 또는 doPost() 메서드를 호출한다.
8. protected doGet(HttpServletRequest request, HttpServletResponse response)
9. doGet() 또는 doPost() 메서드는 인자에 맞게 생성된 적절한 동적 페이지를 Response 객체에 담아 WAS에 전달한다.
10. WAS는 Response 객체를 HttpResponse 형태로 바꾸어 Web Server에 전달한다.
11. 생성된 Thread를 종료하고, HttpServletRequest와 HttpServletResponse 객체를 제거한다.


### WS와 WAS를 분리하는 이유
#### Web Server가 필요한 이유
- 클라이언트(웹 브라우저)에 이미지 파일(정적 컨텐츠)을 보내는 과정을 생각해보자.
- 이미지 파일과 같은 정적인 파일들은 웹 문서(HTML 문서)가 클라이언트로 보내질 때 함께 가는 것이 아니다.
- 클라이언트는 HTML 문서를 먼저 받고 그에 맞게 필요한 이미지 파일들을 다시 서버로 요청하면 그때서야 이미지 파일을 받아온다.
Web Server를 통해 정적인 파일들을 Application Server까지 가지 않고 앞단에서 빠르게 보내줄 수 있다.
- 따라서 Web Server에서는 정적 컨텐츠만 처리하도록 기능을 분배하여 서버의 부담을 줄일 수 있다.


### 그렇다면 WAS가 Web Server의 기능도 모두 수행하면 되지 않을까?
- 기능을 분리하여 서버 부하 방지
  - WAS는 DB 조회나 다양한 로직을 처리하느라 바쁘기 때문에 단순한 정적 컨텐츠는 Web Server에서 빠르게 클라이언트에 제공하는 것이 좋다.
  - WAS는 기본적으로 동적 컨텐츠를 제공하기 위해 존재하는 서버이다.
  - 만약 정적 컨텐츠 요청까지 WAS가 처리한다면 정적 데이터 처리로 인해 부하가 커지게 되고, 동적 컨텐츠의 처리가 지연됨에 따라 수행 속도가 느려진다.
  - 즉, 이로 인해 페이지 노출 시간이 늘어나게 될 것이다.
- 물리적으로 분리하여 보안 강화
  - SSL에 대한 암복호화 처리에 Web Server를 사용
- 여러 대의 WAS를 연결 가능
  - Load Balancing을 위해서 Web Server를 사용
  - fail over(장애 극복), fail back 처리에 유리
  - 특히 대용량 웹 어플리케이션의 경우(여러 개의 서버 사용) Web Server와 WAS를 분리하여 무중단 운영을 위한 장애 극복에 쉽게 대응할 수 있다.
  - 예를 들어, 앞 단의 Web Server에서 오류가 발생한 WAS를 이용하지 못하도록 한 후 WAS를 재시작함으로써 사용자는 오류를 느끼지 못하고 이용할 수 있다.
- 여러 웹 어플리케이션 서비스 가능
  - 예를 들어, 하나의 서버에서 PHP Application과 Java Application을 함께 사용하는 경우
- 기타
  - 접근 허용 IP 관리, 2대 이상의 서버에서의 세션 관리 등도 Web Server에서 처리하면 효율적이다.

```
즉, 자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성 을 위해 Web Server와 WAS를 분리한다.
Web Server를 WAS 앞에 두고 필요한 WAS들을 Web Server에 플러그인 형태로 설정하면 더욱 효율적인 분산 처리가 가능하다.
```

# 🥕 DispatcherServlet
- HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러
- 모든 요청을 DispatcherServlet이 받기에 정적 자원에 대해서는 WebServer에 위임??
```
클라이언틀부터 요청이 오면 Tomcat내부 서블릿 컨테이너가 요청을 받게된다.
그리고 이 모든 요청을 프론트 컨트롤러인 DispatcherServlet이 가장 먼저 받는다.
DispatcherServlet은 공통적인 작업을 먼저 처리한 후에 해당 요청을 처리해야 하는 컨트롤러를 찾아 작업을 위
```

## DispatcherServlet의 동작 과정
![image](https://github.com/user-attachments/assets/acf01df3-019d-4293-8f11-027469928a9b)

1. 클라이언트의 요청을 DispatcherServlet이 받음
2. 요청 정보를 통해 요청을 위임할 컨트롤러를 찾음
3. 요청을 컨트롤러로 위임할 핸들러 어댑터를 찾아서 전달함
4. 핸들러 어댑터가 컨트롤러로 요청을 위임함
5. 비지니스 로직을 처리함
6. 컨트롤러가 반환값을 반환함
7. 핸들러 어댑터가 반환값을 처리함
8. 서버의 응답을 클라이언트로 반환함


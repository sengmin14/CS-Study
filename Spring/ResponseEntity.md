# 🥕 ResponseEntity
- ResponseEntity는 HTTP 응답을 나타내는 Spring Framework의 클래스
- 이 클래스는 요청에 대한 응답의 HttpHeader, HttpBody 및 Status Code를 포함하여 클라이언트에게 전달할 수 있는 다양한 기능을 제공

## ResponseEntity를 사용하는 이유
1. Http 상태 코드 제어<br>
   ResponseEntity를 사용하면 응답에 대한 HTTP 상태 코드를 명시적으로 지정할 수 있다.<br>
   이는 클라이언트에게 정확한 상태 정보를 제공하는 데 도움이 된다.
2. 응답 본문 및 헤더 제어<br>
   ResponseEntity를 통해 응답 본문과 헤더를 세밀하게 제어할 수 있다.
3. 유연성<br>
   ResponseEntity를 사용하면 일반적인 객체 또는 커스텀 클래스를 응답으로 반환할 수 있으며, Spring은 자동으로 해당 객체를 적절한 형식으로 반환한다.


## 자주 사용되는 HTTP 상태 코드
- HttpStatus.OK: 200 OK
- HttpStatus.CREATED: 201 Created
- HttpStatus.NO_CONTENT: 204 No Content
- HttpStatus.BAD_REQUEST: 400 Bad Request
- HttpStatus.UNAUTHORIZED: 401 Unauthorized
- HttpStatus.FORBIDDEN: 403 Forbidden
- HttpStatus.NOT_FOUND: 404 Not Found
- HttpStatus.INTERNAL_SERVER_ERROR: 500 Internal Server Error


   

# 🥕 JDK와 JRE이 차이

## JRE
- JRE(Java Runtime Environment)는 JVM, 자바 클래스 라이브러리, 자바 명령 및 기타 인프라를 포함한 컴파일된 Java프로그램을 실행하는데 필요한 패키지이다.

## JDK
- JDK(Java Development Kit)는 Java를 사용하기 위해 필요한 모든 기능을 갖춘 Java 용 SDK(Software Development Kit)이다.
- JRE에 있는 모든 것뿐만 아니라 컴팡이러(javac)와 jdb, javadoc과 같은 도구도 있다.
- JDK는 프로그램을 생성하고 컴파일 할 수 있다.

### 정리
- Java 프로그램을 실행하는데만 포커스를 둔다면, JRE만 설치하면 된다.
- 반면에, Java 프로그램이릉 할 계획이라면 JDK를 설치해야 한다.
- JSP를 사용하는 웹 애플리케이션을 배포하는 경우 기술적으로 애플리케이션 서버 내에서 Java프로그램을 실행하는 것이기 때문에 JDK가 필요하다.

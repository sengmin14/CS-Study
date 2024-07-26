# 🥕 Spring DI
- Spring은 3가지 핵심 프로그래밍 모델(AOP, DI, IOC)을 지원한다.
- DI는 IOC를 구현하는 방식 중 하나

## Spring DI 사용 방법

### 생성자 주입
- 현재 가장 권장되는 의존 관계 주입 방식
- Spring 4.3부터는 @Autowired가 생략 가능해서 최근에는 생성자를 딱 1개 두고, @Autowired를 생략하는 방법을 주로 사용한다. (단일 생성자일 경우)
- Lombok 라이브러리의 @RequiredArgsContstructor를 함께 사용하면 생성자를 생략 가능해서 코드가 깔끔해진다.
- 생성자 주입만이 final 키워드를 사용할 수 있고, 생성자를 통해 주입되는 방식이다.
   - 필드 주입, 세터 주입 : 주입 시점에서 초기화가 이루어지지 않으므로 final 키워드를 사용할 수 없다.
   - 생성자 주입 : 객체 생성 시 의존성을 주입받아 초기화 할 수 있으므로 final 키워드를 사용할 수 있다.
- final 키워드를 사용해서 값이 한번 할당되면 변경할 수 없기에 객체의 불변성이 보장된다.
- 초기에 할당되기에 NPE이 절대 발생하지 않는다.

``` java
public class Injection {
    private InjectionService injectionService;
    
    // 생성자 DI
    // @Autowired -> Spring4.3부터는 @Autowired 생략 가능
    public Injection(InjectionService injectionService) {
        this.injectionService = injectionService;
    }
}
````

- Lombok라이브러리 적용

``` java
import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor
public class Injection {
    private final InjectionService injectionService; // final 필드
}
````

- @RequiredArgsConstructor는 final이 붙은 주입에만 생성자를 만들어준다.
- final이 없는 주입은 @AllArgsConstructor을 사용한다.

``` java
import lombok.RequiredArgsConstructor;

@AllArgsConstructor
public class Injection {
    private InjectionService injectionService; // final 필드
}
````

### 필드 주입
- @Autowired를 붙여주면 자동으로 의존 관계가 주입된다.
- 간편하게 의존 관계 주입이 가능하지만 참조 관계를 눈으로 확인하기 어렵고, 순환 참조를 막을 수 없다.
  - 예를 들어 A가 B를 가지고 있고, B가 A를 가지고 있어(순환 참조) 실행 전까지 Error를 잡을 수 없다.
- 필드 주입은 모든 생성자 이후에 호출되므로, 필드에 final키워드를 사용할 수 없다.

``` java
public class Injection {
    @Autowired
    private InjectionService injectionService;
}
```

- Solid원칙 중 단일 책임 원칙(SRP)을 위반한다.
- Unit Test가 어렵다.
- 불변성이 보장되지 않아 객체가 변할 수 있다.

### Setter 주입
- @Autowired 어노테이션을 사용해서 Setter 메서드를 통해 주입하는 방법
- NPE발생할 수 있다.
- 필드 주입은 모든 생성자 이후에 호출되므로, 필드에 final키워드를 사용할 수 없다.

``` java
public class Injection {
    private InjectionService injectionService;
    @Autowired
    public void setInjectionService( InjectionService injectionService) {
        this.injectionService = injectionService;
    }
}
```

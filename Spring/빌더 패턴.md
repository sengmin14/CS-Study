## 🥕 1. 필더 패턴(Builder Pattern)

```
빌더 패턴은 객체의 생성 과정을 단계별로 나누어 설정할 수 있게 하는 디자인 패턴
동일한 구성코드를 사용하여 다양한 타입과 표현을 제공
결과적으로 생성자를 가독성 좋게 만들어주는 도구
```


## 2 빌더 패턴을 사용해야 하는 이유

예시로 피자라는 객체가 있다고 가정해보겠습니다.
``` java
public class Pizza {
 
    private String name;
    private String dough;
    private String sauce;
    private String topping;
    private int price;
 
    public Pizza(String name, int price) {
        this.name = name;
        this.price = price;
    }
 
    public Pizza (String name, String dough, String sauce, int price) {
        this.name = name;
        this.dough = dough;
        this.sauce = sauce;
        this.price = price;
    }
 
    public Pizza(String name, String dough, String sauce, String topping, int price) {
        this.name = name;
        this.dough = dough;
        this.sauce = sauce;
        this.topping = topping;
        this.price = price;
    }
 
    @Override
    public String toString() {
        return "name: " + name + ", " + "dough: " + dough + ", " +  "sauce: " + sauce +  ", " + "topping: " + topping + "price: " + price;
    }
}

```
### 3-1 생성자에 인자를 넣어 인스턴스를 생성하는 방법
- 피자에 필수 인자인 name, price만 인자로 받는 Pizza 생성자로 받은 Pizza생성자를 선언
- name, price와 더불어 dough와 sauce를 추가하는 생성자도 선언
- 나머지 모든 인자 값을 사용하는 피자 생성자 선언

<br>
피자라는 객체를 생성할 때 원하는 인자를 상황에 맞게 넣어 피자 객체를 만들어 줄 수 있습니다.

예를 들어 기본적인 피자의 이름과 가격만 정해진 경우에 객체를 생성할 때에는 

Pizza = new Pizza("고구마피자", 20000);  

고구마 피자이며 20000원인 피자의 객체를 생성할 수 있습니다.

이 방식의 장점은 생성할 때 생성자의 안에서 커스터 마이징을 통해 원하는 값을 변경할 수 있다는 점입니다.

예를 들어서 

``` java
public Pizza(String name, String dough, String sauce, String topping, int price) {
        this.name = name + "이벤트상품";
        this.dough = dough;
        this.sauce = sauce;
        this.topping = topping;
        this.price = price + 10000;
    }
```
이름에 이벤트 상품이라는 세팅을 하거나 가격을 추가한다거나 하는 로직을 추가해 줄 수 있다.

<br>

단점도 존재합니다. 
- new Pizza("슈프림프 피자", "씬도우",  "핫소스", 5000)과 같이 호출이 많은 경우 그만큼 생성자를 많이 만들어야 한다.
- 객체의 프로퍼티가 많으면 그에 따른 생성자도 많이 만들어야 한다.
- 프로퍼티가 많으면 그 경우의 수에 맞게 생성자를 만들어야 한다.
- 인자에 대한 설명이 없기 때문에 인자가 많은 경우에는 몇 번째 인자가 어떤 인지에 맞는 건지 의미가 헷갈릴 수 있.

### 3-2 Setter를 이용하는 자바 빈 패

``` java
public class PizzaJavaBean {
 
    private String name;
    private String dough;
    private String sauce;
    private String topping;
    private int price;
 
 
    public PizzaJavaBean() {
        // 기본 생성자
    }
 
    public String getName() {
        return name;
    }
 
    public String getDough() {
        return dough;
    }
 
    public String getSauce() {
        return sauce;
    }
 
    public String getTopping() {
        return topping;
    }
 
    public int getPrice() {
        return price;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    public void setDough(String dough) {
        this.dough = dough;
    }
 
    public void setSauce(String sauce) {
        this.sauce = sauce;
    }
 
    public void setTopping(String topping) {
        this.topping = topping;
    }
 
    public void setPrice(int price) {
        this.price = price;
    }
}
```
```
생성자에 인자를 넣어 인스턴스를 생성하는 방법을 보완해서 만든 방법이다.

setter를 사용하여 생성한 객체에 인자를 정확하게 파악하여 세팅을 한다.
```

장점으로는 
- 각 인자의 의미를 setter메서드에 맞는 값을 넣기 때문에 정확하게 파악이 가능하게 가능하 됩니다.
- 객체 클래스 내부에 복잡하게 여러 개의 생성자를 만들 필요가 없습니다.

단점으로는 
- 1회 호출로 객체 생성이 끝나지 않고, setter를 통해 값을 계속 세팅을 해주어야 한다는 점이 있습니다.
- setter 메서드가 있어서 변경이 불가능(immutable)한 클래스를 만들 수 없음(setter를 호출하면 내부의 값이 계속 바뀐다) ex] setPrice(20000)을 했는데 그다음에 또 setPrice(4000) 세팅이 될 수 있음

### 3-3 빌더 패턴을 통한 객체 생성

``` java
public class PizzaBuilder {
 
    private final String name;
    private final String dough;
    private final String sauce;
    private final String topping;
    private final int price;
 
 
    // 객체 생성 전, 값을 세팅해줄 Builder 내부 클래스
    public static class Builder {
        private String name;
        private String dough;
        private String sauce;
        private String topping;
        private int price;
 
        public Builder name(String name) {
            this.name = name;
            return this;
        }
 
        public Builder dough(String dough) {
            this.dough = dough;
            return this;
        }
 
        public Builder sauce(String sauce) {
            this.sauce = sauce;
            return this;
        }
 
        public Builder topping(String topping) {
            this.topping = topping;
            return this;
        }
 
        public Builder price(int price) {
            this.price = price;
            return this;
        }
 
        // 값 세팅이 끝난 후 내부 클래스를 넘겨주어 본 객체에 값을 세팅해주는 메소드
         public PizzaBuilder build() {
            return new PizzaBuilder(this);
         }
    }
 
    // 값 세팅이 끝난 후 내부 클래스를 넘겨주어 본 객체에 값을 세팅해주는 메서드
    public PizzaBuilder(Builder builder) {
        this.name = builder.name;
        this.dough = builder.dough;
        this.sauce = builder.sauce;
        this.topping = builder.topping;
        this.price = builder.price;
    }
 
    @Override
    public String toString() {
        return "name: " + name + ", " + "dough: " + dough + ", " + "sauce: " + sauce + ", " + "topping: " + topping + "price: " + price;
    }
}
Colored
```

``` java
public class Main {
 
    // 빌더 패턴 사용
    PizzaBuilder pizzaBuilder = new PizzaBuilder.Builder()
            .name("불고기피자")
            .dough("thin")
            .sauce("핫소스")
            .topping("페퍼로니")
            .price(20000)
            .build();
}
```

```
값을 세팅 후 자기 자신을 리턴하여 price, dough나 setting 메서드를 계속 호출할 수 있다. 모든 세팅이 끝나면 마지막에 build()를 호출한다.
```

장점은 다음과 같다

- 객체 생성이 되는 순간 setting을 모두 하기 때문에 변경 불가능한(immutable) 객체를 만들 수 있음.
- 객체 클래스 내부에 복잡하게 여러 개의 생성자를 만들 필요가 없음
- build() 함수로 잘못된 값이 세팅되어있는지 검증이 가능
- 직관적인. 세팅 메서드 네이밍을 통해 인지 값을 파악하는데 용이함

단점은 다음과 같다
- Builder 인터페이스의 변화는 이를 구현한 모든 클래스에 영향을 미친다.
- 어떤 UI 요소는 같은 인터페이스로 처리하기 어려울 수도 있다(예를 들어 HTML vs 스윙)

### 3-4 스프링 시큐리티에서의 빌더 패턴 사용

``` java
import lombok.extern.java.Log;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.web.header.writers.StaticHeadersWriter;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;
 
import javax.sql.DataSource;
 
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
 
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/user/**").permitAll()
                .antMatchers("/subadmin/**").hasRole("SUB")
                .antMatchers("/admin/**").hasRole("ADMIN");
 
        http.formLogin();
        http.exceptionHandling().accessDeniedPage("/accessDenied");
        http.logout().logoutUrl("/logout").invalidateHttpSession(true);
 
    }
```

### 4-4 결론
```
생성 인자가 많을 경우 빌더 패턴을 고려하는게 좋다.
추가적으로 빌더패턴의 개념을 이해하기 위해 메소드 체이닝에 대한 개념도 살펴보면 좋을것 같습니다.
```

# 🥕 DAO, DTO, VO의 개념



## DAO(Data Access Object)
<br>

- 실제로 DB에 데이터를 접근하는 객체
- DAO는 Service와 DB를 연결하는 역할을 하며, 실제로 DB에 접근하여 data를 삽입, 삭제, 조회, 수정 등 CRUD 기능을 수행
- JPA에서는 DB에 데이터를 CRUD하는 JpaRepository<>를 상속받는 Repository 객체들이 DAO라고 볼 수 있다.

```java
public interface itemRepository extends JpaRepository<Item, Long> {
} // itemRository는 Spring이 직접 구현하고, ItemRepository구현체가 DAO가 된다.
```

## Entity
- Entity 클래스는 실제 DataBase의 테이블과 1:1로 매핑되는 클래스
- DB의 테이블내에 존재하는 컬럼만을 속석(필드)으로 가져야 한다.
- 외부에서 getter 메소드를 이요하지 않도록 로직 구현
- 객체의 불변성을 보장해야 하기에 setter 메서드를 지양하고 생성자(Contructor)또는 Builder을 사용
- request, response클래스로 사용 X

## DTO(Data Transfer Object)
<br>

- DTO란 프로세스 간에 데이터를 전달하는 객체를 의미
- 데이터를 전송하기 위해 사용하는 객체라서 그 안에 비즈니스 로직 같은 복잡한 코드는 없고 순수하게 전달하고 싶은 데이터만 담겨있다.

![image](https://github.com/sengmin14/CS-Study/assets/140876841/207056d6-57d0-4b15-8945-6c6dc7066a6f)


### DTO를 사용하는 이유
```
데이터를 움질일때 왜 Entity 객체를 그대로 사용하지 않고 굳이 DTO를 사용하는 것일까?
```
- View Layer와 DB Layer의 역할을 분리하기 위해서
- Entity 객체의 변경을 피하기 위하여
- View와 통신하는 DTO 클래스는 자주 변경된다.
- 도메인 모델링을 지키기 위해

### 예시
Entity
``` java
public class User {

    public Long id;
    public String name;
    public String email;
    public String password; //외부에 노출되서는 안 될 정보
    public DetailInformation detailInformation; //외부에 노출되서는 안 될 정보

    //비즈니스 로직, getter, setter 등 생략
}
```
Controller
``` java
@GetMapping
public ResponseEntity<User> showArticle(@PathVariable long id) {
   User user = userService.findById(id);
   return ResponseEntity.ok().body(user);
}
```

이 상황에서 return ResponseEntity.ok().body(user);을 넘겨준다면 password까지 다 전달된다.

DTO
``` java
public class UserDto {

    public final long id;
    public final String name;
    public final String email;

    //생성자 생략

    public static UserDto from(User user) {
        return new UserDto(user.getId(), user.getName(), user.getEmail());
    }
}
```
Controller
``` java
@GetMapping
public ResponseEntity<UserDto> showArticle(@PathVariable long id) {
    User user = userService.findById(id);
    return ResponseEntity.ok().body(UserDto.from(user));
}
```
필요한것만 담앗 보내는 DTO를 만들어 사용하면 필요한것만 보낼수있다.
<br>
return ResponseEntity.ok().body(UserDto.from(user));


## VO
- VO는 변경 불가능한(Immultable) 객체
- VO는 특정 비즈니스 값을 담는 객체
- 내용물이 값 자체를 의미하기 때문에 read only
- getter 메서드와 베즈니스 로직은 포함할 수 있음


VO는 객체들의 주소 값이 달라도 데이터 값이 같으면 동일한 것으로 여긴다.
<br>
-> 값 비교를 위해 equals()와 hashCode()메서드를 오버라이딩 해줘서 비교한다.
```java
public class DateRangeVO {
    private final LocalDate startDate;
    private final LocalDate endDate;

    public DateRangeVO(LocalDate startDate, LocalDate endDate) {
        this.startDate = startDate;
        this.endDate = endDate;
    }

    // Getters
}
```
<h4> VO는 그럼 어떤 경우에 사용해야 할까?</h4>
  
- 데이터가 불변이어야 하고, 단순히 저장된 값을 불러와야 하는 경우
- 데이터를 전달하거나 비교하는 데 사용
- 값 자체가 중요한 경우 (ex. 금액, 주소, 전화번호)
- 불변성을 유지해야 하는 경우
- 객체 간 비교가 필요한 경우(ex. 두 개의 금액을 비교하는 경우, 금액 객체는 VO로 구현)














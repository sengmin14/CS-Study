## 🥕 @Controller와 @RestController

```
Spring에서 컨트롤러를 지정해주기 위한 어노테이션은 @Controller와 @RestController가 있다.
전통적인 Spring MVC의 컨트롤러인 @Controller와 Restful 웹 서비의 컨트롤러인 @RestController의 주요 차이점은
HTTP Response Body가 생성되는 방식.
```


## @Controller

### [ Controller로 View 반환하기 ]
<br>
```
전통적인 Spring MVC의 컨트롤러인 @Controller는 주로 View를 반환하기 위해 사용
아래와 같은 과정을 통해 Spring MVC Container는 Client의 요청으로부터 View를 반환
```

![image](https://github.com/shinsunyoung/springboot-developer/assets/140876841/947d4fa8-7b1f-4d42-9be6-8a56c6d2a6bc)
1. Client는 URI 형식으로 웹 서비스에 요청을 보낸다.
2. DispatcherServlet이 요청을 처리할 대상을 찾는다.
3. HandlerAdapter를 통해 요청을 Controller로 위임한다.
4. Controller는 요청을 처리한 후에 ViewName을 반환한다.
5. DispatcherServlet은 ViewResolver를 통해 ViewName에 해당하는 View를 찾아 사용자에게 반환한다.
<br>
Controller가 반환한 뷰의 이름으로부터 View를 렌더링하기 위해서는 ViewResolver가 사용되며, ViewResolver 설정에 맞게 View를 찾아 렌더링 한다.


### [ Controller로 Data 반환하기 ]
<br>

```
Spring MVC의 컨트롤러를 사용하면서 Data 를 반환해야 하는 경우도 있다.
컨트롤러에서는 데이터를 반환하기 위해 @ResponseBody 어노테이션을 활용해주어야 한다.
이를 통해 Controller도 Json형태로 데이터를 반환할 수 있다.
```

![image](https://github.com/shinsunyoung/springboot-developer/assets/140876841/03d03fcd-6ddc-465a-88fb-04bf8cc54f43)

<br>

1. Client는 URI 형식으로 웹 서비스에 요청을 보낸다.
2. DispatcherServlet이 요청을 처리할 대상을 찾는다.
3. HandlerAdapter를 통해 요청을 Controller로 위임한다.
4. Controller는 요청을 처리한 후에 객체를 반환한다.
5. 반환되는 객체는 Json으로 Serialize되어 사용자에게 반환된다.

### [ @Controller 코드 ]

``` java
@Controller
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @GetMapping(value = "/users")
    @ResponseBody
    public ResponseEntity<User> findUser(@RequestParam String userName){
        return ResponseEntity.ok(userService.findUser(userName));
    }
    
    @GetMapping(value = "/users/detailView")
    public String detailView(Model model, @RequestParam String userName){
        User user = userService.findUser(userName);
        model.addAttribute("user", user);
        return "/users/detailView";
    }
}
```

## @RestController

### [ RestController ]

```
@RestController는 @Controller에 @ResponseBody가 추가된 것
동작 과정은 @Controller에 @ResponseBody를 붙인 것과 완벽히 동일한다.
```

### [ @RestController 코드]

``` java
@RestController
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @GetMapping(value = "/users")
    public User findUser(@RequestParam String userName){
        return userService.findUser(userName);
    }

    @GetMapping(value = "/users")
    public ResponseEntity<User> findUserWithResponseEntity(@RequestParam String userName){
        return ResponseEntity.ok(userService.findUser(userName));
    }
}
```

# 🥕 AutoCloseable 클래스
- try-with-resouces는 try(...)문에서 선언된 객체들에 대해서 try가 종료될 때 자동으로 자원을 해제해주는 기능
- 주로 외부 자원인 파일 관련 객체와 socket Handler 객체와 같은 자원들은 try-catch-finally문을 사용하여 마지막에 자원을 해제하는 코드를 추가했었다.

## try-with-catch
``` java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class TraditionalResourceCloseable {

    public static void main(String[] args) throws IOException {

        FileInputStream is = null;
        BufferedInputStream bis = null;

        try{
            is = new FileInputStream("/Users/limjun-young/test.txt");
            bis = new BufferedInputStream(is);

            int n = -1;
            while ((n = bis.read()) != -1) {
                System.out.println((char) n);
            }

        }finally {
           if (is != null) {
               is.close();
            }
           
           if (is != null) {
               bis.close();
            }
        }
    }
}
```

- 코드를 보면 try에서 inputStream 객체를 생성하고 finally에서 close해주었다.
- try안의 코드를 실행하다 Exception이 발생하는 경우 모든 코드가 실행되지 않을 수 있기 때문에 finally에 close코드를 넣어주어야 한다.
- 심지어 InputStream객체가 null인지 체크해줘야 하며 close에 대한 Exception 처리도 해야 한다.

## try-with-resources
``` java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class TraditionalResourceCloseable {

    public static void main(String[] args) {

        // try-with-resources로 자원 해제
        try (
                FileInputStream is = new FileInputStream("/Users/limjun-young/test.txt");
                BufferedInputStream bis = new BufferedInputStream(is);
        ) {
            int n = -1;
            while ((n = bis.read()) != -1) {
                System.out.println((char) n);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- try-with-resources에서 자동으로 close가 호출되는 것은 AutoCloseable을 구현한 객체에만 해당된다.

## try-with-resouces로 close() 메소드가 호출되는 객체
- Try-with-resources가 모든 객체의 close()를 호출해주지는 않습니다. AutoCloseable을 구현한 객체만 close()가 호출된다.
``` java
package java.lang;

public interface AutoCloseable {
    void close() throws Exception;
}
```

- 주의할 점은 BufferedInputStream 객체는 InputStream 객체를 상속받는다.
- 아래 코드와 같이 InputStream 객체가 AutoCloseable를 상속받은 Closeable을 구현하였을 때 BufferedInputStream 객체가 Try-with-resources에 의해서 해제될 수 있다.

``` java
public abstract class InputStream extends Object implements Closeable {
    ...
}

public interface Closeable extends AutoCloseable {
    void close() throws IOException;
}
```

### AutoCloseable을 구현하는 클래스 만들기
- 내가 만든 클래스가 try-with-resources으로 자원이 해제되길 원한다면 AutoCloseable을 implements 해야한다.
- 아래 코드에서 CustomResource 클래스는 AutoCloseable을 구현하였다. main에서는 이 객체를 try-with-resources로 사용하고 있다.
``` java
public class AutoCloseableExample {
    public static void main(String[] args) {
        try (CustomResource customResource = new CustomResource()){
            customResource.doSomething();
        } catch (Exception e){
            e.printStackTrace();
        }
    }
}

public class CustomResource implements AutoCloseable {

    public void doSomething(){
        System.out.println("Do something ...");
    }

    @Override
    public void close() throws Exception {
        System.out.println("CustomResource Closed!");
    }
}
```

- 실행해보면 다음과 같이 출력된다.
- 이 예제에서는 close()가 호출될 때 로그를 출력하기 때문에 close()가 호출되는지 눈으로 확인할 수 있다.

### 실행 결과
![image](https://github.com/user-attachments/assets/5147766f-6860-45c5-a3c3-dfe8d56c3160)










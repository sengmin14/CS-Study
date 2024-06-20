## 🥕 1. 추상 클래스 (Abstract Class)

```
추상 클래스란, 추상 메소드를 포함한 클래스를 말한다.
```

``` java
public abstract class Car{
    public void Ride(){
        //...
    }

    public void RideReverse(){
        //...
    }

    //이 부분은 추상 메소드로, Car를 상속받는 클래스 내부에서 구현한다.
    public abstract void ChangeGear();
        
    public abstract double Acceleration();
}
```

<br>
이처럼, 일반 메소드들도 가지고 있지만, 추상 메소드를 포함한 형태의 클래스로 구현된다.
<br>
<h4>추상 클래스의 구체화는 클래스 간의 상속을 통해 이루어진다.</h4>

``` java
public class GasolineCar extends Car{
    //Car 내에 구현된 일반 메소드는 구현하지 않아도 사용 가능
    //Ride, RideReverse

    //추상 메소드 구현
    public void ChangeGear(){
        //...
    }

    //추상 메소드 구현
    public double Acceleration(){
        //...
    }

    //객체 내의 일반 메소드
    public void ChargeGasoline(int Amount){
        //...
    }
}
```

<h4>추상 클래스를 상속받는 클래스는 반드시 부모 추상 클래스의 모든 추상 메소드를 구현해야 한다.</h4>

<br>

## 2. 인터페이스 (Interface)

```
인터페이스란, 추상 메소드만으로 이루어진 클래스이다.
추상 메소드는 구현 내용 없이 선언만 해 놓은 형태로 정의한다.
즉, 인터페이스 내의 모든 메소드는 선언만 되어 있을 뿐, 그 내용은 없다.
```

``` java
public interface Car{
    //인터페이스 내의 변수는 반드시 static final로 지정한다.
    public static final int variable = 10;

    //인터페이스 내에서 지정한 메소드는
    //반드시 이 인터페이스를 구현하는 클래스가 구현해야 한다.
    //즉, 클래스에게 특정 메소드들을 구현하도록 강제할 수 있다.
    public void Ride();

    public void RideReverse();

    public void ChangeGear();

    public double Acceleration();
}
```

클래스를 선언할 때, "이 클래스는 이 인터페이스를 구현한다."고 지정해줄 수 있다.

``` java
public class GasolineCar implements Car{
    public void Ride(){
        //...
    }

    public void RideReverse(){
        //...
    }

    public void ChangeGear(){
        //...
    }

    public double Acceleration(){
        //...
    }

    public void ChargeGasoline(int Amount){
        //...
    }
}
```
클래스 옆에 implements 키워드를 붙여 해당 클래스가 어떤 인터페이스의 구현체인지 나타낸다.
<br>
어떤 인터페이스를 구현하는 클래스는 반드시 자신이 구현하는 인터페이스에 선언된 모든 메소드를 구현해야 한다.
<br>
또한, 인터페이스 내부의 변수가 static final로 설정된다. (생략하더라도 자동으로 public statid final이 붙는다.)
<br>
이는 곧, 해당 인터페이스를 구현하는 클래스는 선언된 변수 값을 변경할 수 없다는 것과 같다. 위 예시에서 GasolineCar 클래스는 variable 변수를 반드시 값 10으로 사용할 수밖에 없다.

### 인터페이스와 추상 클래스의 차이
<h5>implements와 extends</h5>

- 인터페이스의 구현체임을 나타내는 implements 키워드 뒤에는 여러 인터페이스가 올 수 있다.
- 즉, 한 클래스는 여러 인터페이스의 구현체가 될 수 있다.'
- 하지만 추상 클래스는 클래스 간의 상속을 통해 추상 메소드를 이어받기 때문에, 하나의 클래스는 하나의 추상 클래스를 상속받을 수 있다.

### 추상 클래스를 사용하는 이유??
만약 Car 인터페이스를 구현하는 클래스의 종류가 100가지 정도 된다고 가정해 보자

<br>
만약 이 모든 Car 인터페이스를 구현하는 클래스들은 Ride 메소드가 동일한 방식으로 작동한다 해도, 우리는 같은 내용의 Ride 메소드를 100번 작성해야 한다.
인터페이스에 선언된 모든 메소드는 내용이 없기 때문이다.

``` java
public class GasolineCar implements Car{
    public void Ride(){
        //구현 내용
    }
    //(생략)
}

public class GasCar implements Car{
    public void Ride(){
        //구현 내용
    }
    //(생략)
}

public class ElectricCar implements Car{
    public void Ride(){
        //구현 내용
    }
    //(생략)
}

public class HydroCar implements Car{
    public void Ride(){
        //구현 내용
    }
    //(생략)
}
```

하지만 Car 추상 클래스를 상속받는다면, Ride와 같이 공통적인 메소드들을 반복해서 구현하지 않아도 된다.
<br>
따라서 의도하고자 하는 설계에 따라 이들을 적적하게 잘 사용하는 것이 중요하다.

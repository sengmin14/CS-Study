# 🥕 equals와 hashCode는 왜 같이 재정의해야 할까?

## equals만 재정의 할 경우
``` java
public class Car {
    private final String name;

    public Car(String name) {
        this.name = name;
    }

    // intellij Generate 기능 사용
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Car car = (Car) o;
        return Objects.equals(name, car.name);
    }
}
```

- Car 클래스에는 equals만 재정의해두었다.

``` java
public static void main(String[] args){
    Car car1 = new Car("foo");
    Car car2 = new Car("foo");
    
    // true 출력
    System.out.println(car1.equals(car2));
}
```

- equals를 재정의했기 때문에 Car 객체의 name이 같은 car1, car2 객체는 논리적으로 같은 객채로 판단된다.
- 이제 아래 main 메서드의 출력 결과를 예측해보자.

``` java
public static void main(String[] args) {
    List<Car> cars = new ArrayList<>();
    cars.add(new Car("foo"));
    cars.add(new Car("foo"));

    System.out.println(cars.size());
}
```

- Car 객체를 2개 List<Car> cars에 넣어줬으니 출력 결과는 당연히 2 일 것이다.
- 그렇다면 이번엔 Collection에 중복되지 않는 Car 객체만 넣으라는 요구사항이 추가되었다고 가정해보자.
- 요구사항을 반영하기 위해 List에서 중복 값을 허용하지 않는 Set으로 로직을 바꿨다. 마찬가지로 아래 main 메서드의 출력 결과를 예측해보자.

``` java
public static void main(String[] args) {
    Set<Car> cars = new HashSet<>();
    cars.add(new Car("foo"));
    cars.add(new Car("foo"));

    System.out.println(cars.size());
}
```

- 추가된 두 Car 객체의 이름이 같아서 논리적으로 같은 객체라 판단하고 HashSet의 size가 1이 나올 거라 예상했지만, 예상과 다르게 2가 출력된다.
- hashCode를 equals와 함께 재정의하지 않으면 코드가 예상과 다르게 작동하는 위와 같은 문제를 일으킨다. 정확히 말하면 hash 값을 사용하는 Collection(HashSet, HashMap, HashTable)을 사용할 때 문제가 발생한다.

### 이유
- 앞서 말한 hash 값을 사용하는 Collection(HashMap, HashSet, HashTable)은 객체가 논리적으로 같은지 비교할 때 아래 그림과 같은 과정을 거친다.

![image](https://github.com/user-attachments/assets/6c6c30ba-a6e3-46c9-a58e-f8cb99b06eb9)

## hashCode 재정의
- 앞서 살펴봤던 문제를 해결하기 위해 Car 클래스에 hashCode 메서드를 재정의해보겠다.

``` java
public class Car {
    private final String name;

    public Car(String name) {
        this.name = name;
    }

    // intellij Generate 기능 사용
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Car car = (Car) o;
        return Objects.equals(name, car.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
}
```

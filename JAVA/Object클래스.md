# 🥕 Object클래스
- Object클래스는 자바에서 모든 클래스들의 최상위 조상 클래스이다.
- 따로 선언하지 않아도 기본적으로 Object클래스의 속성을 상속받게 된다.
- Object클래스는 11개의 메소드가 포함되어 있다.

## Object클래스의 메소드 11가지
- int  hashCode() :  현재 객체의 해쉬코드 값을 반환합니다.
- String  toString() :  현재 객체의 문자열로된 표현값을 반환합니다.
- boolean  equals (Object obj) :  obj객체와 현재객체가 같은지 비교하여 결과를 반환합니다. (같으면 true, 다르면 false)
- final Class<?>  getClass( ) :  현재 객체의 클래스 정보를 담은 Class타입의 객체를 반환합니다.
- protected Object  clone( ) :  현재 객체의 복사본을 생성후 반환합니다. (Cloneable 인터페이스를 구현한 클래스만 복사 가능함)
- final void  notify() :  현재 객체를 사용하기 위해 대기하고 있는 쓰레드 하나를 깨웁니다.
- final void  notifyAll() :  현재 객체를 사용하기 위해 대기하고 있는 모든 쓰레드를 깨웁니다.
- final void  wait() :  다른 쓰레드가 깨울때까지 현재 쓰레드를 대기시킵니다.
- final void  wait(long timeoutMillis) :  다른  쓰레드가 깨우거나 정해진 시간만큼 현재 쓰레드를 대기시킵니다.
- final void  wait(long timeoutMillis, int nanos) :  다른  쓰레드가 깨우거나 정해진 시간만큼 현재 쓰레드를 대기시킵니다.
- protected void  finalize( ) :  객체가 소멸되기 전 자동으로 호출되는 메소드로, 현재는 Deprecated 되어 사용하지 않습니다. (삭제예정)


### equals() 메소드
- boolean equals(Object obj) 메소드는 obj 객체와 자신의 객체가 같은지 비교하는 메소드이다.
- 두 객체가 같으면 true, 다르면 false를 반환한다.

``` java
public boolean equals(Object obj) {
        return (this == obj);
}
```

- Object클래스에 정의되어 있는 equals() 메소드의 소스코드를 살펴보면 두 객체를 비교연산자(==)를 사용하여 비교한후 그 결과를 반환하게 되어 있는것을 알 수 있다.
- 우리가 보통 기본타입(primitive type)에서 두 값이 같은지를 비교하기 위해서는 비교연산자(==)를 사용합니다. 하지만 객체의 경우 참조타입(reference type)이기 때문에 비교연산자(==)를 사용하게 되면 객체의 주소값을 비교하여 두 객체가 같은 객체인지를 비교하게 된다. 
- 따라서 두 객체의 주소값에 대한 비교가 아니라 두 객체가 가지고 있는 변수의 값이 같은지 여부를 비교하기 위해서는 equlas() 메소드를 오버라이드 하여 우리가 원하는 비교 방법을 새롭게 구현하여 사용해야 한다.
- cf) 두 객체의 equals()의 결과가 같은 경우 해시코드도 같게 하기 위해 보통 equals()와 hashCode()를 함께 오버라이드 한다. 그렇지 않으면 hash값을 사용하는 곳에서 예상과 다르게 동작할 우려가 있는지 확인후 사용하시기 바람. 

#### 클래스 정의 및 equals()메소드 오버라이드
``` java
public class Person {		
	private int age;
	
	public Person(int age) {
		this.age = age;
	}

	@Override
	public boolean equals(Object obj) {		// equals() 오버라이드
		if (obj instanceof Person) {		// 형변환 체크
			Person person = (Person) obj;	// 형변환(다운캐스팅)
			return age == person.age;
		} else {
			return false;			
		}
	}
    
	@Override
	public int hashCode() {			// 보통 equals()와 함께 오버라이드함
		return Objects.hash(age);
	}
}
```

#### 객체생성 및 객체비교
``` java
public class HelloWorld {
	public static void main(String[] args) {
		
		 Person p1 = new Person(10);
		 Person p2 = new Person(10);
		 
		 System.out.println("p1==p2: " + (p1==p2));
		 System.out.println("p1.equals(p2): " + p1.equals(p2));
	}
}
```

#### 실행결과
``` java
p1==p2: false
p1.equals(p2): true
```

### getClass() 메소드
- final Class<?> getClass() 메소드는 객체의 클래스 정보를 알수 있는 메소드로, 해당 정보를 Class타입의 객체로 반환합니다. 반환된 Class타입의 객체는 클래스 정보에 접근할 수 있는 메소드를 가지고 있고, 우리는 이 메소드들을 통해 클래스 정보를 확인할 수 있다.
```
getName() :  클래스의 이름을 반환하는 메소드
getSuperclass() :  부모클래스의 이름을 반환하는 메소드
getDeclaredFields() :  클래스에 선언되어있는 멤버변수이름을 배열로 반환하는 메소드
getDeclaredMethods() :  클래스에 선언되어 있는 멤버함수이름을 배열로 반환하는 메소드
```

#### 클래스 정의
``` java
public class Person {
	public String name;
	private int age;
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
	public void speak() {
		System.out.println(name + "입니다");
	}
}
```

#### 객체생성 및 객체 정보 확인
``` java
public class HelloWorld {
	public static void main(String[] args) {
		
		 Person p1 = new Person("홍길동", 10);
		 Class c1 = p1.getClass();
		 
		 System.out.println(c1.getName());
		 System.out.println(c1.getSuperclass().getName());
		 
		 Field[] field = c1.getDeclaredFields();
		 for (int i=0; i<field.length; i++) {
			 System.out.println("멤버변수" + i + ": " + field[i]);
		 }

		 Method[] method = c1.getDeclaredMethods();
		 for (int i=0; i<method.length; i++) {
			 System.out.println("멤버함수" + i + ": " + method[i]);
		 }
	}
}
```

#### 실행결과
``` java
com.study.Person
java.lang.Object
멤버변수0: public java.lang.String com.study.Person.name
멤버변수1: private int com.study.Person.age
멤버함수0: public void com.study.Person.speak()
```

### toString() 메소드
- String toString() 메소드는 객체를 문자열로 표현된 정보값으로 반환한다.
- Object클래스에 정의되어 있는 toString메소드를 살펴보면 아래와 같이 "클래스이름@해시코드값(16진수)"을 반환하도록 되어있다.
- (toString 기본 반환값 예시)  com.study.Person@1c4af82c
- toString() 메소드 역시 사용자가 유의미한 데이터를 반환하도록 오버라이드하여 사용하는 것이 일반적이다.

#### 클래스 정의 및 toString 오버라이딩
``` java
public class Person {
	public String name;
	private int age;
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
}
```

#### 객체생성 및 toString 실행
``` java
public class HelloWorld {
	public static void main(String[] args) {
		
		 Person p1 = new Person("홍길동", 10);
		 Person p2 = new Person("이순신", 20);
		 
		 System.out.println(p1.toString());
		 System.out.println(p2.toString());
	}
}
```

#### 실행결과
``` java
Person [name=홍길동, age=10]
Person [name=이순신, age=20]
```

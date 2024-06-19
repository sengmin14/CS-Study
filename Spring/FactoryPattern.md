## 🥕 1. 팩토리 패턴(Factory Pattern)


- 팩토리 패턴은 생성패턴(Creational Pattern) 중 하나이다.
- 팩토리 패턴은 객체를 생성하는 인터페이스를 미리 정의하지만, 인스턴스를 만들 클래스의 결정은 서브 클래스가 결정하는 패턴이다.
- 여러개의 서브 클래스를 가진 슈퍼 클래스가 있을떄, 들어오는 인자에 따라서 하나의 자식클래스의 인스턴스를 반환해주는 방식이다.
- 팩토리 패턴은 클래스의 인스턴스를 만드는 시점 자체를 서브 클래스로 미루는 것이다.

### 활용성
- 어떤 클래스가 자신이 생성해야 하는 객체의 클래스를 예측할 수 없을 때
- 생성할 객체를 기술하는 책임을 자신의 서브클래스가 지정했으면 할 


## Code

### 참고자료 (https://www.youtube.com/watch?v=QOX10ntWj5Y&t=651s)

### Diagram
![image](https://github.com/sengmin14/CS-Study/assets/140876841/fdeedba9-3be7-4af0-90ab-a266f93ba4d6)

### Item Interface

``` java
public interface Item {
	
	void use();
}
```

### Sword Class

``` java
public class Sword implements Item {
	@Override
	public void use() {
		System.out.println("Sword");
	}
}
```

### Factroy Class

``` java
public abstract class Factory {
	
	public Item create(String name) {
		boolean bCreatable = this.isCreatable(name);
		if(bCreatable) {
			Item item = this.createItem(name);
			postprocessItem(name);
			return item;
		}
		return null;
	}

	public abstract void postprocessItem(String name);

	public abstract Item createItem(String name);

	public abstract boolean isCreatable(String name);
}

```

### ItemFactory class

``` java
import java.util.HashMap;

public class ItemFactory extends Factory {
	
	private class ItemData {
		int maxCount;
		int currentCount;
		public ItemData(int maxCount) {
			this.maxCount = maxCount;
		}
	}
	
	private HashMap<String, ItemData> repository;
	
	public ItemFactory() {
		repository = new HashMap<>();
		repository.put("sword", new ItemData(3));
		repository.put("shield", new ItemData(2));
		repository.put("bow", new ItemData(1));
	}
	
	@Override
	public void postprocessItem(String name) {
		ItemData itemData = repository.get(name);
		if(itemData != null) itemData.currentCount++;
		
	}

	@Override
	public Item createItem(String name) {
		Item item = null;
		
		if(name == "sword") item = new Sword();
		if(name == "shield") item = new Shield();
		if(name == "bow") item = new Bow();
		
		return item;
	}

	@Override
	public boolean isCreatable(String name) {
		ItemData itemData = repository.get(name);
		
		if(itemData == null) {
			System.out.println(name + "알 수 없는 아이템");
			return false;
		}
		if(itemData.currentCount >= itemData.maxCount) {
			System.out.println(name + "품절");
			return false;
		}
		return true;
	}
}
```

### Main class

``` java
public class Main {
	
	public static void main(String[] args)
	{
		Factory factory = new ItemFactory();
		
		
		Item item1 = factory.create("sword");
		if(item1 != null) item1.use();
		
		Item item2 = factory.create("sword");
		if(item2 != null) item2.use();
		
		Item item3 = factory.create("sword");
		if(item3 != null) item1.use();
		
		Item item4 = factory.create("sword");
		if(item4 != null) item2.use();
		
		Item item5 = factory.create("sword");
		if(item5 != null) item1.use();
		
		Item item6 = factory.create("sword");
		if(item6 != null) item2.use();
	}
}
```

### 출력
```
Sword
Sword
Sword
sword품절
sword품절
sword품절
```

### 팩토리 메서드 패턴의 장점
- 생성자와 구현 객체의 강한 결합을 피할 수 있다.
- 팩토리 메서드를 통해 객체의 생성 후 공통으로 할 일을 수행하도록 지정해줄 수 있다.
- 캡슐화, 추상화를 통해 생성되는 객체의 구체적인 타입을 감출 수 있다.
- 단일 책임 원칙 준수 : 객체 생성 코드를 한 곳 (패키지, 클래스 등)으로 이동하여 코드를 유지보수하기 쉽게 할 수 있다.
- 개방/폐쇄 원칙 준수 : 기존 코드를 수정하지 않고 새로운 유형의 제품 인스턴스를 프로그램에 도입할 수 있다.
- 생성에 대한 인터페이스 부분과 생성에 대한 구현 부분을 따로 나뉘었기 때문에 패키지 분리하여 개별로 여러 개발자가 협업을 통해 개발

### 팩토리 메서드 패턴의 단점
- 각 제품 구현체마다 팩토리 객체들을 모두 구현해주어야 하기 때문에, 구현체가 늘어날때 마다 팩토리 클래스가 증가하여 서브 클래스 수가 증가한다.
- 코드의 복잡성이 증가한다.

### 싱글턴 ? 
* 싱글턴이란 인스턴스를 오직 하나만 만들 수 있는 클래스를 말한다. (함수와 같은 무상태 객체나, 설계상 유일해야 하는 시스템 컴포넌트 등)

### 싱글턴을 만드는 두가지 방법

* 두 가지 방법 모두 생성자를 private 으로 감춰두고 유일한 인스턴스에 접근할 수 있는 수단으로 public static 멤버를 하나 마련해둔다.

#### 1. public static 멤버를 final 필드로 두는 방식. (Better)
``` java
public class Elvis {
	public static final Elvis INSTANCE = new Elvis();
	private Elvis() {...}
	public void leaveTheBuilding() {...}
}
```

* 해당 클래스가 싱글턴임이 API 에 명확하게 드러난다. public static 필드가 final 이니 절대로 다른 객체를 참조할 수 없다.
* 간결하다.
#### 2. 정적 팩터리 메서드를 public static 멤버로 제공하는 방식.
```java
	private static final Elvis INSTANCE = new Elvis();
	private Elvis() {...}
	public static Elvis getInstance() { return INSTANCE; }
	public void leaveTheBuilding() {...}
```

* 객체를 싱글턴이 아니게 변경할 수 있다. (유연하다)
* 정적 팩터리를 싱글턴 팩터리로 만들 수 있다.
* 정적 팩터리의 메서드 참조를 공급자(supplier)로 사용할 수 있다는 점.
* 위 장점들이 굳이 필요한 경우 제외하고는 public 필드 방식이 좋다.

#### 3. 열거 타입의 싱글턴 (바람직한 방법)
```java
public enum Elvis {
	INSTANCE;
	
	public void leaveTheBuilding() {...}
}
```

* public 필드 방식과 유사하지만 더 간결하고 추가 노력 없이 직렬화할 수 있고, 아주 복잡한 직렬화 상황이나 리플렉션 공격에서도 제2의 인스턴스가 생기는 일을 완벽하게 막아준다.
* 대부분의 상황에서는 원소가 하나뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법이다.
### 싱글턴 객체를 직렬화 하기

* 싱글턴 객체를 직렬화하려면 Serializable 구현을 선언하는 것 만으로는 부족하다. 모든 인스턴스 필드를 일시적(transient)이라고 선언하고 readReslove 메서드를 제공해야 한다. ([ITEM 89]())
```java
private Object readResolve() {
	return INSTANCE;
}
```
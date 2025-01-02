## 문제사항 인지
* 정적 팩터리 메서드와 생성자는 매개변수가 많을 때 적절히 대응하기 어렵다는 동일한 제약이 있다.

## 대안
#### 1. 점층적 생성자 패턴

```java

public NutritionFacts(int servingSize, int servings, int calories, int fat) {
	this(servingSize, servings, calories, fat, 0)
}

public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium) {
	this(servingSize, servings, calories, fat, sodium, 0)
}

```

* 매개변수가 많아지면 클라이언트 코드를 작성하거나 읽기 어렵다. 각 값의 의미가 무엇인지 헷갈리고, 매개변수의 개수도 세어보아야 한다. 같은 타입의 매개변수는 값을 바꾸어 호출해도 컴파일러는 알아채지 못한다.

#### 2. 자바 빈즈 패턴

```java
public class NutritionFacts {
	private int servingSize = -1;
	private int servings = -1;
	private int calories = 0;

	public NutritionFacts() {}
	public void setServingSize(int val) {
		this.servingSize = val;
	}
	public void setServing(int val) {
		this.serving = val;
	}
	public void setCalories(int val) {
		this.calories = val;
	}
}
```

* 자바 빈즈 패턴은 객체 하나를 만들기 위해서 여러 메서드를 호출해야 하며, 모든 메서드가 호출되기 전 까지는 객체는 일관성(consistency)이 무너진 상태에 놓이게 된다.

#### 3. 빌더 패턴
```java

public class NutritionFacts {
	private final int servingSize;
	private final int servings;
	private final int calories;
	private final int fat;
	private final int sodium;
	private final int carbohydrate;

	private NutritionFacts(Builder builder) {
		servingSize = builder.servingSize;
		serving = builder.serving;
		calories = builder.calories;
		fat = builder.fat;
		sodium = builder.sodium;
		carbohydrate = builder.carbohydrate;
	}

	public static class Builder {
		// 필수 매개변수
		private final int servingSize;
		private final int servings;
		// 선택 매개변수
		private int calories = 0;
		private int fat = 0;
		private int sodium = 0;
		private int carbohydrate = 0;

		public Builder(int servingSize, int serving) {
			this.servingSize = servingSize;
			this.serving = serving;
		}

		public Builder calories(int val) { calories = val; return this;}
		public Builder fat(int val) { fat = val; return this;}
		public Builder sodium(int val) { sodium = val; return this;}
		public Builder carbohydrate(int val) { carbohydrate = val; return this;}

		public NutritionFacts build() {
			return new NutritionFacts(this);
		}
	}
}

// 클라이언트 코드
NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8)
								.calories(100)
								.sodium(35)
								.carbohydrate(27)
								.build();

```

* 빌더 패턴은 명명된 선택적 매개변수를 흉내 낸 것이다.
* 잘못된 매개변수에 대한 확인은 build 메서드에서 호출하는 생성자에서 여러 매개변수에 걸친 불변식을 검사하자. 만약 잘못된 매개변수가 발견된 경우, 어떤 매개변수가 잘못됐는지를 자세히 알려주는 메시지를 담아 IllegalArgumentException을 던지면 된다.

>[! note] 핵심정리
>생성자나 정적 팩터리가 처리해야 할 매개변수가 많다면 빌더 패턴을 선택하는 게 더 낫다. 매개변수 중 다수가 필수가 아니거나 같은 타입이면 특히 더 그렇다. 빌더는 점층적 생성자보다 클라이언트 코드를 읽고 쓰기가 훨씬 간결하고, 자바빈즈 패턴보다 훨씬 안정적이다.


## 장점

### 1. 이름을 가질 수 있다.
* 정적 팩터리 메서드는 이름만 잘 지으면 반환될 객체의 특성을 쉽게 묘사할 수 있다.
### 2. 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.
* 생성 비용이 큰 객체 생성이 자주 요청되는 상황에서는 성능을 상당히 끌어올릴 수 있다.
* 반복되는 요청에 같은 객체를 반환하는 식으로 언제 어느 인스턴스를 살아 있게 할지를 철저히 통제할 수 있다. (싱글턴 / 인스턴스화 불가 클래스 등)
### 3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
* 반환할 객체의 클래스를 자유롭게 선택하게 할 수 있는 **유연성을 제공한다.**
* 자바 컬렉션 프레임워크는 45개의 유틸리티 클래스를 공개하지 않고, java.util.Collections 에서 정적 팩터리 메서드를 통해 얻도록 했다. 이는 API의 외견을 훨씬 작게 만들 수 있도록 한다. 이 API 를 사용하는 개발자들이 익혀야 하는 개념의 수와 난이도를 낮췄다.
### 4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
* 반환 타입의 하위 타입이기만 하면 어떤 클래스를 반환하든 상관없다.
### 5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.
* 이러한 유연함은 Service Provider Framework 를 만드는 근간이 된다.

## 단점
### 1. 상속을 하려면 public 이나 protected 생성자가 필요하니, 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
* 상속은 유연성을 떨어뜨리기 때문에 조합(composition)을 사용해야한다는 관점에서 보면 오히려 장점으로 받아들일 수도 있다.
### 2. 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.
* 이를 해결하기 위한 정적 팩터리 메서드의 명명 규칙이 있다.
``` java
// from : 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드
Date d = Date.from(instant);

// of : 여러 매개변수를 받아서 적합한 타입의 인스턴스를 반환하는 집계 메서드
Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);

// valueOf: from과 of 의 더 자세한 버전
BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);

// instance 혹은 getInstance : 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임은 보장하지 않는다.
StackWalker luke = StackWalker.getInstance(options);

// create 혹은 newInstance : 매번 새로운 인스턴스를 생성해 반환함을 보장한다.
Object newArray = Array.newInstance(classObject, arrayLen);

// getType : getInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. "Type" 은 팩터리 메서드가 반환할 객체의 타입이다.
FileStore fs = Files.getFileStore(path);

// newType : newInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 저으이할 때 쓴다. "Type" 은 팩터리 메서드가 반환할 객체의 타입이다.
BufferedReader br = Files.newBufferedReader(path);

// type : getType 과 newType 의 간결한 버전
List<Complaint> litany = Collections.list(legacyLitany);

```


>[!note ] 핵심정리
>정적 팩터리 메서드와 public 생성자는 각자의 쓰임새가 있으니, 상대적인 장단점을 이해하고 사용하는 것이 좋다. 그렇다고 하더라도 정적 팩터리를 사용하는 게 유리한 경우가 더 많으므로 무작정 public 생성자를 제공하던 습관이 있다면 고치자.




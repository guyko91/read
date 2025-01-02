
### 문제 인식

* 특정 자원에 의존하는 클래스가 있을 때, 해당 자원이 여러 자원 인스턴스를 지원해야하고, 특히 클라이언트가 원하는 자원을 사용해야 할 때.
```java
// 맞춤법 검사기 SpellChecker 클래스
public class SpellChecker {
	private final Lexicon dictionary = ...;
	
	private SpellChecker(...) {}
	public static SpellChecker INSTANCE = new SpellChecker(...);
}

public class SpellChecker {
	private Lexicon dictionary = ...;
	
	private SpellChecker(...) {}
	public static SpellChecker INSTANCE = new SpellChecker(...);
	public void changeDictionary(Lexicon lexicon) {...}
}
```
* Lexicon 에 final 키워드를 제거하고 다른 사전으로 교체하는 메서드를 추가하는 것은 어색하고, 오류를 발생시키기 쉬우며, 멀티스레드 환경에서는 사용할 수 없다.

### 인스턴스를 생성할 때, 생성자에 필요한 자원을 받아 생성하자

```java
// 객체주입의 한 형태(생성자 주입)로, 맞춤법 검사기를 생성할 때 의존 객체인 사전을 주입해주면 된다.
public class SpellChecker {
	private final Lexicon dictionary = ...;

	public SpellChecker(Lexicon dictionary) {
		this.dictionary = Objects.requireNonNull(dictionary);
	}
	public boolean isValid(String word) {...}
	public List<String> suggestions(String typo) {...}
}
```

* 유연성과 테스트 용이성을 높여준다.
* 불변을 보장하여 여러 클라이언트가 의존 객체들을 안심하고 공유할 수 있기도 하다.
* 의존 객체 주입은 생성자, 정적 팩터리, 빌더 모두에 똑같이 응용할 수 있다.

>[! note] 핵심 정리
>클래스가 내부적으로 하나 이상의 자원에 의존하고, 그 자원이 클래스 동작에 영향을 준다면 싱글턴과 정적 유틸리티 클래스는 사용하지 않는 것이 좋다.
>이 자원들을 클래스가 직접 만들게 해서도 안 된다. 대신 필요한 자원을 (혹은 그 자원을 만들어주는 팩터리를) 생성자에 (혹은 정적 팩터리나 빌더에) 넘겨주자. 의존 객체 주입이라 하는 이 기법은 클래스의 유연성, 재사용성, 테스트 용이성을 기막히게 개선해준다.


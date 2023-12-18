# Generic

---

### Generic 정의

데이터 형식에 의존하지 않고, 하나의 값이 여러 다른 데이터 타입들을 가질 수 있도록 하는 방법.

클래스 내부에서 지정하는 것이 아닌 외부에서 사용자에 의해 지정되는 것.

---

### Generic의 장점

잘못된 타입이 들어올 수 있는 것을 컴파일 단계에서 방지할 수 있음.

클래스 외부에서 타입을 지정해주기 때문에 따로 타입을 체크하고 변환해줄 필요가 없음(관리에 용이).

비슷한 기능을 지원하는 경우, 코드의 재사용성이 높아짐.

- 어떤 자료구조를 만들어 배포하고 싶을 때, String 타입도 지원하고 싶고 Integer 타입도 지원하고 싶다면 그냥 Generic을 이용하여 하나만 만들어주면 됨.

---

### Generic 사용방법

```java
<T> // Type
<E> // Element
<K> // Key
<V> // Value
<N> // Number
```
</br>

**클래스 및 인터페이스 선언**

```java
// T 타입은 해당 블록 {...} 안에서까지 유효함
public class ClassName<T> {...}
public interface InterfaceName<T> {...}

// Generic 타입을 두 개로 구분 가능
public class ClassName<T, K> {...}
public Class HashMap<K, V> {...}

// Generic을 활용한 객체 생성
// T = String, K = Integer
ClassName<String, Integer> ref = new ClassName<>();

// Generic 활용
class ClassName<E> {
	private E element; // Generic 타입 변수
	
	void set(E element) { // Generic 파라미터 메소드
		this.element = element;
	}

	E get() { // Generic 반환 메소드
		return element;
	}
}
```

타입 파라미터로 명시할 수 있는 것은 참조 타입밖에 없음(primitive type 불가).

따라서 primitive type은 Wrapper Type으로 감싸주어야 함.

> **Wrapper Class**
> 
> 기본타입의 데이터를 객체로 취급해야 하는 경우, 기본 타입의 데이터를 객체로 변환해주는 클래스.</br>
> 기본 자료형과 달리 null값을 지원.</br>
> 기본 자료형과 달리 서로 다른 데이터의 값을 비교하기 위해서는 ==(동일 연산자)가 아닌 equals()(동등연산자)를 사용해야 함.
</br></br>

> **동일성 vs 동등성**
> 
> 동일성 : A == B, A와 B의 주소가 같은가?
> 동등성 : A.equals(B), A와 B의 값이 같은가?

**제네릭 메소드**

```java
// 메소드에 한정한 Generic 사용 : 반환타입 이전에 Generic 타입 선언
// 접근제어자 <Generic> 반환타입 메소드명(Generic 파라미터)
public <T> T genericMethod(T o) {...}

// Generic 메소드 사용
genericMethod(3).getClass().getName()   // Integer
genericMethod("3").getClass().getName() // String
```

제네릭 메소드는 정적 메소드로 선언시 필요.

- 클래스의 인스턴스가 생성되기 이전에 메소드의 반환타입이 결정되어야 하는데, 정적 메소드는 인스턴스 생성 없이(제네릭 타입 결정 시점) 반환타입이 결정되어야 하기 때문.

따라서 이러한 경우, 메소드가 속한 클래스의 제네릭 타입과 별도의 제네릭 타입이 선언되어야 함.
</br></br>

**제한된 제네릭과 와일드 카드**

```java
// T와 T의 자손 타입만 가능(K는 들어오는 타입으로 지정됨)
<K extends T>
// T와 T의 부모(조상) 타입만 가능(K는 들어오는 타입으로 지정됨)
<K super T>

// T와 T의 자손 타입만 가능
<? extends T>
// T와 T의 부모(조상) 타입만 가능
<? super T>
// 모든 타입 가능 → <? extends Object>와 동일
<?>

/*
 * Number와 이를 상속하는 Integer, Short, Double, Long 등의 타입이 지정될 수 있으며,
 * 객체 혹은 메소드를 호출할 경우 K는 지정된 타입으로 변환됨
 */
<K extends Number>

/*
 * Number와 이를 상속하는 Integer, Short, Double, Long 등의 타입이 지정될 수 있으며,
 * 객체 혹은 메소드를 호출할 경우 지정되는 타입이 없어 타입 참조는 불가
 */
<? extends Number>
```

? : 와일드 카드로, 알 수 없는 타입을 의미.

extends T : 상한선.

super T : 하한선.

K는 특정 타입으로 지정 되지만, ? 는 타입이 지정되지 않음.

> 보통 데이터가 아닌 기능의 사용에만 관심이 있는 경우, <?>로 사용가능.
>

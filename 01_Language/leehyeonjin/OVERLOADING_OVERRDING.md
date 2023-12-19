# Overriding vs Overloading

---

### Overriding

부모 클래스로부터 상속받은 메소드를 자식 클래스에서 재정의하는 것.

오버라이딩하고자 하는 메소드의 이름, 매개변수, 리턴값이 모두 같아야 함.

런타임 다형성.

```java
public class Person {
	public void cry() {
		System.out.println("흑흑");
	}
}

public class Child extends Person {
	@Override
	public void cry() {
		System.out.println("잉잉");
	}
}
```

---

### Overloading

클래스 내에 이미 사용하려는 이름과 같은 이름을 가진 메소드가 있더라도 매개변수의 개수 또는 타입이 다르면, 같은 이름을 사용해서 메소드를 재정의하는 것.

메소드의 이름이 같고, 매개변수의 개수나 타입이 달라야 함.

컴파일 타임 다형성.

리턴값만 다른 것은 오버로딩 불가.

접근제어자도 자유롭게 지정가능.

> 오버로딩은 매개변수의 차이로만 구현가능.
>

```java
public Class Print {
	public void print() {
		System.out.println("overloading1");
	}

	public String print(int num) {
		System.out.println("overloading2");
		return num;
	}

	void print(String str) {
		System.out.println("overloading3" + str);
	}

	String print(int n1, int n2) {
		System.out.println("overloading4");
		return n1 + n2;
	}
}
```

---

### Overriding vs Overloading

| 구분 | Overriding | Overloading |
| --- | --- | --- |
| 접근 제어자 | 부모 클래스 메소드의 접근 제어자보다 더 넓은 범위의 접근 제어자를 자식 클래스의 메소드에 설정. | 모든 접근 제어자 사용가능. |
| 리턴형 | 동일. | 달라도 됨. |
| 메소드명 | 동일 | 동일. |
| 매개변수 | 동일. | 달라야 함. |
| 적용범위 | 상속관계. | 같은 클래스. |

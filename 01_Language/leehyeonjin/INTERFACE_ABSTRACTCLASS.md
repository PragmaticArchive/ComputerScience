# Interface vs Abstract Class

---

### Abstract Class

하나 이상의 추상 메소드를 포함하는 클래스.

클래스의 선언부에 abstract 키워드가 있다는 말은 내부에 추상 메소드가 있으니, 상속을 통해서 구현한 후 사용하라는 지침이기도 함.
</br></br>

**추상 메소드**

메소드의 선언부만 작성하고, 구현부는 미완성인 채로 남겨둔 메소드.

메소드를 미완성으로 남겨두는 이유는 추상 클래스를 상속받는 자식 클래스의 주제에 따라서 상속받는 메소드의 내용이 달라질 수 있기 때문.

따라서 추상 메소드는 이를 상속받는 자식이 상황에 맞게 적절히 재정의하여 구현해 주어야 비로소 사용이 가능해 짐.

추상 클래스는 추상 메소드를 포함하고 있다는 것을 제외하고는 일반 클래스와 전혀 다르지 않음.

추상 클래스에도 생성자가 있으며 독립적인 인스턴스 멤버 변수와 메소드도 가질 수 있음.
</br></br>

**추상 클래스 생성자**

추상 클래스는 new 생성자를 통해 인스턴스 객체로 직접 생성 불가.

- 추상 클래스는 상속 구조에서 부모 클래스를 나타내는 역할로만 이용되기 때문.
- 따라서 반드시 어느 자식의 클래스에 상속시키고, 자식 클래스를 인스턴스화 하여 사용해야 함.

```java
abstract class Animal {
}

// ERROR!! : 추상 클래스는 인스턴스를 직접 생성 불가
Animal animal = new Animal();

// 추상 클래스 상속
class Cat extends Animal {
}

// 추상 클래스를 상속한 자식 클래스를 인스턴스로 초기화
Cat cat = new Cat();
```

직접적인 인스턴스화는 불가능하지만, super() 메소드를 이용하여 추상 클래스의 생성자 호출이 가능함.

- 부모 추상 클래스 생성자 실행시 인자를 주어 제어하고 싶다면, 자식 클래스 생성자 메소드 내에서 super() 부모 생성자 호출을 통해 제어 가능.

```java
// 추상 클래스
abstract class Shape {
	public String type;
    
    // 추상 클래스 생성자
    public Shape(String type) {
    	this.type = type;
    }
    
    // 추상 메서드
    public abstract void draw();
}

class Figure extends Shape {
	public String name;
    
    public Figure(String type1, String type2) {
			// 부모 추상 클래스 생성자 호출
    	super(type1);
        name = type2;
    }
    
		// 추상 메서드 구현
    @Override
    public void draw() { ... }
}
```
</br></br>

**추상 클래스 활용**

1. 공통 멤버의 통합으로 중복 제거
- 사용처와 이름이 겹치는 필드와 메소드들이 있는 클래스들을 대표할 수 있는 부모 추상 클래스로 묶으면, 코드의 중복 제거 및 재사용성 향상 효과가 있음.
- 이때 자식 클래스마다 로직이 다른 기능에 대해서는 추상 메소드로 선언.

> 자식 클래스의 멤버 변수를 통합 시켜주는 기능 자체는 인터페이스는 불가하고, 추상 클래스(클래스)만 가능함.
>

1. 구현의 강제성을 통한 기능 보장
- Java에서 추상 클래스와 추상 메소드를 선언하여 사용하는 목적은 추상 클래스를 상속받는 자식 클래스가 반드시 추상 메소드를 구현하도록 하기 위함.
- 강제적으로 구현하도록 멘토링 하기 때문에 개발자 실수를 없앨 수 있음.

> 일반 클래스로 상속 관계를 맺는다면, 반드시 수정해야하는 부모 클래스의 메소드를 따로 구현하지 않아도 별다른 에러가 없어 개발자가 실수할 수도 있음.

---

### Interface

추상 메소드의 집합이며, 필드를 선언할 수는 있으나 변수가 아닌 상수(final)로서만 정의 가능.

- 따라서 인터페이스의 public static final(상수)과 public abstract(추상 메소드) 키워드는 생략 가능.
- 인터페이스에 정의된 모든 멤버에 적용되는 사항이기 때문에 편의상 생략가능하며, 컴파일 시에 자동으로 추가됨.

```java
interface 인터페이스이름{
    public static final 타입 상수이름 = 값;
    public abstract 타입 메서드이름(매개변수목록);
}

interface TV {
		// public static final 생략 가능
    int MAX_VOLUME = 10;
    int MIN_VOLUME = 10;

		// public abstract 생략 가능
    void turnOn();
    void turnOff();
    void changeVolume(int volume);
    void changeChannel(int channel);
}
```

그 자체로는 인스턴스를 생성불가하며, 구현부를 만들어주는 클래스에 상속되어야 함.

인터페이스는 다중 상속이 가능함.

또한 자식 클래스에 클래스 상속(extends)과 인터페이스 구현(implements)이 동시에 가능함.
</br></br>

**인터페이스 일부 구현(추상 클래스)**

만일 인터페이스를 상속받은 자식 클래스가 인터페이스의 메소드 중 일부만 구현한다면, abstract를 붙여서 추상 클래스로 선언해야 함.

```java
interface Animal {
    void walk();
    void run();
    void breed();
}

// Animal 인터페이스를 일부만 구현하는 포유류 추상 클래스
abstract class Mammalia implements Animal {
    public void walk() { ... }
    public void run() { ... }
		
		// 자식 클래스에서 구체적으로 구현하도록 일부로 구현하지 않음 (추상 메서드로 처리)
    public abstract void breed();
}

class Lion extends Mammalia {
    @Override
    public void breed() { ... }
}
```
</br></br>

**인터페이스 상속**

인터페이스끼리 확장시에는 extends 키워드를 사용.

메소드는 모두 상속받으나, 필드는 기본적으로 static이기 때문에 구현체를 따라가지 않음(독립 상수).

- 클래스의 상속은 멤버 변수끼리 상속되어 덮어 씌워지지만, 인터페이스의 필드(상수)들은 서로 상속을 해도 독립적으로 운용됨.
</br></br>

**default 메소드**

default 키워드를 사용하며 일반 메소드처럼 구현부가 있어야 함.

자식 클래스에서 default 메소드를 오버라이딩하여 재정의 가능.

인터페이스를 구현한 이후, 수정과정에서 인터페이스의 모든 자식에게 수정 없이 광역으로 함수를 만들어주고 싶을 때 사용.

> 인터페이스의 디폴트 메소드를 호출하기 위해서는 객체의 타입이 반드시 인터페이스 타입으로 업캐스팅 해주어야 함.

```java
interface Calculator {
    int plus(int i, int j);
    int multiple(int i, int j);

    // default로 선언함으로 메소드를 구현할 수 있다.
    default int sub(int i, int j){      
        return i - j;
    }
}

// Calculator인터페이스를 구현한 MyCalculator클래스
class MyCalculator implements Calculator {
    // 추상 메서드만 구현해줌
    @Override
    public int plus(int i, int j) { return i + j; }
    @Override
    public int multiple(int i, int j) { return i * j; }
}

public class Main {
    public static void main(String[] args){
        MyCalculator mycal = new MyCalculator();
        
        // 인터페이스 타입으로 업캐스팅
        Calculator cal = (Calculator) mycal; // 괄호 생략해도 됨

        // 인스턴스의 인터페이스 디폴트 메서드 호출
        int value = cal.sub(5, 10);
        System.out.println(value); // -5
    }
}
```
</br></br>

**default 메소드를 가진 인터페이스의 다중 상속 문제**

인터페이스는 다중 상속이 허용되면서 클래스는 허용되지 않는 이유는 구현해야 하는 메소드가 동일할 경우, 충돌이 생기는 상황의 유무에 따른 차이였음.

- 인터페이스 : 원래는 추상 메소드로만 이루어져있으므로, 충돌될 일 X.
- 클래스 : 추상 메소드 이외에도 모든 메소드가 가능하므로, 충돌가능성 높음.

그러나 Java 8이후로, 인터페이스에 default 메소드가 추가되면서 다중 상속을 할 경우 서로 충돌가능성이 생겼음.

따라서 default 메소드를 가진 인터페이스에 대한 다중 상속 문제는 다음과 같이 해결함.

1. 다중 인터페이스들 간의 default 메소드 충돌
- 똑같은 default 메소드를 가진 두 인터페이스를 하나의 클래스에 구현하면, 컴파일 에러 발생.
- 인터페이스를 구현한 클래스에서 디폴트 메소드를 오버라이딩하여 하나로 통합해야만 함.
1. 인터페이스의 default 메소드와 부모 클래스 메소드 간의 충돌
- 부모 클래스의 메소드가 상속되고 default 메소드는 무시.
- 인터페이스 쪽의 default 메소드를 사용해야 한다면, 필요한 쪽의 메소드와 같은 내용으로 오버라이딩 해야함.

**default 메소드의 super**

`인터페이스명.super.디폴트메소드` 의 형태로 사용해야 함.

```java
interface IPrint{
    default void print(){
        System.out.println("인터페이스의 디폴트 메서드 입니다.");
    }
}

class MyClass implements IPrint {
    @Override
    public void print() {
        IPrint.super.print(); // 인터페이스의 super 메서드를 호출
        System.out.println("인터페이스의 디폴트 메서드를 오버라이딩한 메서드 입니다.");
    }
}
```
</br></br>

**static 메소드**

인스턴스 생성과 상관없이 인터페이스 타입으로 접근해 사용할 수 있는 메소드.

일반 클래스의 static 메소드와 동일.
</br></br>

**private 메소드**

Java 9 버전에 추가된 메소드.

구현부를 가지지만, 인터페이스 내부에서만 돌아감.

> 인터페이스를 구현한 클래스에서 private 메소드 사용 불가.
>
- 인터페이스 내부의 private 메소드는 default 메소드 내부에서만 호출가능.
- 인터페이스 내부의 private static 메소드는 static 메소드 내부에서만 호출가능.

인터페이스의 default, static 메소드들의 로직을 공통화하고 재사용하기 위함.
</br></br>

**인터페이스 사용 장점**

1. 선언과 구현을 분리하여 변경에 유리해짐.
- 기능변경이 필요할 때 기존 코드를 변경하는 것이 아닌, 구현체를 추가하여 변경 가능.
1. 표준화 가능.
- 예를 들어, JDBC 인터페이스를 표준화함으로써 어떤 DBMS를 사용하더라도 똑같은 방식으로 다룰 수 있음.
1. 서로 관계없는 클래스들의 관계를 맺어줄 수 있음.
- 다형성을 지원하여 다양한 객체들을 동일한 타입으로 참조하고 사용가능.

---

### Interface vs Abstract Class

|  | 추상 클래스 | 인터페이스 |
| --- | --- | --- |
| 사용 키워드 | abstract | interface |
| 상속 키워드 | extends | implements |
| 사용 가능 변수 | 제한 X | static final(상수) |
| 사용 가능 메소드 | 제한 X | abstract, default, static, private |
| 사용 가능 접근 제어자 | 제한 X | public(Java 9부터 private 메소드 지원) |
| 다중 상속 가능 여부 | X | O |

| 공통점 |
| --- |
| 1. 추상 메소드를 가져야 함. |
| 2. 인스턴스화 할 수 없음(new 생성자 사용 불가). |
| 3. 인터페이스 혹은 추상 클래스를 상속받아 구현한 구현체의 인터페이스를 사용해야 함. |
| 4. 인터페이스와 추상 클래스를 구현, 상속한 클래스는 추상 메소드를 반드시 구현해야 함. |

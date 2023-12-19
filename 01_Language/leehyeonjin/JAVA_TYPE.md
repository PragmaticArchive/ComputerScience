# Java Type

---

### Primitive Type

| 정수 | byte(1), short(2), int(4), long(8) |
| --- | --- |
| 실수 | float(4), double(8) |
| 문자 | char(2) |
| 논리 | boolean(1) |

---

### Primitive Type vs Reference Type

|  | 원시타입 | 참조타입 |
| --- | --- | --- |
| null 가능여부 | 불가 | 가능 |
| 제네릭 | 불가(Wrapper Class 이용) | 가능 |
| 메모리 영역 | stack | heap |
| 비교 | 성능이 빠르고, 메모리를 적게 사용함 | 동적으로 할당되며, GC의 대상 |

---

### Call by Reference vs Call by Value

**Call by Reference**

함수에 인자를 전달할 때 인자의 메모리 주소를 전달하는 방식.

함수 호출시에 인자로 전달되는 변수의 레퍼런스를 전달하며 해당 주소를 통해 원본 데이터를 직접 수정가능.

C++, Swift, PHP 등.
</br></br>

**Call by Value**

함수에 인자를 전달할 때 인자값을 복사하여 전달하는 방식.

원본데이터와 전달된 복사본이 서로 다른 메모리 주소를 가지기 때문에, 함수 내에서 인자의 값을 변경해도 원본 데이터에는 영향을 미치지 않음.

Java, Python 등.

> 💡 **Java의 참조변수**
>
> Java는 늘 call by value로 작동함.</br>
> Java의 참조변수에는 원본 객체에 대한 참조를 값으로 복사하여 가지고 있음.</br>
> 즉, 변수가 가지는 값이 레퍼런스이므로 인자를 넘길 때 call by value에 의해 변수가 가지고 있는 레퍼런스가 복사되어 전달됨.</br>
> 따라서 메소드 내에서 객체의 속성을 변경하게 되면 원래 객체에 영향을 줄 수 있지만, 메소드 내에서 전달된 참조변수 자체를 변경하면 호출자의 원래 참조에는 영향을 주지 않음.

---

### Java의 null 처리(Java에서 null을 안전하게 다루는 법)

1. assertion 단정문

> 💡 **단정문을 사용하면 안되는 경우**
> 
> - public 메소드의 파라미터 검사.
> - 올바른 수행을 위해 필요한 작업을 수행하는 경우.

```java
assert 식1; // 식1이 거짓이면 AssertionError 발생
assert 식1 : 식2; // 식2는 AssertionError에 포함된 상세 정보를 만드는 생성식
```
</br>

2. java.util.Objects 자바 기본 장치

> 💡 **단정문을 사용하면 안되는 경우**
>
> - public 메소드의 파라미터 검사.
> - 올바른 수행을 위해 필요한 작업을 수행하는 경우.

```java
// Java 8부터 생긴 null 체크와 관련된 메소드
isNull(Object obj);
nonNull(Object obj);
requireNonNull(T obj);
requireNonNull(T obj, String msg);
requireNonNull(T obj, Supplier<String> msgSuppllier);

// Java 9부터 생긴 기본값을 제공하는 메소드
requireNonNullElse(T obj, T defaultObj);
requireNonNullElse(T obj, Supplier<? extends T> supplier);
```
</br>

3. java.util.Optional 자바 기본 장치

- Java 8부터 도입된 null pointer exception 문제를 해결하기 위한 클래스.

- 값의 존재 여부를 체크하고, 값이 없는 경우에 대한 기본 처리를 할 수 있음.

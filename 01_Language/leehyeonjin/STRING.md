# Java 문자열

---

### String vs StringBuffer / StringBuilder

**가변 or 불변**

String은 불변(immutable)이고, StringBuffer와 StringBuilder는 가변(mutable).

- 불변 객체는 간단하게 사용가능하고, 동기화에 대해 신경쓰지 않아도 됨(Thread-Safe).
- 따라서 String 클래스는 내부 데이터를 자유롭게 공유가능.

> 💡 **가변객체**
> 
> 가변객체는 내부 상태가 변경 가능한 객체.</br>
> 가변객체는 멀티 스레드 환경에서 사용하려면 별도의 동기화 처리가 필요함.

> 💡 **불변객체**
> 
> Java에서 Class의 인스턴스가 생성된 이후에 내부 상태를 변경할 수 없는 객체.</br>
> 불변 객체는 멀티 스레드 환경에서도 안전하게 사용할 수 있다는 신뢰성 보장.

> 💡 **불변객체 장점**
>
> 1. 공유 자원 불변객체라면 항상 동일한 값만 반환하므로, 동기화를 고려할 필요가 없어 안정성이 보장됨.
> 2. 불변 객체라면 어떠한 예외가 발생하여도 메소드 호출전의 상태를 유지할 수 있으므로, 예외가 발생해도 변경된 상태로 인한 추가 에러를 방지 가능.
> 3. 부수효과(Side Effect)를 피해 오류 가능성을 최소화할 수 있음.

> 💡 **불변객체 생성**
> 
> 1. 방어적 복사
> - 생성자의 매개변수를 새로운 인스턴스를 생성해 필드에 할당.
> - getter의 반환값을 새로운 인스턴스를 생성해 반환.
> 2. setter 사용 X
> 3. private final 멤버변수 선언.

> 💡 **자바의 동시성 이슈(공유자원 접근)**

> 멀티 스레드 환경에서 여러 스레드가 동시에 하나의 자원을 공유하고 있기 때문에 같은 자원을 두고 경쟁상태(race condition)가 발생.</br>
> 따라서 thread safe한 상태를 만들어야 함.
>
> 1. 암시적 Lock(synchronized) : 문제가 될 가능성이 있는 자원(메소드, 변수)에 synchronized 키워드 사용.
> 2. 명시적 Lock : 메소드보다 더 큰 범위(Lock 클래스 생성)가 필요할 때나, 동시에 여러개의 자원(메소드, 변수)에 Lock을 걸고 싶을때.
> 3.  불변 객체 : setter를 만들지 않거나, final 키워드를 사용할때.
> - final 키워드를 사용하면 무조건 초기화해야 함.
> 4. Stream : 데이터 자체를 Stream() 내부에서 캡슐화해서 결과를 도출할 때.
</br>

**문자열 연산**

String 클래스도 + 연산이나 concat() 메소드로 문자열을 이어붙일 수 있음.

- 그러나 String 클래스의 경우, 불변이므로 내용이 합쳐진 새로운 인스턴스를 생성하게 됨.
- 따라서 문자열을 많이 결합하면 할수록 공간의 낭비 & 속도가 매우 느려짐.

따라서 StringBuffer 혹은 StringBuilder 클래스의 concat()을 통한 연산을 하는 것 권장.

> 💡 **String 클래스의 문자열 연산**
> 1. String 객체의 문자열 연산 호출.
> 2. 새로운 String 객체를 생성 후, 연산된 문자열 저장.
> 3. 새롭게 생성한 String 객체를 참조하도록 함.

> 💡 **String 생성 방식**
> 1. new를 통해 String 생성하면 Heap 영역에 존재하게 됨.
> 2. 리터럴을 이용할 경우, String Constant Pool에 존재하게 됨.
> - String 리터럴로 생성할 경우, String 값도 Heap 영역 내 String Constant Pool에 저장되어 재사용됨.
> - new 연산자로 생성하면 같은 내용이라도 여러 개의 객체가 각각 Heap 영역을 차지하게 됨.

---

### StringBuffer vs StringBuilder

**메모리 할당**

StringBuffer와 StringBuilder는 String과 달리, 문자열 연산 등으로 기존 객체의 공간이 부족한 경우 기존의 버퍼 크기를 늘리며 유연하게 동작.

StringBuffer와 StringBuilder 클래서 메소드 사용법은 동일함.

- StringBuffer의 버퍼(데이터 공간) 크기의 기본값은 16개의 문자를 저장가능한 크기(생성자를 통해 그 크기를 별도로 설정가능), 버퍼가 가득 차면 그만큼 크기 증가.
- StringBuilder 클래스는 버퍼가 가득 차면 버퍼의 크기가 (기존용량 + 1)의 두 배로 변경됨.
</br></br>

**동기화 여부**

StringBuffer는 각 메소드별로 synchronized 키워드가 존재하여, 멀티스레드 환경에서도 동기화 지원.

StringBuilder는 동기화를 보장하지 않음.

- 따라서 멀티 스레드 환경이라면 StringBuffer 을 사용하고, 단일 스레드 환경이라면 StringBuilder를 사용하는 것이 좋음.
- StringBuffer는 동기화 관련 처리로 인해 StringBuilder에 비해 성능이 좋지 않음.

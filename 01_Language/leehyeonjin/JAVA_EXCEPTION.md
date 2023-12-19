# Java Exception

---

### Java의 예외

Java의 예외는 크게 3가지로 나눌 수 있음.

- 체크 예외(Checked Exception)
- 에러(Error)
- 언체크 예외(Unchecked Exception)

![Untitled](https://github.com/hgene0929/hgene0929/assets/90823532/7e8681a5-e564-4586-80da-948ea3a20a23)

---

### 에러(Error)

시스템에 비정상적인 상황이 발생했을 경우에 발생.

개발자가 예측하기도 쉽지 않고, 처리할 수 있는 방법도 없음.

Ex. 메모리 부족, 스택오버플로우와 같이 복구 불가능한 것.

---

### 예외(Exception)

프로그램 실행 중에 개발자의 실수로 예기치 않은 상황이 발생한 경우.

Ex. 배열의 범위를 벗어난 경우, null이 참조변수를 참조하는 경우, 존재하지 않는 파일의 이름을 입력 등, 복구가능한 수준.
</br></br>

**체크 예외(Checked Exception)**

RuntimeException의 하위 클래스가 아니면서 Exception 클래스의 하위 클래스.

반드시 에러 처리를 해야함(try~catch or throw).

예외발생시 트랜잭션을 rollback 하지 않고, commit까지 완료됨.

Ex. 존재하지 않는 파일의 이름을 입력(FileNotFoundException), 실수로 클래스의 이름을 잘못 적음(ClassNotFoundException) 등.
</br></br>

**언체크 예외(Unchecked Exception)**

RuntimeException의 하위 클래스들 → 실행 중 발생가능한 예외를 의미.

에러 처리를 강제하지 않음.

예외발생시 트랜잭션을 rollback 함.

Ex. 배열의 범위를 벗어난(ArrayIndexOutOfBoundException), 값이 null인 참조변수를 참조(NullPointerException) 등.

> 컴파일러가 에러처리를 확인하지 않는 RuntimeException 클래스들은 unchecked 예외라고 부르고, 예외처리를 확인하는 클래스들은 checked 예외임.

> 💡 try ~ with ~ resource
>
> - Java 7에서 추가됨.
> - 기존 : try문에서 finally를 통해 자원반납을 하므로, 개발자의 실수에 의해 자원반납이 되지 않을 수도 있음.
> - 변경후 : try()에서 자원을 선언하면 JVM이 자동으로 반납해주며, 간편하고 개발자의 실수도 방지 가능.

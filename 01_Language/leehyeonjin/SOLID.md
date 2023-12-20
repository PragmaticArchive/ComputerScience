# SOLID

---

### S : 단일 책임 원칙(SRP, Single Responsibility Principle)

하나의 클래스는 하나의 책임만 가져야 함 → 하나의 클래스가 변경되는 이유는 하나뿐.

Ex. Controller에서 비즈니스 로직을 처리하지 않고, 요청과 응답만을 처리해야 하며 비즈니스 로직은 Service에 넘겨줘야 함.

---

### O : 개방-폐쇄 원칙(OCP, Open Closed Principle)

자신의 확장에는 열려있고, 주변의 변화에는 닫혀있어야 함.

기존의 코드를 변경하지 않고 새로운 기능을 추가할 수 있어야 함.

Ex. JDBC 인터페이스를 표준화하여 DBMS를 변경할 때마다 각자 제공하는 API에 따라 Connection을 설정하는 부분만 해당 드라이버로 교체해주면 됨.

---

### L : 리스코프 치환 원칙(LSP, Liskov Substitution Principle)

하위클래스는 상위클래스의 역할을 하는데 문제가 없어야 함.

자식클래스는 부모클래스의 역할을 모두 할 수 있어야 함.

---

### I : 인터페이스 분리 원칙(ISP, Interface Segregation Principle)

하나의 일반적인 인터페이스보다는 여러개의 구체적인(클라이언트에 특화된) 인터페이스를 사용해야 함.

---

### D : 의존 역전 원칙(DIP, Dependency Inversion Principle)

의존관계를 맺을 때 변화하기 쉬운 것보다 거의 변화하지 않는 것에 의존해야 함.

구체화에 의존하지 말고, 추상화에 의존해야 함 → 구현클래스가 아닌 인터페이스에 의존해야 함.

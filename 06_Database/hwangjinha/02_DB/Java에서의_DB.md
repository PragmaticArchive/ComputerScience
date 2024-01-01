tags : #CS #DB #java #connection-pool #ORM

---

### Connection Pool

> 애플리케이션 로딩 시점에 Connection 객체를 미리 생성하고, DB 연결이 필요할 경우 미리 준비된 Connection 객체를 사용하여 성능 향상.

JDBC API를 사용하여 데이터베이스와 연결하기 위해 Connection 객체를 생성하는 작업은 비용이 굉장히 많이 듦.
따라서 DB를 연결할 떄마다 Connection 객체를 새로 만드는 것은 비용이 많이 들며, 굉장히 비효율적임.
이러한 문제를 해결하기 위해 커넥션 풀 등장.

### ORM(Object Relational Mapping)

객체지향 프로그래밍과 DB의 패러다임의 불일치를 해결하기 위한 부분을 객체간의 관계를 바탕으로 자동으로 SQL 연산을 수행하여 해결.
ex) JPA, Hibernate.

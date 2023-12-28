# Database & Language

---

### 데이터베이스 정의

여러 사람이 공유하고 사용할 목적으로 통합관리되는 데이터들의 모임.

중복된 데이터를 없애고, 자료를 구조화하여 효율적인 처리를 하도록 관리됨.

DBMS(DataBase Management System)라는 미들웨어에 의해 관리됨.
</br></br>

**데이터베이스 특징**

1. 실시간 접근성(Real-Time Accessiblity) : 비정형적인 질의(조회)에 대하여 실시간 응답이 가능해야 한다.
2. 지속적인 변화(Continuous Evolution) : 데이터베이스의 상태는 동적이므로, 새로운 데이터의 삽입, 삭제, 갱신으로 항상 최신의 데이터를 유지해야 한다.
3. 동시 공용(Concurrent Sharing) : 데이터베이스는 서로 다른 목적을 가진 여러 응용자들을 위한 것이므로, 다수의 사용자가 동시에 같은 내용의 데이터를 이용할 수 있어야 한다.
4. 내용에 의한 참조(Content Reference) : 데이터베이스에 있는 데이터를 참조할 때 데이터 레코드의 주소나 위치에 의해서가 아니라 사용자가 요구하는 데이터 내용으로 찾아야 한다.
</br></br>

**데이터베이스 사용이유**

데이터베이스를 사용하지 않으면 프로그램의 종료 시점에 그전까지 생성한 많은 데이터들이 모두 날아가게 됨.

이런 현상을 방지하기 위해 데이터들을 데이터베이스에 넣고 보관하는 방법을 사용함.
</br></br>

**DBMS(DataBase Management System)**

데이터베이스 관리 시스템이란 다수의 사용자들이 데이터베이스 내의 데이터를 접근할 수 있도록 하는 소프트웨어.

기존의 파일 시스템이 갖는 데이터의 종속성과 중복성의 문제를 해결하기 위해 제안된 시스템으로 모든 응용 프로그램들이 데이터베이스를 공유할 수 있도록 관리해줌.

데이터베이스의 구성, 접근방법, 유지관리에 대한 모든 책임을 지게됨.

---

### 데이터베이스 언어(DDL, DML, DCL)

**DDL(정의어, Data Definition Language)**

데이터베이스 구조를 정의, 수정, 삭제하는 언어.

create, alter, drop.
</br></br>

**DML(조작어, Data Manipulation Language)**

데이터베이스 내의 자료 검색, 삽입, 갱신, 삭제를 위한 언어.

insert, update, delete, select.
</br></br>

**DCL(제어어, Data Control Language)**

데이터에 대해 무결성 유지, 병행 수행 제어, 보호와 관리를 위한 언어.

grant, revoke.

---

### Select 쿼리의 수행 순서

```java
from, on, join

where, group by, having

select

distinct

order by, limit
```

1. from, on, join
- from : 각 테이블을 확인.
- on : join 조건 확인.
- join : 데이터를 모으고, 서브쿼리도 포함하여 임시 테이블 생성.
1. where, group by, having
- where : from 절로 가져온 테이블의 개별 행에 적용됨.
- group by : where의 조건 적용 후 나머지 행은 group by절에 지정된 열의 공통 값을 기준으로 그룹화됨( 쿼리에 집계 기능이 있는 경우에만 사용할 것 ).
- having : group by 절이 쿼리에 있을 경우 having 절의 제약조건이 그룹화된 행에 적용됨.
3. select
4. distinct
5. order by, limit

---

### group by

특정 칼럼을 기준으로 연산한 결과를 집계 키로 정의하여 그룹을 짓는 역할.

집합 연산자는 count, sum, avg, max, min 등이 있고, distinct와 같이 중복 데이터를 제거하는 특징을 가짐.

---

### where vs having vs on

**where vs having**

having은 그룹화 또는 집계가 발생한 후 필터링에 사용되고,

where은 그룹화 또는 집계가 발생하기 전 필터링에 사용됨.

- 집계함수는 having절과 함께 사용할 수 있지만, where절은 불가.
- 집계함수를 사용할 수있는 group by절보다 where절이 먼저 수행되기 때문.
</br></br>

**where vs on**

on은 join을 하기 전 필터링에 사용되고,

where은 join 이후 필터링에 사용됨.

- on 조건으로 필터링된 레코드들간 join이 이루어짐.

---

### Delete vs Truncate vs Drop

**Delete**

데이터는 지우지만 테이블 용량은 줄어들지 않음.

원하는 데이터만 선택해서 삭제 가능.

삭제후 되돌릴 수 있음.
</br></br>

**Truncate**

테이블 용량이 줄어들고 인덱스 등도 삭제됨.

전체 데이터를 한번에 삭제.

테이블 자체는 삭제 불가.

삭제후 되돌릴 수 없음.
</br></br>

**Drop**

테이블 자체를 완전히 삭제(인덱스, 객체 모두 삭제).

삭제 되돌릴 수 없음.

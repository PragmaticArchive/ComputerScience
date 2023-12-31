# 트랜잭션(Transaction)

---

### 트랜잭션 정의

데이터베이스의 상태를 변화시키기 위해 수행하는 작업 단위.

- 상태를 변화시킨다는 것 : SQL 질의어를 통해 DB에 접근하는 것.
- 작업 단위 : 많은 SQL 명령문들을 사람이 정하는 기준에 따라 정하는 것.

Ex. 사용자 A가 사용자 B에게 만원을 송금한다.

[DB 작업]

1. 사용자 A의 계좌에서 만원을 차감 → update문을 통해 사용자 A의 잔액 변경.
2. 사용자 B의 계좌에 만원을 추가 → update문을 통해 사용자 B의 잔액 변경.

[작업 단위 = 출금 update 문 + 입금 update 문]

- 이를 통틀어 하나의 트랜잭션이라고 함.
- 위 2개의 쿼리문 모두 성공적으로 완료되어야만, “하나의 작업(트랜잭션)”이 완료됨 ⇒ commit.
- 작업 단위에 속하는 쿼리 중 하나라도 실패하면 모든 쿼리문을 취소하고 이전 상태로 되돌려놓음 ⇒ rollback.

> 💡 **Commit & Rollback**
>
> - Commit : 하나의 트랜잭션이 성공적으로 끝났고, DB가 일관성있는 상태일때 이를 알려주기 위해 사용.
> - Rollback : 하나의 트랜잭션 처리가 비정상적으로 종료되어 트랜잭션 원자성이 깨진 경우.

---

### 트랜잭션 특징(ACID)

**1. 원자성(Atomicity)**

트랜잭션이 DB에 모두 반영되거나, 혹은 전혀 반영되지 않아야 함.
</br></br>

**2. 일관성(Consistency)**

트랜잭션 작업 처리 결과는 항상 일관성 있어야 함.

트랜잭션이 반영된 후에도 기존에 데이터베이스에 걸려있던 제약조건들을 만족.
</br></br>

**3. 독립성(Isolation)**

둘 이상의 트랜잭션이 동시에 병행 실행되고 있을 때, 어떤 트랜잭션도 다른 트랜잭션 연산에 끼어들 수 없음.
</br></br>

**4. 지속성(Durability)**

트랜잭션이 성공적으로 완료되었으면, 결과는 영구적으로 반영되어야 함.

---

### 트랜잭션 관리를 위한 DBMS 전략

이해를 위한 2가지 개념 : DBMS 구조 / Buffer 관리 정책.
</br></br>

**1. DBMS의 구조**

<img width="469" alt="Untitled (6)" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/94fbec0c-7e73-45a9-9411-ce6d80e436dd">

- 구조 : 질의 처리기(Query Processor) | 저장 시스템(Storage System).
- 입출력 단위 : 고정 길이의 page 단위로 disk에 읽거나 쓴다.
- 저장 공간 : 비휘발성 저장 장치인 disk에 저장, 일부는 main memory에 저장.
</br></br>

**Page Buffer Manager(Buffer Manager)**

DBMS 저장시스템에 속하는 모듈 중 하나로, main memory에 유지하는 페이지를 관리하는 모듈.

- Buffer 관리 정책에 따라, UNDO 복구와 REDO 복구가 요구되거나 그렇지 않게 되므로, 트랜잭션 관리에 매우 중요한 결정을 가져옴.
</br></br>

**2. Buffer 관리 정책(복구 종류)**

**[UNDO]**

수정된 page들이 Buffer 교체 알고리즘에 따라서 디스크에 출력됨.

Buffer 교체는 트랜잭션과는 무관하게 Buffer의 상태에 따라서 결정됨.

이로 인해 정상적으로 종료되지 않은 트랜잭션이 변경한 page들은 원상 복구되어야 하는데 이를 UNDO 복구라 함.

2개의 정책( 수정된 페이지를 디스크에 쓰는 시점으로 분류 ).

- steal : 수정된 페이지를 언제든지 디스크에 쓸 수 있는 정책.
    - 대부분의 DBMS가 채택하는 Buffer 관리 정책.
    - UNDO logging과 복구를 필요로 함.
- ¬steal : 수정된 페이지들을 EOT(End Of Transaction)까지 버퍼에 유지하는 정책.
    - UNDO 작업이 필요하지 않지만, 매우 큰 메모리 버퍼가 필요함.
</br></br>

**[REDO]**

이미 commit한 트랜잭션의 수정을 재반영하는 복구 작업.

Buffer 관리 정책에 영향을 받음.

2개의 정책( 트랜잭션이 종료되는 시점에 해당 트랜잭션이 수정한 page를 디스크에 쓸 것인가 아닌가로 분류 ).

- FORCE : 수정했던 모든 페이지를 트랜잭션 커밋 시점에 디스크에 반영.
    - 트랜잭션이 커밋되었을 때 수정된 page들이 디스크 상에 반영되므로 REDO 필요없음.
- ¬FORCE : 커밋 시점에 반영하지 않음.
    - 트랜잭션이 디스크 상의 DB에 반영되지 않을 수 있으므로 REDO 복구가 필요(대부분의 DBMS 정책).

---

### 트랜잭션 격리 수준(Transaction Isolation Level)

트랜잭션에서 일관성 없는 데이터를 허용하도록 하는 수준.
</br></br>

**트랜잭션 격리 수준 필요성**

데이터베이스는 ACID 특징과 같이 트랜잭션이 독립적인 수행을 하도록 함.

따라서 Locking을 통해 트랜잭션이 DB를 다루는 동안 다른 트랜잭션이 관여하지 못하도록 막는 것이 필요함.

그러나 무조건 Locking으로 동시에 수행되는 수많은 트랜잭션을 순서대로 처리하도록 하면 DB 성능이 저하됨.

그렇다고 해서 성능을 높이기 위해 Locking의 범위를 줄인다면 잘못된 값이 처리될 문제가 발생.

따라서 최대한 효율적인 Locking 방법이 필요함.
</br></br>

**트랜잭션 격리 수준 종류**

**1.  Read Uncommited (레벨 0)**

select 문장이 수행되는 동안 해당 데이터에 Shared Lock이 걸리지 않음.

트랜잭션에 처리중이거나 아직 커밋되지 않은 데이터들을 다른 트랜잭션이 읽는 것을 허용함.

> 데이터베이스의 일관성을 유지하는 것이 불가능하다.

```
1. A 트랜잭션에서 10번의 사원의 나이를 27 -> 28로 업데이트.
2. 커밋 X.
3. B 트랜잭션에서 10번의 사원의 나이를 조회.

조회결과 : 28 => dirty read 문제 발생 가능.

[dirty read 상황 : 데이터 정합성 문제]
4. A 트랜잭션에서 문제가 발생해서 롤백.
5. B 트랜잭션이 활용하는 데이터는 여전히 28.
```
</br>

**2. Read Commited (레벨 1)**

select 문장이 수행되는 동안 해당 데이터에 Shared Lock이 걸림.

트랜잭션이 수행되는 동안 다른 트랜잭션이 접근할 수 없어 대기하게 됨.

커밋이 이뤄진 트랜잭션만 조회 가능 ⇒ 다른 트랜잭션 진행과정 중에 커밋이 발생하면 Repeatable Read를 보장하지 않음.

대부분의 SQL 서버가 default로 사용하는 격리 수준.

```
1. B 트랜잭션에서 10번 사원의 나이를 조회.
2. 27살이 조회됨.
3. A 트랜잭션에서 10번 사원의 나이를 27 -> 28로 변경.
4. B 트랜잭션에서 10번 사원의 나이를 다시 조회.
5. A 트랜잭션에서 커밋.
6. B 트랜잭션에서 10번 사원의 나이를 또 다시 조회.

1st 조회 결과 : 27. 
2nd 조회 결과 : 28 => Non-Repeatable Read 문제.
```
</br></br>
**3. Repeatable Read (레벨2)**

트랜잭션이 완료될 때까지 select 문장이 사용하는 모든 데이터에 Shared Lock이 걸림.

트랜잭션이 범위 내에서 조회한 데이터 내용이 항상 동일함을 보장 → 트랜잭션 시작 전에 커밋된 내용만 볼 수 있음.

- InnoDB의 경우 트랜잭션 번호를 순차적으로 부여하는데, Reapeatable Read 격리 수준을 따른다면 현재 자신의 트랜잭션 번호보다 낮은 트랜잭션들의 데이터만 조회.

다른 사용자는 트랜잭션 영역에 해당되는 데이터에 대한 수정 불가능.

MySQL에서 default로 사용하는 격리 수준.

```
1. B 트랜잭션에서 10번 사원의 나이를 조회.
2. 27살이 조회됨.
3. A 트랜잭션에서 10번 사원의 나이를 27 -> 28로 변경.
4. B 트랜잭션에서 10번 사원의 나이를 다시 조회.
5. A 트랜잭션에서 커밋.
6. B 트랜잭션에서 10번 사원의 나이를 또 다시 조회.

1st 조회 결과 : 27. 
2nd 조회 결과 : 27.
```

```sql
-- Phantom Read 현상
-- UPDATE 문의 영향을 받은 후 부터 Phantom Read 현상 발생(스냅샷을 적용 시점)
-- DELETE에 대해서는 적용되지 않음
START TRANSACTION; -- transaction id : 1 
SELECT * FROM Member; -- 0건 조회

    START TRANSACTION; -- transaction id : 2
    INSERT INTO MEMBER VALUES(1,'철수',28);
    COMMIT;

SELECT * FROM Member; -- 여전히 0건 조회 
UPDATE Member SET name = '철수' WHERE id = 1; -- 1 row(s) affected
SELECT * FROM Member; -- 1건 조회 
COMMIT;
```
</br></br>
**4. Serializable (레벨 3)**

트랜잭션이 완료될 때까지 select 문장이 사용하는 모든 데이터에 Shared Lock이 걸림.

완벽한 읽기 일관성 모드를 제공.

다른 사용자는 트랜잭션 영역에 해당되는 데이터에 대한 수정 및 입력이 불가능.

> 성능에 좋지 않음.

</br></br>
**트랜잭션 격리 수준 선택시 고려사항**

격리 수준에 대한 조정은 동시성과 데이터 무결성에 연관되어 있음.

- 동시성을 증가시키면, 데이터 무결성에 문제 발생.
- 무결성을 유지시키면, 동시성이 떨어짐.

</br></br>
**낮은 단계 격리 수준을 활용할 때 발생하는 현상들**

**1. dirty read**

커밋되지 않은 수정중인 데이터를 다른 트랜잭션에서 읽을 수 있도록 허용할 때 발생.

- Read Uncommitted.

**2. Non-Repeatable Read**

한 트랜잭션에서 같은 쿼리를 두번 수행할 때 그 사이에 다른 트랜잭션 값을 수정 또는 삭제하면서 두 쿼리의 결과가 상이하게 나타나는 일관성이 깨진 현상.

- Read Committed, Read Uncommitted.

**3. Phantom Read**

한 트랜잭션 안에서 일정 범위의 레코드를 두 번 이상 읽었을 때, 첫번째 쿼리에서 없던 레코드가 두번째 쿼리에서 나타나는 현ㄴ상.

- Repeatable Read, Read Committed, Read Uncommitted.

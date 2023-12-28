# 데이터베이스 락(Lock)

---

### 락의 정의

데이터베이스 락은 트랜잭션 처리의 순차성을 보장하기 위한 방법.

동시성 제어 : 트랜잭션들이 동시에 수행될 때 일관성을 해치지 않도록 데이터 접근을 제어하는 것.

---

### 락의 종류

**1. 낙관적 락**

<img width="819" alt="Untitled" src="https://github.com/hgene0929/ComputerScience/assets/90823532/7adfc498-27d1-4f26-9f42-afdb06f6d0c4">

데이터 갱신시 경합이 발생하지 않을 것을 가정.

한 사용자가 업데이트를 완료하면, 동시 업데이트 확정을 시도하는 다른 사용자에게 충돌이 있음을 알림.

- 업데이트 확정 전 해당 데이터를 다시 읽어와 이전의 값과 변함이 없으면 업데이트 확정, 변경이 있다면 롤백.

롤백을 할 가능성이 높기 때문에 → 동시 업데이트가 거의 없는 경우 사용.
</br></br>

**2. 비관적 락**

<img width="586" alt="Untitled (1)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/3caf28e2-937d-4115-8e86-127d4b58c4e9">

동일한 데이터를 동시에 수정할 가능성이 높다는 것을 가정.

다른 사용자는 먼저 시도한 사용자가 변경을 확약해서 레코드 잠금을 릴리스할 때까지 대기.

동시업데이트가 빈번한 경우, 롤백을 하기 힘든 외부 시스템과 연동한 경우 사용.
</br></br>

**2-1 공유(Shared) 락**

<img width="557" alt="Untitled (2)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/92116121-51d5-480e-9289-ffc3ccba016c">

데이터를 읽을 때 사용되어지는 락.

공유 락까리는 동시에 접근 가능 ⇒ 하나의 데이터를 읽는 것은 여러 사용자가 동시에 할 수 있음.

그러나 공유 락이 설정된 데이터에 베타 락을 사용할 수는 없음.

read 연산 실행 가능, write 연산 실행 불가능.

read 연산은 데이터에 대해 어떠한 영향도 미칠 수 없음.

따라서 데이터에 대한 사용권을 여러 트랜잭션이 함께 가질 수 있음.
</br></br>

**2-2 베타(Exclusive) 락**

<img width="566" alt="Untitled (3)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/9b1c3b40-042c-4f65-a9c9-14016aca8411">

read 연산과 write 연산을 모두 실행 가능.

write 연산은 데이터를 업데이트 시켜줄 가능성이 있음.

따라서 베타 락 연산을 실행한 트랜잭션만 해당 데이터에 대한 독점권을 가짐.

**공유 락 X 베타 락**

<img width="481" alt="Untitled (4)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/c19f26b9-eb06-4571-94b3-219191eb1be7">

공유락 : 공유락끼리는 양립 가능.

베타락 : 베타락이 들어가는 순간 양립 불가.

---

### 락의 단위

<img width="415" alt="Untitled (5)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/303b1104-7a8d-4e95-92a9-b2b577d5cf63">

> 락의 단위는 데이터베이스 종류마다 상이함.

---

> 💡 **작은 단위의 락을 여기저기에 사용하면 발생하는 문제점?**
> 블로킹, 데드락!

### 블로킹(Blocking)

<img width="558" alt="Untitled (6)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/beef0cc9-b6d8-479d-a837-3db31522203e">

락간(베타-베타, 베타-공유)의 경합이 발생하여 특정 트랜잭션 작업을 진행하지 못하고 멈춰선 상태.

블로킹을 해소하기 위해서는 이전의 트랜잭션이 완료(커밋 혹은 롤백)되어야 함 → 성능에 좋지 않은 영향을 미칠 수 있음.

따라서 경합을 최소화할 필요가 있음.

- 한 트랜잭션의 길이를 너무 길게하는 것은 경합의 확률을 높임.
- 처음부터 설계할 때 같은 데이터를 갱신하는 트랜잭션이 동시에 수행되지 않도록 해야 함.
- Lock Timeout을 이용하여 잠금해제 시간 조절.

---

### 교착상태(Deadlock)

<img width="588" alt="Untitled (7)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/d1a2c548-eb3d-438d-9c52-03c33d7db5e8">

두 트랜잭션이 각각 락을 설정하고 다음 서로의 락에 접근하여 값을 얻어오려고 할 때 이미 각각의 트랜잭션에 의해 락이 설정되어 있기 때문에 양쪽 트랜잭션 모두 영원히 처리가 되지 않도록 되는 상태.

교착상태 발생시 DBMS가 둘 중 한 트랜잭션에 에러를 발생시킴으로써 문제 해결.

- 트랜잭션 진행방향과 같은 방향으로 처리 :
    - 교착상태가 발생할 가능성을 줄이기 위해서는 미리 업데이트 규칙을 정해 해당 순서대로 프로그래밍을 하는 것을 권장.
- 트랜잭션 처리속도를 최소화.
- Lock Timeout을 이용하여 잠금해제 시간 조절.

> 💡 **InneoDB의 LockingRead와 Non-Locking Read**
>
> InnoDB 기반 MySQL은 Repeatable Read를 기본으로 하고 이를 수행하기 위해 Locking Read, Non-Locking Read를 사용함.
>
> - Non-Locking Read : 트랜잭션 시작 후 첫 select 결과를 스냅샷으로 저장 ⇒ 다른 트랜잭션이 조회시 항상 일관성 보장.
> - Locking Read : Non-Locking Read만으로는 다른 트랜잭션이 변경해버릴 수도 있으므로 → 모든 트랜잭션 작업 내용에 공유락을 설정하여 ⇒ 다른 트랜잭션이 변경하지 못하도록 함.

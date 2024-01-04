# 메모리 관리 기법

---

### 메모리 관리 기법 정의

단편화 현상을 줄이고, 적절한 swap을 통해 효율적으로 메모리를 관리하기 위한 방법.

---

### 1. 연속 메모리 할당(Contiguous Allocation)

프로세스들이 연속적인 메모리 공간을 차지하게 되는 것.

- 고정 분할(Fixed Partition) : 각 프로세스를 메모리에 담기 위해 메모리가 미리 고정된 크기로 공간을 분할해두는 것.
- 가변 분할(Variable Partition) : 프로세스의 크기를 고려해서 할당하기 때문에 분할의 크기나 개수가 동적으로 변하는 것.

> 외부 단편화 발생.

</br>


**메모리 할당 기법(Dynmic Storage Allocation)**

가변 분할 방식에서 크기가 n인 프로세스가 들어갈 가장 적절한 Hole을 찾는 문제.

연속 메모리 할당에서 외부 단편화를 줄이기 위한 할당 방식.

1. First-fit : 크기가 n 이상인 Hole 중 최초로 발견한 Hole에 할당.
2. Best-fit : 크기가 n 이상인 가장 작은 Hole을 찾아 할당.
3. Worst-fit : 가장 큰 Hole에 할당.

---

### 2. 페이징(Paging)

<img width="667" alt="Untitled (4)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/592aff99-e65a-4764-bab5-d7a3f11160b2">

불연속적 메모리 할당 방식.

메모리는 프레임(Frame), 프로세스는 페이지(Page)로 분할되어, 각각의 페이지가 순서와 관계 없이 메모리의 프레임에 맵핑되어 저장됨.
</br></br>

**page table**

프로세스가 순서대로 메모리에 저장되지 않기 때문에 프로세스 실행을 위해 페이지가 어떤 프레임에 맵핑되어 있는지 알아야 함.

따라서 이에 대한 정보가 페이지 테이블에 저장되어 있고, 이를 사용하여 논리적 주소를 물리적 주소로 변환함.
</br></br>

**페이징 장점**

- 페이지들이 연속할 필요가 없어 외부 단편화 해결가능.
</br></br>

**페이징 단점**

> 내부 단편화 문제 발생.

- 페이지 크기를 아주 작게하여 내부 단편화를 해결할 수는 있으나 페이지 맵핑 과정이 많아져 오히려 효율이 떨어질 수 있음.
- 페이지 테이블을 저장하기 위한 메모리가 추가로 소모됨.
- 페이지 테이블에 메모리에 상주하기 때문에 메모리에 접근하는 연산은 2번(페이지 테이블 접근 + 페이지 연산)의 메모리 접근이 필요하게 되어 속도가 느려짐.
</br></br>

**TLB(Tranlation Look-aside Buffer)**

<img width="695" alt="Untitled (5)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/c7b914fa-63e9-43e0-bb3a-2da03d69526c">

메모리 주소 변환을 위한 별도의 캐시 메모리.

페이지 테이블에서 빈번히 참조되는 일부 엔트리를 캐싱하고 있음.

위의 페이지 테이블에 대한 연산으로 인한 문제를 해결하기 위해(속도 향상) TLB라는 고속의 하드웨어 캐시를 사용함.

Page Hit : CPU는 페이지 테이블보다 TLB를 우선적으로 참조하여, 만약 원하는 페이지가 있다면 곧바로 frame number를 얻고,

Page Miss : 그렇지 않은 경우 메인 메모리에 있는 페이지 테이블로부터 frame number을 얻을 수 있음.
</br></br>

**페이지 부재시 절차**

1. 프로세스에 대한 내부 테이블을 검사하여 그 메모리 참조가 유효(valid) / 무효(invalid)인지 판별.
2. 만약 무효한 페이지에 대한 참조라면 해당 프로세스 중단, 만약 유효한 참조인데 메모리에 올라오지 않았다면 디스크 → 메모리.
3. 프레임의 빈 공간을 찾아서 페이지에 할당.
4. 페이지 테이블 갱신.
5. 트랩에 의해 중단되었던 명령 재수행.

> 💡 **스레싱(thrashing)**
>
> 하드디스크의 입출력이 너무 많아져서 잦은 페이지 부재로 마치 작업이 멈춘 것 같은 상태.

---

### 3. 세그멘테이션(Segmentation)

일정한 크기의 연속된 주소 공간.

가상 메모리를 논리적인 데이터 단위로 조각화 하는 것(= 사용자 관점에서 연관성 있는 코드 조각으로 분리하는 것).

세그먼트들의 크기가 다르기 때문에 미리 분할해 둘 수 없고, 메모리에 적재될 때 빈 공간을 찾아 할당하는 기법.

> 외부 단편화 발생.

> 💡 **메모리 풀(Memory Pool)**
>
> 필요한 메모리 공간을 필요한 크기, 개수만큼 사용자가 직접 지정하여 미리 할당받아 놓고, 필요할 때마다 사용함.
>
> 외부/내부 단편화는 모두 일어나지 않지만, 메모리가 메모리 낭비량보다 커졌을 떄는 단점이 됨.
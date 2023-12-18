# Garbage Collector 동작원리

---

### Garbage Collection(GC) 정의

Java의 메모리 관리 방법 중 하나.

JVM의 Heap 영역에서 동적으로 할당했던 메모리 중 필요없게 된 메모리 객체(garbage)를 모아 주기적으로 제거하는 프로세스.

---

### GC의 장/단점

**장점**

Java 프로세스가 한정된 메모리를 효율적으로 사용할 수 있도록 함.

개발자 입장에서 메모리 관리, 메모리 누수(Memory Leak) 문제에 대해서도 관리하지 않아도 되어 오롯이 개발에만 집중 가능.
</br></br>

**단점**

메모리가 언제 해제되는지 정확하게 알 수 없어 제어가 힘듦.

GC가 동작하는 동안에는 다른 동작을 멈추(Stop-The-World)기 때문에 오버헤드가 발생.

> 💡 **Stop-The-World**
> 
> GC를 수행하기 위해 JVM 프로그램 실행을 멈추는 현상.</br>
> GC가 작동하는 동안 GC 관련 스레드를 제외한 모든 스레드가 멈추게 되어 서비스 이용에 차질이 생길 수 있음.</br>
> 따라서 이 시간을 최소화 시키는 것이 중요.

---

### GC 대상

<img width="627" alt="Untitled" src="https://github.com/hgene0929/ComputerScience/assets/90823532/11c28c57-5574-4af4-bf5e-630ca0046f70">

특정 객체가 GC 대상(garbage)인지 여부를 판단하기 위해 도달능력(Reachability)이라는 개념을 적용.

객체에 레퍼런스가 있다면 Reachable로 구분되고, 객체에 유효한 레퍼런스가 없다면 Unreachable로 구분하여 수거.

- Reachable : 객체가 참조되고 있는 상태.
- Unreachable : 객체가 참조되고 있지 않은 상태(GC의 대상).

---

### GC 청소 방식

**Mark and Sweep**

<img width="719" alt="Untitled (1)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/104304ab-82b7-4aa9-af14-7a717ac8f8c1">

GC에서 사용되는 객체를 걸러내는 내부 알고리즘.

GC 대상이 될 대상을 식별(Mark)하고 제거(Sweep)하며, 객체가 제거되어 파편화된 메모리 영역을 앞에서부터 채워나가는 작업(Compaction) 수행.

1. Mark : GC Root로부터 그래프 순회를 통해 연결된 객체들을 찾아내어 각각 어떤 객체를 참고하고 있는지 찾아서 마킹.
2. Sweep : 참고하고 있지 않은 객체(Unreachable)들을 찾아서 Heap에서 제거.
3. Compaction : Sweep 후에 분산된 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 압축(Garbage Collector 종류에 따라 하지 않는 경우도 존재).

---

### GC 동작 과정

<img width="642" alt="Untitled (2)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/82be4a29-9d94-46dc-9b1b-75104c33b2ef">

**Heap 메모리 구조**

Heap 영역은 동적으로 레퍼런스 데이터가 저장되는 공간으로서, GC 대상이 되는 공간.

Heap 영역 설계의 2가지 전제(Weak Generational Hypothesis) :

1. 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
2. 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

> 객체는 대부분 일회성이며, 메모리에 오랫동안 남아있는 경우는 드물다.

이러한 특성을 이용해 JVM 개발자들은 보다 효율적인 메모리 관리를 위해, 객체의 생존기간에 따라 물리적인 Heap 영역을 나누어 Young과 Old 총 2가지 영역으로 설계(초기에는 Perm 영역도 존재하였으나, Java 8부터 제거되었음).

<img width="670" alt="Untitled (3)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/c609249f-ed2d-41e5-a4b5-f962402cde31">

1. Young 영역
- 새롭게 생성된 객체가 할당되는 영역.
- 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다가 사라짐.
- Young 영역에 대한 GC는 Minor GC.

1. Old 영역
- Young 영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역.
- Young 영역보다 크게 할당되며, 영역의 크기가 큰 만큼 garbage는 적게 발생.
    - Old 영역이 Young 영역보다 큰 이유는 수명이 긴 객체(Old 영역)들이 큰 공간을 필요로 하기 때문.
- Old 영역에 대한 GC는 Major GC(Full GC).

> 💡 **Perm 영역**
> 
> 생성된 객체들의 정보의 주소값이 저장되는 공간.</br>
> 리플렉션을 사용하여 동적으로 클래스가 로딩되는 경우에 사용됨.</br>
> Java 7까지는 Heap 영역에 존재했지만, Java 8 이후에는 Native Method Stack에 편입됨.

> 💡 **Reflection**
>
> 객체를 통해 클래스의 정보를 분석해 내는 프로그래밍 기법.</br>
> 구체적인 클래스 타입을 알지 못해도, 컴파일된 바이트 코드를 통해 역으로 클래스의 정보를 알아내어 사용가능.

Heap 영역은 더욱 효율적인 GC를 위해 Young 영역을 3가지 영역으로 나눔.

<img width="660" alt="Untitled (4)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/f8bab304-6a7c-4cc5-ad88-a8f656a91864">

1. Eden
- new 를 통해 새로 생성된 객체가 위치.
- 정기적인 garbage 수집 후 살아남은 객체들은 Survivor 영역으로 보냄.

1. Survivor 0, 1
- 최소 1번의 GC 이상 살아남은 객체가 존재하는 영역.
- Survivor 0 또는 Survivor 1 둘 중 하나는 반드시 비어있어야 함.
</br></br>

**Minor GC 과정**

1. 처음 생성된 객체는 Young 영역의 Eden 영역에 위치.
2. 객체가 계속 생성되어 Eden 영역이 꽉차게 되고, Minor GC 실행(Mark).
3. Eden 영역에서 살아남은 객체는 1개의 Survivor 영역으로 이동.
4. Eden 영역에서 사용되지 않는 객체의 메모리 해제(Sweep).
5. 살아남은 모든 객체들의 age 값 +1.
6. 1 ~ 5 번 과정 반복하며 앞서 Survivor 영역에도 Mark and Sweep 하며 Survivor 영역을 서로 번갈아가며 객체를 위치시킴.
</br>

**Major GC 과정**

1. Young 영역의 age가 임계값에 다다르면 Old 영역으로 이동시킴(promotion).
2. 1 번 과정을 반복하며 Old 영역의 메모리가 부족해지면 Major GC.
</br></br>

**Minor GC vs Major GC**

Minor GC에 비해 Major GC는 시간이 10배 이상 걸림.

여기서 Stop-The-World 문제 발생.

---

### GC 알고리즘 종류

1. **Serial GC**
- 서버의 코어가 1개일 때 사용하기 위해 개발된 가장 단순한 GC.
- GC를 처리하는 스레드가 1개 이므로 가장 Stop-The-World 시간이 김.
- 보통 실무에서 사용 X.
1. **Parallel GC**
- Java 8의 디폴트.
- Minor GC를 멀티 스레드로 수행(Old 영역은 싱글 스레드).
- Serial GC에 비해 Stop-The-World 시간 감소.
1. **Parallel Old GC(Parallel Compacting Collector)**
- Parallel GC 개선.
- Young & Old 영역 모두 멀티 스레드로 수행.

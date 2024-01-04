# 가상메모리, 스와핑, 단편화

---

### 가상 메모리

모든 프로세스에 메모리를 할당하기에는 메모리의 크기에 한계가 있음.

따라서 프로세스에서 사용하는 부분만 메모리에 올리고, 나머지는 디스크에 보관하는 기법을 가상 메모리라고 함.

---

### Swapping

메모리는 크기가 크지 않으므로, 프로세스를 임시로 디스크에 보냈다가 다시 메모리에 로드해야 하는 상황이 생김.

- swap out : 메모리 → 디스크.
- swap in : 디스크 → 메모리.

우선순위가 낮은 프로세스를 swap out 시키고, 높은 프로세스를 메모리에 올려놓음.
</br></br>

**메모리 단편화(Fragmentation)**

메모리의 공간이 작은 조각으로 나뉘어 사용 가능한 메모리가 충분히 존재하지만 할당(사용)이 불가능한 상태.

1. 내부 단편화 :

<img width="671" alt="Untitled" src="https://github.com/hgene0929/ComputerScience/assets/90823532/b1df729d-4121-46fe-a55a-4303f2bd1b2e">

- 메모리를 할당할 때 프로세스가 필요한 양보다 더 큰 메모리가 할당됨 → 프로세스에서 사용하는 메모리 공간이 낭비됨.

1. 외부 단편화 :

<img width="688" alt="Untitled (1)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/4083853a-59c4-4212-a12b-5f061770e22b">

- 메모리 공간 중 사용하지 못하게 되는 일부분.
- RAM에서 사이사이 남는 공간들을 모두 합치면 충분한 공간이 되는 부분들이 분산되어 있을 때 발생.

---

### **압축(Compaction)**

<img width="694" alt="Untitled (2)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/3379fa16-2429-43d8-9c54-45fed51d3d16">

외부 단편화를 해소하기 위해 프로세스가 사용하는 공간들을 한쪽으로 몰아, 자유공간을 확보.

작업효율이 좋지 않음.

---

### 통합(Coalecsing)

<img width="695" alt="Untitled (3)" src="https://github.com/hgene0929/ComputerScience/assets/90823532/f97b288a-15a5-4866-8295-d1e7278c2c5c">

단편화로 인해 분산된 메모리 공간들을 인접해 있는 것끼리 통합시켜 큰 메모리 공간으로 합침.

> 💡 **압축 vs 통합**
>
> 압축은 재배치되는 것이고, 통합은 인접 공간끼리 통합되는 것임.

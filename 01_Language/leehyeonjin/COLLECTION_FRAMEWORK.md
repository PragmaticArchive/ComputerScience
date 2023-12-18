# Collection Framework

---

### Collcetion Framework 정의

Collection : 다수의 데이터.

Framework : 표준화된 프로그래밍 방식.

Collection + Framework : 데이터 그룹을 저장하는 클래스들을 표준화한 설계.

Collection Framework를 활용하면 객체지향적이고 재사용성이 높은 코드를 작성 가능.

![Untitled](https://github.com/hgene0929/ComputerScience/assets/90823532/9ca6b4ad-c499-4c7a-a054-e3635deae78d)

Collection Framework의 주요 인터페이스로는 List, Set, Map이 있음.

| 인터페이스 | 특징 | 구현 클래스 |
| --- | --- | --- |
| List | 순서 유지, 저장, 중복 저장 O | ArrayList, Vector, Stack, Queue,  LinkedList  |
| Set | 순서 유지, 저장, 중복 저장 X | HashSet, TreeSet |
| Map | 키와값을 쌍으로 저장, 순서 유지 X, 키 중복 X | HashMap, Hashtable, TreeMap, Properties |

---

### List<E>

List 인터페이스는 순서를 유지해주고, 저장과 중복 저장을 허용.
</br></br>

**ArrayList**

Object 배열을 이용해 데이터를 순차적으로 저장.

객체가 인덱스로 관리됨.

배열은 생성시 크기가 고정되는 반면, ArrayList는 배열의 저장 공간을 초과하게 되는 경우에 더 큰 배열을 생성하여 기존 배열을 복사해서 새롭게 사용하기 때문에 자동으로 용량을 늘려줌.

중간의 객체를 제거하면, 바로 뒤의 객체부터 마지막 객체까지 모두 앞으로 1 인덱스씩 당겨짐.

> 객체를 자주 넣었다 빼게 되는 경우에는 ArrayList보다는 LinkedList를 사용하는 것이 좋음.
>

```java
List<타입> 객체명 = new ArrayList<타입>(초기 저장용량);
// 생성자의 타입 파라미터 생략 가능
List<Integer> num = new ArrayList<>(50);
// 초기 저장용량 생략시, 디폴트 용량은 10
List<String> name = new ArrayList<String>();
```
</br></br>
**LinkedList**

![Untitled (1)](https://github.com/hgene0929/ComputerScience/assets/90823532/90b570a5-0d5a-4bda-a9c0-c4a0ebe7e5e7)

불연속적으로 저장된 데이터를 서로 연결한 형태.

데이터를 효율적으로 추가, 삭제, 변경하기 위해 사용됨.

각 요소(node)들은 연결된 요소의 주소값과 데이터로 구성되어 있기 때문에, 데이터를 지우고자 할 때 이전 요소가 지우고자 하는 요소의 다음 요소를 참조하도록 변경하면 됨.

데이터를 복사하고 이동할 필요가 없기 때문에 처리 속도가 빠름.

---

### Set<E>

Set 인터페이스는 저장 순서를 유지하지 않고, 요소의 중복을 허용하지 않음.
</br></br>

**HashSet**

이미 저장된 요소를 또 추가하려고 하면, add() / addAll() 메서드가 false를 반환하여 추가에 실패했다는 것을 알림.

컬렉션 내 중복 저장을 방지.

입력 순서와 출력 순서가 서로 다름.

> 저장 순서를 지키고자 한다면 LinkedHashSet을 사용해야 함.

</br></br>
**TreeSet**

이진 검색 트리(binary search tree) 형태로 데이터를 저장.

> 💡 이진 검색 트리
> 
> 하나의 노드에 2개의 노드가 연결된 구조.</br>
> root라는 하나의 노드로부터 시작해 계속 확장됨.</br>
> 부모로부터 오른쪽으로 확장된 자식 노드는 반드시 부모 노드의 값보다 커야 함.</br>
> 왼쪽으로 확장된 자식 노드는 반드시 부모 노드의 값보다 작아야 함.</br>
> 정렬과 검색(범위검색)에 특화되어 있으면, 비순차적으로 저장하기 때문에 노드의 추가와 삭제에 시간이 소요됨.

중복저장을 허용하지 않고, 순서가 유지되지 않음.

---

### Map<K, V>

키-값 쌍으로 구성된 Entry 객체를 저장하는 구조.

해싱을 사용하기 때문에 방대한 양의 데이터를 검색하는데 있어 뛰어난 성능을 보임.
</br></br>

**HashMap**

저장되는 키는 유일한 것이어야 하고, 값은 중복이 허용됨.

HashMap은 키와 값에 null을 허용하지만, Hashtable은 허용하지 않음.

---

### Iterator

Java Collection 인터페이스에서 표준화된 컬렉션에 저장된 요소를 읽어오는 방법.

각 요소에 접근하는 기능(hasNext(), next())

Map은 Collection인터페이스를 상속받지 않기 때문에 사용할 수 없음.

---

### Comparator & Comparable

Collection 정렬에 필요한 메서드를 정의.

```java
public interface Comparator {
	// 두 객체가 같으면 0, 비교하는 값보다 작으면 음수, 크면 양수
	int compare(Object o1, Object o2);
	boolean equals(Object obj);
}

public interface Comparable {
	// 두 객체가 같으면 0, 비교하는 값보다 작으면 음수, 크면 양수
	public int compareTo(Object o);
}
```

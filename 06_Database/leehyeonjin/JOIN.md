# Join

---

### Join 정의

두 개 이상의 테이블이나 데이터베이스를 연결하여 데이터를 검색하는 방법.

테이블을 연결하려면, 적어도 하나의 컬럼을 서로 공유하고 있어야 하므로, 이를 이용하여 데이터 검색.

---

### Join 종류

1. inner join
2. left outer join
3. right outer join
4. full outer join
5. cross join
6. self join

---

### inner join

<img width="429" alt="Untitled (1)" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/6914daea-fa88-4e8c-8340-14ac5d748c02">

교집합으로, 기준 테이블과 join 테이블의 중복된 값을 보여줌.

```sql
select a.name, b.age
from std_table a
	inner join join_table b on a.id = b.a_id -- inner 생략가능
```

---

### left outer join

<img width="439" alt="Untitled (2)" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/99a75a45-9d51-40f0-af5f-13ccd5a00a4a">

기준 테이블과 조인 테이블의 중복된 값을 보여줌.

- 왼쪽 테이블을 기준으로 join 수행.

```sql
select a.name, b.age
from std_table a
	left outer join join_table b on a.id = b.a_id -- outer 생략가능
```

---

### right outer join

<img width="462" alt="Untitled (3)" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/89a045e7-ba16-4e48-a385-52a1aff66c3b">

left outer join과는 반대로 오른쪽 테이블을 기준으로 join 수행.

```sql
select a.name, b.age
from std_table a
	right outer join join_table b on a.id = b.a_id -- outer 생략가능
```

---

### full outer join

<img width="471" alt="Untitled (4)" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/6a28de6c-10fa-4f29-9d01-2b845bfa5db1">

합집합을 의미.

```sql
select a.name, b.age
from std_table a
	full outer join join_table b on a.id = b.a_id -- outer 생략가능
```

---

### cross join

<img width="436" alt="Untitled (5)" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/b366c103-1e60-4cc7-aac9-ffbf0350eb9d">

모든 경우의 수를 전부 표현.

join 대상 테이블의 튜플 개수를 곱해준 값과 동일한 개수의 결과 반환.

```sql
select a.name, b.age
from std_table a
	cross join join_table b
```

---

### self join

<img width="401" alt="Untitled (6)" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/d28aeebb-fab1-4045-8c9c-47f1004616c0">

자기자신과 자기자신을 조인.

하나의 테이블을 여러번 복사해서 조인하는 것과 동일.

자신이 가진 컬럼을 다양하게 변형시켜 활용할 때 자주 사용.

```sql
select a.name, b.age
from std_table a, join_table b
```

> 💡 **내부조인 vs 외부조인**
>
> - 내부조인은 동일한 값이 있는 행만 반환.
> - 외부조인은 동일한 값이 없어도 null의 형태로 반환.

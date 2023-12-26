# 정규화(Normalization)

---

### 정규화 정의

데이터베이스에서 중복을 최소화하기 위해 데이터를 분해하는 작업.

장점 : 중복으로 인한 이상현상을 방지할 수 있음 ⇒ DB 저장 용량 또한 효율적으로 관리 가능.

단점 : 릴레이션 간 연산이 많아질 수 있음.
</br></br>

**정규화 목적**

데이터의 중복을 없아면서 불필요한 데이터를 최소화.

무결성을 지키고, 이상현상을 방지.

테이블 구성을 논리적이고 직관적으로 할 수 있음.

데이터베이스 구조를 확장에 용이해짐.

---

### 정규화 과정

정규화에는 여러가지 단계가 있지만, 대체적으로 1 ~ 3 단계 정규화까지의 과정을 거침.
</br></br>

**제 1 정규화(1NF)**

도메인 제약조건 만족 : 테이블 컬럼이 원자값(하나의 값)을 갖도록 분리.

- 어떤 릴레이션에 속한 모든 도메인이 원자값만으로 되어 있어야 함.
- 모든 속성에 반복되는 그룹이 나타나지 않아야 함.
- 기본키를 사용하여 관련 데이터의 각 집합을 고유하게 식별할 수 있어야 함.

<img width="766" alt="Untitled" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/c1810d23-5f01-4e61-8960-c0eac73815f2">

위 테이블은 전화번호를 여러개 가지고 있어 원자값이 아님 → 1NF에 맞추기 위해서는 아래와 같이 분리해야 함.

<img width="769" alt="Untitled (1)" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/2bef88cd-5501-4522-979b-b5a75295c98b">
</br></br>

**제 2 정규화(2NF)**

부분적 함수종속 제거 : 테이블의 모든 컬럼이 완전 함수적 종속을 만족.

테이블에서 기본키가 복합키(키1, 키2)로 묶여 있을 때, 두 키 중 하나의 키만으로 다른 컬럼을 결정지을 수 있으면 안됨.

<img width="775" alt="Untitled (2)" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/69d443f8-5276-49b1-9223-31bd4178ddf3">

위 테이블에서는 Manufacturer + Model 복합키로 Model Full Name과 Manufacturer Country를 알 수 있음.

다만, 여기서 Manufacturer Country는 Manufacturer로 인해 결정됨 → Model과는 상관 없음.

따라서 부분적 함수 종속을 해결하기 위해 테이블을 아래와 같이 나눠 2NF를 만족시킴.

<img width="703" alt="Untitled (3)" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/0de1b7d0-d611-4ff2-8b0e-7d22666c9968">
</br></br>

**제 3 정규화(3NF)**

2NF가 진행된 테이블에서 이행적 종속을 없애기 위해 테이블을 분리.

> 💡 **이행적 종속**
>
> A → B, B → C이면 A → C가 성립된다.

- 릴레이션이 2NF에 만족한다.
- 기본키가 아닌 속성들은 키본키에 의존한다.

<img width="784" alt="Untitled (4)" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/d7eef5a2-80f6-4bd3-985d-393bfa059853">

위 테이블에서는 Tournament와 Year이 기본키 → Winner를 결정함.

그런데, Winner Date of Birth는 기본키가 아니라 Winner에 의해 결정됨.

따라서 아래와 같이 분리하여 3NF를 완성.

<img width="767" alt="Untitled (5)" src="https://github.com/PragmaticArchive/ComputerScience/assets/90823532/10f3cf6f-7529-4278-a17c-327b3e636367">

---

### 역정규화

데이터베이스의 사용 성능향상을 위해 의도적으로 데이터에 중복을 허용하고, 데이터를 합치는 것.

정규화를 거치면 릴레이션 간의 연산(JOIN)이 많아져 성능 저하가 될 수 있음.

따라서 성능 문제가 있는(읽기 작업이 많은) DB의 전반적인 성능을 향상시키기 위해 역정규화 수행.

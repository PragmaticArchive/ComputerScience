# 이상현상(Anomaly)

---

### 이상현상 정의

데이터의 중복현상으로 인해 발생하는 부작용.

정규화를 통해 이상현상이 발생하지 않도록 해야함.

---

### 이상 현상 종류

{stuendt_id, course_id, deparment, grade} 속성을 가지는 student 테이블 가정.
</br></br>

**1. 삽입 이상(Insertion Anomaly)**

> 불필요한 데이터를 추가해야만, 삽입할 수 있는 상황.
>

student 테이블의 기본키가 {student_id, course_id}인 경우, course를 수강하지 않은 학생은 course_id가 없음(null).

그런데, 기본키에는 null이 올 수 없으므로 테이블에 해당 데이터가 추가될 수 없음.

따라서 삽입하기 위해서는 ‘미수강’ 과 같은 course_id를 만들어야 함.
</br></br>

**2. 갱신 이상(Update Anomaly)**

> 일부만 변경하여 데이터가 불일치하는 모순의 문제.
>

만약 어떤 학생의 전공(deparment)이 변경되는 경우, 모든 deparment를 음악으로 변경해주어야 함.

그런데, 일부를 깜빡하고 변경하지 못하는 경우에 대한 파악을 확실하게 할 수 없음.
</br></br>

**3. 삭제 이상(Deletion Anomaly)**

> 튜플 삭제로 인해 꼭 필요한 데이터까지 함께 삭제되는 문제.
>

만약 어떤 학생이 수강을 철회하는 경우, {student_id, course_id, deparment, course_id, grade} 의 정보 중 student_id, deparment와 같은 학생에 대한 정보도 함꼐 삭제됨.

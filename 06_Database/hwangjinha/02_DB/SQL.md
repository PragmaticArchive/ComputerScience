tags: #CS #DB #SQL #DDL #DML #DCL #쿼리

---
# 종류
### DDL (Data Definition Language, 정의어)
- 데이터베이스 구조를 정의, 수정, 삭제하는 언어.
- `create`, `alter`, `drop`

### DML (Data Manipulation Language, 정의어)
- 데이터베이스 내의 자료 검색, 삽입, 갱신, 삭제를 위한 언어
- `insert`, `update`, `delete`, `select`

### DCL (Data Control Language, 정의어)
- 데이터에 대해 무결성 유지, 병행 수행 제어, 보호와 관리를 위한 언어
- `grant`, `revoke`

# select 쿼리의 수행 순서
```
from, on, join
where, group by, having
select
distinct
order by, limit
```
1. `from`, `on`, `join`
	- 어디에서 (어떤 데이터에서)
2. `where`, `group by`, `having`
	- 어떤 (어떤 조건으로)
3. `select`
	- 가져온다
 4. `distinct`
	 - 같은 데이터 구분, 삭제
  5. `order by`, `limit`
	  - 어떻게 (어떻게 보여줄지, 정렬을 할지 몇개를 보여줄지)

# group by
특정 칼럼을 기준으로 연산한 결과를 집게 키로 정의하여 그룹짓는 역할
집합 연산자는 `count`, `sum`, `avg`, `max`, `min` 등이 있고, `distinct`와 같이 중복 데이터를 제거

# Delete VS. Truncate VS. Drop
## Delete
- 원하는 데이터 삭제
- Roll Back 가능
## Truncate
- 테이블 용량이 줄어들고 인덱스 등도 삭제
- 전체 데이터를 한번에 삭제
- 테이블 자체는 삭제하지 않는다.
- Roll Back 불가능
## Drop
- 테이블 자체를 완전히 삭제 (인덱스, 객체 모두 삭제)
- Roll Back 불가능
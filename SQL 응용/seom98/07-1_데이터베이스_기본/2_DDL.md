# 1. DDL의 개념

**DDL(Data Definition Language)**은 데이터베이스의 구조를 정의하고 변경하며 삭제하는 언어입니다. 데이터베이스 스키마를 생성, 수정, 삭제하는 데 사용.

# 2. DDL의 대상

## 2-1. 스키마

### 스키마의 개념

**스키마(Schema)**는 데이터베이스의 구조와 제약조건을 정의한 메타데이터의 집합.

### 스키마의 구성

-   **외부스키마**: 사용자나 응용프로그램이 보는 데이터베이스의 논리적 구조
-   **개념스키마**: 데이터베이스의 전체적인 논리적 구조
-   **내부스키마**: 물리적 저장 구조와 접근 방법

## 2-2. 테이블

### 테이블의 개념

**테이블(Table)**은 관계형 데이터베이스에서 데이터를 저장하는 기본 단위.

### 테이블 용어

-   **튜플/행(Tuple/Row)**: 테이블의 각 행을 의미
-   **속성/열(Attribute/Column)**: 테이블의 각 열을 의미
-   **카디널리티(Cardinality)**: 튜플의 개수
-   **차수(Degree)**: 속성의 개수
-   **도메인(Domain)**: 각 속성이 가질 수 있는 값의 범위

## 2-3. 뷰

### 뷰의 개념

**뷰(View)**는 하나 이상의 테이블에서 유도된 가상의 테이블입니다. 실제로 데이터를 저장하지 않고, 저장된 데이터를 기반으로 정의.

### 뷰의 장단점

#### 장점

-   논리적 독립성 제공
-   데이터 조작 연산 간소화
-   보안 기능(접근 제어) 제공

#### 단점

-   뷰 자체 인덱스 불가
-   뷰 변경 불가
-   데이터 변경 제약 존재

## 2-4. 인덱스

### 인덱스의 개념

**인덱스(Index)**는 데이터베이스에서 데이터 검색 속도를 향상시키기 위한 자료구조.

### 인덱스의 특징

-   기본키 컬럼은 자동으로 인덱스가 생성됨
-   연월일이나 이름을 기준으로 하는 인덱스는 자동생성 안됨
-   컬럼에 인덱스가 없으면 테이블의 전체 내용을 검색함
-   인덱스가 생성되어 있을 때 빠르게 데이터를 찾을 수 있음
-   조건절에 "="로 비교되는 칼럼을 대상으로 인덱스를 생성하면 검색속도를 높일수 있음

# 3. DDL 명령어

-   **CREATE**: 데이터베이스 객체 생성
-   **ALTER**: 데이터베이스 객체 구조 변경
-   **DROP**: 데이터베이스 객체 삭제
-   **TRUNCATE**: 테이블의 모든 데이터 삭제

# 4. TABLE 관련 DDL

## 4-1. CREATE TABLE

### 설명

새로운 테이블을 생성하는 명령어.

### 문법

```sql
CREATE TABLE 테이블명 (
    컬럼명1 데이터타입 [제약조건],
    컬럼명2 데이터타입 [제약조건],
    ...
    [테이블 제약조건]
);
```

### 예시

```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50) NOT NULL,
    salary DECIMAL(10,2),
    hire_date DATE DEFAULT CURRENT_DATE
);
```

### 주요 옵션

-   **PRIMARY KEY**: 기본키 설정
-   **FOREIGN KEY**: 외래키 설정
-   **NOT NULL**: NULL 값 허용하지 않음
-   **UNIQUE**: 중복 값 허용하지 않음
-   **DEFAULT**: 기본값 설정
-   **CHECK**: 조건 검사

## 4-2. ALTER TABLE

### 설명

기존 테이블의 구조를 변경하는 명령어.

### 문법

```sql
-- 컬럼 추가
ALTER TABLE 테이블명 ADD 컬럼명 데이터타입 [제약조건];

-- 컬럼 수정
ALTER TABLE 테이블명 MODIFY 컬럼명 데이터타입 [제약조건];

-- 컬럼 삭제
ALTER TABLE 테이블명 DROP COLUMN 컬럼명;

-- 제약조건 추가
ALTER TABLE 테이블명 ADD CONSTRAINT 제약조건명 제약조건;

-- 제약조건 삭제
ALTER TABLE 테이블명 DROP CONSTRAINT 제약조건명;
```

### 예시

```sql
-- 컬럼 추가
ALTER TABLE employees ADD department VARCHAR(30);

-- 컬럼 수정
ALTER TABLE employees MODIFY salary DECIMAL(12,2);

-- 컬럼 삭제
ALTER TABLE employees DROP COLUMN hire_date;

-- 제약조건 추가
ALTER TABLE employees ADD CONSTRAINT fk_dept
    FOREIGN KEY (department) REFERENCES departments(dept_name);
```

## 4-3. DROP TABLE

### 설명

테이블을 완전히 삭제하는 명령어입니다.

### 문법

```sql
DROP TABLE 테이블명 [CASCADE | RESTRICT];
```

### 예시

```sql
DROP TABLE employees;
DROP TABLE employees CASCADE;
```

### 옵션

-   **CASCADE**: 관련된 모든 객체(뷰, 인덱스 등) 함께 삭제
-   **RESTRICT**: 관련 객체가 있으면 삭제 거부 (기본값)

## 4-4. TRUNCATE TABLE

### 설명

테이블의 모든 데이터를 삭제하지만 테이블 구조는 유지하는 명령어입니다.

### 문법

```sql
TRUNCATE TABLE 테이블명 [CASCADE];
```

### 예시

```sql
TRUNCATE TABLE employees;
```

### 특징

-   DELETE보다 빠른 실행 속도
-   롤백 불가능
-   외래키 제약조건이 있는 경우 CASCADE 옵션 필요

# 5. VIEW 관련 DDL

## 5-1. CREATE VIEW

### 설명

하나 이상의 테이블에서 데이터를 조회하는 가상 테이블을 생성합니다.

### 문법

```sql
CREATE VIEW 뷰명 AS
SELECT 컬럼1, 컬럼2, ...
FROM 테이블명
WHERE 조건;
```

### 예시

```sql
CREATE VIEW emp_dept_view AS
SELECT e.emp_id, e.emp_name, e.salary, d.dept_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
WHERE e.salary > 50000;
```

### 옵션

-   **WITH CHECK OPTION**: 뷰를 통한 데이터 변경 시 뷰 조건을 만족해야 함
-   **WITH READ ONLY**: 읽기 전용 뷰 생성

## 5-2. CREATE OR REPLACE VIEW

### 설명

기존 뷰가 있으면 교체하고, 없으면 새로 생성하는 명령어입니다.

### 문법

```sql
CREATE OR REPLACE VIEW 뷰명 AS
SELECT 컬럼1, 컬럼2, ...
FROM 테이블명
WHERE 조건;
```

### 예시

```sql
CREATE OR REPLACE VIEW emp_dept_view AS
SELECT e.emp_id, e.emp_name, e.salary, d.dept_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
WHERE e.salary > 60000;
```

## 5-3. DROP VIEW

### 설명

뷰를 삭제하는 명령어입니다.

### 문법

```sql
DROP VIEW 뷰명 [CASCADE];
```

### 예시

```sql
DROP VIEW emp_dept_view;
DROP VIEW emp_dept_view CASCADE;
```

### 옵션

-   **CASCADE**: 해당 뷰를 참조하는 다른 뷰들도 함께 삭제

# 6. INDEX 관련 DDL

## 6-1. CREATE INDEX

### 설명

테이블의 특정 컬럼에 인덱스를 생성하여 검색 성능을 향상시킵니다.

### 문법

```sql
CREATE [UNIQUE] INDEX 인덱스명
ON 테이블명 (컬럼1, 컬럼2, ...)
[옵션];
```

### 예시

```sql
-- 단일 컬럼 인덱스
CREATE INDEX idx_emp_name ON employees(emp_name);

-- 복합 인덱스
CREATE INDEX idx_emp_dept_salary ON employees(dept_id, salary);

-- 유니크 인덱스
CREATE UNIQUE INDEX idx_emp_email ON employees(email);
```

### 주요 옵션

-   **UNIQUE**: 중복 값 허용하지 않는 유니크 인덱스
-   **ASC/DESC**: 정렬 순서 지정
-   **CLUSTERED/NONCLUSTERED**: 클러스터형/비클러스터형 인덱스

## 6-2. ALTER INDEX

### 설명

기존 인덱스의 속성을 변경하거나 재구성합니다.

### 문법

```sql
ALTER INDEX 인덱스명 ON 테이블명
REBUILD | REORGANIZE | DISABLE;
```

### 예시

```sql
-- 인덱스 재구성
ALTER INDEX idx_emp_name ON employees REBUILD;

-- 인덱스 비활성화
ALTER INDEX idx_emp_name ON employees DISABLE;
```

### 옵션

-   **REBUILD**: 인덱스 완전 재생성
-   **REORGANIZE**: 인덱스 재구성 (단편화 제거)
-   **DISABLE**: 인덱스 비활성화

## 6-3. DROP INDEX

### 설명

인덱스를 삭제하는 명령어입니다.

### 문법

```sql
DROP INDEX 인덱스명 ON 테이블명;
```

### 예시

```sql
DROP INDEX idx_emp_name ON employees;
```

### 주의사항

-   기본키나 유니크 제약조건으로 자동 생성된 인덱스는 제약조건 삭제 후 삭제 가능
-   인덱스 삭제 시 해당 인덱스를 사용하던 쿼리의 성능이 저하될 수 있음

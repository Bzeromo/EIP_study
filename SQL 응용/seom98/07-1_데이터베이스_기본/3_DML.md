# 1. DML(데이터 조작어)의 개념

**DML(Data Manipulation Language)**은 데이터베이스에 저장된 자료들을 입력, 수정, 삭제, 조회하는 언어입니다.

-   데이터 조작어는 데이터베이스에 저장된 자료들을 입력, 수정, 삭제, 조회하는 언어

# 2. DML 명령어

-   **SELECT**: 조회. 테이블 내 칼럼에 저장된 데이터를 조회
-   **INSERT**: 삽입. 테이블 내 칼럼에 데이터를 추가
-   **UPDATE**: 갱신. 테이블 내 칼럼에 저장된 데이터를 수정
-   **DELETE**: 삭제. 테이블 내 칼럼에 저장된 데이터를 삭제

# 3. SELECT 명령어

## 3-1. SELECT 절

### 설명

조회할 컬럼을 지정하는 절입니다.

### 문법

```sql
SELECT [DISTINCT] 컬럼명1, 컬럼명2, ...
FROM 테이블명;
```

### 예시

```sql
-- 모든 컬럼 조회
SELECT * FROM employees;

-- 특정 컬럼만 조회
SELECT emp_id, emp_name, salary FROM employees;

-- 중복 제거
SELECT DISTINCT department FROM employees;

-- 별칭 사용
SELECT emp_id AS "사원번호", emp_name AS "이름" FROM employees;
```

### 주요 옵션

-   **DISTINCT**: 중복 행 제거
-   **AS**: 컬럼 별칭 지정
-   **\***: 모든 컬럼 선택

## 3-2. WHERE 절

### 설명

조건을 지정하여 특정 행만 조회하는 절입니다.

### 문법

```sql
SELECT 컬럼명
FROM 테이블명
WHERE 조건;
```

### 예시

```sql
-- 단순 조건
SELECT * FROM employees WHERE salary > 50000;

-- 복합 조건
SELECT * FROM employees
WHERE salary > 50000 AND department = 'IT';

-- IN 연산자
SELECT * FROM employees
WHERE department IN ('IT', 'HR', 'Sales');

-- LIKE 연산자
SELECT * FROM employees
WHERE emp_name LIKE '김%';

-- NULL 검사
SELECT * FROM employees
WHERE manager_id IS NULL;
```

### 주요 연산자

-   **비교 연산자**: =, <>, >, <, >=, <=
-   **논리 연산자**: AND, OR, NOT
-   **IN**: 여러 값 중 하나와 일치
-   **LIKE**: 패턴 매칭
-   **IS NULL/IS NOT NULL**: NULL 값 검사

## 3-3. GROUP BY 절

### 설명

특정 컬럼을 기준으로 그룹을 만들어 집계 함수를 적용하는 절입니다.

### 문법

```sql
SELECT 컬럼명, 집계함수(컬럼명)
FROM 테이블명
GROUP BY 컬럼명;
```

### 예시

```sql
-- 부서별 평균 급여
SELECT department, AVG(salary) as avg_salary
FROM employees
GROUP BY department;

-- 부서별, 직급별 인원수
SELECT department, job_title, COUNT(*) as emp_count
FROM employees
GROUP BY department, job_title;

-- 부서별 최고/최저 급여
SELECT department,
       MAX(salary) as max_salary,
       MIN(salary) as min_salary
FROM employees
GROUP BY department;
```

### 주요 집계 함수

-   **COUNT()**: 행 개수
-   **SUM()**: 합계
-   **AVG()**: 평균
-   **MAX()**: 최대값
-   **MIN()**: 최소값

## 3-4. HAVING 절

### 설명

GROUP BY 절의 결과에 조건을 적용하는 절입니다.

### 문법

```sql
SELECT 컬럼명, 집계함수(컬럼명)
FROM 테이블명
GROUP BY 컬럼명
HAVING 집계함수 조건;
```

### 예시

```sql
-- 평균 급여가 50000 이상인 부서
SELECT department, AVG(salary) as avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) >= 50000;

-- 직원 수가 5명 이상인 부서
SELECT department, COUNT(*) as emp_count
FROM employees
GROUP BY department
HAVING COUNT(*) >= 5;
```

### WHERE vs HAVING

-   **WHERE**: GROUP BY 전에 적용되는 조건
-   **HAVING**: GROUP BY 후에 적용되는 조건

## 3-5. ORDER BY 절

### 설명

조회 결과를 특정 컬럼을 기준으로 정렬하는 절입니다.

### 문법

```sql
SELECT 컬럼명
FROM 테이블명
ORDER BY 컬럼명 [ASC|DESC];
```

### 예시

```sql
-- 급여 오름차순 정렬
SELECT emp_name, salary
FROM employees
ORDER BY salary ASC;

-- 급여 내림차순 정렬
SELECT emp_name, salary
FROM employees
ORDER BY salary DESC;

-- 다중 정렬 (부서 오름차순, 급여 내림차순)
SELECT emp_name, department, salary
FROM employees
ORDER BY department ASC, salary DESC;
```

### 정렬 옵션

-   **ASC**: 오름차순 (기본값)
-   **DESC**: 내림차순

# 4. INSERT 명령어

### 설명

테이블에 새로운 데이터를 삽입하는 명령어입니다.

### 문법

```sql
-- 모든 컬럼에 값 삽입
INSERT INTO 테이블명 VALUES (값1, 값2, ...);

-- 특정 컬럼에만 값 삽입
INSERT INTO 테이블명 (컬럼1, 컬럼2, ...)
VALUES (값1, 값2, ...);

-- 서브쿼리를 이용한 삽입
INSERT INTO 테이블명 (컬럼1, 컬럼2, ...)
SELECT 컬럼1, 컬럼2, ... FROM 다른테이블명;
```

### 예시

```sql
-- 모든 컬럼에 값 삽입
INSERT INTO employees
VALUES (1001, '김철수', 50000, 'IT', '2023-01-15');

-- 특정 컬럼에만 값 삽입
INSERT INTO employees (emp_id, emp_name, salary)
VALUES (1002, '이영희', 45000);

-- 서브쿼리를 이용한 삽입
INSERT INTO emp_backup (emp_id, emp_name, salary)
SELECT emp_id, emp_name, salary
FROM employees
WHERE department = 'IT';
```

### 주의사항

-   컬럼 수와 값의 수가 일치해야 함
-   데이터 타입이 일치해야 함
-   NOT NULL 제약조건이 있는 컬럼은 반드시 값 입력 필요

# 5. UPDATE 명령어

### 설명

테이블의 기존 데이터를 수정하는 명령어입니다.

### 문법

```sql
UPDATE 테이블명
SET 컬럼1 = 값1, 컬럼2 = 값2, ...
WHERE 조건;
```

### 예시

```sql
-- 단일 행 수정
UPDATE employees
SET salary = 55000
WHERE emp_id = 1001;

-- 여러 컬럼 동시 수정
UPDATE employees
SET salary = salary * 1.1, department = 'IT'
WHERE department = '개발팀';

-- 조건부 수정
UPDATE employees
SET salary = CASE
    WHEN salary < 40000 THEN salary * 1.15
    WHEN salary < 60000 THEN salary * 1.10
    ELSE salary * 1.05
END
WHERE department = 'IT';
```

### 주의사항

-   WHERE 절이 없으면 모든 행이 수정됨
-   제약조건 위반 시 수정 실패
-   트랜잭션 내에서 실행하여 롤백 가능

# 6. DELETE 명령어

### 설명

테이블에서 데이터를 삭제하는 명령어입니다.

### 문법

```sql
DELETE FROM 테이블명
WHERE 조건;
```

### 예시

```sql
-- 특정 행 삭제
DELETE FROM employees
WHERE emp_id = 1001;

-- 조건부 삭제
DELETE FROM employees
WHERE department = '폐쇄팀';

-- 서브쿼리를 이용한 삭제
DELETE FROM employees
WHERE emp_id IN (
    SELECT emp_id
    FROM emp_backup
    WHERE backup_date < '2023-01-01'
);
```

### 주의사항

-   WHERE 절이 없으면 모든 행이 삭제됨
-   외래키 제약조건 확인 필요
-   트랜잭션 내에서 실행하여 롤백 가능
-   TRUNCATE와 달리 로그가 남아 롤백 가능

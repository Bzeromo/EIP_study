# 1. 조인

## 1-1. 조인 개념

**조인(Join)**은 두 개 이상의 테이블에서 관련된 데이터를 결합하여 하나의 결과 집합을 만드는 연산입니다.

### 조인의 필요성

-   관계형 데이터베이스의 정규화로 인해 분산된 데이터를 통합
-   여러 테이블의 데이터를 한 번에 조회하여 효율성 증대
-   데이터의 일관성과 무결성 유지

## 1-2. 조인 종류

### 교차 조인 (CROSS JOIN)

#### 설명

두 테이블의 모든 행을 조합하여 카티션 곱을 생성하는 조인입니다.

#### 문법

```sql
SELECT 컬럼명
FROM 테이블1 CROSS JOIN 테이블2;
-- 또는
SELECT 컬럼명
FROM 테이블1, 테이블2;
```

#### 예시

```sql
SELECT e.emp_name, d.dept_name
FROM employees e CROSS JOIN departments d;
```

### 세타 조인 (THETA JOIN)

#### 설명

조인 조건에 비교 연산자(=, <>, >, <, >=, <=)를 사용하는 조인입니다.

#### 문법

```sql
SELECT 컬럼명
FROM 테이블1, 테이블2
WHERE 테이블1.컬럼 연산자 테이블2.컬럼;
```

#### 예시

```sql
SELECT e.emp_name, d.dept_name
FROM employees e, departments d
WHERE e.salary > d.budget;
```

### 동등 조인 (EQUI JOIN)

#### 설명

조인 조건에 등호(=) 연산자를 사용하여 두 테이블의 공통 컬럼 값이 같은 행들을 결합하는 조인입니다.

#### 문법

```sql
SELECT 컬럼명
FROM 테이블1, 테이블2
WHERE 테이블1.컬럼 = 테이블2.컬럼;
```

#### 예시

```sql
SELECT e.emp_name, d.dept_name
FROM employees e, departments d
WHERE e.dept_id = d.dept_id;
```

### 자연 조인 (NATURAL JOIN)

#### 설명

두 테이블의 공통 컬럼을 자동으로 찾아서 동등 조인을 수행하는 조인입니다.

#### 문법

```sql
SELECT 컬럼명
FROM 테이블1 NATURAL JOIN 테이블2;
```

#### 예시

```sql
SELECT emp_name, dept_name
FROM employees NATURAL JOIN departments;
```

### 외부 조인 (OUTER JOIN)

#### 설명

조인 조건을 만족하지 않는 행들도 결과에 포함시키는 조인입니다.

#### 문법

```sql
-- LEFT OUTER JOIN
SELECT 컬럼명
FROM 테이블1 LEFT OUTER JOIN 테이블2
ON 테이블1.컬럼 = 테이블2.컬럼;

-- RIGHT OUTER JOIN
SELECT 컬럼명
FROM 테이블1 RIGHT OUTER JOIN 테이블2
ON 테이블1.컬럼 = 테이블2.컬럼;

-- FULL OUTER JOIN
SELECT 컬럼명
FROM 테이블1 FULL OUTER JOIN 테이블2
ON 테이블1.컬럼 = 테이블2.컬럼;
```

#### 예시

```sql
-- 모든 직원과 부서 정보 (부서가 없는 직원도 포함)
SELECT e.emp_name, d.dept_name
FROM employees e LEFT OUTER JOIN departments d
ON e.dept_id = d.dept_id;

-- 모든 부서와 직원 정보 (직원이 없는 부서도 포함)
SELECT e.emp_name, d.dept_name
FROM employees e RIGHT OUTER JOIN departments d
ON e.dept_id = d.dept_id;
```

### 세미 조인 (SEMI JOIN)

#### 설명

EXISTS나 IN 연산자를 사용하여 한 테이블의 행이 다른 테이블에 존재하는지 확인하는 조인입니다.

#### 문법

```sql
SELECT 컬럼명
FROM 테이블1
WHERE EXISTS (SELECT 1 FROM 테이블2 WHERE 조건);
```

#### 예시

```sql
-- 부서가 있는 직원만 조회
SELECT emp_name
FROM employees e
WHERE EXISTS (
    SELECT 1 FROM departments d
    WHERE e.dept_id = d.dept_id
);
```

### 셀프 조인 (SELF JOIN)

#### 설명

같은 테이블을 두 번 조인하여 자기 참조 관계를 표현하는 조인입니다.

#### 문법

```sql
SELECT a.컬럼명, b.컬럼명
FROM 테이블 a, 테이블 b
WHERE a.컬럼 = b.컬럼;
```

#### 예시

```sql
-- 직원과 상사 정보 조회
SELECT e.emp_name as "직원", m.emp_name as "상사"
FROM employees e, employees m
WHERE e.manager_id = m.emp_id;
```

# 2. 서브쿼리

## 2-1. 서브쿼리 개념

**서브쿼리(Subquery)**는 SQL 문 안에 포함된 또 다른 SQL 문입니다. 메인 쿼리의 조건이나 데이터를 제공하는 역할을 합니다.

### 서브쿼리의 특징

-   괄호로 묶어서 사용
-   단일 행 또는 다중 행 반환 가능
-   메인 쿼리보다 먼저 실행됨

## 2-2. 서브쿼리 유형

### FROM 절 서브쿼리 (인라인 뷰)

#### 설명

FROM 절에 서브쿼리를 사용하여 임시 테이블을 만드는 방식입니다.

#### 문법

```sql
SELECT 컬럼명
FROM (서브쿼리) 별칭
WHERE 조건;
```

#### 예시

```sql
-- 부서별 평균 급여보다 높은 급여를 받는 직원 조회
SELECT e.emp_name, e.salary, dept_avg.avg_salary
FROM employees e,
     (SELECT dept_id, AVG(salary) as avg_salary
      FROM employees
      GROUP BY dept_id) dept_avg
WHERE e.dept_id = dept_avg.dept_id
  AND e.salary > dept_avg.avg_salary;
```

### WHERE 절 서브쿼리

#### 설명

WHERE 절의 조건에 서브쿼리를 사용하여 동적인 조건을 만드는 방식입니다.

#### 문법

```sql
SELECT 컬럼명
FROM 테이블명
WHERE 컬럼명 연산자 (서브쿼리);
```

#### 예시

```sql
-- 평균 급여보다 높은 급여를 받는 직원 조회
SELECT emp_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- IT 부서 직원들과 같은 급여를 받는 직원 조회
SELECT emp_name, salary
FROM employees
WHERE salary IN (SELECT salary FROM employees WHERE dept_id = 'IT');

-- 가장 높은 급여를 받는 직원 조회
SELECT emp_name, salary
FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees);
```

### 스칼라 서브쿼리

#### 설명

단일 값을 반환하는 서브쿼리로, SELECT 절이나 UPDATE의 SET 절에서 사용됩니다.

#### 예시

```sql
-- 각 직원의 급여와 부서 평균 급여를 함께 조회
SELECT emp_name, salary,
       (SELECT AVG(salary) FROM employees e2 WHERE e2.dept_id = e1.dept_id) as dept_avg
FROM employees e1;
```

# 3. 집합연산자

## 3-1. 집합연산자 개념

**집합연산자**는 두 개 이상의 SELECT 문의 결과를 하나로 결합하는 연산자입니다. 수학의 집합 연산과 유사한 개념을 사용합니다.

### 집합연산자의 특징

-   두 SELECT 문의 컬럼 수가 같아야 함
-   대응하는 컬럼의 데이터 타입이 호환되어야 함
-   ORDER BY는 마지막 SELECT 문에만 사용 가능

## 3-2. 집합연산자 유형

### UNION

#### 설명

두 SELECT 문의 결과를 합집합으로 결합하며, 중복된 행은 제거합니다.

#### 문법

```sql
SELECT 컬럼명 FROM 테이블1
UNION
SELECT 컬럼명 FROM 테이블2;
```

#### 예시

```sql
-- IT 부서와 HR 부서 직원들의 이름 조회
SELECT emp_name FROM employees WHERE dept_id = 'IT'
UNION
SELECT emp_name FROM employees WHERE dept_id = 'HR';
```

### UNION ALL

#### 설명

두 SELECT 문의 결과를 합집합으로 결합하며, 중복된 행도 포함합니다.

#### 문법

```sql
SELECT 컬럼명 FROM 테이블1
UNION ALL
SELECT 컬럼명 FROM 테이블2;
```

#### 예시

```sql
-- 모든 직원과 퇴사한 직원들의 이름 조회 (중복 포함)
SELECT emp_name FROM employees
UNION ALL
SELECT emp_name FROM retired_employees;
```

### INTERSECT

#### 설명

두 SELECT 문의 결과에서 공통으로 존재하는 행들만 반환합니다 (교집합).

#### 문법

```sql
SELECT 컬럼명 FROM 테이블1
INTERSECT
SELECT 컬럼명 FROM 테이블2;
```

#### 예시

```sql
-- IT 부서와 개발팀에 모두 속한 직원들의 이름 조회
SELECT emp_name FROM employees WHERE dept_id = 'IT'
INTERSECT
SELECT emp_name FROM employees WHERE team = '개발팀';
```

### MINUS (EXCEPT)

#### 설명

첫 번째 SELECT 문의 결과에서 두 번째 SELECT 문의 결과를 제외한 행들만 반환합니다 (차집합).

#### 문법

```sql
SELECT 컬럼명 FROM 테이블1
MINUS
SELECT 컬럼명 FROM 테이블2;
```

#### 예시

```sql
-- IT 부서에 속하지만 개발팀에는 속하지 않은 직원들의 이름 조회
SELECT emp_name FROM employees WHERE dept_id = 'IT'
MINUS
SELECT emp_name FROM employees WHERE team = '개발팀';
```

### 집합연산자 사용 시 주의사항

1. **컬럼 수 일치**: 모든 SELECT 문의 컬럼 수가 같아야 함
2. **데이터 타입 호환**: 대응하는 컬럼의 데이터 타입이 호환되어야 함
3. **ORDER BY 위치**: 마지막 SELECT 문에만 ORDER BY 사용 가능
4. **컬럼명**: 첫 번째 SELECT 문의 컬럼명이 결과의 컬럼명이 됨
5. **NULL 값 처리**: NULL 값도 하나의 값으로 취급하여 비교

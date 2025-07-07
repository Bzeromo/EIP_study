# SQL 응용 정리

## 1. DDL (Data Definition Language)

### 정의

- 데이터 정의 언어로 DB의 구조와 형식, 접근 방식을 정의함
- 데이터베이스를 구축하거나 수정할 목적으로 사용

### 주요 명령어

| 명령어 | 기능                                    |
| ------ | --------------------------------------- |
| CREATE | SCHEMA, DOMAIN, TABLE, VIEW, INDEX 정의 |
| ALTER  | TABLE 구조 변경                         |
| DROP   | SCHEMA, DOMAIN, TABLE, VIEW, INDEX 삭제 |

## 2. CREATE TABLE

### 문법

```sql
CREATE TABLE 테이블명 (
  속성명 데이터_타입 [DEFAULT 기본값] [NOT NULL],
  ...
  PRIMARY KEY(속성명),
  UNIQUE(속성명),
  FOREIGN KEY(속성명) REFERENCES 참조테이블(속성명)
    [ON DELETE 옵션]
    [ON UPDATE 옵션],
  CONSTRAINT 제약조건명 CHECK(조건식)
);
```

### 제약 조건 설명

- **PRIMARY KEY**: 기본키 지정
- **UNIQUE**: 중복 불가
- **FOREIGN KEY**: 외래키, 참조 테이블과 연결

  - ON DELETE / ON UPDATE

    - NO ACTION: 아무런 조치 없음
    - CASCADE: 연쇄 삭제/변경
    - SET NULL: NULL로 설정
    - SET DEFAULT: 기본값으로 설정

- **CHECK**: 속성 값에 대한 제약 조건 정의
- **CONSTRAINT**: 제약 조건에 이름 부여

### 예제

```sql
CREATE TABLE 학생 (
  이름 VARCHAR(15) NOT NULL,
  학번 CHAR(8),
  전공 CHAR(5),
  성별 SEX,
  생년월일 DATE,
  PRIMARY KEY(학번),
  FOREIGN KEY(전공) REFERENCES 학과(학과코드)
    ON DELETE NO ACTION
    ON UPDATE CASCADE,
  CONSTRAINT 생년월일체크 CHECK(생년월일 >= '1980-01-01')
);
```

## 3. CREATE VIEW

### 문법

```sql
CREATE VIEW 뷰명(속성1, 속성2, ...) AS SELECT문;
```

### 예제

```sql
CREATE VIEW 안산고객(성명, 전화번호)
AS SELECT 성명, 전화번호
FROM 고객
WHERE 주소 = '안산시';
```

### 복합 JOIN 예제

```sql
CREATE VIEW CC(ccid, ccname, instname)
AS SELECT Course.id, Course.name, Instructor.name
FROM Course, Instructor
WHERE Course.instructor = Instructor.id;
```

## 4. CREATE INDEX

### 문법

```sql
CREATE [UNIQUE] INDEX 인덱스명
ON 테이블명(속성명 [ASC | DESC], ...)
[CLUSTER];
```

- **UNIQUE**: 중복 불가 인덱스 생성
- **CLUSTER**: 클러스터드 인덱스로 설정

### 예제

```sql
CREATE UNIQUE INDEX 고객번호_idx
ON 고객(고객번호 DESC);
```

## 5. ALTER TABLE

### 문법

```sql
ALTER TABLE 테이블명 ADD 속성명 데이터_타입 [DEFAULT 값];
ALTER TABLE 테이블명 ALTER 속성명 SET DEFAULT '기본값';
ALTER TABLE 테이블명 DROP COLUMN 속성명 [CASCADE];
```

### 예제

```sql
ALTER TABLE 학생 ADD 학번 VARCHAR(3);

ALTER TABLE 학생 ALTER 학번 VARCHAR(10) NOT NULL;
```

## 6. DROP

### 문법

```sql
DROP SCHEMA 스키마명 [CASCADE | RESTRICT];
DROP DOMAIN 도메인명 [CASCADE | RESTRICT];
DROP TABLE 테이블명 [CASCADE | RESTRICT];
DROP VIEW 뷰명 [CASCADE | RESTRICT];
DROP INDEX 인덱스명 [CASCADE | RESTRICT];
DROP CONSTRAINT 제약조건명;
```

- **CASCADE**: 참조하는 모든 객체 함께 삭제
- **RESTRICT**: 참조 중이면 삭제 취소

### 예제

```sql
DROP TABLE 학생 CASCADE;
```

## 7. DCL (Data Control Language)

### 정의

- 데이터 보안, 무결성, 회복, 병행 제어 등을 정의
- 주로 DBA가 사용

### 명령어

| 명령어   | 기능           |
| -------- | -------------- |
| COMMIT   | 변경 사항 저장 |
| ROLLBACK | 변경 사항 취소 |
| GRANT    | 권한 부여      |
| REVOKE   | 권한 회수      |

## 8. GRANT / REVOKE

### 문법

```sql
GRANT 권한리스트 ON 개체 TO 사용자 [WITH GRANT OPTION];
REVOKE [GRANT OPTION FOR] 권한리스트 ON 개체 FROM 사용자 [CASCADE];
```

- **WITH GRANT OPTION**: 재부여 가능
- **GRANT OPTION FOR**: 권한 부여 권한 회수
- **CASCADE**: 연쇄 회수

### 예제

```sql
GRANT ALL ON 고객 TO NABI WITH GRANT OPTION;

REVOKE GRANT OPTION FOR UPDATE ON 고객 FROM STAR;
```

## 9. ROLLBACK

- 아직 COMMIT되지 않은 변경 내용 모두 취소
- 트랜잭션 이전 상태로 되돌림

## 10. DML (Data Manipulation Language)

### 명령어

| 명령어 | 기능        |
| ------ | ----------- |
| SELECT | 데이터 검색 |
| INSERT | 데이터 삽입 |
| DELETE | 데이터 삭제 |
| UPDATE | 데이터 수정 |

## 11. 삽입문 (INSERT INTO)

### 문법

```sql
INSERT INTO 테이블명(속성1, 속성2, ...)
VALUES (값1, 값2, ...);
```

- 속성 수와 데이터 수가 일치해야 함
- 모든 속성 입력 시 속성명 생략 가능
- SELECT문으로 다른 테이블의 결과 삽입 가능

### 예제

```sql
INSERT INTO 사원(이름, 부서) VALUES ('홍승현', '인터넷');

INSERT INTO 사원 VALUES ('정보국', '기획', '05/03/73', '홍제동', 90);

INSERT INTO 편집부원(이름, 생일, 주소, 기본급)
SELECT 이름, 생일, 주소, 기본급
FROM 사원
WHERE 부서 = '편집';
```

## 12. 삭제문 (DELETE FROM)

### 문법

```sql
DELETE FROM 테이블명 [WHERE 조건];
```

- WHERE절 생략 시 모든 튜플 삭제
- 테이블 구조는 남고 데이터만 제거됨

### 예제

```sql
DELETE FROM 사원 WHERE 이름 = '임걱정';

DELETE FROM 사원 WHERE 부서 = '인터넷';

DELETE FROM 사원;
```

## 13. 갱신문 (UPDATE)

### 문법

```sql
UPDATE 테이블명
SET 속성1 = 값1, 속성2 = 값2
[WHERE 조건];
```

- 특정 튜플의 데이터 변경 시 사용

### 예제

```sql
UPDATE 사원
SET 주소 = '수색동'
WHERE 이름 = '홍길동';

UPDATE 사원
SET 부서 = '기획', 기본급 = 기본급 + 5
WHERE 이름 = '황진이';
```

## 14. 검색문 (SELECT)

### 문법

```sql
SELECT [PREDICATE] 속성명 [, ...]
FROM 테이블명 [, ...]
[WHERE 조건]
[GROUP BY 속성명]
[HAVING 조건]
[ORDER BY 속성명 [ASC|DESC]];
```

### 주요 절

- **PREDICATE**: 검색 튜플 수 제한
- **DISTINCT**: 중복 제거
- **AS**: 별칭
- **GROUP BY**: 그룹화
- **HAVING**: 그룹 조건
- **ORDER BY**: 정렬 기준

## 15. 기본 검색

### 예제

```sql
SELECT * FROM 사원;

SELECT DISTINCT 주소 FROM 사원;
```

## 16. 조건 지정 검색 (WHERE절)

### 비교 연산자

| 연산자 | 의미        |
| ------ | ----------- |
| =      | 같다        |
| >      | 크다        |
| <      | 작다        |
| >=     | 크거나 같다 |
| <=     | 작거나 같다 |
| <>     | 다르다      |

### 논리 연산자

- NOT, AND, OR

### LIKE 연산자

| 기호 | 의미      |
| ---- | --------- |
| %    | 모든 문자 |
| \_   | 한 문자   |

### IN 연산자

- 특정 값들 중 포함 여부 확인

### 예제

```sql
SELECT * FROM 사원 WHERE 부서 = '기획';

SELECT * FROM 사원 WHERE 부서 = '기획' AND 주소 = '대홍동';

SELECT * FROM 사원 WHERE 이름 LIKE '김%';

SELECT * FROM 사원 WHERE 생일 BETWEEN '01/01/69' AND '12/31/73';

SELECT * FROM 사원 WHERE 주소 IS NULL;
```

## 17. 정렬 검색 (ORDER BY)

### 정의

- ORDER BY 절을 이용해 특정 속성 기준으로 결과 정렬
- 오름차순 `ASC` (기본값), 내림차순 `DESC`

### 예제

```sql
SELECT *
FROM 사원
ORDER BY 부서 ASC, 이름 DESC;

SELECT name, score
FROM 성적
ORDER BY score DESC;
```

## 18. 하위 질의 (Subquery)

### 정의

- 조건절 안에 또 다른 SELECT문이 들어가는 구조
- `IN`, `NOT IN`, `ALL`, `ANY`, `EXISTS` 등과 함께 사용

### 예제

```sql
-- '나이트댄스'가 취미인 사원의 이름과 주소
SELECT 이름, 주소
FROM 사원
WHERE 이름 = (
  SELECT 이름 FROM 여가활동
  WHERE 취미 = '나이트댄스'
);
```

```sql
-- 여가활동 하지 않는 사원
SELECT *
FROM 사원
WHERE 이름 NOT IN (
  SELECT 이름 FROM 여가활동
);
```

```sql
-- 망원동 거주자의 급여보다 적은 급여
SELECT 이름, 기본급, 주소
FROM 사원
WHERE 기본급 < ALL (
  SELECT 기본급 FROM 사원
  WHERE 주소 = '망원동'
);
```

## 19. 그룹 함수 (집계 함수)

### 정의

- GROUP BY절과 함께 사용되어 그룹별 통계 계산

| 함수     | 기능          |
| -------- | ------------- |
| COUNT    | 개수 구함     |
| SUM      | 합계 구함     |
| AVG      | 평균 구함     |
| MAX      | 최대값 구함   |
| MIN      | 최소값 구함   |
| STDEV    | 표준편차 구함 |
| VARIANCE | 분산 구함     |

### 예제

```sql
SELECT COUNT(*)
FROM EMP_TBL
WHERE EMPNO > 100 AND SAL >= 3000
OR EMPNO = 200;
```

## 20. 그룹 지정 검색 (GROUP BY / HAVING)

### 정의

- `GROUP BY`: 특정 속성 기준으로 그룹화
- `HAVING`: 그룹에 조건 부여

### 예제 1

```sql
SELECT 부서, AVG(상여금) AS 평균
FROM 상여금
GROUP BY 부서;
```

### 예제 2

```sql
SELECT 부서, COUNT(*) AS 사원수
FROM 상여금
WHERE 상여금 > 100
GROUP BY 부서
HAVING COUNT(*) >= 2;
```

## 21. 집합 연산자 (UNION, INTERSECT, EXCEPT)

### 정의

- 두 SELECT문의 결과를 합치거나 비교하는 연산자

| 연산자    | 설명               | 분류   |
| --------- | ------------------ | ------ |
| UNION     | 합집합 (중복 제거) | 합집합 |
| UNION ALL | 합집합 (중복 유지) | 합집합 |
| INTERSECT | 교집합             | 교집합 |
| EXCEPT    | 차집합             | 차집합 |

### 예제

```sql
SELECT A FROM R
UNION
SELECT A FROM S
ORDER BY A DESC;
```

## 22. JOIN

### 정의

- 두 개 이상의 테이블을 연결하여 결과 반환
- FROM절 또는 WHERE절에 사용

### 종류

#### INNER JOIN

- 공통된 값이 있는 튜플만 반환

#### OUTER JOIN

- LEFT OUTER JOIN: 왼쪽 테이블의 모든 행 + 오른쪽 매칭 행
- RIGHT OUTER JOIN: 오른쪽 테이블의 모든 행 + 왼쪽 매칭 행
- FULL OUTER JOIN: 양쪽 모두의 모든 행 포함

#### EQUI JOIN (동등 조인)

- `= 연산자`를 이용하여 연결

```sql
SELECT *
FROM A, B
WHERE A.id = B.id;
```

#### NATURAL JOIN

```sql
SELECT *
FROM A NATURAL JOIN B;
```

#### USING JOIN

```sql
SELECT *
FROM A JOIN B USING(공통속성);
```

### 예제

```sql
SELECT 학번, 이름, 학생.학과코드, 학과명
FROM 학생 LEFT OUTER JOIN 학과
ON 학생.학과코드 = 학과.학과코드;
```

## 23. 트리거 (Trigger)

### 정의

- 데이터베이스에서 `INSERT`, `UPDATE`, `DELETE` 이벤트 발생 시 자동 실행되는 SQL 절차

### 용도

- 데이터 무결성 유지
- 자동 로그 기록
- 데이터 변경 알림

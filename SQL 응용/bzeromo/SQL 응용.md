***
# ⚡ SQL 응용
***

## 📌 SQL(Structed Query Language)

🔷 **관계형 데이터 베이스에서 데이터 조작과 데이터 정의를 위해 사용하는 언어**

1. 데이터 조회
2. 데이터 삽입, 삭제, 수정
3. DB Object 생성 및 변경, 삭제
4. DB 사용자 생성 및 삭제, 권한 제어

> 💡 표준 SQL은 모든 DBMS에서 사용가능하다.

![](https://velog.velcdn.com/images/bzeromo/post/d2c29fa5-25d6-489e-8c77-ce9705ee5aaf/image.png)

🔷 **SQL 특징**
- 배우고 사용하기 쉽다
- 대소문자를 구별하지 않는다. (데이터의 대소문자는 구분)
- 절차적인 언어가 아니라 선언적 언어이다.
- DBMS에 종속적이지 않다.

🔷 `DML ( Data Manipulation Language)` : **데이터 조작 언어**
- 데이터베이스에서 데이터를 조작하거나 조회할 때 사용
- 테이블의 레코드를 `CRUD` (Create, Read, Update, Delete)

|문장|설명|
|:---:|:---:|
|SELECT|데이터베이스에서 데이터를 조회할 때 사용|
|INSERT|테이블에 새 행을 입력할 때 사용|
|UPDATE|기존 행을 변경할 때 사용|
|DELETE|테이블에서 행을 삭제할 때 사용|

> 💡 SELECT가 가장 중요하다!

🔷 `DDL ( Data Definition Language)` : **데이터 정의 언어**
- 데이터 베이스 객체(table, view, user, index 등)의 구조를 정의

|문장|설명|
|:---:|:---:|
|CREATE|테이블 등 데이터 객체를 생성할 때 사용|
|ALTER|테이블 등 데이터 객체를 변경할 때 사용|
|DROP|테이블 등 데이터 객체를 삭제할 때 사용|
|RENAME|테이블 등 데이터 객체의 이름을 변경할 때 사용|

🔷 `TCL ( Transaction Control Language)` : **트랜잭션 제어 언어**

- 트랜잭션단위로 실행한 명령문을 적용하거나 취소
- `COMMIT`, `ROLLBACK` 문장이 있다.

> 💡 `COMMIT`, `ROLLBACK`
DML문이 변경한 내용을 관리. 변경사항을 저장하거나 취소할 때 사용한다.
이 때, DML 변경 내용은 트랜잭션 단위로 그룹화 가능하다.

🔷 `DCL ( Data Control Language)` : **데이터 제어 언어**

- Database, Table 접근권한이나 CRUD권한 정의
- 특정 사용자에게 테이블의 검색권한 부여/금지

|문장|설명|
|:---:|:---:|
|GRANT|데이터베이스 접근권한 부여|
|REVOKE|데이터베이스 접근권한 삭제|

***

## 📌 DDL(Data Definition Language)

🔷 **데이터베이스 생성**

```sql
CREATE DATABASE databasename;
```

- `CREATE DATABASE` 명령문은 새 데이터 베이스를 생성하는데 사용된다.
- 데이터 베이스는 여러 테이블을 포함하고 있다.
- 데이터 베이스 생성시 관리자 권한으로 생성해야 한다.

🔷 **데이터베이스 목록 조회**

```sql
SHOW DATABASES;
```

![](https://velog.velcdn.com/images/bzeromo/post/f462f291-c513-45ed-9e5e-ff8938533660/image.png)


🔷 **데이터베이스 수정 및 데이터베이스 문자 집합(Character set) 설정하기**
- 데이터 베이스 생성 시 설정 또는 생성 후 수정 가능
- 문자집합은 각 문자가 컴퓨터에 저장될 때 어떠한 ‘코드’ 로 저장되는지 규칙을 지정한 집합이다.
- `Collation`은 특정 문자 집합에 의해 데이터베이스에 저장된 값들을 비교, 검색, 정렬 등의 작업을 수행할 때 사용하는 비교 규칙 집합이다.

```sql
-- 주석
-- 기본값 확인
SHOW VARIABLES LIKE 'c%';
-- 사용하는 CHARSET 확인
SHOW CHARACTER SET;

-- 데이터베이스 수정
ALTER DATABASE bzeromodb
-- utf8mb3는 다국어처리 utf8mb4는 이모지까지 가능
DEFAULT CHARACTER SET utf8mb4 collate utf8mb4_general_ci;
```

🔷 **데이터베이스 삭제**
- 데이터베이스의 모든 테이블을 삭제하고 데이터베이스를 삭제
- 삭제 시, `DROP DATABASE` 권한 필요
- `DROP SCHEMA` 는 `DROP DATABASE`와 동의어
- `IF EXISTS`는 데이터베이스가 없을 시 발생할 수 있는 에러를 방지

```sql
DROP DATABASE bzeromodb;
DROP DATABASE IF EXISTS bzeromodb;
```

![](https://velog.velcdn.com/images/bzeromo/post/b9c38e0e-f071-4c31-bfb8-c1b732ca1a5e/image.png)


***

## 📌 Data Type (자료형)

🔷 **숫자 자료형(Numeric Data Types)**

|데이터유형|크기|(Byte) 정의|
|:---:|:---:|:---:|
|`BIT[(M)]`|1|비트 값 유형. M은 값 당 비트 수를 나타냄. 1 ~ 64 사이의 값.|
|`TINYINT[(M)]`|1|(signed) -128 ~ 127 (unsigned) 0 ~ 255|
|`BOOL`, `BOOLEAN`||TINYINT (1)의 동의어. 0은 false, 0이 아닌 값은 true|
|`SMALLINT[(M)]`|2|(signed) -32768 ~ 32767 (unsigned) 0 ~ 65535|
|`MEDIUMINT[(M)]`|3|(signed) -8388608 ~ 8388607 (unsigned) 0 ~ 16777215|
|`INT[(M)]`, `INTEGER[(M)]`|4|(signed) -2147483648 ~ 2147483647 (unsigned) 0 ~ 4294967295|
|`BIGINT[(M)]`|8|(signed) -9223372036854775808 ~ 9223372036854775807 (unsigned) 0 ~ 18446744073709551615|

🔷 **문자 자료형(String Data Types)**

|데이터유형|정의|
|:---:|:---:|
|`CHAR[(M)]`|고정길이를 갖는 문자열을 저장.M은 0 ~255. CHAR(20)인 컬럼에 10글자 저장, 20글자 만큼의 공간을 차지|
|`VARCHAR[(M)]`|가변길이를 갖는 문자열을 저장. M은 1 ~65535. VARCHAR(20)인 컬럼에 10글자 저장, 10글자 만큼의 공간을 차지.
|`TINYTEXT[(M)]`|최대 255byte|
|`TEXT[(M)]`|최대 65535byte
|`MEDIUMTEXT[(M)]`|최대 16777215byte
|`LONGTEXT[(M)]`|최대 4294967295byte|
|`ENUM('value1','value2',...)`|열거형. 정해진 몇 가지의 값들 중 하나만 저장. 최대 65535개의 개별 값을 가질 수 있고, 내부적으로 정수 값으로 표현된다.|
|`SET('value1','value2',...)`|집합형. 정해진 몇 가지의 값 들 중 여러 개를 저장. 최대 64개의 요소로 구성 될 수 있고, 내부적으로는 정수 값 이다.

🔷 **날짜 자료형 (Date and Time Data Types)**

|데이터유형|크기(Byte)|정의|
|:---:|:---:|:---:|
|`DATE`|3|YYYY-MM-DD (‘1000-01-01'~'9999-12-31')|
|`TIME`|3|HH:MM:SS('-838:59:59' ~ '838:59:59')
|`DATETIME`|8|YYYY-MM-DD HH:MM:SS(‘1000-01-01 00:00:00' ~ '9999-12-31 23:59:59')
|`TIMESTAMP[(M)]`|4|1970-01-01 00:00:01 ~ 2038-01-19 03:14:07 (1970-01-01 00:00:00 를 0으로 해서 1초단위로 표기)
`YEAR[(4)]`|1| 1901 ~2155

***

## 📌 테이블(Table) 생성하기

🔷 **자주 사용하는 Options**

|옵션|설명|
|:---:|:---:|
|`NOT NULL`|해당 열의 값은 항상 존재 해야하고, Null이 될 수 없음.|
|`DEFAULT`|기본 값 설정|
|`AUTO INCREMENT`|새 레코드가 추가 시 값을 자동으로 1 증가 시켜 저장|
|`PRIMARY KEY`|테이블에서 행을 고유하게 식별하기 위해 사용.|

🔷 **제약 조건 (CONSTRAINT)**

- 컬럼에 저장될 데이터의 조건을 설정
- 제약조건에 위배되는 데이터는 저장 불가
- 테이블 생성시 컬럼에 지정하거나, constraint로 지정가능(ALTER 를 이용하여 설정가능)

|제약사항|설명|
|:---:|:---:|
|`NOT NULL`|각 행은 해당열의 값을 포함해야 하며 null값은 허용 되지 않음|
|`UNIQUE`|컬럼에 중복된 값을 저장 할 수 없음 , NULL 값은 허용|
|`PRIMARY KEY`|기본키, 컬럼에 중복된 값을 저장 할 수 없고 , NULL 값도 허용하지 않음. 주로 레코드를 구분하기 위한 유일한 값을 지정할 때 사용.|
|`FOREIGN KEY`|특정 테이블의 PK 컬럼에 저장되어 있는 값만 저장. ‘참조키’, ‘외래키’ 라고 불림.|
|`NULL`|값 허용, 어떤 컬럼에 어떤 데이터를 참조하는지 반드시 지정|
|`DEFAULT`|레코드 입력 시, 해당 열의 값이 입력되지 않으면 넣어줄 값을 지정|
|`CHECK`|값의 범위나 종류를 지정. MYSQL 8 버전부터 사용가능. 이전 버전의 경우, 제약조건 설정은 가능하나 적용이 안됨|

🔷 **테이블(Table) 스키마**
- `스키마(Schema)` : 테이블에 저장될 데이터의 구조와 형식
- 테이블(Table) 스키마 확인하기
    - `DESCRIBE` 또는 `DESC` 명령어를 이용하여 생성된 테이블 스키마 확인

ex) _유저의 정보를 저장하기 위한 테이블_

```sql
CREATE TABLE user (
    user_num INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    user_id VARCHAR(20) NOT NULL,
    user_name VARCHAR(20) NOT NULL,
    user_password VARCHAR(20) NOT NULL,
    user_email VARCHAR(30),
    signup_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- 테이블 정보 확인
DESC user;
```

![](https://velog.velcdn.com/images/bzeromo/post/93762af1-e6db-4420-9a3b-63ec215df158/image.png)


***

## 📌 DML

> 👍 실습에 사용할 DB는 이렇게 생성하였다.

```sql
-- 데이터 베이스 생성 및 사용
CREATE DATABASE IF NOT EXISTS bzerodb;
USE bzerodb;

-- 테이블 생성
CREATE TABLE bzerodb_user (
    user_num INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    user_id VARCHAR(20) NOT NULL,
    user_name VARCHAR(20) NOT NULL,
    user_password VARCHAR(20) NOT NULL,
    user_email VARCHAR(30),
    signup_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- 테이블 정보 확인
DESC bzerodb_user;

-- 전체 테이블 데이터 조회
SELECT * FROM bzerodb_user;
```

![](https://velog.velcdn.com/images/bzeromo/post/08a4d0ba-635c-4533-be09-f036b2649a6e/image.png)


🔷 **INSERT 문**
- 생성시 작성한 모든 컬럼에 입력 값이 주어지면 컬럼이름 생략가능
- 컬럼이름과 입력 값의 순서가 일치하도록 작성
  (`NULL`, `DEFAULT`, `AUTO INCREMENT` 설정 필드 생략가능)

```sql
-- 모든 컬럼 입력
INSERT INTO bzerodb_user
VALUES (1, "bzeromo", "박영규", "12345", "dudrb5260@naver.com", now()); -- now() -> 현재 시각

-- 원하는 컬럼만 입력
INSERT INTO bzerodb_user (user_id, user_name, user_password)
VALUES("kimzeromo", "김영규", "1q2w3e4r!@");

-- 여러행 입력
INSERT INTO bzerodb_user (user_id, user_name, user_password)
VALUES  ("leezeromo", "이영규", "0000"),
		("jozeromo", "조영규", "1111"),
        ("5zeromo", "오영규", "2222");
        
SELECT * FROM bzerodb_user;
```

![](https://velog.velcdn.com/images/bzeromo/post/53c829fb-476b-43e3-8c3b-dba0a4458f99/image.png)

🔷 **UPDATE 문**
-  기존 레코드를 수정한다.
-  WHERE 절을 이용해 하나의 레코드 또는 다수의 레코드를 한 번에 수정할 수 있다.

> ❗ WHERE 절을 생략하면 테이블의 모든 행이 수정된다.

```sql
-- 데이터 수정 조건x(safe mode 해제) Edit -> preferences -> SQLEditor -> 맨 아래 체크박스 해제
UPDATE bzerodb_user
SET user_name = 'anonymous';

-- user_num가 3번인 유저의 비밀번호를 1234로 수정
UPDATE bzerodb_user
SET user_password = '1234'
WHERE user_num = 3;

SELECT * FROM bzerodb_user;
```

![](https://velog.velcdn.com/images/bzeromo/post/027af82a-5f40-4645-9d8c-7be1c1343042/image.png)

🔷 **DELETE 문**
- 기존 레코드를 삭제한다.
- WHERE 절을 이용해 하나의 레코드 또는 다수의 레코드를 한 번에 삭제 할 수 있다

```sql
-- 삭제
-- user_num가 4인 유저 삭제
DELETE FROM bzerodb_user
WHERE user_num = 4;


SELECT * FROM bzerodb_user;
```

![](https://velog.velcdn.com/images/bzeromo/post/b782e6a0-a2e1-4694-92bf-9a0ccb6177d0/image.png)

> 💡 삭제 후에 새로 유저 행을 추가하여도 `Auto-Increment`로 설정된 `user_num`은 삭제되었던 번호로 내려가는 것이 아니라 그대로 올라가기만 한다.
![](https://velog.velcdn.com/images/bzeromo/post/d3541749-fad5-474b-9d0b-5cb99533607a/image.png)

***

## 📌 SELECT 문

> 👍 `SELECT`는 가장 중요한만큼 SSAFY에서 제공한 더 복잡한 데이터베이스를 사용한다. DB에 대한 자세한 내용은 이곳에 공개할 수 없다는 점을 양해바란다.

🔷 **SELECT 문**
- 테이블에서 레코드를 조회하기 위해 사용
- 조회 시 컬럼이름이나 표현식을 조회할 수 있고 `별칭(AS, alias)` 사용이 가능하다
- `*` 는 모든 속성을 조회한다.
- `WHERE` 조건식을 이용하여 원하는 레코드를 조회할 수 있다.


```sql
-- 모든 사원 정보 검색
SELECT * FROM emp;
```
![](https://velog.velcdn.com/images/bzeromo/post/6d296b26-585c-4894-941a-bb159e8b243c/image.png)


```sql
-- 사원이 근무하는 부서번호
SELECT deptno
FROM emp;
```

![](https://velog.velcdn.com/images/bzeromo/post/f62bed9e-6a09-4cbc-88b3-493fa274e99c/image.png)


```sql
-- 사원이 근무하는 부서번호 (중복제거)
-- AS -> Alias(별칭)
SELECT DISTINCT deptno AS "부서번호"
FROM emp;
```

![](https://velog.velcdn.com/images/bzeromo/post/b0aee8b9-1eee-4d73-afe8-eb6b32b8bf33/image.png)


```sql
-- 별명 및 사칙연산
-- as 를 이용하여 조회 시 컬럼이름을 변경할 수 있다. (띄어 쓰기 포함 시 “” 으로 묶어준다)
-- 사원의 이름, 사번, 급여*12 (연봉), 업무 조회
SELECT ename 이름, empno "사번", sal*12 AS 연봉, job AS "업 무"  
FROM emp;
```

![](https://velog.velcdn.com/images/bzeromo/post/f2131bae-49ad-4366-be3b-3c4273c407a7/image.png)


```sql
-- 사원의 이름, 사번, 커미션, 급여, 커미션 포함 급여 조회
-- 사칙연산에 null 포함 시 계산이 불가능, IFNULL을 이용해 null일 때 0으로 처리
SELECT ename 이름, empno AS "사번", comm 커미션, 
	   sal AS 급여, sal + comm AS "커미션 포함급여", 
       sal + IFNULL(comm, 0) AS "커미션 포함급여2"
FROM emp;
```

![](https://velog.velcdn.com/images/bzeromo/post/ffb13797-0d1e-4aa9-9fb0-38a2e511d025/image.png)


```sql
-- CASE FUNCTION
-- CASE 문은 조건을 통과하고 첫 번째 조건이 충족될 때 값을 반환한다.
-- 조건이 충족되지 않으면 ELSE 절의 값을 반환한다.
SELECT empno, ename, sal,
	CASE WHEN sal >= 5000 THEN "고액연봉"
		 WHEN sal >= 2000 THEN "평균연봉"
		 ELSE "저액연봉"
	END AS "연봉등급"
FROM emp;
```

![](https://velog.velcdn.com/images/bzeromo/post/9792b4bf-d5ca-4826-81ec-d48db114e745/image.png)


```sql
-- WHERE 절은 조건에 맞는 레코드를 조회하기 위해서 사용한다.
-- 부서 번호가 30인 사원중 급여가 1500 이상인 사원의 이름, 급여, 부서번호 조회
SELECT ename, sal, deptno
FROM emp
WHERE deptno = 30 AND sal >= 1500;
```
![](https://velog.velcdn.com/images/bzeromo/post/de7fcc39-9e10-4218-8994-b8da0b867e33/image.png)

> ❗ WHERE 절은 SELECT 문장 뿐아니라, UPDATE, DELETE 등 다른 문장에서도 사용됨을 기억하자!


```sql
-- 부서번호가 20 또는 30인 부서에서 근무하는 사원의 사번, 이름, 부서번호 조회
SELECT empno, ename, deptno
  FROM emp
 WHERE deptno = 30
    OR deptno = 20;
```

![](https://velog.velcdn.com/images/bzeromo/post/04e75177-ebed-4f9f-a74d-514d9637bfc7/image.png)


```sql
-- != 와 < > 모두 not equal을 의미한다. 
-- 부서번호가 20,30이 아닌 부서에서 근무하는 사원의 사번, 이름, 부서번호 조회
SELECT empno, ename, deptno
FROM emp
WHERE deptno != 30 AND deptno <> 20;
```

![](https://velog.velcdn.com/images/bzeromo/post/2533a447-2c13-499e-b701-73bdb683c774/image.png)



```sql
-- NOT - 조건문이 NOT TRUE일 때 레코드를 조회
SELECT empno, ename, deptno
FROM emp
WHERE NOT (deptno = 30 OR deptno = 20);
```

![](https://velog.velcdn.com/images/bzeromo/post/ccd51c0e-8c83-41d2-8c22-356c9c990413/image.png)


```sql
-- IN - 피연산자가 여러 표현 중 하나라도 같다면 TRUE
-- 업무가 MANAGER, ANALYST, PRESIDENT 인 사원의 이름, 사번, 업무조회
SELECT ename, empno, job
FROM emp
WHERE job IN ('MANAGER', 'ANALYST', 'PRESIDENT');
```

![](https://velog.velcdn.com/images/bzeromo/post/df5cfd6a-2675-4ecf-953b-5686e3e4cb3c/image.png)


```sql
-- 부서번호가 10, 20이 아닌 사원의 사번, 이름, 부서번호 조회
SELECT empno, ename, deptno
FROM emp
WHERE deptno NOT IN (10, 20);
```

![](https://velog.velcdn.com/images/bzeromo/post/5e3b1278-f84f-4eff-88d2-a26f429b2af9/image.png)


```sql
-- BETWEEN – 값이 주어진 범위의 범위 안에 있으면 조회
-- 값은 숫자나, 문자, 날짜가 될 수 있다.
-- 급여가 2000이상 3000이하 인 사원의 사번, 이름, 급여조회
SELECT empno, ename, sal
FROM emp
WHERE sal BETWEEN 2000 AND 3000; 
-- WHERE sal >= 2000 AND sal <=3000;
```

![](https://velog.velcdn.com/images/bzeromo/post/9b533aa4-d942-44cf-9a8f-ce4ecf407a56/image.png)


```sql
-- 입사일이 1981년인 직원의 사번, 이름, 입사일 조회
SELECT empno, ename, hiredate
FROM emp
WHERE hiredate BETWEEN '1981-01-01' AND '1981-12-31';
```

![](https://velog.velcdn.com/images/bzeromo/post/bef8ef6a-388f-4b26-9dea-47936e0ac585/image.png)


```sql
-- 커미션인 NULL 인 사원의 사번, 이름, 커미션 조회
SELECT empno, ename, comm
FROM emp
-- WHERE comm = NULL; -> NULL은 이런 식으로 가져올 수 없다.
WHERE comm IS NULL;
```

![](https://velog.velcdn.com/images/bzeromo/post/a8f7e247-c3a7-4ade-b34f-7999abe05852/image.png)


```sql
-- 커미션 NULL이 아닌 사원의 사번, 이름, 업무, 커미션 조회
SELECT empno, ename, comm
FROM emp
WHERE comm IS NOT NULL;
```

![](https://velog.velcdn.com/images/bzeromo/post/d90b0240-091d-43aa-8f97-8ad30f026a22/image.png)



```sql
-- LIKE – WHERE 절에서 칼럼의 값이 특정 패턴을 가지는지 검사하기 위해 사용
-- 와일드 카드(%,_)를 이용해 패턴을 표현한다.
-- 이름이 M으로 시작하는 사원의 사번, 이름 조회
SELECT empno, ename
FROM emp
WHERE ename LIKE 'M%';
```

![](https://velog.velcdn.com/images/bzeromo/post/fbd31942-d029-4f4e-98aa-ea8ee9bbd535/image.png)

> 💡 **와일드 카드**
`%` : 0개 이상의 문자를 의미
`_` : 문자 하나를 의미


```sql
-- 이름에 E가 포함된 사원의 사번 이름 조회
SELECT empno, ename
FROM emp
WHERE ename LIKE '%E%';
```

![](https://velog.velcdn.com/images/bzeromo/post/293abf9f-d8d3-44ba-94e4-1d25f07b0e40/image.png)


```sql
-- 이름의 세번째 알파벳이 'A'인 사원의 사번, 이름 조회
SELECT empno, ename
FROM emp
WHERE ename LIKE '__A%';
```

![](https://velog.velcdn.com/images/bzeromo/post/8340b34a-a7a0-4607-9cc2-c03628b83b40/image.png)


```sql
-- ORDER BY: 조회 결과를 오름차순(ASC) 또는 내림차순(DESC)으로 정렬할 때 사용한다. (default : ASC)
-- 정렬 기준(칼럼)을 지정할 수 있다.
-- 모든 직원의 모든 정보를 이름을 기준으로 내림차순 정룔
SELECT *
FROM emp
ORDER BY ename DESC;
```

![](https://velog.velcdn.com/images/bzeromo/post/1a95c1c6-1547-41e3-954b-15105c01e622/image.png)


```sql
-- 모든 사원의 사번 이름, 급여를 조회 (급여 내림차순)
SELECT empno, ename, sal
FROM emp
ORDER BY sal DESC;
```

![](https://velog.velcdn.com/images/bzeromo/post/8fa66195-a34d-4320-ae27-fdf9063a77c6/image.png)


```sql
-- 20, 30번 부서에 근무하는 사원의 사번, 이름, 부서번호, 급여 조회 (부서별 오름차순, 급여순 내림차순)
SELECT empno, ename, deptno, sal
FROM emp
WHERE deptno IN (20, 30)
ORDER BY deptno, sal DESC;
```

![](https://velog.velcdn.com/images/bzeromo/post/1dd1251b-922b-440d-81fe-453b139cfd3a/image.png)

> 💡 **SQL Query문의 실행 순서**
`FROM` and `JOIN` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `SELECT` -> `ORDER BY` -> `LIMIT`

***

## 📌 MySQL 내장 함수

🔷 **MySQL은 문자열, 숫자, 날짜, 그 외에 대해서 다양한 함수를 제공한다. 그 중에서 당장 필요한 것만 살펴보겠다.**

🔷 **숫자 관련 함수(Numeric Functions)**

|함수|설명|
|:---:|:---:|
|`ABS(숫자)`|절대값.
`CEIL(숫자)`, `CEILING(숫자)`|값보다 크거나 같은 정수 중 가장 작은 수 반환.
`FLOOR(숫자)`|값 보다 작거나 같은 정수 중 가장 큰 수 반환.
`ROUND(숫자, 자릿수)` |숫자를 자릿수를 기준으로 반올림하여 반환.
`TRUNCATE(숫자, 자릿수)` |숫자를 자릿수를 기준으로 버림한 수를 반환
`POW(X, Y)`|X의 Y제곱 수를 반환
`MOD(X, Y)` |X를 Y로 나눈 나머지를 반환.
`GREATEST(숫자1, 숫자2, 숫자3, …)`| 주어진 수들 중에서 가장 큰 수를 반환.
`LEAST(숫자1, 숫자2, 숫자3, …)`| 주어진 수들 중에서 가장 작은 수를 반환.

```sql
-- 2의 3제곱
SELECT POW(2, 3) AS "2^3"
FROM dual;
```

![](https://velog.velcdn.com/images/bzeromo/post/f1720af4-cd8b-4f85-9d9d-3604f910f532/image.png)

```sql
-- 8 나누기 3의 나머지
SELECT MOD(8, 3) AS "8을 3으로 나눈 나머지";
```

![](https://velog.velcdn.com/images/bzeromo/post/18b5bc50-9d42-40a8-b614-165a244d82b0/image.png)


```sql
-- 최대값, 최솟값
SELECT greatest(8,17,86,17,100,77,999,2,13,31,97), least(8,17,86,17,100,77,999,2,13,31,97);
```

![](https://velog.velcdn.com/images/bzeromo/post/7f680e10-43a5-46d6-bc68-5eff223130c0/image.png)


```sql
-- 반올림
SELECT round(1526.159), round(1526.159, 0), round(1526.159, 1), round(1526.159,2), round(1526.159, 3);
```

![](https://velog.velcdn.com/images/bzeromo/post/a81c85a3-651c-4b5f-94ce-5b4b8a6588b5/image.png)

🔷 **문자 관련 함수(String Functions)**

함수|설명
|:---:|:---:|
`ASCII` | 문자의 아스키 코드 값 반환
`CONCAT` |두 개 이상의 문자열들을 결합하여 반환
`FORMAT` |숫자를 “#,###,###,###.##” 와 같은 형식으로 지정하고 반환
`INSERT` |문자열 내의 지정된 위치에서 특정 수의 문자에 대해 문자열을 삽입
`INSTR` |문자열에서 지정한 문자열이 처음 나타나는 위치를 반환
`LOWER` |문자열을 소문자로 변환
`CHAR_LENGTH` |문자열의 길이를 반환(문자단위)
`LENGTH` |문자열의 길이를 반환(바이트 단위)
`TRIM` |문자열에서 선행 및 후행 공백 제거
`UPPER` |문자열을 대문자로 변환
`REPLACE` |문자열 내의 모든 부분 문자열을 새 부분 문자열로변환
`STRCMP` |두 문자열 비교
`SUBSTR`| 문자열에서 부분 문자열 추출
`LPAD` |특정 길이로 문자열을 다른 문자열의 왼쪽에 채움
`RPAD` |특정 길이로 문자열을 다른 문자열의 오른쪽에 채움
`REPEAT` |지정된 횟수만큼 문자열을 반복
`REVERSE` |문자열을 뒤집고 결과를 반환

```sql
-- 아스키 코드값 얻기
SELECT ascii('0'), ascii('A'), ascii('a'); 
```

![](https://velog.velcdn.com/images/bzeromo/post/5c9cc7f9-116a-4eed-9d5b-37ea962d4af9/image.png)


```sql
-- concat
SELECT CONCAT('PRESIDENT의 이름은 ', ename, ' 입니다.') AS 소개
FROM emp
WHERE job = 'PRESIDENT'; 
```

![](https://velog.velcdn.com/images/bzeromo/post/b118e52d-205c-480c-95f6-8c80d0916c48/image.png)


```sql
-- 이름의 길이가 5인 직원의 이름을 조회
SELECT ename
FROM emp
WHERE length(ename) = 5;
```

![](https://velog.velcdn.com/images/bzeromo/post/742b9682-79f0-4944-a286-5c96aa08f3a1/image.png)


```sql
-- 박영규 (length: 데이터의 길이, char_length: 문자열의 길이)
SELECT length('박영규'), char_length('박영규');
```

![](https://velog.velcdn.com/images/bzeromo/post/c5c4dfbf-dfec-4b51-892d-06d02e029c11/image.png)


```sql
-- 문자열 변경
SELECT replace('Hello abc abc', 'abc', 'bzeromo');
```

![](https://velog.velcdn.com/images/bzeromo/post/20b7ba8e-7f25-4b40-bc1e-e250c0eb1d34/image.png)


```sql
-- 모든 직원의 이름 3자리조회
SELECT substr(ename, 1, 3)
FROM emp;
```

![](https://velog.velcdn.com/images/bzeromo/post/fa98733e-32bb-4198-bbf6-75077d4e7fa1/image.png)


```sql
-- LPAD RPAD
-- 두 번째 넣은 수에 처음 넣은 문자열의 길이만큼 오른쪽 혹은 왼쪽에 세번째로 지정한 문자열을 채운다.
SELECT LPAD('BZEROMO',10,'*'), RPAD('BZEROMO',10,'*');
```

![](https://velog.velcdn.com/images/bzeromo/post/1edb0f23-2bb7-4ebf-8d62-ef05553a17c5/image.png)


```sql
-- REVERSE
SELECT REVERSE('Hi Bzeromo!');
```

![](https://velog.velcdn.com/images/bzeromo/post/eca8603c-3e2b-4892-bb68-542fd84b92e3/image.png)


🔷 **날짜 관련 함수(Date Functions)**

함수| 설명
|:---:|:---:|
`DATE`| datetime 표현식에서 날짜 부분을 추출하여 날짜 반환
`ADDTIME` |시간/날짜/시간에 시간 간격을 추가한 다음 시간/날짜/시간을 반환
`DATEDIFF` |두 날짜 값 사이의 일 수를 반환
`DAY`| 주어진 날짜의 해당 달의 날짜를 반환
`NOW`, `SYSDATE`, `CURRENT_TIMESTAMP`|현재 날짜와 시간을 반환
`YEAR` |주어진 날짜의 연도 부분을 반환
`YEARWEEK` |주어진 날짜의 연도 및 주 번호를 반환
`DAYNAME` |주어진 날짜의 요일 이름을 반환
`MONTH` |주어진 날짜의 월 부분을 반환

```sql
-- 2초 더하기
SELECT addtime("2022-02-13 17:29:21", "2");
```

![](https://velog.velcdn.com/images/bzeromo/post/b3f05226-88cf-4148-bb1e-1e491a9d8b00/image.png)


```sql
-- 날짜차이
SELECT datediff("2008-02-18", "2006-02-21");
```

![](https://velog.velcdn.com/images/bzeromo/post/8009e10e-008f-4cd6-afce-2b5386d4b7a5/image.png)


```sql
-- 오늘은?
SELECT now(), day(now()), month(now()), year(now()), yearweek(now());
```

![](https://velog.velcdn.com/images/bzeromo/post/ef6c6679-a2c0-4ea3-95f3-b4c289d40969/image.png)


🔷 **그 외 기타 중요 함수(Advanced Functions)**

함수| 설명
|:---:|:---:|
`BIN` |숫자의 이진 표현을 반환
`BINARY` |값을 이진 문자열로 변환
`CAST` |값(모든 유형)을 지정된 데이터 유형으로 변환
`CONVERT` |값을 지정된 데이터 유형 또는 문자 집합으로 변환
`IF` |조건이 TRUE이면 값을 반환하고 조건이 FALSE이면 다른 값을 반환
`NULLIF` |두 표현식을 비교하고 같으면 NULL을 반환. 그렇지 않으면 첫 번째 표현식이 반환
`IFNULL` |표현식이 NULL이면 지정된 값을 반환하고, 그렇지 않으면 표현식을 반환
`LAST_INSERT_ID` |테이블에 삽입되거나 업데이트된 마지막 행의 AUTO_INCREMENT ID를 반환


🔷 **집계 함수(Aggregate Function)**

- 여러 값의 집합(복수 행)에 대해서 동작한다.(복수 행 함수, 통계 함수, 그룹함수)
- `GROUP BY` 절과 함께 사용해 전체 집합의 하위 집합을 그룹화 한다

함수| 설명
|:---:|:---:|
`AVG`| 인수의 평균 값을 반환
`COUNT`| 조회된 행의 수를 반환
`MAX`| 최대값 반환
`MIN`| 최소값 반환
`SUM`| 합 반환
`STD`| 표준편차 반환

```sql
-- 모든 사원에 대하여 사원수, 급여총액, 평균급여, 최고급여, 최저급여 조회
SELECT COUNT(*) 사원수, SUM(sal) 급여총액 , AVG(sal) 평균급여, MAX(sal) AS "최고급여", MIN(sal) AS "최저급여"
FROM emp;
```

![](https://velog.velcdn.com/images/bzeromo/post/42808d3a-b528-431b-b7c4-56fcc22941c5/image.png)



```sql
-- 모든 사원에 대하여 부서, 사원수, 급여총액, 평균급여, 최고급여, 최저급여를 부서별로 조회하고, 소수점 둘쨰자리 반올림
SELECT deptno 부서, COUNT(*) 사원수, SUM(sal) 급여총액, ROUND(AVG(sal), 2) 평균급여, MAX(sal) 최고급여, MIN(sal) 최저급여
FROM emp
GROUP BY deptno;
```

![](https://velog.velcdn.com/images/bzeromo/post/23b237a5-5ded-4eab-98ea-cec1d851ab38/image.png)


```sql
-- 모든 사원에 대하여 부서, 업무, 사원수, 급여총액, 평균급여, 최고급여, 최저급여를 부서별, 직급별로 조회
SELECT deptno 부서, job 업무, COUNT(*) 사원수, SUM(sal) 급여총액, ROUND(AVG(sal), 2) 평균급여, MAX(sal) 최고급여, MIN(sal) 최저급여
FROM emp
GROUP BY deptno, job
ORDER BY deptno;
```

![](https://velog.velcdn.com/images/bzeromo/post/dbcfe56c-1f24-4c52-b953-c04549ff04a3/image.png)


```sql
-- 모든 사원에 대하여 이름, 부서, 업무, 사원수, 급여총액, 평균급여, 최고급여, 최저급여를 부서별, 직급별로 조회
SELECT ename 이름, deptno 부서,job 업무, COUNT(*) 사원수, SUM(sal) 급여총액, 
       ROUND(AVG(sal),2) 평균급여, MAX(sal) 최고급여, MIN(sal) 최저급여
FROM emp
GROUP BY deptno,job;
```

![](https://velog.velcdn.com/images/bzeromo/post/e053fde7-1c8f-44da-bd06-74d06cfdacab/image.png)


```sql
-- 급여(커미션포함) 평균이 2000이상인 부서번호, 부서별 사원수, 평균급여(커미션포함) 조회 
SELECT deptno, COUNT(*) 사원수, ROUND(AVG(sal+IFNULL(comm, 0)),2) AS "평균급여(커미션포함)"
FROM emp
GROUP BY deptno
HAVING AVG(sal + IFNULL(comm, 0)) >= 2000;
```

![](https://velog.velcdn.com/images/bzeromo/post/7448cf6a-cd46-4e83-b74d-d411948c37b3/image.png)


***

## 📌 트랜잭션 (Transaction)

🔷 **커밋(Commit) 하거나 롤백(Rollback) 할 수 있는 가장 작은 작업 단위**

- `커밋(Commit)` : 트랜잭션을 종료하여 변경사항에 대해서 영구적으로 저장하는 SQL

- `롤백(Rollback)` : 트랜잭션에 의해 수행된 모든 변경사항을 실행 취소하는 SQL

이름|설명
|:---:|:---:|
`START TRANSACTION` |트랜잭션을 시작함. `COMMIT`, `ROLLBACK`이 나오기 전까지 모든 SQL 을 의미
`COMMIT`|트랜잭션에서 변경한 사항을 영구적으로 DB에 반영
`ROLLBACK` |`START TRANSACTION` 실행 전 상태로 되돌림.

> 💡 MySQL에서는 기본적으로 세션이 시작하면 autocommit 설정 상태이다. 그러므로 MySQL은 각 SQL 문장이 오류를 반환하지
않으면 commit 을 수행한다.

```sql
-- 오토커밋 안하겠다고 설정
set autocommit = 0;

-- 트랜잭션을 위한 새 테이블 생성
USE bzerodb;
CREATE TABLE test_table(val VARCHAR(20));

-- 롤백
START TRANSACTION;
INSERT INTO test_table VALUES ('A');
INSERT INTO test_table VALUES ('B');
INSERT INTO test_table VALUES ('C');
INSERT INTO test_table VALUES ('D');
ROLLBACK;
SELECT * FROM test_table;
```

![](https://velog.velcdn.com/images/bzeromo/post/a3fdd13c-bf1b-4167-ae58-ad8cb2cd7b9f/image.png)

```sql
-- 커밋
START TRANSACTION;
INSERT INTO test_table VALUES ('B');
INSERT INTO test_table VALUES ('z');
INSERT INTO test_table VALUES ('e');
INSERT INTO test_table VALUES ('r');
INSERT INTO test_table VALUES ('o');
COMMIT;
SELECT * FROM test_table;
```

![](https://velog.velcdn.com/images/bzeromo/post/200b87f0-8f86-4516-b619-fea5c5c31888/image.png)

***

## 📌 JOIN

🔷 **둘 이상의 테이블에서 데이터를 조회하기 위해서 사용**

🔷 **일반적으로 조인 조건은 `PK(Primary Key)` 및 `FK(Foreign Key)`로 구성된다.**
- PK 및 FK 관계가 없더라도 논리적인 연관만으로도 `JOIN`이 가능하다.

🔷 **JOIN의 종류**

- `INNER JOIN`: 조인 조건에 해당하는 칼럼 값이 양쪽 테이블에 모두 존재하는 경우에만 조회, `동등 조인(Equi-join)`이라고도 한다. N개의 테이블 조인 시 N-1개의 조인 조건이 필요

- `OUTER JOIN` : 조인 조건에 해당하는 칼럼 값이 한 쪽 테이블에만 존재하더라도 조회 기준 테이블에 따라 `LEFT OUTER JOIN`, `RIGHT OUTER JOIN`으로 구분

- `SELF JOIN`: 같은 테이블 2개를 조인

- `Non-Equi JOIN`: 조인 조건이 table의 PK, FK 등으로 정확히 일치하는 것이 아닐 때 사용

> 💡 두 테이블을 조인할 때 어떤 테이블을 먼저 읽느냐에 따라 작업량이 달라질 수 있다! 물론 요즘 DB는 알아서 최적화를 해주기 때문에 크게 신경 쓸 필요 없다.

🔷 **카타시안 곱(Cartesian Product)**
- 두 개 이상의 테이블에서 데이터를 조회할 때
    - 조인 조건을 지정하지 않음
    - 조인 조건이 부적합 함
- 첫 번째 테이블의 모든 행이 두 번째 테이블의 모든 행에 조인되어 처리됨

```sql
-- 2번의 조회를 합치자
SELECT empno, ename, job
FROM emp;

SELECT deptno, dname
FROM dept;

-- 카타시안 곱 
-- WHERE 사용 없이는 단순히 두 테이블 행을 곱한만큼 나온다.
SELECT empno, ename, job, emp.deptno, dname
FROM emp, dept;

-- WHERE 사용하여 유의미하게 데이터를 뽑기
SELECT empno, ename, job, emp.deptno, dname
FROM emp, dept
WHERE emp.deptno = dept.deptno;
```

![](https://velog.velcdn.com/images/bzeromo/post/15a6e371-4683-4197-b603-290c651d48f3/image.png)

```sql
-- 사번 7788인 사원의 이름, 업무, 부서번호, 부서이름 조회
SELECT ename, job, deptno
FROM emp
WHERE empno = 7788;

SELECT dname
FROM dept
WHERE deptno = 20;

-- 두 번 조회할 필요 없이 조인!
-- 조인을 이용하여 작성 (INNER JOIN과 같다)
SELECT ename, job, emp.deptno, dname
FROM emp, dept
WHERE emp.deptno = dept.deptno
AND empno = 7788;
```

![](https://velog.velcdn.com/images/bzeromo/post/d5590d52-9632-48dc-aa28-b6a16eb8fc12/image.png)


```sql
-- INNER JOIN
-- 두 테이블에서 일치하는 값을 가진 레코드를 조회
-- table alias를 지정해 SQL 작성시 쉽게 사용할 수 있다.
SELECT e.ename, e.job, e.deptno, d.dname
FROM emp e
INNER JOIN dept d
ON e.deptno = d.deptno
WHERE e.empno = 7788;

-- 위랑 같다 (USING 키워드는 두 개의 테이블에 동일한 컬럼명이 존재할 때만 사용)
SELECT ename, job, deptno, dname
FROM emp
INNER JOIN dept
USING (deptno)
WHERE empno = 7788;
```

![](https://velog.velcdn.com/images/bzeromo/post/02c8ea02-3f3d-4585-858f-7a042ad0b287/image.png)


```sql
-- 새로운 인원이 들어왔다고 했을 때
INSERT INTO emp
VALUES (1236, "박영규", "MANAGER", 7139, "2023-10-11", 2000, NULL, NULL);

-- 동등조인으로 이름, 부서번호, 부서 이름을 가져오기
-- 새로 추가한 인물은 부서가 없어서 나오지 않는다!
SELECT e.ename, e.deptno, d.dname
FROM emp e, dept d
WHERE e.deptno = d.deptno;
```

![](https://velog.velcdn.com/images/bzeromo/post/88602b79-1b6d-4e9e-b69f-d1ad400aa3bc/image.png)


```sql
-- OUTER JOIN
-- 한쪽에 기준을 주고 붙이기
-- 없으면 없는 데로 
-- 모든 사원을 기준으로 (좌측 테이블) 
SELECT e.ename, e.deptno, d.dname
FROM emp e LEFT OUTER JOIN dept d
ON e.deptno = d.deptno;
```

![](https://velog.velcdn.com/images/bzeromo/post/0cddf3a0-80a5-4bd3-a444-a6db5a4e6f54/image.png)


```sql
-- 부서 테이블을 기준으로 (우측 테이블)
SELECT e.ename, e.deptno, d.dname
FROM emp e RIGHT OUTER JOIN dept d
ON e.deptno = d.deptno;
```

![](https://velog.velcdn.com/images/bzeromo/post/c6e5b8c6-7bc9-4c9e-8638-55ce6c729197/image.png)


```sql
-- 셀프 조인
-- 모든 사원번호, 이름, 매니저 번호, 매니저 이름 
SELECT e1.empno, e1.ename, e1.mgr, e2.ename
FROM emp e1, emp e2
WHERE e1.mgr = e2.empno
ORDER BY e2.ename;
```

![](https://velog.velcdn.com/images/bzeromo/post/b169b8b2-547d-489b-ab7a-99e4efeab37b/image.png)


```sql
-- INNER JOIN 이라는 키워드를 사용해보자
SELECT e1.empno AS "사번", e1.ename AS 사원이름, e2.empno AS "매니저 번호", e2.ename AS "매니저 이름"
FROM emp e1
INNER JOIN emp e2
ON e1.mgr = e2.empno;
```

![](https://velog.velcdn.com/images/bzeromo/post/b9b17d36-a492-406c-947a-c90357e202fd/image.png)


```sql
-- KING 이 안나오는 위의 쿼리문과 다르게 KING이 나온다
-- OUTER JOIN이니까!
SELECT e1.empno AS "사번", e1.ename AS 사원이름, e2.empno AS "매니저 번호", e2.ename AS "매니저 이름"
FROM emp e1 LEFT OUTER JOIN emp e2
ON e1.mgr = e2.empno;
```

![](https://velog.velcdn.com/images/bzeromo/post/48fadb86-1ade-4925-a85f-e926751d2d4a/image.png)


```sql
-- 비 동등 조인(Non-Equi JOIN)
-- 조인조건이 table의 PK, FK 등으로 정확히 일치하는 것이 아닐 때 사용
-- 모든 사원의 사번, 이름, 급여, 급여등급을 조회하고 싶다면?
SELECT e.empno, e.ename, e.sal AS "급여", sg.grade AS "급여등급"
FROM emp e, salgrade sg
WHERE e.sal BETWEEN sg.LOSAL AND sg.HISAL
-- WHERE e.sal >= sg.LOSAL AND e.sal <= sg.HISAL
ORDER BY sg.grade DESC, e.sal DESC;
```

![](https://velog.velcdn.com/images/bzeromo/post/da769143-f0a9-4bd8-9466-0acdd56230aa/image.png)



***

## 📌 서브 쿼리(Subquery)

🔷 **하나의 SQL 문안에 포함되어 있는 SQL문을 의미한다.**
- 서브 쿼리를 포함하는 SQL을 `외부 쿼리(outer query)` 또는 메인 쿼리라고 부르며, 서브 쿼리는 `내부 쿼리(inner query)`라고도 부른다.
- JOIN의 경우 경우에 따라 쿼리가 복잡해지거나 카타시안 곱으로 인해 속도가 느려질 수 있는데 이 때 JOIN 없이 조회하기 위해서 서브 쿼리가 필요하다.

🔷 **서브 쿼리의 종류**
- **중첩 서브 쿼리(Nested Subquery)** - WHERE 절에 작성하는 서브 쿼리 (단일-행, 다중-행, 다중-열)
- **인라인 뷰(Inline-view)** - FROM 절에 작성하는 서브 쿼리
- **스칼라 서브 쿼리(Scalar Subquery)** - SELECT 문에 작성하는 서브 쿼리

🔷 **서브 쿼리를 포함할 수 있는 SQL 문**
- `SELECT`, `FROM`, `WHERE`, `HAVING`, `ORDER BY`
- `INSERT`문의 `VALUES`
- `UPDATE`문의 `SET`

🔷 **서브 쿼리의 사용 시 주의사항**
- 서브 쿼리는 반드시 ()로 감싸서 사용한다.
- 서브 쿼리는 단일 행 또는 다중 행 비교 연산자와 함께 사용 가능하다.

> 💡 단일 행 비교연산자는 서브 쿼리 결과가 1건 이하이어야 하고, 복수 행 비교 연산자는 결과 건수와 상관없다.

### ⭐ 중첩 서브 쿼리

🔷 **단일 행**
- 서브 쿼리 결과가 단일 행을 반환

```sql
-- 1. 매니저의 이름이 KING인 사원의 사번, 이름, 부서번호, 업무 
SELECT empno, ename, deptno, job
FROM emp
WHERE mgr = (SELECT empno
			 FROM emp
			 WHERE ename = "KING");
```

![](https://velog.velcdn.com/images/bzeromo/post/60c618a7-de5c-43ba-8c6e-dc550a2d8fd0/image.png)

```sql
-- 2. 7566번 사원보다 급여를 많이 받는 사원의 이름, 급여를 조회
-- 7566번의 급여
SELECT sal
FROM emp
WHERE empno = 7566;

SELECT ename, sal
FROM emp 
WHERE sal > (SELECT sal
			 FROM emp
			 WHERE empno = 7566);
```

![](https://velog.velcdn.com/images/bzeromo/post/4928db41-7c83-48d0-833e-e5759f035eeb/image.png)

```sql
-- 3. 20번 부서의 평균 급여보다 급여가 많은 사원의 사번, 이름, 업무, 급여조회
SELECT AVG(sal)
FROM emp
WHERE deptno = 20;

SELECT empno, ename, job, sal
FROM emp
WHERE sal > (SELECT AVG(sal)
			 FROM emp
			 WHERE deptno = 20);
```

![](https://velog.velcdn.com/images/bzeromo/post/32909d92-4383-4c16-9db8-2fdbe3b32440/image.png)

```sql
-- 4. 업무가 TURNER와 같고/ 사번 7934인 직원보다 급여/가 많은 사원의 사번, 이름, 업무를 조회
SELECT job
FROM emp
WHERE ename = "TURNER";

SELECT sal
FROM emp
WHERE empno = 7934;

SELECT empno, ename, job, sal
FROM emp
WHERE job = (SELECT job
			 FROM emp
			 WHERE ename = "TURNER")
AND sal > (SELECT sal
		   FROM emp
		   WHERE empno = 7934);
```

![](https://velog.velcdn.com/images/bzeromo/post/f2a15134-0599-4709-a9be-31e9f04ac819/image.png)

🔷 **다중 행**
- 서브 쿼리 결과가 다중행을 반환 : `IN`, `ANY`, `ALL` 연산자와 함께 사용

```sql
-- 다중행 IN / ANY / ALL
-- 5. 업무가 SALESMAN 인 직원들 중 최소 한명 이상보다 많은 급여를 받는 사원의 이름, 급여, 업무를 조회하시오.
SELECT sal
FROM emp
WHERE job = 'SALESMAN';
-- > ANY (최소값보다는 큼) / < ANY (최대값보다는 작음)
SELECT ename, sal, job
FROM emp
WHERE sal > ANY (SELECT sal
				 FROM emp
				 WHERE job = 'SALESMAN');
```

![](https://velog.velcdn.com/images/bzeromo/post/d7c87baa-98bf-4671-a282-25034059276e/image.png)

```sql
-- 6. 업무가 'SALESMAN'인 모든 직원보다 급여(커미션포함)를 많이 받는 사원의 이름, 급여, 업무, 입사일, 부서번호를 조회하시오.
SELECT sal + IFNULL(comm, 0)
FROM emp
WHERE job = 'SALESMAN';
-- > ALL (최대값보다 큼) / < ALL (최소값보다 작음)
SELECT ename, sal, job, hiredate, deptno
FROM emp
WHERE sal > ALL (SELECT sal + IFNULL(comm, 0)
				 FROM emp
				 WHERE job = 'SALESMAN');
```

![](https://velog.velcdn.com/images/bzeromo/post/a895b37d-8463-4a42-9576-85a6099df110/image.png)

```sql
-- 7. 직원이 최소 한명이라도 근무하는 부서의 부서번호, 부서이름, 위치
SELECT DISTINCT deptno
FROM emp;

-- DISTINCT 키워드를 이용해 중첩되는 행은 제거
-- IN은 다중행에 하나라도 일치하면 조회, = ANY 와 같다
SELECT deptno, dname, loc
FROM dept
WHERE deptno IN (SELECT DISTINCT deptno
				 FROM emp);
```

![](https://velog.velcdn.com/images/bzeromo/post/78806735-ab75-4982-96dc-3f5242c7965a/image.png)

🔷 **다중 열**
- 서브 쿼리의 결과값이 두 개 이상의 칼럼을 반환하는 서브 쿼리
- PK가 복합키(Composite Key) 이거나, 여러 칼럼의 값을 한꺼번에 비교해야 할 경우 사용
- `행 생성자(row constructor)`를 이용하여 다중 열 서브 쿼리를 비교

```sql
-- 다중열
-- 8. 이름이 FORD인 사원과 매니저 및 부서가 같은 사원의 이름, 매니저번호, 부서번호를 조회 
SELECT mgr, deptno
FROM emp
WHERE ename = 'FORD';

SELECT ename, mgr, deptno
FROM emp
WHERE (mgr, deptno) = (SELECT mgr, deptno
					   FROM emp
                       WHERE ename = 'FORD')
AND ename <> 'FORD';
-- AND ename != 'FORD';
```

![](https://velog.velcdn.com/images/bzeromo/post/28080e13-395a-4317-a4ea-ec9ee0594a89/image.png)

```sql
-- 9. 각 부서별 / 입사일이 가장 빠른 / 사원의 사번, 이름, 부서번호, 입사일을 조회
SELECT IFNULL(deptno, '대기발령') , MIN(hiredate)
FROM emp
GROUP BY deptno;

-- 지난 포스팅에서도 언급했다시피 NULL은 연산이 불가능해 걸러줄 필요가 있다.
SELECT empno, ename, deptno, hiredate
FROM emp
WHERE (deptno, hiredate) IN (SELECT IFNULL(deptno, '대기발령') , MIN(hiredate)
							 FROM emp
							 GROUP BY deptno);
```

![](https://velog.velcdn.com/images/bzeromo/post/552620e2-62a9-409e-bd26-6cc3728c60e8/image.png)

### ⭐ 상호연관 서브 쿼리

🔷 **외부 쿼리에 있는 테이블에 대한 참조를 하는 서브 쿼리를 의미한다.**

```sql
-- 상호연관 서브쿼리
-- 외부 쿼리의 e를 서브 쿼리가 참조하고 있다.
-- 10. 소속 부서의 평균 급여보다 많은 급여를 받는 사원의 이름, 급여, 부서번호, 입사일, 업무를 조회
SELECT ename, sal, deptno, hiredate, job
FROM emp e
WHERE sal > (SELECT AVG(sal)
			 FROM emp
             WHERE deptno = e.deptno);
```

![](https://velog.velcdn.com/images/bzeromo/post/f7d4577d-1cc2-4f45-a38a-bf2530b72845/image.png)

- 테이블에서 행을 먼저 읽어서 각 행의 값을 관련된 데이터와 비교하는 방법 중 하나이다.
- 기본 질의에서 고려된 각 후보행에 대해 서브 쿼리가 다른 결과를 반환해야 하는 경우에 사용한다.
- **서브 쿼리에서는 메인 쿼리의 컬럼명을 사용할 수 있으나, 메인 쿼리에서는 서브 쿼리의 컬럼명을 사용할 수 없다.**

### ⭐ 인라인 뷰(Inline View)

🔷 **FROM절에서 사용되는 서브 쿼리**
- 동적으로 생성된 테이블로 사용 가능하다. 뷰(View) 와 같은 역할을 한다.

> 💡 인라인 뷰는 SQL 문이 실행될 때만 임시적으로 생성되는 뷰 이기 때문에 데이터베이스에 해당 정보가 저장되지 않는다. 그래서 `동적 뷰(Dynamic View)`라고도 한다.

```sql
-- 인라인 뷰(FROM 절에 서브쿼리)
-- 11. 모든 사원의 평균급여보다 적게 받는 사원들과 같은 부서에서 근무하는 사원의 사번, 이름, 급여, 부서번호를 조회

SELECT AVG(sal)
FROM emp;

SELECT DISTINCT deptno
FROM emp
WHERE sal < (SELECT AVG(sal) FROM emp);


SELECT e.ename, e.empno, e.sal, e.deptno
FROM emp e, (SELECT DISTINCT deptno FROM emp WHERE sal < (SELECT AVG(sal) FROM emp)) AS d
WHERE e.deptno = d.deptno;
```

![](https://velog.velcdn.com/images/bzeromo/post/a93fac0f-7824-4541-ab89-a077eaea728c/image.png)


```sql
-- 12. 모든 사원에 대하여 사원의 이름, 부서번호, 급여, 사원이 소속된 부서의 평균 급여를 조회 (단, 이름 오름차순)
SELECT deptno, AVG(sal) AS avgsal
FROM emp
GROUP BY deptno;

SELECT e.ename, e.deptno, e.sal, d.avgsal
FROM emp e, (SELECT deptno, AVG(sal) AS avgsal
			 FROM emp
			 GROUP BY deptno) d
WHERE e.deptno = d.deptno
ORDER BY e.ename;
```

![](https://velog.velcdn.com/images/bzeromo/post/0b0138b8-165a-4e78-8aef-0b853fbaa14c/image.png)

### ⭐ TOP-N
🔷 **`LIMIT`**
- 결과 집합에서 지정된 수의 행만 필요한 경우 `LIMIT` 절을 사용
- 하나 또는 2개 양의 정수를 인자로 받음
- 첫 번째 인자 : offset, 두 번째 인자 개수, 첫 번째 인자를 생략 할 경우 기본값은 0

```sql
SELECT empno, ename, sal
FROM emp
ORDER BY sal DESC limit 5, 5;
```

![](https://velog.velcdn.com/images/bzeromo/post/dd5bee65-06d1-408e-8ad5-3f0de3d818bd/image.png)

### ⭐ 스칼라 서브 쿼리 (Scalar Subquery)

🔷 **하나의 행에서 하나의 컬럼 값만 반환하는 서브 쿼리**

🔷 **다음과 같은 경우에 사용가능**

1. GROUP BY를 제외한 SELECT의 모든 절
2. INSERT 문의 VALUES
3. 조건 및 표현식 부분
4. UPDATE 문의 SET 또는 WHERE절에서 연산자 목록

```sql
-- 스칼라서브쿼리
-- SELECT 에 스칼라 서브 쿼리 사용
-- 13. 사원의 이름, 부서번호, 급여, 소속부서의 평균 급여를 조회
SELECT ename, deptno, sal, (SELECT AVG(sal) FROM emp WHERE deptno = e.deptno) AS avgsal
FROM emp e;
```

![](https://velog.velcdn.com/images/bzeromo/post/38bfdfb1-43c3-4ccb-8cba-46d7f48f9b8c/image.png)

```sql
-- SELECT 에 스칼라 서브 쿼리 사용
-- 14. 부서번호가 10인 부서의 총 급여, 20인 부서의 평균 급여, 30인 부서의 최고, 최저 급여
SELECT  (SELECT SUM(sal) FROM emp WHERE deptno = 10) AS SUM10,
		(SELECT AVG(sal) FROM emp WHERE deptno = 20) AS AVG20,
		(SELECT MAX(sal) FROM emp WHERE deptno = 30) AS MAX30,
		(SELECT MIN(sal) FROM emp WHERE deptno = 30) AS MIN30
FROM dual;
```

![](https://velog.velcdn.com/images/bzeromo/post/319b9ec8-b760-4b6d-8f2e-56e7a1e0d193/image.png)

```sql
-- ORDER BY 에 스칼라 서브 쿼리 사용
-- 15. 모든사원의 번호, 이름, 부서번호, 입사일을 조회 (단, 부서이름기준으로 내림차순)
UPDATE emp SET deptno = 40 WHERE empno = 4168;
SELECT empno, ename, deptno, hiredate
  FROM emp e
 ORDER BY (SELECT dname
             FROM dept
            WHERE deptno = e.deptno) DESC;
```

![](https://velog.velcdn.com/images/bzeromo/post/d8197d41-1130-4962-81b0-2121c291b1d1/image.png)

### ⭐ 그 외의 서브 쿼리 활용

```sql
-- 서브 쿼리를 이용한 CREATE 문
-- 테이블을 카피 
CREATE TABLE emp_copy
(SELECT * FROM emp);

-- emp table 구조만 emp_blank 라는 이름으로 복사하여 생성
-- WHERE 1 = 0 은 약속과도 같다.
CREATE TABLE emp_blank
(SELECT * FROM emp WHERE 1 = 0);

-- 서브 쿼리를 이용한 INSERT 문
-- 부서 번호가 30인 사원의 모든 정보를 emp_blank에 INSERT
INSERT INTO emp_blank
(SELECT * FROM emp WHERE deptno = 30);

SELECT * FROM emp_blank;
```

![](https://velog.velcdn.com/images/bzeromo/post/26e6565b-0430-458f-a8a3-5b808150c685/image.png)



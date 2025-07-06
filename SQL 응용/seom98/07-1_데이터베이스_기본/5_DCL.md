# 1. DCL(데이터 제어어)의 개념

**DCL(Data Control Language)**은 데이터베이스의 보안을 관리하고 사용자의 권한을 제어하는 언어입니다. 데이터베이스 관리자(DBA)가 주로 사용하여 데이터베이스의 보안과 무결성을 유지합니다.

### DCL의 목적

-   데이터베이스 보안 관리
-   사용자 권한 제어
-   데이터 무결성 보장
-   접근 제어 및 감사

## 1-1. DCL의 유형

### 권한 부여 (GRANT)

-   사용자나 역할에게 특정 권한을 부여
-   데이터베이스 객체에 대한 접근 권한 설정
-   시스템 권한과 객체 권한으로 구분

### 권한 회수 (REVOKE)

-   부여된 권한을 회수하거나 취소
-   보안 정책 변경 시 사용
-   계층적 권한 구조에서 상위 권한 회수 시 하위 권한도 함께 회수

### 트랜잭션 제어

-   COMMIT: 트랜잭션 확정
-   ROLLBACK: 트랜잭션 취소
-   SAVEPOINT: 트랜잭션 내 저장점 설정

## 1-2. GRANT

### 설명

사용자나 역할에게 데이터베이스 객체에 대한 권한을 부여하는 명령어입니다.

### 문법

```sql
-- 시스템 권한 부여
GRANT 시스템권한 TO 사용자명 [WITH ADMIN OPTION];

-- 객체 권한 부여
GRANT 객체권한 ON 객체명 TO 사용자명 [WITH GRANT OPTION];

-- 역할에 권한 부여
GRANT 역할명 TO 사용자명;
```

### 예시

```sql
-- 시스템 권한 부여
GRANT CREATE TABLE TO user1;
GRANT CREATE SESSION TO user2;
GRANT CREATE ANY TABLE TO user3 WITH ADMIN OPTION;

-- 객체 권한 부여
GRANT SELECT ON employees TO user1;
GRANT SELECT, INSERT, UPDATE ON departments TO user2;
GRANT ALL ON employees TO user3 WITH GRANT OPTION;

-- 역할에 권한 부여
GRANT CONNECT, RESOURCE TO user1;
GRANT DBA TO admin_user;
```

### 주요 시스템 권한

-   **CREATE SESSION**: 데이터베이스 접속 권한
-   **CREATE TABLE**: 테이블 생성 권한
-   **CREATE VIEW**: 뷰 생성 권한
-   **CREATE INDEX**: 인덱스 생성 권한
-   **CREATE PROCEDURE**: 프로시저 생성 권한
-   **CREATE USER**: 사용자 생성 권한
-   **DROP ANY TABLE**: 테이블 삭제 권한
-   **SELECT ANY TABLE**: 모든 테이블 조회 권한

### 주요 객체 권한

-   **SELECT**: 데이터 조회 권한
-   **INSERT**: 데이터 삽입 권한
-   **UPDATE**: 데이터 수정 권한
-   **DELETE**: 데이터 삭제 권한
-   **ALTER**: 객체 구조 변경 권한
-   **INDEX**: 인덱스 생성 권한
-   **REFERENCES**: 외래키 참조 권한
-   **ALL**: 모든 권한

### 옵션

-   **WITH ADMIN OPTION**: 시스템 권한을 다른 사용자에게 부여할 수 있는 권한
-   **WITH GRANT OPTION**: 객체 권한을 다른 사용자에게 부여할 수 있는 권한

## 1-3. REVOKE

### 설명

사용자나 역할에게 부여된 권한을 회수하는 명령어입니다.

### 문법

```sql
-- 시스템 권한 회수
REVOKE 시스템권한 FROM 사용자명;

-- 객체 권한 회수
REVOKE 객체권한 ON 객체명 FROM 사용자명;

-- 역할에서 권한 회수
REVOKE 역할명 FROM 사용자명;
```

### 예시

```sql
-- 시스템 권한 회수
REVOKE CREATE TABLE FROM user1;
REVOKE CREATE SESSION FROM user2;

-- 객체 권한 회수
REVOKE SELECT ON employees FROM user1;
REVOKE ALL ON departments FROM user2;

-- 역할에서 권한 회수
REVOKE CONNECT FROM user1;
REVOKE DBA FROM admin_user;
```

### 권한 회수 시 주의사항

#### 계층적 권한 회수

```sql
-- user1이 user2에게 부여한 권한도 함께 회수됨
GRANT SELECT ON employees TO user1 WITH GRANT OPTION;
GRANT SELECT ON employees TO user2; -- user1이 부여

REVOKE SELECT ON employees FROM user1; -- user2의 권한도 함께 회수됨
```

#### CASCADE 옵션

```sql
-- CASCADE를 사용하여 연쇄적으로 권한 회수
REVOKE SELECT ON employees FROM user1 CASCADE;
```

### 권한 관리 모범 사례

#### 1. 최소 권한 원칙

```sql
-- 필요한 최소한의 권한만 부여
GRANT SELECT ON employees TO user1;
GRANT SELECT, INSERT ON orders TO user2;
```

#### 2. 역할 기반 권한 관리

```sql
-- 역할 생성
CREATE ROLE manager_role;
CREATE ROLE employee_role;

-- 역할에 권한 부여
GRANT SELECT, INSERT, UPDATE ON employees TO manager_role;
GRANT SELECT ON employees TO employee_role;

-- 사용자에게 역할 부여
GRANT manager_role TO user1;
GRANT employee_role TO user2;
```

#### 3. 정기적인 권한 감사

```sql
-- 현재 권한 확인
SELECT * FROM USER_SYS_PRIVS; -- 시스템 권한
SELECT * FROM USER_TAB_PRIVS; -- 객체 권한
SELECT * FROM USER_ROLE_PRIVS; -- 역할 권한
```

### 데이터베이스별 DCL 차이점

#### Oracle

```sql
-- Oracle의 경우
GRANT SELECT ON schema.table TO user;
REVOKE SELECT ON schema.table FROM user;
```

#### MySQL

```sql
-- MySQL의 경우
GRANT SELECT ON database.table TO 'user'@'host';
REVOKE SELECT ON database.table FROM 'user'@'host';
```

#### SQL Server

```sql
-- SQL Server의 경우
GRANT SELECT ON schema.table TO user;
REVOKE SELECT ON schema.table FROM user;
```

### 보안 고려사항

1. **정기적인 권한 검토**: 불필요한 권한은 즉시 회수
2. **강력한 패스워드 정책**: 사용자 계정 보안 강화
3. **세션 관리**: 비활성 세션 자동 종료
4. **감사 로그**: 권한 변경 이력 추적
5. **백업 및 복구**: 권한 설정 백업 유지

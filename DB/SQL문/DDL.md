# DDL (Data Definiition Language)
* 데이터 정의 언어 
* 데이터를 저장할 테이블의 구조를 조작(생성, 수정, 삭제)하는 명령어
* 대표적으로 `CREATE(생성)`, `ALTER(수정)`, `DROP(삭제)` 

<br></br>

## CREATE 
* 테이블 구성, 속성(속성에 관한 제약), 기본키와 외래키를 정의함
```sql
CREATE TABLE 테이블명 (
    컬럼명1 데이터타입 [옵션],
    컬럼명2 데이터타입 [옵션],
    ...
    [PRIMARY KEY (컬럼명)],
    [FOREIGN KEY (컬럼명) REFERENCES 다른테이블(컬럼)],
    [UNIQUE (컬럼명)],
    [CHECK (조건)]
);
```
**🔩 옵션**

| 제약 조건        | 설명                                 |
|------------------|--------------------------------------|
| PRIMARY KEY      | 기본 키 (고유값 + NOT NULL)           |
| NOT NULL         | 값이 꼭 있어야 함                    |
| UNIQUE           | 중복 금지                            |
| AUTO_INCREMENT   | 자동 증가 숫자 (MySQL 한정)          |
| DEFAULT 값       | 기본값 설정                          |
| FOREIGN KEY      | 외래키 (다른 테이블과 연결)           |
| CHECK 조건       | 값이 조건을 만족해야 함               |

<br></br>

## ALTER 
* 생성된 테이블의 속성과 속성의 관한 제약을 변경
* 기본키, 외래키를 변경
```sql
ALTER TABLE 테이블명
[작업 종류에 따라 다른 명령어];
```
**🔩 명령어 종류**

| 명령어                           | 설명              |
|-------------------------------|-------------------|
| ADD 컬럼명 데이터타입         | 컬럼 추가         |
| DROP COLUMN 컬럼명            | 컬럼 삭제         |
| RENAME COLUMN A TO B          | 컬럼 이름 변경     |
| MODIFY COLUMN 컬럼명 타입     | 컬럼 타입 변경 (MySQL) |
| ALTER COLUMN 컬럼명 TYPE 타입 | 컬럼 타입 변경 (PostgreSQL) |
| ADD CONSTRAINT 제약명 ...     | 제약조건 추가     |
| RENAME TABLE A TO B           | 테이블 이름 변경   |

<br></br>

## DROP
* 테이블을 삭제함
* 테이블의 구조 뿐만 아니라 안에 들어있는 데이터까지 전부 사라짐
```sql
DROP TABLE 테이블명;
```
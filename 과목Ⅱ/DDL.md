`DDL` (Data Definition Language)
- CREATE, ALTER, RENAME, DROP
- 테이블 구조를 생성, 변경, 삭제하는 등 데이터 구조를 정의
- 스키마, 도메인, 테이블, 뷰, 인덱스를 정의, 변경, 삭제할 때 사용

<BR>

### PK키
- 한 테이블 당 한개
- NULL ❌
- 중복 ❌
- 테이블 데이터의 고유 인식번호 (id)
- 
### FK키 = 외래키
- 다른 테이블의 기본키로 사용되면서 테이블과의 관계를 연결하는 역할을 하는 칼럼
- 테이블 생성 시 설정 가능
- NULL 값 ⭕
- 한 테이블에 여러개 존재
- `참조 무결성 제약`

  
### Unique키 (고유키)
- NULL ⭕
- 중복 ❌. 고유해야하기 때문에.
- 중복되면 안되는 데이터 => 주민번호, 학번

<BR>

### 테이블명, 컬럼명 규칙
1. 테이블명과 컬럼명은 반드시 문자로 시작
2. A-Z, a-z, 0-9, #,$,_만 사용 가능
3. 다른 테이블명과 중복되면 안됨
4. 칼럼 뒤 데이터유형 지정해야함. varchar, char, number, date(time)



### ERD (Entity Relationship Diagram)
ERD의 구성 3요소 : 엔터티(Entity), 관계(Relationship), 속성(Attribute)

# 데이터 유형
데이터 유형| 설명|
-|-|
CHARACTER(s)|고정길이 문자 ex) 주민 번호
VARCHAR(s)|가변길이 문자 ex) 집 주소
NUMERIC|정수, 실수
DATETIME|날짜와 시각정보(Oracle은 DATE로 표현)


# CREATE TABLE
- 테이블 생성 -> 데이터 정의 -> 데이터 유형
- PK, FK 설정
- 테이블 생성 구문
```SQL
CREATE TABLE 테이블명 (
    칼럼명1 DATATYPE [DEFAULT형식],
    칼럼명2 DATATYPE [DEFAULT형식];
)
```
- 테이블명과 칼럼명은 반드시 숫자가 아닌 문자로 시작
- 특수문자 '-'는 ❌
- A-Z, a-z, 0-9, $, # 만 허용
- 칼럼과 칼럼의 구분은 콤마(,)로 하고 마지막 칼럼에는 ,를 찍지 않고 ;를 쓴다.
- 칼럼에 대한 제약 조건은 CONSTRAINT를 이용한다.

# 제약조건(CONSTRAINT)의 종류

구분|설명|
-|-|
PRIMARY KEY(기본키)|- 하나의 테이블 당 하나 <BR> - 기본키 = 고유키 & NOT NULL|
UNIQUE KEY(고유키)| - 행을 고유하게 식별(주민번호) <BR> - NOT NULL이어도 괜찮다.|
NOT NULL| DEFAULT는 NULL|
CHECK | TRUE or FALSE|
FOREIGN KEY(외래키)|관계형 데이터베이스에서 테이블 간의 관계를 정의, 외래키를 다른 테이블에서 기본키로 설정할 수 있음|

# 생성된 테이블 구조 확인
```SQL
-- Oracle
DESCRIBE 테이블명;

-- SQL SERVER
sp_help 'dbo.테이블명'
```
# ALTER TABLE
- 한번 생성된 테이블은 생성 당시의 구조를 유지하는 것이 좋지만 변경해야할 일이 생길 수 있다.
- 칼럼을 추가/삭제
- 제약조건을 추가/삭제

## 1. ADD COLUMN
- 기존 테이블에 칼럼 추가하기

```sql
--ORACLE
ALTER TABLE 테이블명 ADD (칼럼명 DATATYPE);

--SQL SERVER
ALTER TABLE 테이블명 ADD 칼럼명 DATATYPE;
```
## 2. DROP COLUMN
- 테이블에서 필요없는 칼럼을 삭제
- 한번에 하나의 칼럼만 삭제 가능
- 삭제 후 최소 하나 이상의 칼럼이 테이블에 존재해야한다.
- 주의: 한번 삭제된 컬럼은 복구가 불가능하다.

```SQL
ALTER TABLE 테이블명 DROP COLUMN 삭제할 컬럼명;
```

## 3. MODIFY COLUMN
칼럼의 데이터 유형, 디폴트 값, NOT NULL 제약조건에 대한 변경등 `칼럼에 대한 정의를 변경`

```SQL
--ORACLE
ALTER TABLE 테이블명 MODIFY (칼럼명1 데이터유형 DEFAULT식 NOT NULL)

-- SQL SERVER
ALTER TABLE 테이블명 ALTER (칼럼명1 데이터유형 DEFAULT식 NOT NULL)
```
## 4. RENAME COLUMN
- 칼럼명 변경
```SQL
ALTER TABLE 테이블명 RENAME COLUMN 변경해야할 칼럼명 TO 새로운 칼럼명
```
## 5. DROP CONSTRAINT
- 테이블 생성 시 부여했던 제약 조건을 삭제하는 명령어
- 외래키 제약조건 삭제

```sql
ALTER TABLE 테이블명 DROP CONSTRAINT 제약조건명_FK;
```

## 6. ADD CONSTRAINT
- 컬럼에 제약조건 추가
- 외래키 제약조건 추가

```SQL
ALTER TABLE 테이블명 ADD CONSTRAINT 제약조건명 

--[예제] PLAYER 테이블에 TEAM 테이블과의 외래키 제약조건을 추가한다. 제약조건명은 PLAYER_FK로 하고, PLAYER 테이블의 TEAM_ID 칼럼이 TEAM 테이블의 TEAM_ID를 참조하는 조건이다.
-- ALTER TABLE PLAYER ADD CONSTRAINT PLAYER_FK FOREIGN KEY (TEAM_ID) REFERENCES TEAM(TEAM_ID);
```

## 6. RENAME TABLE
- 테이블 이름 변경

```SQL
RENAME OLD_TABLE명 TO NEW_TABLE명
```

## 7. DROP TABLE
- 테이블 삭제
```SQL
DROP TABLE 테이블명 [CASCADE CONSTRAINT];
```

## 8. TRUNCATE TABLE
- TRUNCATE TABLE은 테이블 자체가 삭제되는 것은 아니고 테이블에 있던 모든 행들이 제거되고 저장공간을 재사용 가능하도록 한다.

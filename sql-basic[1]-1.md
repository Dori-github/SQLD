# 2-1. SQL 기본 및 활용 - SQL기본 
## 관계형 데이터베이스 개요
### 1. 데이터베이스 
#### [가.데이터베이스/ DBMS]
데이터베이스 : 특정 기업이나 조직 또는 개인이 필요에 의해 데이터를 일정한 형태로 저장해 놓은 것을 의미.     
데이터베이스시스템(DBMS) : 데이터 손상을 피하고, 필요시 필요한 데이터를 복구하기 위한 강력한 기능을 만족시켜주는 시스템.
#### [나.데이터베이스 발전]
- 1960년대 : 플로우차트 중심의 개발 방법을 사용, 파일 구조를 통해 데이터를 저장하고 관리함.
- 1970년대 : 데이터베이스 관리 기법이 처음 태동되던 시기, 계층형(Hierarchical) 데이터베이스, 망형(Network) 데이터베이스 상용화됨.  
- 1980년대 :  관계형 데이터베이스가 상용화되었으며 Oracle, Sybase, DB2와 같은 제품이 사용됨.  
- 1990년대 : Oracle, Sybase, Informix, DB2, Teradata, SQL Server 외 많은 제품들이 보다 향상된 기능으로 정보시스템의 확실한 핵심 솔루션으로 자리잡게 되었으며,  객체 관계형 데이터베이스로 발전. 
#### [다.관계형 데이터베이스(Relational Database)]
[기존 파일시스템]
- 사용자 동시 검색 기능
- 동시에 입력, 수정,삭제 X -> 관리가 어려움.  
- 데이터 정합성 보장 어려움 
이로인해, 여러개 데이터 파일이 존재 할 경우 동일한 데이터가 여러곳에 저장되는 문제가 발생하고, 변경 작업 발생시 한 서로 다른 정보파일이 존재 하므로 데이터 불일치성이 발생. 
[관계형데이터베이스]
- 정규화를 통해 기존 파일 시스템 이상 현상을 제거
- 데이터 중복성을 피할 수 있음 
- 동시성 관리,병행 제어를 통해 많은 사용자들이 동시에 데이터 공유 및 조작 가능
- 메타 데이터 총괄 관리 -> 데이터 성격,속성,표현방법 체계화 가능
- 데이터 표준화를 통한 데이터 품질을 확보 할 수 있음
- 인증된 사용자 만이 참조 할 수 있는 보안 기능 제공
- 무결성 보장 
- 갑작 스런 장애로부터 사용자가 입력, 수정, 삭제 데이터가 제대로 반영 될 수 있도록 보장해줌
- 시스템 다운,재해 등 상황에서도 데이터 회복/복구 기능 제공
### 2. SQL(Strucured Query Language)
#### [가. SQL의 의미]
SQL : 관계형 데이터베이스에서 데이터 정의, 데이터 조작, 데이터 제어를 하기 위해 사용하는 언어. 일반적 개발 언어 처럼 독립된 하나의 개발 언어. 
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_147.jpg)  
#### [나.SQL 명령어]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_148.jpg)  
### 3. TABLE
#### [가. 테이블]
테이블 : 어느 특정한 주제와 목적으로 만들어지는 일종의 집합
K-League에 등록 되어 있는 팀들의 정보와 선수들에 관련된 데이터를 데이터베이스 화 하면, 
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_150.jpg)  
이렇게 정리된다. 데이터는 관계형 데이터베이스의 기본 단위인 테이블 형태로 저장. 모든 자료는 테이블에 등록되고, 우리는 테이블로부터 원하는 자료를 꺼낼 수 있음.
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_151.jpg)  
[표 Ⅱ-1-3]의 내용을 보면 선수, 팀, 팀연고지, 포지션, 등번호, 생년월일, 키, 몸무게가 각각의 칼럼이 되며, 해당 테이블은 반드시 하나 이상의 칼럼을 가져야 함.

테이블에는 등룍된 자료들이 있으며, 삭제 하지 않는 한 지속적으로 유지됨.
#### [나. Table의 구조]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_154.jpg)  
#### [다.Table 용어]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_155.jpg)  
모든 데이터를 하나의 테이블에 저장 하지 않음. 복수의 테이블로 분할 하여 저장. 그리고 분할된 테이블은 값에 의해 연결됨. 
#### [다. 정규화]
정규화(Normalization): 테이블을 분할하여 데이터의 불필요한 중복을 줄이는 프로세스. 이상현상을 방지하기 위해 매우 중요함.  
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_156.jpg)  
#### [라. 테이블 관계 용어]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_157.jpg)  

### 4. ERD(Entity Relationship Diagram)  
팀 정보와 선수 정보 간에는 어떤 의미의 관계가 존재하며, 다른 테이블과 어떤 의미의 연관성을 가지고 있다.
#### [가. ERD]
ERD(Entity Relationship Diagram) : 테이블 관계 의미를 직관적으로 표현할 수 있는 수단
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_158.jpg)  
 팀과 선수 간에 "소속" 관계로 맺어져 있음. 
#### [나. ERD 구성요소 ]
 - 엔터티
 - 관계
 - 속성
 
 #### [다. ERD표기법]
 ![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_159.jpg)
#### K-리그 테이블 간의 양방향 관계 설명
- 하나의 팀은 여러 명의 선수를 포함할 수 있다. 
- 한 명의 선수는 하나의 팀에 꼭 속한다.
- 하나의 팀은 하나의 전용 구장을 꼭 가진다. 
- 하나의 운동장은 하나의 홈팀을 가질 수 있다.
- 하나의 운동장은 여러 게임의 스케줄을 가질 수 있다. 
- 하나의 스케줄은 하나의 운동장에 꼭 배정된다.
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_160.jpg)  
#### 사원-부서 간 양방향 관계 설명
- 하나의 부서는 여러 명의 사원을 보유할 수 있다. 
- 한 명의 사원은 하나의 부서에 꼭 소속된다.
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_161.jpg)

## DDL 
### 1. 데이터유형
#### [가. 데이터 유형]
- 데이터베이스 테이블에 특정 자료 입력시, 그 공간을 유형별로 나누는 기준.
- 칼럼이 받아 들일 수 있는 자료의 유형을 규정함.
- 다른 종류의 데이터가 들어오면 에러 발생시킴.
#### [나.자주쓰는 데이터 유형]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_162.jpg)  
#### CHAR  VS VARCHAR
<저장영역방법의차이> 
VARCHAR 유형은 가변 길이 이므로, 길이가 다양한 칼럼과, 정의된 길이와 실제 데이터 길이에 차이가 있는 칼럼에 적합.
또한, 저장도 CHAR 유형보다 작은 영역에 저장 할 수 있음
<비교방법의 차이>
- CHAR: 문자열을 비교 할 때 공백을 채워서 비교하는 방법 사용. 공백 채우기 비교에서는 짧은 쪽 끝의 공백을 추가하여 2개의 데이터가 같은 길이가 되도록 함 . 그리고 앞에서부터 한 문자씩 비교, 끝 공백만 다른 문자열은 같다고 판단.
- VARCHAR : 맨 처음 부터 한 문자씩 비교, 공백도 하나의 문자로 취급, 끝의 공백이 다르면 다른 문자로 판단.
```
예) CHAR 유형'AA'='AA'
VARCHAR 유형 'AA'<>'AA
```
#### VARCHAR의 특징
40바이트로 지정되더라도, 데이터가 입력되는 경우 11 바이트 공간 만을 차지. 주민등록번호,사번 처럼 고정된 길이의 문자열을 가지지 않는다면 VARCHAR 유형 적용. 
### 2. CREATE TABLE
테이블 생성을 위해서는 해당 테이블에 입력 데이터를 정의하고, 정의한 데이터를 어떠한 유형을 선언할것인지 결정해야함.
#### [가. 테이블과 칼럼 정의]
#### 기본키 칼럼 지정
1. 테이블 존재하는 모든 데이터를 고유하게 식별 가능 한 경우
2. 반드시 값이 존재하는 단일 칼럼이나 칼럽의 조합들 중 하나를 선정
3. 테이블과 테이블 간 정의된 관계는 기본키와 외부키를 활용하여 설정. 
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_163.jpg)
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_164.jpg)

#### [나. CREATE TABLE]
#### 테이블 설정 형식
```
CREATE TABLE 테이블이름( 칼럼명1 DATATYPE[DEFAULT형식], 칼럼명2 DATATYPE[DEFAULT 형식], 칼럼명2 DATATYPE [DEFAULT 형식]
```
#### 테이블 생성 시 주의해야할 규칙
- 테이블 명은 객체를 의미 할 수 있는 적절한 이름 사용
- 테이블 명은 다른 테이블의 이름과 중복X
- 한 테이블 내 칼럼명이 중복되게 지정X
- 테이블 이름을 지정하고, 각 칼럼은 괄호로 묶어 지정.
- 각 칼럼들을 콤마로 구분되고, 생성문의 끝은 세미콜론으로 끝내기.
- 칼럼을 일관성 있게 사용.
- 칼럼 뒤 데이터 유형은 꼭 지정.
- 테이블과 칼럼명은 반드시 문자로 시작. 벤더별로 길이에 대해 한계가 있음.
- 벤더에서 사전에 정의한 예약어는 쓸수X
- 문자만 허용됨.
ex) A-Z, a-z,0-9,_,$
< 테이블 명 잘못된 사례 >
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_165.jpg)
#### 예제
```
CREATE TABLE PLAYER( PLAYER_ID CHAR(7) NOT NULL, PLAYER_NAME VARCHAR(20) NOT NULL, TEAM_ID CHAR(3) NOT NULL, CONSTRAINT PLAYER_PK PRIMARY KEY (PLAYER_ID) CONSTRAINT PLAYER_FK FOREIGN KEY (TEAM_ID) REFERENCES TEAM(TEAM_ID) );
```
#### 추가적인 주의사항 
- 테이블 생성시 대/소문자 구분X 
- DATETIME 데이터 유형에는 별도로 크기를 지정X
- 문자 데이터 유형은 반드시 가질 수 있는 최대 길이를 표시 - 칼럼과 칼럼의 구분은 콤마로 하되, 마지막 칼럼은 콤마를 찍지 X
- 칼럼에 대한 제약조건이 있으면 CONSTRAINT를 이용하여 추가가능.
#### 테이블 구조확인
Oracle : DESCRIBE 테이블명;  
SQL server : exec sp_help 'dbo.테이블명'
#### [다. 제약조건(CONSTRAINT)]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_166.jpg)
DESC(RIBE) 테이블명; -> 테이블 구조 확인(Oracle)  
exec sp_help ‘db0.테이블명’ -> (SQL Server)  
go
#### NULL의미
정의되지 않은 미지의 값, 현재 데이터를 입력하지 못하는 경우
#### DEFAULT의미
기본값을 사전에 설정 할 수 있음. 데이터를 지정하지 않는 경우 사전에 정의된 기본값 자동 입력됨.
#### 예제
```
테이블명 : TEAM 테이블 설명 : K-리그 선수들의 소속팀에 대한 정보를 가지고 있는 테이블 칼럼명 : TEAM_ID (팀 고유 ID) 문자 고정 자릿수 3자리,REGION_NAME (연고지 명) 문자 가변 자릿수 8자리,TEAM_NAME (한글 팀 명) 문자 가변 자릿수 40자리,E-TEAM_NAME (영문 팀 명) 문자 가변 자릿수 50자리 ,ORIG_YYYY (창단년도) 문자 고정 자릿수 4자리,STADIUM_ID (구장 고유 ID) 문자 고정 자릿수 3자리,ZIP_CODE1 (우편번호 앞 3자리) 문자 고정 자릿수 3자리,ZIP_CODE2 (우편번호 뒷 3자리) 문자 고정 자릿수 3자리,ADDRESS (주소) 문자 가변 자릿수 80자리,DDD (지역번호) 문자 가변 자릿수 3자리,TEL (전화번호) 문자 가변 자릿수 10자리,FAX (팩스번호) 문자 가변 자릿수 10자리,HOMEPAGE (홈페이지) 문자 가변 자릿수 50자리OWNER (구단주) 문자 가변 자릿수 10자리, 제약조건 : 기본 키(PRIMARY KEY) → TEAM_ID (제약조건명은 TEAM_ID_PK) NOT NULL → REGION_NAME, TEAM_NAME, STADIUM_ID (제약조건명은 미적용)
```
```
[예제] Oracle CREATE TABLE TEAM ( TEAM_ID CHAR(3) NOT NULL, REGION_NAME VARCHAR2(8) NOT NULL, TEAM_NAME VARCHAR2(40) NOT NULL, E_TEAM_NAME VARCHAR2(50), ORIG_YYYY CHAR(4), STADIUM_ID CHAR(3) NOT NULL, ZIP_CODE1 CHAR(3), ZIP_CODE2 CHAR(3), ADDRESS VARCHAR2(80), DDD VARCHAR2(3), TEL VARCHAR2(10), FAX VARCHAR2(10), HOMEPAGE VARCHAR2(50), OWNER VARCHAR2(10), CONSTRAINT TEAM_PK PRIMARY KEY (TEAM_ID), CONSTRAINT TEAM_FK FOREIGN KEY (STADIUM_ID) REFERENCES STADIUM(STADIUM_ID) ); 테이블이 생성되었다.
```
#### [라. 생성된 테이블 구조 확인]
1) ORACLE : DESCRIBE 테이블명, DESC 테이블명 
2) SQL : "sp_help" "dbo.테이블명" 
```
[실행 결과] Oracle DESCRIBE PLAYER; 칼럼 NULL 가능 데이터 유형 ------------------ ----------- -------------- PLAYER_ID NOT NULL CHAR(7) PLAYER_NAME NOT NULL VARCHAR2(20) TEAM_ID NOT NULL CHAR(3) E_PLAYER_NAME VARCHAR2(40) NICKNAME VARCHAR2(30) JOIN_YYYY CHAR(4) POSITION VARCHAR2(10) BACK_NO NUMBER(2) NATION VARCHAR2(20) BIRTH_DATE DATE SOLAR CHAR(1) HEIGHT NUMBER(3) WEIGHT NUMBER(3)
```
```
[실행 결과] SQL Server exec sp_help 'dbo.PLAYER' go 칼럼이름 데이터 유형 길이 NULL 가능 --------------- ------------ ------ -------- PLAYER_ID CHAR(7) 7 NO PLAYER_NAME VARCHAR(20) 20 NO TEAM_ID CHAR(3) 3 NO E_PLAYER_NAME VARCHAR(40) 40 YES NICKNAME VARCHAR(30) 30 YES JOIN_YYYY CHAR(4) 4 YES POSITION VARCHAR(10) 10 YES BACK_NO TINYINT 1 YES NATION VARCHAR(20) 20 YES BIRTH_DATE DATE 3 YES SOLAR CHAR(1) 1 YES HEIGHT SMALLINT 2 YES WEIGHT SMALLINT 2 YES
```
#### [ 마. SELECT 문장을 통한 테이블 생성 사례]
1) ORACLE CRAS : CREATE TABLE ~ AS SELECT ~
2) SQL SERVER SELECT * INTO TABLE1FROM TABLE2
기존 테이블의 제약조건 중에 NOT NULL만 새로운 복제 테이블에 적용되고, 기본키, 고유키, 외래키, CHECK 등의 다른 제약조건은 없어진다.
제약 조건을 추가하기 위해서 뒤에 나오는 ALTER TABLE 기능을 사용해야 한다.
### 3. ALTER TABLE
생성 당시의 구조를 유지하게된다. 업무적인 요구사항이나 시스템 운영상 테이블을 사용 하는 도중에 변경하는 일이 발생 할 수도 있다. 
#### [가. ADD COLUMN]
기존 테이블에 필요한 칼럼을 추가하는 명령 
```
ALTER TABLE 테이블명 ADD 추가할 칼럼명 데이터 유형;
```
주의 할 것은 새롭게 추가된 칼럼은 테이블의 마지막 칼럼이 되며 칼럼 위치를 지정 할 수 없다.
```
ALTER TABLE PLAYER ---------테이블변경 ADD (ADDRESS VARCHAR2(80)); ---------칼럼추가 ALTER TABLE PLAYER DROP COLUMN ADDRESS; ---------칼럼삭제 ALTER TABLE PLAYER MODIFY (ADDRESS VARCHAR2(80));-------Oracle 칼럼수정 ALTER TABLE PLAYER ALTER COLUMN ADDRESS VARCHAR2(80);  SQL 칼럼수정 ALTER TABLE PLAYER RENAME COLUMN ADDRESS TO ADD----Oracle 칼럼이름 변경 ALTER TABLE PLAYER DROP CONSTRAINT 제약조건명; ALTER TABLE PLAYER ADD CONSTRAINT 제약조건명 제약조건 (칼럼명);
```
RENAME 변경 전 테이블명 TO 변경 후 테이블 명 : -----oracle 
sp_rename 변경 전 테이블명, 변경후 테이블 ; ---sql server
#### [나. DROP COLUMN]
#### DROP COLUMN
DROP TABLE PLAYER [CASCADE CONSTRAINT]; 테이블과 관계 있었던 참조 제약조건도 삭제. 
한 번에 하나의 칼럼만 삭제 가능하며, 칼럼 삭제 후 최소 하나 이상의 칼럼이 테이블에 존재해야함.
```
ALTER TABLE 테이블명 DROP COLUMN 삭제할 칼럼명;
```
[예제] 앞에서 PLAYER 테이블에 새롭게 추가한 ADDRESS 칼럼을 삭제한다.
```
[예제] Oracle ALTER TABLE PLAYER DROP COLUMN ADDRESS; 테이블이 변경되었다
```
```
[예제] SQL Server ALTER TABLE PLAYER DROP COLUMN ADDRESS; 명령이 완료되었다.
```
실행 결과에서 삭제된 칼럼 ADDRESS가 존재하지 않는 것을 확인 할 수 있다.

#### [다. MODIFY COLUMN]
 ALTER TABLE 명령을 이용해 칼럼의 데이터 유형, 디폴트(DEFAULT)값, NOT NULL 제약조건을 변경 할 수 있음.
#### 테이블칼럼 정의 변경 명령
```
[Oracle] ALTER TABLE 테이블명 MODIFY (칼럼명1 데이터 유형 [DEFAULT 식] [NOT NULL], 칼럼명2 데이터 유형 …);
```
```
[SQL Server] ALTER TABLE 테이블명 ALTER (칼럼명1 데이터 유형 [DEFAULT 식] [NOT NULL], 칼럼명2 데이터 유형 …);
```
#### 칼럼 변경 시 고려사항
1. 해당 칼럼 크기를 줄이지 못함.
2. 해당 칼럼이 NULL값만 가지거나, 테이블에 아무 행도 없으면 칼럼의 폭을 줄일 수 있음.
3. 해당 칼럼이 NULL 값 만 가지고 있으면 데이터 유형 변경 가능
4. 해당 칼럼의 DEFAULT 값을 바꾸면 변경작업 이후 발생하는 행 삽입에 영향을 줌.
5. 해당 칼럼에 NULL값이 없을 경우에만 NOT NULL 제약조건 추가 가능.
#### [라. DROP CONSTRAINT]
테이블 생성 시 부여했던 제약 조건을 삭제하는 명령어 형태.
[예제]  PLAYER 테이블의 외래키 제약조건 삭제.
```
[예제] Oracle ALTER TABLE PLAYER DROP CONSTRAINT PLAYER_FK; 테이블이 변경되었다.
 ```
```
[예제] SQL Server ALTER TABLE PLAYER DROP CONSTRAINT PLAYER_FK; 명령이 완료되었다.
```
#### [마. ADD CONSTRAINT]
특정 칼럼에 제약 조건을 추가하는 형태.
[예제]  PLAYER  테이블에 TEAM 테이블과 외래키 제약조건 추가. (제약조건명은 PLAYER_FK, PLAYER 테이블의 TEAM_ID 칼럼이 TEAM 테이블의 TEAM_ID를 참조하는 조건)
```
[예제]Oracle ALTER TABLE PLAYER ADD CONSTRAINT PLAYER_FK FOREIGN KEY (TEAM_ID) REFERENCES TEAM(TEAM_ID); 테이블이 변경되었다
```
```
[예제]SQL Server ALTER TABLE PLAYER ADD CONSTRAINT PLAYER_FK FOREIGN KEY (TEAM_ID) REFERENCES TEAM(TEAM_ID); 명령이 완료되었다.
```
### 4. RENAME TABLE
RENAME 의 명령어를 사용하여 테이블 이름 변경.
```
RENAME 변경전 테이블명 TO 변경후 테이블명;
```
SQL Server은 sp_rename을 이용하여 이름을 변경.
```
sp_rename 변경전 테이블명, 변경후 테이블명;
```
[예제] RENAME 문장을 이용하여 TEAM 테이블명을 다른 이름으로 변경하고, 다시 TEAM 테이블로 변경한다.
```
[예제] Oracle RENAME TEAM TO TEAM_BACKUP; 테이블 이름이 변경되었다. RENAME TEAM_BACKUP TO TEAM; 테이블 이름이 변경되었다.
```
```
[예제] SQL Server sp_rename 'dbo.TEAM','TEAM_BACKUP'; 주의: 엔터티 이름 부분을 변경하면 스크립트 및 저장 프로시저를 손상시킬 수 있다. sp_rename 'dbo.TEAM_BACKUP','TEAM'; 주의: 엔터티 이름 부분을 변경하면 스크립트 및 저장 프로시저를 손상시킬 수 있다.
```
### 5. DROP TABLE
테이블을 삭제하는 명령어. 해당 테이블과 관계가 있었던 참조되는 제약 조건도 삭제함. (CASCADE-CONSTRAINT)
```
DROP TABLE 테이블명 [CASCADE CONSTRAINT];
```
[예제] PLAYER 테이블을 제거한다.
```
[예제] Oracle DROP TABLE PLAYER; 테이블이 삭제되었다. DESC PLAYER; ERROR: 설명할 객체를 찾을 수 없다.
```
```
[예제] SQL Server DROP TABLE PLAYER; 명령이 완료되었다. exec sp_help 'dbo.PLAYER'; 메시지 15009, 수준 16, 상태 1, 프로시저 sp_help, 줄 66 데이터베이스 ‘northwind'에 엔터티 'dbo.player'이(가) 없거나 이 작업에 적합하지 않다.
```
### 6. TRUNCATE TABLE
테이블 구조 유지한채 해당 테이블에 있던 행들을 제거.
```
TRUNCATE TABLE PLAYER;
```
[예제] TRUNCATE TABLE을 사용하여 해당 테이블의 모든 행을 삭제하고 테이블 구조를 확인한다.
```
[예제] Oracle TRUNCATE TABLE TEAM; 테이블이 트렁케이트되었다.
```
```
[예제] SQL Server TRUNCATE TABLE TEAM; 명령이 완료되었다.
```
## 테이블 구조 변경 DDL 정리
ALTER TABLE 테이블명  
ADD 칼럼명 데이터 유형;  
DROP COLUMN 칼럼명;  
MODIFY (칼럼명 데이터유형 DEFAULT식 NOT NULL); -> 칼럼 데이터 유형, 조건 등 변경 Oracle   
ALTER (칼럼명 데이터유형 DEFAULT식 NOT NULL); -> SQL Server  
RENAME COLUMN 변경전칼럼명 TO 뉴칼럼명; Ora  
sp_rename 변경전칼럼명, 뉴칼럼명, ‘COLUMN’; SQ   
DROP CONSTRAINT 조건명; 제약조건 삭제  
ADD CONSTRAINT 조건명 조건 (칼럼명); 조건 추가  
RENAME 변경전테이블명 TO 변경후테이블명; Ora  
sp_rename ‘db0.TEAM’,‘TEAM_BACKUP’; SQL  
DROP TABLE 테이블명 [CASCADE CONSTRAINT]  
CASCADE CONSTRAINT : 참조되는 제약조건 삭제  
TRUNCATE TABLE 테이블명: 행 제거, 저장공간 재사용


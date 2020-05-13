# 2-1. SQL 기본 및 활용 - SQL기본
## DML 
DDL : 테이블을 생성하고 생성된 테이블의 구조를 변경하는 명령어.  
DML : 만들어진 테이블에 관리하기를 원하는 자료들을 입력, 수정, 삭제 조회하는 명령어.(SELECT-INSERT-UPDATE-DELETE), 실시간으로 테이블에 영향을 미치지 않음.  
★ DDL 명령어의 경우 실행시 AUTO COMMIT 하지만 DML의 경우 COMMIT을 입력해야 한다.
SQL Server의 경우 DML도 AUTO COMMIT
### 1. INSERT
INSERT : 데이터 입력 명령어
```
▶ INSERT INTO 테이블명 (COLUMN_LIST)VALUES (COLUMN_LIST에 넣을 VALUE_LIST); ▶ INSERT INTO 테이블명VALUES (전체 COLUMN에 넣을 VALUE_LIST);
```
해당 칼럼명과 입력되어야 하는 값을 서로 1:1로 매핑하여 입력함. 
 
[예제] 선수 테이블에 박지성 선수의 데이터를 일부 칼럼만 입력한다.
```
[예제] ▶ 테이블명 : PLAYER INSERT INTO PLAYER (PLAYER_ID, PLAYER_NAME, TEAM_ID, POSITION, HEIGHT, WEIGHT, BACK_NO) VALUES ('2002007', '박지성', 'K07', 'MF', 178, 73, 7); 1개의 행이 만들어졌다.
```
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_167.jpg)  
칼럼 명이 정의되지 않은 경우, NULL로 입력됨.
### 2. UPDATE
UPDATE : 잘못 입력되거나, 변경이 발생하여 정보를 수정해야 하는 경우. 
UPDATE 다음에 수정되어야 할 칼럼이 존재하는 테이블 명을 입력하고 SET 다음에 수정되어야할 칼럼명을 입력 
```
UPDATE 테이블명 SET 수정되어야 할 칼럼명 = 수정되기를 원하는 새로운 값;  
```

[예제] 선수 테이블의 백넘버를 일괄적으로 99로 수정한다.
```
[예제] UPDATE PLAYER SET BACK_NO = 99; 480개의 행이 수정되었다.
```
### 3. DELETE 
DELETE : 삭제를 수행하는 명령어 
FROM 은 생략이 가능. 뒤에 WHERE 절을 사용하지 않는 다면 테이블의 전체 데이터가 삭제됨.
```
DELETE [FROM] 삭제를 원하는 정보가 들어있는 테이블명;
```

[예제] 선수 테이블의 데이터를 전부 삭제한다.
```
[예제] DELETE FROM PLAYER; 480개의 행이 삭제되었다.
```
#### DDL과 DML의 차이 
 - DDL(CREATE,ALTER,RENAME,DROP)  : 직접 데이터베이스에 영향을 미치므로, DDL을 입력하는 순간 명령어 해당 작업이 즉시 완료됨.
 - DML(INSERT, UPDATE, DELETE, SELECT) : 조작 하려는 테이블을 메모리 버퍼에 올려놓고, 작업 하므로 실시간으로 테이블에 영향을 미치지 않음. 명령어가 반영 되려면 COMMIT 명령어를 입력하여 TRANSACTION 종료.
 
### 4. SELECT 
#### [가.SELECT]
SELECT : 조회 명령어. 

[예제] 조회하기를 원하는 칼럼명을 SELECT 다음에 콤마 구분자(,)로 구분하여 나열하고, FROM 다음에 해당 칼럼이 존재하는 테이블명을 입력하여 실행시킨다. 입력한 선수들의 데이터를 조회한다.
```
[예제] SELECT PLAYER_ID, PLAYER_NAME, TEAM_ID, POSITION, HEIGHT, WEIGHT, BACK_NO FROM PLAYER
```
#### [나.DISTINCT 옵션]
DISTINCT : SELECT 와 같은 결과를 출력.
```
[예제] SELECT DISTINCT POSITION FROM PLAYER;
```
#### [다.WILDCARD] 
WILDCARD : 보고싶은 정보들이 있는 칼럼들을 선택하여 조회. 모든 칼럼 정보를 보고 싶은 경우 애스터리스크(*)를 사용하여 조회.  
- * : 모든
- % : 모든
- - : 한 글자
```
SELECT * FROM 테이블명;
```

[예제] 입력한 선수들의 정보를 모두 조회한다.
```
[예제] SELECT * FROM PLAYER;
```
#### [라.ALIAS 부여하기]
ALIAS : 조회된 결과에 일종 별명을 부여하여 칼럼 레이블을 변경함. 
- 칼럼 뒤에 온다.
- 칼럼명과 ALIAS 사이에 AS, as 키워드 사용 가능.
- 이중 인용부호는 ALIAS가 공백, 특수 문자를 포함할 경우와 대소문자 구분이 필요할 경우 사용됨.

[예제] 입력한 선수들의 정보를 칼럼 별명을 이용하여 출력한다.
```
[예제] SELECT PLAYER_NAME AS 선수명, POSITION AS 위치, HEIGHT AS 키, WEIGHT AS 몸무게 FROM PLAYER; 칼럼 별명에서 AS를 꼭 사용하지 않아도 되므로, 아래 SQL은 위 SQL과 같은 결과를 출력한다. SELECT PLAYER_NAME 선수명, POSITION 위치, HEIGHT 키, WEIGHT 몸무게 FROM PLAYER;
```
```
[실행 결과] 선수명 위치 키 몸무게 ----- --- -- ---- 정경량 MF 173 65 정은익 MF 176 63 레오마르 MF 183 77 명재용 MF 173 63 변재섭 MF 170 63 보띠 MF 174 68 비에라 MF 176 73 서동원 MF 184 78 안대현 MF 179 72 양현정 MF 176 72 유원섭 MF 180 77 김수철 MF 171 68 임다한 DF 181 67 ：：：： 480개의 행이 선택되었다.
```
[예제] 칼럼 별명을 적용할 때 별명 중간에 공백이 들어가는 경우 『" " 』를 사용해야 한다. SQL Server의 경우『" "』, 『' 』', 『[ ]』와 같이 3가지의 방식으로 별명을 부여할 수 있다.
```
[예제] SELECT PLAYER_NAME "선수 이름", POSITION "그라운드 포지션", HEIGHT "키", WEIGHT "몸무게" FROM PLAYER;
```
```
[실행 결과] 선수 이름 그라운드 포지션 키 몸무게 ------ ----------- -- ---- 정경량 MF 173 65 정은익 MF 176 63 레오마르 MF 183 77 명재용 MF 173 63 변재섭 MF 170 63 보띠 MF 174 68 비에라 MF 176 73 서동원 MF 184 78 안대현 MF 179 72 양현정 MF 176 72 유원섭 MF 180 77 김수철 MF 171 68 임다한 DF 181 67 ：：：： 480개의 행이 선택되었다.
```
### 5. 산술 연산자와 합성 연산자
#### [가. 산술 연산자]
산술 연산자는 NUMBER 과 DATE 자료형에 적용되며, 우선순위를 위한 괄호 적용이 가능. 우선순위를 가짐.
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_169.jpg)  
[예제] 선수들의 키에서 몸무게를 뺀 값을 알아본다.
```
[예제] SELECT PLAYER_NAME 이름, HEIGHT - WEIGHT "키-몸무게" FROM PLAYER;
```
```
[실행 결과] 이름 키-몸무게 --- ------- 정경량 108.00 정은익 113.00 레오마르 106.00 명재용 110.00 변재섭 107.00 보띠 106.00 비에라 103.00 서동원 106.00 안대현 107.00 양현정 104.00 유원섭 103.00 김수철 103.00 임다한 114.00 … … 480개의 행이 선택되었다.
```
#### [나. 합성 연산자]
합성 연산자 : 문자와 문자를 연결하는 명령어를 말함. 
#### 합성연산자 특징 
- 문자와 문자를 연결하는 경우 2개의 수직 바(||)에 의해 이루어진다.
- (Oracle) 문자와 문자를 연결하는 경우 + 표시에 의해 이루어진다.
- (SQL Server) 두 벤더 모두 공통적으로 CONCAT (string1, string2) 함수를 사용할 수 있다.
- 칼럼과 문자 또는 다른 칼럼과 연결시킨다.
- 문자 표현식의 결과에 의해 새로운 칼럼을 생성한다.
[예제] 다음과 같은 선수들의 출력 형태를 만들어 본다.
```
출력 형태) 선수명 선수, 키 cm, 몸무게 kg 예) 박지성 선수, 176 cm, 70 kg
```
```
[예제] Oracle SELECT PLAYER_NAME || '선수,' || HEIGHT || 'cm,' || WEIGHT || 'kg' 체격정보 FROM PLAYER;
```
```
[예제] SQL Server SELECT PLAYER_NAME +'선수, '+ HEIGHT +'cm, '+ WEIGHT +'kg'체격정보 FROM PLAYER;
```
```
[실행 결과] 체격정보 정경량선수,173cm,65kg 정은익선수,176cm,63kg 레오마르선수,183cm,77kg 명재용선수,173cm,63kg 변재섭선수,170cm,63kg 보띠선수,174cm,68kg 비에라선수,176cm,73kg 서동원선수,184cm,78kg 안대현선수,179cm,72kg 양현정선수,176cm,72kg 유원섭선수,180cm,77kg 김수철선수,171cm,68kg 임다한선수,181cm,67kg … 480개의 행이 선택되었다.
```
## TCL 
### 1. 트랜잭션 개요
#### [가. 트랜잭션]
트랜잭션 : 데이터베이스 논리적 연산단위로, 밀접히 관련되어 분리될 수 없는 한개 이상의 데이터베이스 조작. 분할 할 수 없는 최소의 단위.  
트랜잭션의 대상이 되는 SQL문 : UPDATE, INSERT, DELETE 등 데이터를 수정하는 DML문 
#### TCL ( DML에 의해 조작된 결과를 트랜잭션 별로 제어하는 명령어)
COMMIT : 올바르게 반영된 데이터를 데이터베이스에 반영.
ROLLBACK : 트랜잭션 시작 이전의 상태로 되돌리는 것.
SAVEPOINT : 저장점. 
#### 트랜잭션 특성 
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_170.jpg)  

여기서 데이터베이스는 트랜잭션 특성인 원자성을 충족하고자 잠금(LOCKING) 기능을 제공함.  
잠금 기능이란 다른 트랜잭션이 동시에 접근하지 못하도록 제한하는 기법을 말함.
### 2. COMMIT
#### [가. COMMIT ]
COMMIT :  트랜잭션을 완료하는 명령어. INSERT 문장, UPDATE 문장, DELETE 문장을 사용한 후 이런 변경 작업이 완료되었음을 데이터베이스에 알려주기 위해 사용함.올바르게 반영된 데이터를 DB에 반영.  
COMMIT, ROLLBACK 효과 : 데이터 무결성 보장, 영구적인 변경 하기전 테이터 변경사항 확인 가능, 논리적 연관된 작업 그룹 핑하여 처리가능 
#### COMMIT 특징
- 단지 메모리 BUFFER에만 영향을 받았기 때문에 데이터의 변경 이전 상태로 복구 가능.
- 현재 사용자는 SELECT 문장으로 결과를 확인 가능.
- 다른 사용자는 현재 사용자가 수행한 명령의 결과를 볼 수 없다.
- 변경된 행은 잠금(LOCKING)이 설정되어서 다른 사용자가 변경할 수 없다.

[예제] PLAYER 테이블에 데이터를 입력하고 COMMIT을 실행한다.
```
[예제] Oracle INSERT INTO PLAYER (PLAYER_ID, TEAM_ID, PLAYER_NAME, POSITION, HEIGHT, WEIGHT, BACK_NO) VALUES ('1997035', 'K02', '이운재', 'GK', 182, 82, 1); 1개의 행이 만들어졌다. COMMIT; 커밋이 완료되었다.
```
#### COMMIT이후 데이터 상태
- 데이터 변경 사항이 데이터베이스에 반영
- 이전 데이터는 영원히 잃어버림
- 모든 사용자가 결과를 볼 수 있음
- 관련 행에 대한 잠금이 풀리고, 다른 사용자들이 행을 조작할 수 있게됨 
#### [나. SQL server 의 COMMIT]
SQL Server는 기본적으로 AUTO COMMIT 모드이기 때문에 DML 수행 후 사용자가 COMMIT이나 ROLLBACK을 처리할 필요가 없다.(ORACLE 일 경우 사용자가 수행해야함.)  
 DML 구문이 성공이면 자동으로 COMMIT이 되고 오류가 발생할 경우 자동으로 ROLLBACK 처리된다.
[예제] PLAYER 테이블에 데이터를 입력한다.
```
[예제] SQL Server INSERT INTO PLAYER (PLAYER_ID, TEAM_ID, PLAYER_NAME, POSITION, HEIGHT, WEIGHT, BACK_NO) VALUES ('1997035', 'K02', '이운재', 'GK', 182, 82, 1); 1개의 행이 만들어졌다.
```
[예제] PLAYER 테이블에 있는 데이터를 수정한다.
```
[예제] SQL Server UPDATE PLAYER SET HEIGHT = 100; 480개의 행이 수정되었다.
```
[예제] PLAYER 테이블에 있는 데이터를 삭제한다.
```
[예제] SQL Server DELETE FROM PLAYER; 480개의 행이 삭제되었다.
```
#### SQL Server트랜잭션 3가지방식 
- AUTO COMMIT : 명령어 성공적으로 수행 -> 자동으로 COMMIT, 오류 발생 ->ROLLBACK 
- 암시적 트랜잭션 : Oracle과 같은 방식. 트랜잭션의 끝을 사용자가 명시적으로 COMMIT, ROLLBACK으로 처리 
- 명시적 트랜잭션 : 트랜잭션 시작과 끝을 모두 사용자가 지정 BEGIN TRANSACTION(BEGIN TRAN)으로 트랜잭션시작

### 3. ROLLBACK 
ROLLBACK : 테이블 내 입력한 데이터나, 수정한 데이터, 삭제한 데이터에 대해 COMMIT이전에 변경 사항을 취소 할 수 있는 명령어.잠금이 풀리고 다른 사용자들이 데이터 변경을 할 수 있게 됨.트랜잭션 시작 이전의 상태로 되돌림.  
[예제] PLAYER 테이블에 데이터를 입력하고 ROLLBACK을 실행한다.
```
[예제] Oracle INSERT INTO PLAYER (PLAYER_ID, TEAM_ID, PLAYER_NAME, POSITION, HEIGHT, WEIGHT, BACK_NO) VALUES ('1999035', 'K02', '이운재', 'GK', 182, 82, 1); 1개의 행이 만들어졌다. ROLLBACK; 롤백이 완료되었다.
```
[예제] PLAYER 테이블에 있는 데이터를 수정하고 ROLLBACK을 실행한다.
```
[예제] Oracle UPDATE PLAYER SET HEIGHT = 100; 480개의 행이 수정되었다. ROLLBACK; 롤백이 완료되었다.
```
[예제] PLAYER 테이블에 있는 데이터를 삭제하고 ROLLBACK을 실행한다.
```
[예제] Oracle DELETE FROM PLAYER; 480개의 행이 삭제되었다. ROLLBACK; 롤백이 완료되었다.
```
#### SQL의 ROLLBACK
SQL Server는  AUTO COMMIT이 기본 방식이므로 임의적으로 ROLLBACK을 수행하려면 명시적으로 트랜잭션을 선언이 필요.
[예제] PLAYER 테이블에 데이터를 입력하고 ROLLBACK을 실행한다.
```
[예제] SQL Server BEGIN TRAN INSERT INTO PLAYER (PLAYER_ID, TEAM_ID, PLAYER_NAME, POSITION, HEIGHT, WEIGHT, BACK_NO) VALUES ('1999035', 'K02', '이운재', 'GK', 182, 82, 1); 1개의 행이 만들어졌다. ROLLBACK; 롤백이 완료되었다.
```
[예제] PLAYER 테이블에 있는 데이터를 수정하고 ROLLBACK을 실행한다.
```
[예제] SQL Server BEGIN TRAN UPDATE PLAYER SET HEIGHT = 100; 480개의 행이 수정되었다. ROLLBACK; 롤백이 완료되었다.
```
[예제] PLAYER 테이블에 있는 데이터를 삭제하고 ROLLBACK을 실행한다.
```
[예제] SQL Server BEGIN TRAN DELETE FROM PLAYER; 480개의 행이 삭제되었다. ROLLBACK; 롤백이 완료되었다.
```
#### ROLLBACK 후 데이터 상태
- 데이터에 대한 변경 사항은 취소된다. 
- 이전 데이터는 다시 재저장된다. 
- 관련된 행에 대한 잠금(LOCKING)이 풀리고, 다른 사용자들이 행을 조작할 수 있게 된다.
#### COMMIT 과 ROLLBACK 사용시 효과
- 데이터 무결성 보장 
- 영구적인 변경을 하기 전에 데이터의 변경 사항 확인 가능 
- 논리적으로 연관된 작업을 그룹핑하여 처리 가능
### 4. SAVEPOINT
SAVEPOINT : 현 시점에서 SAVEPOINT 까지 트랜잭션 일부만 롤백 할 수 있는 명령어. 복잡한 대규모 트랜잭션 에러 발생 시 SAVEPOINT까지의 트랜잭션만 롤백하고 실패한 부분 다시 실행 가능. 
```
SAVEPOINT 저장점명; ROLLBACK TO 저장점명;  ---Oracle SAVE TRANSACTION 저장점명; ROLLBACK TRANSACTION 저장점명; ---SQL Sever
```
#### [가.SVPT1 저장점을 가지는 경우]
```
SAVEPOINT SVPT1;
```
저장점 까지 롤백 할 때는 ROLLBACK 뒤 저장점 명 저장
```
ROLLBACK TO SVPT1;
```
#### [나.SQL 에서 STR1 저장점을 가지는 경우]
저장점까지 롤백할 때는 ROLLBACK 뒤에 저장점 명을 지정한다.

ROLLBACK TRANSACTION SVTR1;

[예제] SAVEPOINT를 지정하고, PLAYER 테이블에 데이터를 입력한 다음 롤백(ROLLBACK)을 이전에 설정한 저장점까지 실행한다.
```
[예제] Oracle SAVEPOINT SVPT1; 저장점이 생성되었다. INSERT INTO PLAYER (PLAYER_ID, TEAM_ID, PLAYER_NAME, POSITION, HEIGHT, WEIGHT, BACK_NO) VALUES ('1999035', 'K02', '이운재', 'GK', 182, 82, 1); 1개의 행이 만들어졌다. ROLLBACK TO SVPT1; 롤백이 완료되었다.
```
```
[예제] SQL Server SAVE TRAN SVTR1; 저장점이 생성되었다. INSERT INTO PLAYER (PLAYER_ID, TEAM_ID, PLAYER_NAME, POSITION, HEIGHT, WEIGHT, BACK_NO) VALUES ('1999035', 'K02', '이운재', 'GK', 182, 82, 1); 1개의 행이 만들어졌다. ROLLBACK TRAN SVTR1; 롤백이 완료되었다.
```
[예제] 먼저 SAVEPOINT를 지정하고 PLAYER 테이블에 있는 데이터를 수정한 다음 롤백(ROLLBACK)을 이전에 설정한 저장점까지 실행한다.
```
[예제] Oracle SAVEPOINT SVPT2; 저장점이 생성되었다. UPDATE PLAYER SET WEIGHT = 100; 480개의 행이 수정되었다. ROLLBACK TO SVPT2; 롤백이 완료되었다.
```
```
[예제] SQL Server SAVE TRAN SVTR2; 저장점이 생성되었다. UPDATE PLAYER SET WEIGHT = 100; 480개의 행이 수정되었다. ROLLBACK TRAN SVTR2; 롤백이 완료되었다.
```
[예제] SAVEPOINT를 지정하고, PLAYER 테이블에 있는 데이터를 삭제한 다음 롤백(ROLLBACK)을 이전에 설정한 저장점까지 실행한다.
```
[예제] Oracle SAVEPOINT SVPT3; 저장점이 생성되었다. DELETE FROM PLAYER; 480개의 행이 삭제되었다. ROLLBACK TO SVPT3; 롤백이 완료되었다.
```
```
[예제] SQL Server SAVE TRAN SVTR3; 저장점이 생성되었다. DELETE FROM PLAYER; 480개의 행이 삭제되었다. ROLLBACK TRAN SVTR3; 롤백이 완료되었다.
```
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_171.jpg)

### 정리 
1. 해당 테이블에 데이터 변경을 발생시키는 조작어 
- 입력 (INSERT)
- 수정 (UPDATE)
- 삭제(DELETE)
2. 변경되는 데이터의 무결성을 보장
- 커밋(COMMIT) : 반영 
- 롤백(ROLLBACK) : 복귀
```
SAVEPOINT SVPT1; (Oracle)
ROLLBACK TO SVPT1; (Oracle)
SAVE TRAN SVPT1; (SQL Server)
ROLLBACK TRAN SVPT1; (SQL Server)
COMMIT;
```
3. 저장점 (SAVEPOINT/SAVE TRANSACTION)
데이터 변경을 사전에 지정한 저장점 까지 롤백하라 
Oracle : 트랜잭션 대상이 되는 SQL 문장 실행시 자동으로 시작됨. 
SQL : 자동으로 시작되고, COMMIT/ROLLBACK 실생 지점에서 종료된다.
4. COMMIT 과 ROLLBACK 를 실행하지 않아도 자동으로 트랜잭션이 종료되는 경우
- DDL 문장을 실행하면 그 전후 시점에 자동으로 커밋된다.
-   DML 문장 이후에 커밋 없이 DDL 문장이 실행되면 DDL 수행 전에 자동으로 커밋된다. 
- 데이터베이스를 정상적으로 접속을 종료하면 자동으로 트랜잭션이 커밋된다. 
- 애플리케이션의 이상 종료로 데이터베이스와의 접속이 단절되었을 때는 트랜잭션이 자동으로 롤백된다.
5. SQL 트랜잭션이 자동으로 종료되는 경우(원래는 AUTO COMMIT 이 기본)
- 애플리케이션의 이상 종료로 데이터베이스(인스턴스)와의 접속이 단절되었을 때는 트랜잭션이 자동으로 롤백된다.

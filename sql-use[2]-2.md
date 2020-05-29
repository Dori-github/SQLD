# 3-1.SQL 기본 및 활용-SQL활용(2)
## DCL
### 1. DCL 개요
DDL : 테이블 생성과 조작에 관련된 명령어 
DML : 데이터를 조작하기 위한 명령어
TCL : 트랜잭션을 제어하기 위한 명령어
DCL : 이런 명령어들 이외에도 유저를 생성하고 권한을 제어 할 수 있는 DCL 명령어
### 2. 유저와 권한
대부분의 데이터베이스는 데이터 보호와 보안을 위해서 유저와 권한을 관리하고 있음.   
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_223.jpg)  
Oracle : 유저를 통해 데이터베이스에 접속하는 형태  
SQL server : 인스턴스에 접속하기 위해 로그인을 생성하며, 인스턴스 내 존재하는 다수의 데이터베이스에 연결하여 작업하기 위해 유저 생성 후 로그인과 유저를 매핑해줌. 
#### SQL server 로그인 
- Windows 인증 방식으로 Windows 로그인한 정보를 가지고 SQL server 에 접속(트러스트 된 연결)
- 혼합모드 방식으로 기본적으로 Windows 인증으로도 SQL server 에 접속 가능. 사용자 아이디와 비밀번호로 접속하는 방식. 
- ![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_224.jpg)  
#### [가. 유저생성과 시스템 권한 부여]
유저를 생성 한 후 시스템 권한을 일일이 사용자에게 설정하는 것이 아니라, 롤을 이용하여 사용자에게 간편하게 부여하게 됨.   

유저를 생성하려면 유저 생성 권한(CREATE USER)이 있어야함.  
[예제] SCOTT 유저로 접속한 다음 PJS 유저(패스워드: KOREA7)를 생성해 본다.
```
[예제] Oracle CONN SCOTT/TIGER 연결되었다. CREATE USER PJS IDENTIFIED BY KOREA7; CREATE USER PJS IDENTIFIED BY KOREA7; * 1행에 오류: ERROR: 권한이 불충분하다
```
유저생성권한을 부여받지 못했기 때문에 권한이 불충분 하다는 오류 발생. 권한을 가진 SYSTEM 유저로 접속 시 유저 생성 권한을 다른 유저에게 부여 가능.  

[예제] SCOTT 유저에게 유저생성 권한(CREATE USER)을 부여한 후 다시 PJS 유저를 생성한다.  
```
[예제 및 실행 결과] Oracle GRANT CREATE USER TO SCOTT; 권한이 부여되었다. 
CONN SCOTT/TIGER 연결되었다. CREATE USER PJS IDENTIFIED BY KOREA7; 사용자가 생성되었다.
```
유저 생성 전 로그인 생성 (로그인=sa)  
[예제] sa로 로그인을 한 후 SQL 인증을 사용하는 PJS라는 로그인(패스워드: KOREA7)을 생성해 본다. 로그인 후 최초로 접속할 데이터베이스는 AdventureWorks 데이터베이스로 설정한다.
```
[예제 및 실행 결과] SQL Server CREATE LOGIN PJS WITH PASSWORD='KOREA7', DEFAULT_DATABASE=AdventureWorks
```
SQL server 유저는 데이터베이스 마다 존재. 그러므로 생성하고자 하는 유저가 속할 데이터베이스로 이동후 처리.
```
[예제 및 실행 결과] SQL Server USE ADVENTUREWORKS; GO CREATE USER PJS FOR LOGIN PJS WITH DEFAULT_SCHEMA = dbo;
```
[예제] 생성된 PJS 유저로 로그인한다.
```
[예제 및 실행 결과] Oracle CONN PJS/KOREA7; 오류: ERROR: 사용자 PJS는 CREATE SESSION 권한을 가지고 있지 않음; 로그온이 거절되었다.
```
PJS 유저가 유저가 로그인을 하려면 CREATE SESSION 권한을 부여받아야 한다.  
[예제] PJS 유저가 로그인할 수 있도록 CREATE SESSION 권한을 부여한다.
```
[예제 및 실행 결과] Oracle CONN SCOTT/TIGER 연결되었다. GRANT CREATE SESSION TO PJS; 권한이 부여되었다. CONN PJS/KOREA7 연결되었다.
```
[예제] PJS 유저로 테이블을 생성한다.
```
[예제 및 실행 결과] Oracle SELECT * FROM TAB; 선택된 레코드가 없다. CREATE TABLE MENU ( MENU_SEQ NUMBER NOT NULL, TITLE VARCHAR2(10) ); CREATE TABLE MENU ( * 1행에 오류: ERROR: 권한이 불충분하다.
```
```
[예제 및 실행 결과] SQL Server CREATE TABLE MENU ( MENU_SEQ INT NOT NULL, TITLE VARCHAR(10) ); 데이터베이스 ‘AdventureWorks'에서 CREATE TABLE 사용 권한이 거부되었다.
```
PJS 유저는 로그인 권한만 부여되었기 때문에 테이블을 생성하려면 테이블 생성 권한(CREATE TABLE)이 불충분하다는 오류가 발생.

[예제] SYSTEM 유저를 통하여 PJS 유저에게 CREATE TABLE 권한을 부여한 후 다시 테이블을 생성한다.
```
[예제 및 실행 결과] Oracle CONN SYSTEM/MANAGER 연결되었다. GRANT CREATE TABLE TO PJS; 권한이 부여되었다. CONN PJS/KOREA7 연결되었다. CREATE TABLE MENU ( MENU_SEQ NUMBER NOT NULL, TITLE VARCHAR2(10) ); 테이블이 생성되었다.
```
```
[예제 및 실행 결과] SQL Server GRANT CREATE TABLE TO PJS; 권한이 부여되었다. 스키마에 권한을 부여한다. GRANT Control ON SCHEMA::dbo TO PJS 권한이 부여되었다. PJS로 로그인한다. CREATE TABLE MENU ( MENU_SEQ INT NOT NULL, TITLE VARCHAR(10) ); 테이블이 생성되었다.
```
#### [나. OBJECT 에 대한 권한 부여]
오브젝트 권한 : 특정 오브젝트인 테이블, 뷰에 대한 SELECT, INSERT, DELETE, UPDATE 작업 명령어를 의미.  
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_225.jpg)  

테이블과 같은 오브젝트는 유저가 소유하는게 아닌 스키마가 소유를 하게되므로, 유저는 스키마에 대한 권한을 가짐. 소유한 객체에 접근하려면 객체앞에 객체를 소유한 유저의 이름을 붙임. SQL 서버의 경우, 객체가 속한 스키마의 이름을 붙여야함. 
[예제] PJS 유저로 접속하여 SCOTT 유저에게 MENU 테이블을 SELECT 할 수 있는 권한을 부여한다.  
```
[예제 및 실행 결과] Oracle CONN PJS/KOREA7 연결되었다. INSERT INTO MENU VALUES (1, '화이팅'); 1개의 행이 만들어졌다. COMMIT; 커밋이 완료되었다. GRANT SELECT ON MENU TO SCOTT; 권한이 부여되었다.
```
```
[예제 및 실행 결과] SQL Server PJS로 로그인한다. INSERT INTO MENU VALUES (1, '화이팅'); 1개의 행이 만들어졌다. GRANT SELECT ON MENU TO SCOTT; 권한이 부여되었다.
```
### 3. Role 을 이용한 권한 부여
ROLE : 유저와 권한들 사이에서 중개 역할.      
ROLE 을 생성하고, ROLE 에 각종 권한들을 부여한 후, ROLE 을 다른 ROLE 혹은 유저에 부여 ROLE 을 만들어 사용하는 것이 권한을 직접 부여하는 것보다 빠르고 안전하게 관리 가능
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_226.jpg)  
ROLE 에는 시스템 권한과 오브젝트 권한을 모두 부여 가능. ROLE 은 유저에게 직접 부여될 수도 있고 다른 ROLE에 포함하여 유저에게 부여 할 수 있음.
[예제] JISUNG 유저에게 CREATE SESSION과 CREATE TABLE 권한을 가진 ROLE을 생성한 후 ROLE을 이용하여 다시 권한을 할당한다. 권한을 취소할 때는 REVOKE를 사용한다.  
```
[예제 및 실행 결과] Oracle CONN SYSTEM/MANAGER 연결되었다. REVOKE CREATE SESSION, CREATE TABLE FROM JISUNG; 권한이 취소되었다. CONN JISUNG/KOREA7 ERROR: 사용자 JISUNG은 CREATE SESSION 권한을 가지고 있지 않음. 로그온이 거절되었다.
```
```
[예제 및 실행 결과] SQL Server sa로 로그인한다. REVOKE CREATE TABLE FROM PJS; 권한이 취소되었다. PJS로 로그인한다. CREATE TABLE MENU ( MENU_SEQ INT NOT NULL, TITLE VARCHAR(10) ); 데이터베이스 ‘AdventureWorks'에서 CREATE TABLE사용 권한이 거부되었다.
```
[예제] 이제 LOGIN_TABLE이라는 ROLE을 만들고, 이 ROLE을 이용하여 JISUNG 유저에게 권한을 부여한다.
```
[예제 및 실행 결과] Oracle CONN SYSTEM/MANAGER 연결되었다. CREATE ROLE LOGIN_TABLE; 롤이 생성되었다. GRANT CREATE SESSION, CREATE TABLE TO LOGIN_TABLE; 권한이 부여되었다. GRANT LOGIN_TABLE TO JISUNG; 권한이 부여되었다. CONN JISUNG/KOREA7 연결되었다. CREATE TABLE MENU2( MENU_SEQ NUMBER NOT NULL, TITLE VARCHAR2(10)); 테이블이 생성되었다.
```
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_227.jpg)  
CONNECT 는 로그인 권한, RESOURCE 는 오브젝트 생성 권한이 포함됨.
유저를 삭제하는 명령어는 DROP USER 이고 CASCADE 옵션을 주면 해당 유저가 생성한 오브젝트를 먼저 삭제 한 후 유저를 삭제함.     
[예제] 앞에서 MENU라는 테이블을 생성했기 때문에 CASCADE 옵션을 사용하여 JISUNG 유저를 삭제한 후, 유저 재생성 및 기본적인 ROLE을 부여한다.
```
[예제 및 실행 결과] Oracle CONN SYSTEM/MANAGER 연결되었다. DROP USER JISUNG CASCADE; 사용자가 삭제되었다. ☞ JISUNG 유저가 만든 MENU 테이블도 같이 삭제되었다. CREATE USER JISUNG IDENTIFIED BY KOREA7; 사용자가 생성되었다. GRANT CONNECT, RESOURCE TO JISUNG; 권한이 부여되었다. CONN JISUNG/KOREA7 연결되었다. CREATE TABLE MENU ( MENU_SEQ NUMBER NOT NULL, TITLE VARCHAR2(10)); 테이블이 생성되었다.
```
#### 특정 로그인이 멤버로 참여할 수 있는 서버수준 역할
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_228.jpg)
#### 데이터베이스에 존재하는 유저에 대해서의 수준 역할명
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_229.jpg)

## 절차형 SQL 
절차형 SQL : 절차형 SQL 사용시 SQL 문의 연속적인 실행이나 조건에 따른 분기처리를 이용하여 특정 기능을 수행하는 저장모듈 생성 가능. 
### 1. PL/SQL 개요
PL/SQL : Block 구조, DML 문장과 QUERY 문장 그리고 절차형 언어 등을 사용 할 수 있으며, 절차적 프로그래밍을 가능하게 하는 트랜잭션 언어. 다양한 저장 모듈 개발 가능.사용자와 애플리케이션 사이에 공유할 수 있도록 만든 SQL 컴포넌트 프로그램.
#### PL/SQL특징 
- PL/SQL은 Block 구조로 되어있어 각 기능별로 모듈화가 가능하다.
 - 변수, 상수 등을 선언하여 SQL 문장 간 값을 교환한다.
  - IF, LOOP 등의 절차형 언어를 사용하여 절차적인 프로그램이 가능하도록 한다. 
  - DBMS 정의 에러나 사용자 정의 에러를 정의하여 사용할 수 있다. 
  - PL/SQL은 Oracle에 내장되어 있으므로 Oracle과 PL/SQL을 지원하는 어떤 서버로도 프로그램을 옮길 수 있다. 
  - PL/SQL은 응용 프로그램의 성능을 향상시킨다. - PL/SQL은 여러 SQL 문장을 Block으로 묶고 한 번에 Block 전부를 서버로 보내기 때문에 통신량을 줄일 수 있다
  ![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_230.jpg)
 -  프로그램 문장은 PL/SQL 엔진이 처리하고 SQL 문장은 Oracle 서버의 SQL Statement Executor가 실행하도록 작업을 분리하여 처리.
#### [나. PL/SQL 구조]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_231.jpg)
#### [다. PL/SQL 기본 문법]
```
CREATE [OR REPLACE] Procedure [Procedure_name] ( argument1 [mode] data_type1, argument2 [mode] date_type2, ... ... ) IS [AS] ... ... BEGIN ... ... EXCEPTION ... ... END; /
```
다음은 생성된 프로시저를 삭제하는 명령어이다.
```
DROP Procedure [Procedure_name];
```
### 2. T-SQL 개요
#### [가. T-SQL 특징]
T-SQL : 근본적으로 SQLserver 을 제어하기 위한 언어. 
#### 기능
- 변수 선언 기능 :  @@이라는 전역변수(시스템 함수)와 @이라는 지역변수가 있다. 
(지역변수는 사용자가 자신의 연결 시간 동안만 사용하기 위해 만들어지는 변수, 전역변수는 이미 SQL서버에 내장된 값) - 데이터 유형(Data Type)을 제공한다. (int, float, varchar 등의 자료형을 의미)
- 연산자(Operator) 산술연산자( +, -, *, /)와 비교연산자(=, <, >, <>) 논리연산자(and, or, not) 사용가능. 
- 흐름 제어 기능: IF-ELSE와 WHILE, CASE-THEN 사용가능하다. 
- 주석 기능한줄 주석 : -- 뒤의 내용은 주석범위 주석 : /* 내용 */ 형태를 사용하며, 여러 줄도 가능함
#### [나. T-SQL 구조]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_232.jpg)
#### [다. T-SQL 기본문법]
```
CREATE Procedure [schema_name.]Procedure_name @parameter1 data_type1 [mode], @parameter2 date_type2 [mode], ... ... WITH AS ... ... BEGIN ... ... ERROR 처리 ... ... END;
```
프로시저를 삭제하는 명령어
```
DROP Procedure [schema_name.]Procedure_name;
```
프로시저의 변경이 필요할 경우 Oracle은 [CREATE OR REPLACE]와 같이 하나의 구문으로 처리,  SQL Server는 CREATE 구문을 ALTER 구문으로 변경.   

@parameter는 프로시저가 호출될 때 프로시저 안으로 어떤 값이 들어오거나 혹은 프로시저에서 처리한 결과 값을 리턴 시킬 매개 변수를 지정시 사용. 

#### [mode] 부분에 지정할 수 있는 매개 변수(@parameter)의 유형
1. VARYING결과 집합이 출력 매개 변수로 사용되도록 지정. CURSOR 매개변수에만 적용.
2. DEFAULT지정된 매개변수가 프로시저를 호출할 당시 지정되지 않을 경우 지정된 기본값으로 처리.
3.  OUT, OUTPUT프로시저에서 처리된 결과 값을 EXECUTE 문 호출 시 반환. 
4. READONLY자주 사용X. 프로시저 본문 내에서 매개 변수를 업데이트하거나 수정할 수 없음을 나타냄. 매개 변수 유형이 사용자 정의 테이블 형식인 경우 READONLY를 지정.
#### WITH 부분에 지정할수 있는 옵션
1. . 데이터베이스 엔진에서 저장 프로시저 안에 있는 개별 쿼리에 대한 계획을 삭제하려 할 때 RECOMPILE 쿼리 힌트를 사용한다. 
2.  ENCRYPTIONCREATE PROCEDURE 문의 원본 텍스트가 알아보기 어려운 형식으로 변환된다. 원본을 볼 수 있는 방법이 없기 때문에 반드시 원본은 백업을 해두어야 한다. 
3.  EXECUTE AS 해당 저장 프로시저를 실행할 보안 컨텍스트를 지정한다.
### 3. Procedure 의 생성과 활용
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_233.jpg)
#### DEPT 테이블 구조
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_234.jpg)
```
[예제] Oracle CREATE OR REPLACE Procedure p_DEPT_insert -------------①  
( v_DEPTNO in number, v_dname in varchar2, v_loc in varchar2, v_result out varchar2) IS cnt number := 0; BEGIN SELECT COUNT(*) INTO CNT -------------②  
FROM DEPT WHERE DEPTNO = v_DEPTNO AND ROWNUM = 1; if cnt > 0 then -------------③  
v_result := '이미 등록된 부서번호이다'; else INSERT INTO DEPT (DEPTNO, DNAME, LOC) -------------④  
VALUES (v_DEPTNO, v_dname, v_loc); COMMIT; -------------⑤   
v_result := '입력 완료!!'; end if; EXCEPTION -------------⑥   
WHEN OTHERS THEN ROLLBACK; v_result := 'ERROR 발생'; END; /
```
```
[예제] SQL Server CREATE Procedure dbo.p_DEPT_insert -------------①  
@v_DEPTNO int, @v_dname varchar(30), @v_loc varchar(30), @v_result varchar(100) OUTPUT AS DECLARE @cnt int SET @cnt = 0 BEGIN SELECT @cnt=COUNT(*) -------------②  
FROM DEPT WHERE DEPTNO = @v_DEPTNO IF @cnt > 0 -------------③  
BEGIN SET @v_result = '이미 등록된 부서번호이다' RETURN END ELSE BEGIN BEGIN TRAN INSERT INTO DEPT (DEPTNO, DNAME, LOC) -------------④   VALUES (@v_DEPTNO, @v_dname, @v_loc) IF @@ERROR<>0 BEGIN ROLLBACK -------------⑥  
SET @v_result = 'ERROR 발생' RETURN END ELSE BEGIN COMMIT -------------⑤  
SET @v_result = '입력 완료!!' RETURN END END END
```
① DEPT 테이블에 들어갈 칼럼 값(부서코드, 부서명, 위치)을 입력 받는다. 
② 입력 받은 부서코드가 존재하는지 확인한다. 
③ 부서코드가 존재하면 '이미 등록된 부서번호입니다'라는 메시지를 출력 값에 넣는다. 
④ 부서코드가 존재하지 않으면 입력받은 필드 값으로 새로운 부서 레코드를 입력한다. 
⑤ 새로운 부서가 정상적으로 입력됐을 경우에는 COMMIT 명령어를 통해서 트랜잭션을 종료한다.
 ⑥ 에러가 발생하면 모든 트랜잭션을 취소하고 'ERROR 발생'라는 메시지를 출력값에 넣는다.
#### 프로시저를 작성하면서 주의해야할 문법적요소
1. SCALAR 변수는 사용자의 임시 데이터를 하나만 저장 가능한 변수. 거의 모든 형태의 데이터 유형 지정 가능
2. PL/SQL에서 사용하는 SELECT 문장은 반드시 결과값이 존재. 그결과 역시 반드시 하나여야함. 
3. PL/SQL에서는 대입 연산자를 ":=" 사용. 
4. 에러처리 담당 EXCEPTION 에는 WHEN~THEN 절 사용하여 에러를 종류별로 처리 
#### Oracle
```
[실행 결과] Oracle SQL> SELECT * FROM DEPT; -----------------① DEPTNO DNAME LOC ------- ------- --------- 10 ACCOUNTING NEW YORK 20 RESEARCH DALLAS 30 SALES CHICAGO 40 OPERATIONS BOSTON SQL> variable rslt varchar2(30); -----------------② SQL> EXECUTE p_DEPT_insert(10,'dev','seoul',:rslt); -----------------③ PL/SQL 처리가 정상적으로 완료되었다. SQL> print rslt; -----------------④ RSLT -------------------------------- 이미 등록된 부서번호이다 SQL> EXECUTE p_DEPT_insert(50,'NewDev','seoul',:rslt); ----------------⑤ PL/SQL 처리가 정상적으로 완료되었다. SQL> print rslt; ----------------⑥ RSLT -------------------------------- 입력 완료!! SQL> SELECT * FROM DEPT; ----------------⑦ DEPTNO DNAME LOC ------ -------- --------- 10 ACCOUNTING NEW YORK 20 RESEARCH DALLAS 30 SALES CHICAGO 40 OPERATIONS BOSTON 50 NewDev SEOUL 5개의 행이 선택되었다.
```
① DEPT 테이블을 조회하면 총 4개 행의 결과가 출력된다. ② Procedure를 실행한 결과 값을 받을 변수를 선언한다. (BIND 변수) ③ 존재하는 DEPTNO(10)를 가지고 Procedure를 실행한다. ④ DEPTNO가 10인 부서는 이미 존재하기 때문에 변수 rslt를 print해 보면 '이미 등록된 부서번호이다' 라고 출력된다. ⑤ 이번에는 새로운 DEPTNO(50)를 가지고 입력한다. ⑥ rslt를 출력해 보면 '입력 완료!!' 라고 출력된다. ⑦ DEPT 테이블을 조회하여 보면 DEPTNO가 50인 데이터가 정확하게 저장되었음을 확인할 수 있다
#### sql server
```
[실행 결과] SQL Server SELECT * FROM DEPT; -----------------① DEPTNO DNAME LOC ------- ---------- --------- 10 ACCOUNTING NEW YORK 20 RESEARCH DALLAS 30 SALES CHICAGO 40 OPERATIONS BOSTON DECALRE @v_result VARCHAR(100) -----------------② EXECUTE dbo.p_DEPT_insert 10, 'dev', 'seoul', @v_result=@v_result OUTPUT -----------------③ SELECT @v_result AS RSLT -----------------④ RSLT -------------------------------- 이미 등록된 부서번호이다 DECALRE @v_result VARCHAR(100) -----------------⑤ EXECUTE dbo.p_DEPT_insert 50, 'dev', 'seoul', @v_result=@v_result OUTPUT -----------------⑥ SELECT @v_result AS RSLT -----------------⑦ RSLT -------------------------------- 입력 완료! SELECT * FROM DEPT; ----------------⑧ DEPTNO DNAME LOC ------- -------- --------- 10 ACCOUNTING NEW YORK 20 RESEARCH DALLAS 30 SALES CHICAGO 40 OPERATIONS BOSTON 50 NewDev SEOUL 5개의 행에서 선택되었다.
```
① DEPT 테이블을 조회하면 총 4개 행의 결과가 출력된다. ② Procedure를 실행한 결과 값을 받을 변수를 선언한다. ③ 존재하는 DEPTNO(10)를 가지고 Procedure를 실행한다. ④ DEPTNO가 10인 부서는 이미 존재하기 때문에 변수 rslt를 print해 보면 ‘이미 등록된 부서번호이다’라고 출력된다. ⑤ Procedure를 실행한 결과 값을 받을 변수를 선언한다. ⑥ 이번에는 새로운 DEPTNO(50)를 가지고 입력한다. ⑦ rslt를 출력해 보면 ‘입력 완료!’라고 출력된다. ⑧ DEPT 테이블을 조회하여 보면 DEPTNO가 50인 데이터가 정확하게 저장되었음을 확인할 수 있다.

### 4. User Defined Function 의 생성과 활용
User Defined Function  : 데이터베이스내에 저장해놓은 명령문 집합을 의미.   
[예제]에서 사용한 ABS 함수를 만드는데, INPUT 값으로 숫자만 들어온다고 가정한다.
```
[예제] Oracle CREATE OR REPLACE Function UTIL_ABS (v_input in number) ---------------- ①   
return NUMBER IS v_return number := 0; ----------------②  
BEGIN if v_input < 0 then ---------------- ③   
v_return := v_input * -1; else v_return := v_input; end if; RETURN v_return; ---------------- ④   
END; /
```
```
[예제] SQL Server CREATE Function dbo.UTIL_ABS (@v_input int) ---------------- ①     
RETURNS int AS BEGIN DECLARE @v_return int ---------------- ② SET @v_return=0 IF @v_input < 0 ---------------- ③   
SET @v_return = @v_input * -1 ELSE SET @v_return = @v_input RETURN @v_return; ---------------- ④
END
```
[예제]에서 생성한 UTIL_ABS Function의 처리 과정은 다음과 같다.

① 숫자 값을 입력 받는다. 예제에서는 숫자 값만 입력된다고 가정한다. ② 리턴 값을 받아 줄 변수인 v_return를 선언한다. ③ 입력 값이 음수이면 -1을 곱하여 v_return 변수에 대입한다. ④ v_return 변수를 리턴한다.
### 5. Trigger 의 생성과 활용
Trigger : 특정한 테이블에 INSERT, UPDATE, DELETE와 같은 DML문이 수행되었을 때, 데이터베이스에서 자동으로 동작하도록 작성된 프로그램. (데이터베이스 자동 수행) 
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_235.jpg)

Trigger의 역할은 ORDER_LIST에 주문 정보가 입력되면 주문 정보의 주문 일자(ORDER_LIST.ORDER_DATE)와 주문 상품(ORDER_LIST.PRODUCT)을 기준으로 판매 집계 테이블(SALES_PER_DATE)에 해당 주문 일자의 주문 상품 레코드가 존재하면 판매 수량과 판매 금액을 더하고 존재하지 않으면 새로운 레코드를 입력한다.
```
[예제] Oracle CREATE OR REPLACE Trigger SUMMARY_SALES ---------------- ①  
AFTER INSERT ON ORDER_LIST FOR EACH ROW DECLARE ---------------- ②  
o_date ORDER_LIST.order_date%TYPE; o_prod ORDER_LIST.product%TYPE; BEGIN o_date := :NEW.order_date; o_prod := :NEW.product; UPDATE SALES_PER_DATE ---------------- ③  
SET qty = qty + :NEW.qty, amount = amount + :NEW.amount WHERE sale_date = o_date AND product = o_prod; if SQL%NOTFOUND then ---------------- ④   
INSERT INTO SALES_PER_DATE VALUES(o_date, o_prod, :NEW.qty, :NEW.amount); end if; END; /
```
① Trigger를 선언한다.CREATE OR REPLACE Trigger SUMMARY_SALES : Trigger 선언문AFTER INSERT : 레코드가 입력이 된 후 Trigger 발생 ON ORDER_LIST : ORDER_LIST 테이블에 Trigger 설정FOR EACH ROW : 각 ROW마다 Trigger 적용 ② o_date(주문일자), o_prod(주문상품) 값을 저장할 변수를 선언하고, 신규로 입력된 데이터를 저장한다. : NEW는 신규로 입력된 레코드의 정보를 가지고 있는 구조체 : OLD는 수정, 삭제되기 전의 레코드를 가지고 있는 구조체  ③ 먼저 입력된 주문 내역의 주문 일자와 주문 상품을 기준으로 SALES_PER_DATE 테이블에 업데이트한다. ④처리 결과가 SQL%NOTFOUND이면 해당 주문 일자의 주문 상품 실적이 존재하지 않으며, SALES_ PER_DATE 테이블에 새로운 집계 데이터를 입력한다.
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_236.jpg)
```
[예제] SQL Server CREATE Trigger dbo.SUMMARY_SALES ---------------- ①  
ON ORDER_LIST AFTER INSERT AS DECLARE @o_date DATETIME,@o_prod INT,@qty int, @amount int BEGIN SELECT @o_date=order_date, @o_prod=product, @qty=qty, @amount=amount FROM inserted ---------------- ②  
UPDATE SALES_PER_DATE ---------------- ③  
SET qty = qty + @qty, amount = amount + @amount WHERE sale_date = @o_date AND product = @o_prod; IF @@ROWCOUNT=0 ---------------- ④    INSERT INTO SALES_PER_DATE VALUES(@o_date, @o_prod, @qty, @amount) END
```
① Trigger를 선언한다.CREATE Trigger SUMMARY_SALES : Trigger 선언문ON ORDER_LIST : ORDER_LIST 테이블에 Trigger 설정AFTER INSERT : 레코드가 입력이 된 후 Trigger 발생 ② o_date(주문일자), o_prod(주문상품), qty(수량), amount(금액) 값을 저장할 변수를 선언하고, 신규로 입력된 데이터를 저장한다.inserted는 신규로 입력된 레코드의 정보를 가지고 있는 구조체deleted는 수정, 삭제되기 전의 레코드를 가지고 있는 구조체. (표참조) ③ 먼저 입력된 주문 내역의 주문 일자와 주문 상품을 기준으로 SALES_PER_DATE 테이블에 업데이트한다. ④ 처리 결과가 0건이면 해당 주문 일자의 주문 상품 실적이 존재하지 않으며, SALES_PER_DATE 테이블에 새로운 집계 데이터를 입력한다.
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_237.jpg)
### 6. 프로시저와 트리거의 차이점
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_238.jpg)

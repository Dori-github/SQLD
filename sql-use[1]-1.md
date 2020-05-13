# 3-1.SQL 기본 및 활용-SQL활용(1)
## 표준조인 
### 1. STANDARD SQL 개요
#### ANSI/ISO SQL의 JOIN
- STANDARD JOIN 기능 : CROSS, OUTER JOIN 등 새로운 FROM 절 JOIN 기능들
- SCALAR SUBQUERY, TOP-N QUERY 등의 새로운 SUBQUERY 기능들 
- ROLLUP, CUBE, GROUPING SETS 등의 새로운 리포팅 기능
- WINDOW FUNCTION 같은 새로운 개념의 분석 기능들
#### [가. 일반 집합 연산자]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_200.jpg)
1. UNION 연산은 UNION 기능으로  
→ 수학적 합집합을 제공하기 위해, 교집합의 중복을 없애기 위한 사전 작업.  
→ 공통집합을 중복해서 그대로 보여줌으로 정렬 작업이 일어나지 않음(UNION ALL).
2. INTERSECTION 연산은 INTERSECT 기능으로  
→INTERSECTION은 수학의 교집합으로 두 집합의 공통집합을 추출.
3. DIFFERENCE 연산은 EXPECT 기능으로  
→ DIFFRENCE은 수학의 차집합으로 첫번째 집합에서 두번째 집합과 공통집합을 제외한 부분. 
4. PRODUCT 연산은 CROSS JOIN 기능으로 구현   
→ PRODUCT는 CROSS PRODUCT 라고 불리는 곱집합으로, JOIN 이 없는 경우 생길 수 있는 모든 데이터 조합.   
→ 양쪽 집합의 M*N 건의 데이터 조합이 발생.
#### [나. 순수 관계 연산자]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_201.jpg)  
5. SELECT 연산은 WHERE 절로 구현한다.  
→ SELECT 연산은 SQL 문장에서 WHERE절의 조건 기능으로 구현.
6. PROJECT 연산은 SELECT 절로 구현되었다.  
→ PROJECT 연산은 SQL 문장에서 SELECT 절의 칼럼 선택 기능으로 구현.
7. JOIN 연산은 다양한 JOIN 기능으로 구현되었다.  
→ JOIN 연산은 WHERE 조건의 INNER JOIN 조건과 하께 FROM 절의 NATURAL JOIN, INEER JOIN, OUTER JOIN, USING 조건절, ON 조건절 등으로 발전. 
8. DIVIDE 연산은 현재 사용되지 않는다.  
→ DIVIDE  연산은 나눗셈과 비슷한 개념.
#### 관계형 데이터베이스 과정 
```
요구사항 분석→ 개념적 데이터 모델링 → 논리적 데이터 모델링 → 물리적 데이터 모델링(엔터티 확정 및 정규화, 다대다 관계 분해) 
```
여기서 정규화 과정을 거치면서 흩어진 데이터를 연결해서 원하는 데이터를 가져오는 작업→ "JOIN" 
### 2. FROM 절 JOIN 형태
#### ANSI/ISO SQL FROM JOIN 형태
- INNER JOIN
- NATURAL JOIN
- USING 조건절
- ON 조건절
- CROSS JOIN
- OUTER JOIN
#### ANSI/ISO JOIN vs WHERE JOIN
사용자는 기존 WHERE 절의 검색 조건과 테이블 간 JOIN 조건을 구분없이 사용, ANSI/ISO JOIN 은 추가된 기능으로 테이블간 JOIN 조건을 FROM 절에서 명시적으로 정의 할 수 있게됨.
### 3. INNER JOIN
INNER JOIN :  내부 JOIN 이라고 하며, JOIN 조건에서는 동일한 값이 있는 행만 반환, WHERE 절에서 사용하던 JOIN 조건을 FROM 절에서 정의하겠다는 의미, (USING 조건절, ON 조건절 필수적 사용)

[예제] 사원 번호와 사원 이름, 소속부서 코드와 소속부서 이름을 찾아본다.
```
[예제] WHERE 절 JOIN 조건 SELECT EMP.DEPTNO, EMPNO, ENAME, DNAME FROM EMP, DEPT WHERE EMP.DEPTNO = DEPT.DEPTNO; 위 SQL과 아래 SQL은 같은 결과를 얻을 수 있다. FROM 절 JOIN 조건 SELECT EMP.DEPTNO, EMPNO, ENAME, DNAME FROM EMP INNER JOIN DEPT ON EMP.DEPTNO = DEPT.DEPTNO; INNER는 JOIN의 디폴트 옵션으로 아래 SQL문과 같이 생략 가능하다. SELECT EMP.DEPTNO, EMPNO, ENAME, DNAME FROM EMP JOIN DEPT ON EMP.DEPTNO = DEPT.DEPTNO;
```
```
[실행 결과] DEPTNO EMPNO ENAME DNAME ------ ----- ------ --------- 20 7369 SMITH RESEARCH 30 7499 ALLEN SALES 30 7521 WARD SALES 20 7566 JONES RESEARCH 30 7654 MARTIN SALES 30 7698 BLAKE SALES 10 7782 CLARK ACCOUNTING 20 7788 SCOTT RESEARCH 10 7839 KING ACCOUNTING 30 7844 TURNER SALES 20 7876 ADAMS RESEARCH 30 7900 JAMES SALES 20 7902 FORD RESEARCH 10 7934 MILLER ACCOUNTING 14개의 행이 선택되었다.
```
### 4. NATURAL JOIN
NATURAL JOIN : 두 테이블간의 동일한 이름을 갖는 모든 칼럼들에 대해 , EQUI(=)JOIN 수행( USING 조건절, ON 조건절, WHERE 절에서 JOIN 조건 정의X , SQL server 지원하지 않는 기능)  

[예제] 사원 번호와 사원 이름, 소속부서 코드와 소속부서 이름을 찾아본다.
```
[예제] SELECT DEPTNO, EMPNO, ENAME, DNAME FROM EMP NATURAL JOIN DEPT;
```
```
[실행 결과] DEPTNO EMPNO ENAME DNAME ------ ------ ------ ------ 20 7369 SMITH RESEARCH 30 7499 ALLEN SALES 30 7521 WARD SALES 20 7566 JONES RESEARCH 30 7654 MARTIN SALES 30 7698 BLAKE SALES 10 7782 CLARK ACCOUNTING 20 7788 SCOTT RESEARCH 10 7839 KING ACCOUNTING 30 7844 TURNER SALES 20 7876 ADAMS RESEARCH 30 7900 JAMES SALES 20 7902 FORD RESEARCH 10 7934 MILLER ACCOUNTING 14개의 행이 선택되었다.
```
위 테이블은 DEPTNO 한 것. 
JOIN 에 사용된 칼럼들은 같은 데이터 유형이어야 하며, ALIAS나 테이블과 같은 접두사를 붙일 수 없다.

```
[예제] SELECT EMP.DEPTNO, EMPNO, ENAME, DNAME FROM EMP NATURAL JOIN DEPT; ERROR: NATURAL JOIN에 사용된 열은 식별자를 가질 수 없음
```
NATURAL JOIN은 JOIN에 사용된 같은 이름의 칼럼을 하나로 처리, INNER JOIN 은 2개의 칼럼으로 처리
### 5. USING 조건절
USING 조건절 : FROM 절에 USING 조건 절을 이용하면 같은 이름을 가진 칼럼들 중에서 원하는 칼럼에 대해서만 EQUI JOIN을 할 수 있음.(SQL server 지원X)

[예제] 세 개의 칼럼명이 모두 같은 DEPT와 DEPT_TEMP 테이블을 DEPTNO 칼럼을 이용한 [INNER] JOIN의 USING 조건절로 수행한다.
```
[예제] SELECT * FROM DEPT JOIN DEPT_TEMP USING (DEPTNO);
```
```
[실행 결과] DEPTNO DNAME LOC DNAME LOC ------ ---------- --------- ---------- --------- 10 ACCOUNTING NEW YORK ACCOUNTING NEW YORK 20 RESEARCH DALLAS R&D DALLAS 30 SALES CHICAGO MARKETING CHICAGO 40 OPERATIONS BOSTON OPERATIONS BOSTON 4개의 행이 선택되었다.
```
위 SQL의 '*'와일드 카드 처럼 별도의 칼럼 순서를 지정하지 않으면, USING 조건절의 기준이 되는 칼럼보다 먼저 출력됨. (ex. DEPTNO 가 첫번째 칼럼), USING JOIN 은 JOIN 에 사용된 같은 이름의 칼럼을 하나로 처리.

[예제] USING 조건절을 이용한 EQUI JOIN에서도 NATURAL JOIN과 마찬가지로 JOIN 칼럼에 대해서는 ALIAS나 테이블 이름과 같은 접두사를 붙일 수 없다. (DEPT.DEPTNO → DEPTNO)
```
[예제] 잘못된 사례: SELECT DEPT.DEPTNO, DEPT.DNAME, DEPT.LOC, DEPT_TEMP.DNAME, DEPT_TEMP.LOC FROM DEPT JOIN DEPT_TEMP USING (DEPTNO); ERROR: USING 절의 열 부분은 식별자를 가질 수 없음 바른 사례: SELECT DEPTNO, DEPT.DNAME, DEPT.LOC, DEPT_TEMP.DNAME, DEPT_TEMP.LOC FROM DEPT JOIN DEPT_TEMP USING (DEPTNO);
```
### 6. ON 조건절
ON 조건절 : JOIN 서술부(ON조건절)와 비 JOIN 서술부(WHERE 조건절)을 분리하여 이해가 쉬우며, 칼럼명이 다르더라도 JOIN조건 사용 가능.

[예제] 사원 테이블과 부서 테이블에서 사원 번호와 사원 이름, 소속부서 코드, 소속부서 이름을 출력한다.
```
[예제] SELECT E.EMPNO, E.ENAME, E.DEPTNO, D.DNAME FROM EMP E JOIN DEPT D ON (E.DEPTNO = D.DEPTNO);
```
두 테이블 간 특정 칼럼으로 Equal Join 수행,  칼럼명 다르더라도 JOIN 조건 사용 할 수 있는 장점 가짐, 임의로 JOIN 지지정 가능,JOIN 칼럼을 명시할 때 사용  

※ JOIN 에서는 JOIN 칼럼에 대해서 접두사를 사용하면 에러 발생, ON 조건절을 사용한 JOIN 칼럼에서는 접두사를 사용해야함. 

#### [가. WHERE 절과의 혼용]
[예제] ON 조건절과 WHERE 검색 조건은 충돌 없이 사용할 수 있다. 부서코드 30인 부서의 소속 사원 이름 및 소속 부서 코드, 부서 코드, 부서 이름을 찾아본다.
```
[예제] SELECT E.ENAME, E.DEPTNO, D.DEPTNO, D.DNAME FROM EMP E JOIN DEPT D ON (E.DEPTNO = D.DEPTNO) WHERE E.DEPTNO = 30;
```
```
[실행 결과] ENAME DEPTNO DEPTNO DNAME ------- ------ ------ ------ ALLEN 30 30 SALES WARD 30 30 SALES MARTIN 30 30 SALES BLAKE 30 30 SALES TURNER 30 30 SALES JAMES 30 30 SALES 6개의 행이 선택되었다
```
#### [나. ON 조건절+ 데이터 검증 조건 추가]
ON 조건절에 JOIN 조건 외에도 데이터 검색 조건 추가 가능, BUT 검색 조건 목적인 경우 WHERE 사용 권고.

[예제] 매니저 사원번호가 7698번인 사원들의 이름 및 소속 부서 코드, 부서 이름을 찾아본다.
```
[예제] SELECT E.ENAME, E.MGR, D.DEPTNO, D.DNAME FROM EMP E JOIN DEPT D ON (E.DEPTNO = D.DEPTNO AND E.MGR = 7698); 위 SQL과 아래 SQL은 같은 결과를 얻을 수 있다. SELECT E.ENAME, E.MGR, D.DEPTNO, D.DNAME FROM EMP E JOIN DEPT D ON (E.DEPTNO = D.DEPTNO) WHERE E.MGR = 7698;
```
```
[실행 결과] ENAME MGR DEPTNO DNAME ------- ---- ------ ------ ALLEN 7698 30 SALES WARD 7698 30 SALES MARTIN 7698 30 SALES TURNER 7698 30 SALES JAMES 7698 30 SALES 5개의 행이 선택되었다.
```
#### [다. ON 조건절 예제]
[예제] 팀과 스타디움 테이블을 스타디움ID로 JOIN하여 팀이름, 스타디움ID, 스타디움 이름을 찾아본다.
```
[예제] SELECT TEAM_NAME, TEAM.STADIUM_ID, STADIUM_NAME FROM TEAM JOIN STADIUM ON TEAM.STADIUM_ID = STADIUM.STADIUM_ID ORDER BY STADIUM_ID;; 위 SQL은 STADIUM_ID라는 공통된 칼럼이 있기 때문에 아래처럼 USING 조건절로 구현할 수도 있다. SELECT TEAM_NAME, STADIUM_ID, STADIUM_NAME FROM TEAM JOIN STADIUM USING (STADIUM_ID) ORDER BY STADIUM_ID; 위 SQL은 고전적인 방식인 WHERE 절의 INNER JOIN으로 구현할 수도 있다. SELECT TEAM_NAME, TEAM.STADIUM_ID, STADIUM_NAME FROM TEAM, STADIUM WHERE TEAM.STADIUM_ID = STADIUM.STADIUM_ID ORDER BY STADIUM_ID
```
```
[실행 결과] TEAM_NAME STADIUM_ID STADIUM_NAME ------------- --------- ------------- 광주상무 A02 광주월드컵경기장 강원FC A03 강릉종합경기장 제주유나이티드FC A04 제주월드컵경기장 대구FC A05 대구월드컵경기장 유나이티드 B01 인천월드컵경기장 일화천마 B02 성남종합운동장 삼성블루윙즈 B04 수원월드컵경기장 FC서울 B05 서울월드컵경기장 아이파크 C02 부산아시아드경기장 울산현대 C04 울산문수경기장 경남FC C05 창원종합운동장 스틸러스 C06 포항스틸야드 드래곤즈 D01 광양전용경기장 시티즌 D02 대전월드컵경기장 15개의 행이 선택되었다.
```
#### [라. 다중테이블 JOIN]
[예제] 홈팀이 3점 이상 차이로 승리한 경기의 경기장 이름, 경기 일정, 홈팀 이름과 원정팀 이름 정보를 출력한다.
```
[예제] SELECT ST.STADIUM_NAME, SC.STADIUM_ID, SCHE_DATE, HT.TEAM_NAME, AT.TEAM_NAME, HOME_SCORE, AWAY_SCORE FROM SCHEDULE SC JOIN STADIUM ST ON SC.STADIUM_ID = ST.STADIUM_ID JOIN TEAM HT ON SC.HOMETEAM_ID = HT.TEAM_ID JOIN TEAM AT ON SC.AWAYTEAM_ID = AT.TEAM_ID WHERE HOME_SCORE > = AWAY_SCORE +3; 위 SQL은 고전적인 방식인 WHERE 절의 INNER JOIN으로 구현할 수도 있다. SELECT ST.STADIUM_NAME, SC.STADIUM_ID, SCHE_DATE, HT.TEAM_NAME, AT.TEAM_NAME, HOME_SCORE, AWAY_SCORE FROM SCHEDULE SC, STADIUM ST, TEAM HT, TEAM AT WHERE HOME_SCORE> = AWAY_SCORE +3 AND SC.STADIUM_ID = ST.STADIUM_ID AND SC.HOMETEAM_ID = HT.TEAM_ID AND SC.AWAYTEAM_ID = AT.TEAM_ID; FROM 절에 4개의 테이블이 JOIN에 참여하였으며, HOME TEAM과 AWAY TEAM의 팀 이름을 구하기 위해 TEAM 테이블을 HT와 AT 두 개의 ALIAS로 구분하였다.
```
```
[실행 결과] STADIUM_NAME STADIUM_ID SCHE_DATE TEAM_NAME TEAM_NAME HOME_SCORE AWAY_SCORE ------------ --------- -------- --------- --------- --------- --------- 서울월드컵경기장 B05 20120714 FC서울 삼성블루윙즈 3 0 부산아시아드경기장 C02 20120727 아이파크 시티즌 3 0 울산문수경기장 C04 20120803 울산현대 스틸러스 3 0 성남종합운동장 B02 20120317 일화천마 유나이티드 6 0 창원종합운동장 C05 20120427 경남FC 아이파크 5 2 5개의 행이 선택되었다
```
### 7. CROSS JOIN
CROSS JOIN : 일반 집합 연산자의 PRODUCT 개념으로 테이블 간 JOIN 조건이 없는 경우 생길 수 있는 모든 데이터의 조합. 두개 테이블의 결과는 양쪽 집합의 M*N 건의 데이터 조합이 발생. 
[예제] 사원 번호와 사원 이름, 소속부서 코드와 소속부서 이름을 찾아본다.
```
[예제] SELECT ENAME, DNAME FROM EMP CROSS JOIN DEPT ORDER BY ENAME;
```
### 8. OUTER JOIN
OUTER JOIN : JOIN 조건에서 동일한 값이 없는 행도 반환할 때 사용 할 수 있다.
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_202.jpg)
#### [가. LEFT OUTER JOIN]
LEFT OUTER JOIN : 조인 수행시 먼저 표기된 좌측 테이블에 해당하는 데이터를 먼저 읽은 후, 나중 표기된 우측 테이블에서 JOIN 대상 데이터를 읽어온다. 

[예제] STADIUM에 등록된 운동장 중에는 홈팀이 없는 경기장도 있다. STADIUM과 TEAM을 JOIN 하되 홈팀이 없는 경기장의 정보도 같이 출력하도록 한다.
```
[예제] SELECT STADIUM_NAME, STADIUM.STADIUM_ID, SEAT_COUNT, HOMETEAM_ID, TEAM_NAME FROM STADIUM LEFT OUTER JOIN TEAM ON STADIUM.HOMETEAM_ID = TEAM.TEAM_ID ORDER BY HOMETEAM_ID; OUTER는 생략 가능한 키워드이므로 아래 SQL은 같은 결과를 얻을 수 있다. SELECT STADIUM_NAME, STADIUM.STADIUM_ID, SEAT_COUNT, HOMETEAM_ID, TEAM_NAME FROM STADIUM LEFT JOIN TEAM ON STADIUM.HOMETEAM_ID = TEAM.TEAM_ID ORDER BY HOMETEAM_ID;
```
```
[실행 결과] STADIUM_NAME STADIUM_ID SEAT_COUNT HOMETEAM_ID TEAM_NAME ------------ --------- ---------- ----------- ---------- 울산문수경기장 C04 46102 K01 울산현대 수원월드컵경기장 B04 50000 K02 삼성블루윙즈 포항스틸야드 C06 25000 K03 스틸러스 인천월드컵경기장 B01 35000 K04 유나이티드 전주월드컵경기장 D03 28000 K05 현대모터스 부산아시아드경기장 C02 30000 K06 아이파크 광양전용경기장 D01 20009 K07 드래곤즈 성남종합운동장 B02 27000 K08 일화천마 서울월드컵경기장 B05 66806 K09 FC서울 대전월드컵경기장 D02 41000 K10 시티즌 창원종합운동장 C05 27085 K11 경남FC 광주월드컵경기장 A02 40245 K12 광주상무 강릉종합경기장 A03 33000 K13 강원FC 제주월드컵경기장 A04 42256 K14 제주유나이티드FC 대구월드컵경기장 A05 66422 K15 대구FC 안양경기장 F05 20000 마산경기장 F04 20000 일산경기장 F03 20000 부산시민경기장 F02 30000 대구시민경기장 F01 30000 20개의 행이 선택되었다
```
INNER JOIN 이라면 홈팀이 배정된 15개의 경기장만 출력 되었겠지만, LEFT OUTER JOIN 을 사용하였으므로, 홈팀이 없는 대구시민경기장, 부산시민경기장, 일산경기장, 마산경기장, 안양경기장 정보까지 추가로 출력됨.

#### [나. RIGHT OUTER JOIN]
RIGHT OUTER JOIN : 조인수행시 LEFT JOIN 과 반대로 우측 테이블이 기준이 되어 결과를 생성. 

[예제] DEPT에 등록된 부서 중에는 사원이 없는 부서도 있다. DEPT와 EMP를 조인하되 사원이 없는 부서 정보도 같이 출력하도록 한다.
```
[예제] SELECT E.ENAME, D.DEPTNO, D.DNAME FROM EMP E RIGHT OUTER JOIN DEPT D ON E.DEPTNO = D.DEPTNO; OUTER는 생략 가능한 키워드이므로 아래 SQL은 같은 결과를 얻을 수 있다. SELECT E.ENAME, D.DEPTNO, D.DNAME, D.LOC FROM EMP E RIGHT JOIN DEPT D ON E.DEPTNO = D.DEPTNO;
```
#### [다. FULL OUTER JOIN]
FULL OUTER JOIN : 조인 수행시 좌측, 우측 테이블의 모든 데이터를 읽어 JOIN 하여 결과를 생성한다.

[예제] DEPT 테이블과 DEPT_TEMP 테이블의 FULL OUTER JOIN 사례를 만들기 위해 DEPT_TEMP의 DEPTNO를 수정한다. 결과적으로 DEPT_TEMP 테이블의 새로운 DEPTNO 데이터는 DETP 테이블의 DEPTNO와 2건은 동일하고 2건은 새로운 DEPTNO가 생성된다.
```
[예제] UPDATE DEPT_TEMP SET DEPTNO = DEPTNO + 20; SELECT * FROM DEPT_TEMP;
```
```
[실행 결과] DEPTNO DNAME LOC ------ ---------- ---------- 30 ACCOUNTING NEW YORK 40 R&D DALLAS 50 MARKETING CHICAGO 60 OPERATIONS BOSTON 4개의 행이 선택되었다.
```
### 9. INNER vs OUTER vs CROSS JOIN 비교
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_203.jpg)
- INNER JOIN의 결과는 다음과 같다. 양쪽 테이블에 모두 존재하는 키 값이 B-B, C-C 인 2건이 출력된다. 
- LEFT OUTER JOIN의 결과는 다음과 같다. TAB1을 기준으로 키 값 조합이 B-B, C-C, D-NULL, E-NULL 인 4건이 출력된다. 
-  RIGHT OUTER JOIN의 결과는 다음과 같다. TAB2를 기준으로 키 값 조합이 NULL-A, B-B, C-C 인 3건이 출력된다. 
-  FULL OUTER JOIN의 결과는 다음과 같다. 양쪽 테이블을 기준으로 키 값 조합이 NULL-A, B-B, C-C, D-NULL, E-NULL 인 5건이 출력된다. 
-  CROSS JOIN의 결과는 다음과 같다. JOIN 가능한 모든 경우의 수를 표시하지만 단, OUTER JOIN은 제외한다. 
- 양쪽 테이블 TAB1과 TAB2의 데이터를 곱한 개수인 4 * 3 = 12건이 추출됨 키 값 조합이 B-A, B-B, B-C, C-A, C-B, C-C, D-A, D-B, D-C, E-A, E-B, E-C 인 12건이 출력된다.
#### 정리
INNER JOIN : B-B, C-C (2건) 
LEFT OUTER JOIN : B-B, C-C, D-NULL, E-NULL (4건) RIGHT OUTER JOIN : NULL-A, B-B, C-C (3건)
 FULL OUTER JOIN : NULL-A, B-B, C-C, D-NULL, E-NULL (5건) 
 CROSS JOIN : B-A, B-B, B-C, C-A, C-B, C-C, D-A, D-B, D-C, E-A, E-B, E-C (3*4 12건)
## 집합연산자
### 1. 집합연산자
집합연산자 : 두개 테이블에서 조인 사용하지 않고 연관된 데이터를 조회하는 방법, 2개 이상의 질의 결과를 연결하여 하나로 결합하는 방식. 
#### [가. 집합연산자 조건]
- SELECT 절 칼럼수 동일 
- SELECT 절 동일 위치에 존재하는 칼럼 데이터 타입이 상호 호환 가능 
두개의 조건을 만족시키지 못하면, 데이터베이스 오류 반환.
#### [나. 집합연산자 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_204.jpg)  
#### 일반 집합 연산자
1. UNION : 합집합(중복 행은 1개로 처리)
2. UNION ALL : 합집합(중복 행도 표시)
3. INTERSECT : 교집합(INTERSECTION)
4. EXCEPT,MINUS : 차집합(DIFFERENCE)
5. CROSS JOIN : 곱집합(PRODUCT)
#### 순수 관계 연산자  : 관계형 DB를 새롭게 구현
1. SELECT -> WHERE
2. PROJECT -> SELECT
3. NATRUAL JOIN -> 다양한 JOIN
4. DIVIDE -> 사용x
{a,x}{a,y}{a,z} divdie {x,z} = {a}
#### [다. 집합연산자의 연산 결과]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_205.jpg)  
- R1, R2는 각각 SQL 문을 실행해서 생성된 개별 결과 집합을 의미.
- UNION ALL을 제외한 다른 집합 연산자에서는 SQL문의 결과 집합에서 먼저 중복된 건을 배제하는 작업을 수행한 후, 집합 연산 적용.
- UNION 연산의 결과는 {1,2,3,4,5}
- UNION ALL 연산의 결과는 2개의 결과 집합을 단순히 합친것과 동일한 결과. {1,1,2,3,4,4,5,1,1,2,2,2,3,4}
- INSERSECT  연산의 차집합(R1,R2)의 결과는 {5}
- EXCEPT 연산에서, 순서 중요. 순서가 바뀐다면 결과는 {4} 가됨. 
```
SELECT 칼럼명1, 칼럼명2, ... FROM 테이블명1 [WHERE 조건식 ] [[GROUP BY 칼럼(Column)이나 표현식 [HAVING 그룹조건식 ] ] 집합 연산자 SELECT 칼럼명1, 칼럼명2, ... FROM 테이블명2 [WHERE 조건식 ] [[GROUP BY 칼럼(Column)이나 표현식 [HAVING 그룹조건식 ] ] [ORDER BY 1, 2 [ASC또는 DESC ] ; SELECT PLAYER_NAME 선수명, BACK_NO 백넘버 FROM PLAYER WHERE TEAM_ID = 'K02' UNION SELECT PLAYER_NAME 선수명, BACK_NO 백넘버 FROM PLAYER WHERE TEAM_ID = 'K07' ORDER BY 1;
```
#### 예시
```
[질문1] 1) K-리그 소속 선수들 중에서 소속이 삼성블루윙즈팀인 선수들과전남드레곤즈팀인 선수들에 대한 내용을 모두보고 싶다. 1) K-리그 소속 선수 중 소속이 삼성블루윙즈팀인 선수들의 집합과K-리그 소속 선수 중 소속이 전남드레곤즈팀인 선수들의 집합의 합집합
```
```
[예제] SELECT TEAM_ID 팀코드, PLAYER_NAME 선수명, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE TEAM_ID = 'K02' UNION SELECT TEAM_ID 팀코드, PLAYER_NAME 선수명, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE TEAM_ID = 'K07'
```
```
[실행 결과] 팀코드 선수명 포지션 백넘버 키 ---- ---- ---- ---- -- K02 가비 MF 10 177 K02 강대희 MF 26 174 K02 고종수 MF 22 176 K02 고창현 MF 8 170 K02 김강진 DF 43 181 K07 강철 DF 3 178 K07 김반 MF 14 174 K07 김영수 MF 30 175 K07 김정래 GK 33 185 K07 김창원 DF 5 183 100개의 행이 선택되었다.
```

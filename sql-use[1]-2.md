# 3-1.SQL 기본 및 활용-SQL활용(1)
## 계층형 질의와 셀프조인
### 1. 계층형 질의
계층형 질의 : 테이블에 계층형 데이터가 존재하는 경우 데이터를 조회하기 위해 사용 
계층형 데이터 : 동일 테이블에 계층적으로 상위와 하위 데이터가 포함된 데이터 (ex 사원테이블-상위사원,하위), 순환관계 데이터 모델로 설계 할 경우 발생(ex 조직,사원,메뉴)
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_206.jpg)
#### [가. Oracle 계층형 질의]
#### 계층형 질의구문 
계층형 질의를 지원하기 위해서 계층형 질의 구문 제공
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_207.jpg)
- START WITH절은 계층 구조 전개의 시작 위치를 지정하는 구문. (루트데이터)
- CONNECT BY절은 다음에 전개될 자식 데이터를 지정하는 구문. 자식 데이터는 CONNECT BY절에 주어진 조건을 만족.
- PRIOR(조인) : CONNECT BY절에 사용되며, 현재 읽은 칼럼을 지정. 
PRIOR 자식 = 부모 형태를 사용시 계층구조에서 자식 → 부모 방향으로 전개하는 순방향 전개를 함. 그리고  
 PRIOR 부모 = 자식 형태를 사용하면 반대로 부모 → 자식 방향으로 전개하는 역방향 전개를 함. 
 - NOCYCLE : 데이터를 전개하면서 이미 나타난 동일한 데이터가 전개 중에 다시 나타난다면 이것을 가리켜 사이클(Cycle)이 형성되었다라고 말함. 사이클이 발생한 데이터는 런타임 오류가 발생. 그렇지만 NOCYCLE를 추가하면 사이클이 발생이후의 데이터는 전개X. 
 - ORDER SIBLINGS BY : 형제 노드(동일 LEVEL) 사이에서 정렬을 수행.
  - WHERE(필터링) : 모든 전개를 수행한 후에 지정된 조건을 만족하는 데이터만 추출
#### 계층형 질의에서 사용되는 가상칼럼
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_208.jpg)
다음은 [그림II-2-6]의 (3)샘플 데이터를 계층형 질의 구문을 이용해서 조회한 예시(LPAD 함수 사용)
```
[예제] SELECT LEVEL, LPAD(' ', 4 * (LEVEL-1)) || 사원 사원, 관리자, CONNECT_BY_ISLEAF ISLEAF FROM 사원 START WITH 관리자 IS NULL CONNECT BY PRIOR 사원 = 관리자;
```
```
[실행 결과] LEVEL 사원 관리자 ISLEAF ----- -------- ----- ------ 1 A 0 2 B A 1 2 C A 0 3 D C 1 3 E C 1
```
1. A는 루트데이터 이므로 레벨 1 
2. A의 하위 데이터 B,C는 레벨 2
3. C의 하위데이터 D,E는 레벨 3 
4. 리프데이터는 B,D,E
5. 관리자 → 사원 방향 전개(순방향 전개)
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_209.jpg)
#### 계층형 질의에서 사용되는 함수
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_211.jpg)
SYS_CONNECT_BY_PATH, CONNECT_BY_ROOT를 사용한 예시. 
```
[예제] SELECT CONNECT_BY_ROOT 사원 루트사원, SYS_CONNECT_BY_PATH(사원, '/') 경로, 사원, 관리자 FROM 사원 START WITH 관리자 IS NULL CONNECT BY PRIOR 사원 = 관리자
```
```
[실행 결과] 루트사원 경로 사원 관리자 ------- ------- ---- ----- A /A A A /A/B B A A /A/C C A A /A/C/D D C A /A/C/E E C
```
1. START WITH 를 통해 추출된 루트데이터가 1건 이므로 루트사원은 모두 A
2. D의 경로는 A → C → D
#### [나. SQL server 계층형 질의]
#### 재귀적 쿼리 처리과정
1. CTE 식을 앵커 멤버와 재귀 멤버로 분할한다.
 2. 앵커 멤버를 실행하여 첫 번째 호출 또는 기본 결과 집합(T0)을 만든다.
 3. Ti는 입력으로 사용하고 Ti+1은 출력으로 사용하여 재귀 멤버를 실행한다. 
 4. 빈 집합이 반환될 때까지 3단계를 반복한다. 
 5. 결과 집합을 반환한다. 이것은 T0에서 Tn까지의 UNION ALL이다.
 ```
 SELECT LEVEL, LPAD(' '. 4*(LEVEL-1) || EMPNO 사원, MGR 관리자, CONNECT_BY_ISLEAF ISLEAF FROM EMP START WITH MGR IS NULL CONNECT BY PRIOR EMPNO=MGR; 
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_212.jpg)
```
```
EmployeeID ManagerID -------- --------- 1000 NULL 1100 1000 1110 1100 1120 1100 1121 1120 1122 1120 1200 1000 1210 1200 1211 1210 1212 1210 1220 1200 1221 1220 1222 1220 1300 100
```
아래에 t_emp 데이터 최상위 부터 시작해 하위방향으로 계층 구조를 전개하도록 작성한 쿼리와 그 결과
```
WITH T_EMP_ANCHOR AS ( SELECT EMPLOYEEID, MANAGERID, 0 AS LEVEL FROM T_EMP WHERE MANAGERID IS NULL /* 재귀 호출의 시작점 */ UNION ALL SELECT R.EMPLOYEEID, R.MANAGERID, A.LEVEL + 1 FROM T_EMP_ANCHOR A, T_EMP R WHERE A.EMPLOYEEID = R.MANAGERID ) SELECT LEVEL, REPLICATE(' ', LEVEL) + EMPLOYEEID AS EMPLOYEEID, MANAGERID FROM T_EMP_ANCHOR GO ************************************************************************** Level EmployeeID ManagerID --- ------- --------- 0 1000 NULL 1 1100 1000 1 1200 1000 1 1300 1000 2 1210 1200 2 1220 1200 3 1221 1220 3 1222 1220 3 1211 1210
```
```
3 1212 1210 2 1110 1100 2 1120 1100 3 1121 1120 3 1122 1120 (14개 행 적용됨)
```
조직도와 같은 모습으로 출력하려면 order by 절을 추가해 정렬 할 수 있다. (CTE에 Sort 정렬용 칼럼 추가, 쿼리 마지막에 order by 추가)
### 2. 셀프조인 
셀프조인 : 동일 테이블 사이의 조인. FROM절에 동일 테이블이 두번이상 나타나는 것. 반드시 별칭을 사용(Alias) 
#### 사용법
```
SELECT ALIAS명1.칼럼명, ALIAS명2.칼럼명, ... FROM 테이블1 ALIAS명1, 테이블2 ALIAS명2 WHERE ALIAS명1.칼럼명2 = ALIAS명2.칼럼명1;
```
```
SELECT WORKER.ID 사원번호, WORKER.NAME 사원명, MANAGER.NAME 관리자명 FROM EMP WORKER, EMP MANAGER WHERE WORKER.MGR = MANAGER.ID;
```
사원이라는 테이블 속에 사원,관리자가 모두 하나의 사원이라는개념으로 입력됨. 이 문제를 셀프조인으로 해결하려면, 
FROM 절에 사원 테이블을 두번 사용해야함. (상위,차상위 관리자를 같은줄에)
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_213.jpg)
동일 테이블을 다른 테이블인 것처럼 처리하려면 테이블 별칭 사용. (E1-사원,E2-관리자)
```
[예제] SELECT E1.사원, E1.관리자, E2.관리자 차상위_관리자 FROM 사원 E1, 사원 E2 WHERE E1.관리자 = E2.사원 ORDER BY E1.사원;
```
```
[실행 결과] 사원 관리자 차상위_관리자 ---- ------ ---------- B A C A D C A E C A
```
1. 결과 표시를 위해 SELECT 절에 2개의 관리자 칼럼 사용
2. 한명은 직속관리자(E1.관리자), 다른 한명은 차상위관리자(E2.관리자) 
3. B와C의 관리자는 A 이고 차상위 관리자 없음.
4. D와E의 관리자는 C이고 차상위 관리자는 A 
5. A에 대한 정보 누락 (내부조인 사용시 관리자가 존재하지 않으면 누락. 방지하기 위해 아우터조인 사용) 

자신과 자신의 직속 관리자는 동일한 행에서 구할 수 있지만 차상위 관리자는 구할수 없음. 구하려면 한번더 셀프조인 수행. 

#### 아우터 조인 
```
[예제] SELECT E1.사원, E1.관리자, E2.관리자 차상위_관리자 FROM 사원 E1 LEFT OUTER JOIN 사원 E2 ON (E1.관리자 = E2.사원) ORDER BY E1.사원;
```
## 서브쿼리
### 1. 서브쿼리란
서브쿼리 : 하나의 SQL 문 안에 포함된 다른 SQL 문, 알려지지 않은 기준을 이용한 검색을 위해 사용, 메인 쿼리가 서브 쿼리를 포함하는 종속적 관계
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_214.jpg)
#### [가.서브쿼리 특징]
- 서브쿼리는 메인 쿼리 칼럼 사용 , 메인쿼리는 서브쿼리 사용 X 질의 결과에 서브쿼리 칼럼 표시 시 조인 방식 변환 및 함수, 스칼라 서브쿼리 등 사용.
- 조인은 집합간의 곱 관계(Product) , 1:1 관계 테이블 조인시 1(=1*1) 레벨의 집합 생성, 1:M관계 시 M(1*M) 레벨 집합 생성,M:N은 MN(=M*N) 생성  
(ex. 메인쿼리로 조직(1), 서브쿼리로 사원(M) 테이블을 사용하면 결과집합은 조직(1) 레벨이 된다.)
#### [나. 서브쿼리 사용 시 주의사항]
1. 서브쿼리를 괄호로 감싸서 사용한다.
2. 서브쿼리는 단일 행(Single Row) 또는 복수 행(Multiple Row) 비교 연산자와 함께 사용 가능하다. 단일 행 비교 연산자는 서브쿼리의 결과가 반드시 1건 이하이어야 하고 복수 행 비교 연산자는 서브쿼리의 결과 건수와 상관 없다.
3. 서브쿼리에서는 ORDER BY를 사용하지 못한다. ORDER BY절은 SELECT절에서 오직 한 개만 올 수 있기 때문에 ORDER BY절은 메인쿼리의 마지막 문장에 위치해야 한다.
#### 서브쿼리 사용 가능한 곳
- SELECT 절 
- FROM 절 
- WHERE 절 
- HAVING 절 
- ORDER BY 절 
- INSERT 문의 VALUES 절 
- UPDATE 문의 SET 절 
#### [다. 서브쿼리 분류]
#### 동작 방식에 따른 서브쿼리 분류
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_215.jpg)
#### 반환되는 데이터 형태에 따른 서브쿼리 분류
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_216.jpg)
### 2. 단일행 서브쿼리 
단일행 서브쿼리 : 서브쿼리가 단일행 비교연산자(=,<,<=,>,>=,<>)와 함께 사용할 때는 결과변수가 반드시 1건 이하여야 한다. (2건 이상 오류) 
### 3. 다중행 서브쿼리 
다중행 서브쿼리 : 서브쿼리 결과가 2건이상 반환 될 수 있다면 반드시 다중행 비교연산자(IN,ALL,ANY,SOME)와 함께 사용.
#### 다중행 비교연산자
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_219.jpg)
### 4. 다중 칼럼 서브쿼리
다중칼럼 서브쿼리 : 서브쿼리의 결과로 여러개의 칼럼이 반환되어 메인쿼리의 조건과 동시에 비교되는 것을 의미 
ex) WHERE (TEAM_ID, HEIGHT) IN (SELECT TEAM_ID, MIN(HEIGHT) FROM PLAYER GROUP BY TEAM_ID  
### 5. 연관 서브쿼리
연관 서브쿼리 : 서브쿼리가 메인 쿼리 칼럼을 가지는 형태 
### 6. 그밖에 서브쿼리
#### [가. SELECT 절에서 서브쿼리 사용하기]
스칼라 서브쿼리 : SELECT 절에서 한 행, 한 칼럼 만을 반환하는 서브쿼리. 칼럼을 쓸 수 있는 대부분의 곳에서 사용 가능
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_220.jpg)
#### [나. FROM 절에서 서브쿼리 사용하기]
인라인 뷰(동적뷰): FROM 절에서 사용되는 서브쿼리. SQL문이 실행될 때만 임시적으로 생성되는 동적인 뷰이므로 데이터베이스에 해당 정보가 저장되지 않음. 테이블 명이 올 수 
정적뷰 : 일반적인 뷰
#### [다. HAVING 절에서 서브쿼리 사용하기]
HAVING 절은 그룹합수와 함께 사용 될 때 그룹핑된 결과에 대해 부가적인 조건을 주기 위해 사용. 
```
[예제] SELECT P.TEAM_ID 팀코드, T.TEAM_NAME 팀명, AVG(P.HEIGHT) 평균키 FROM PLAYER P, TEAM T WHERE P.TEAM_ID = T.TEAM_ID GROUP BY P.TEAM_ID, T.TEAM_NAME HAVING AVG(P.HEIGHT) < (SELECT AVG(HEIGHT) FROM PLAYER WHERE TEAM_ID ='K02')
```
#### [라. UPDATE 문의 SET 절 사용하기]
STADIUM_NAME 값을 STADIUM 테이블을 이용하여 변경하고자 할때 
```
UPDATE TEAM A SET A.STADIUM_NAME = (SELECT X.STADIUM_NAME FROM STADIUM X WHERE X.STADIUM_ID = A.STADIUM_ID);
```
서브쿼리를 사용한 변경 작업을 할 때 서브쿼리의 결과가 NULL을 반환할 경우 해당 컬럼의 결과가 NULL이 될 수 있기 때문에 주의.
#### [마. INSERT 문의 VALUES 절에서 사용하기]
PLAYER 테이블에 홍길동 선수 삽입. PLAYER_ID에 1을 더한 값을 넣으려고 할 때
```
INSERT INTO PLAYER(PLAYER_ID, PLAYER_NAME, TEAM_ID) VALUES((SELECT TO_CHAR(MAX(TO_NUMBER(PLAYER_ID))+1) FROM PLAYER), '홍길동', 'K06');
```
### 6. 뷰(View)
뷰(가상테이블) : 실제 데이터를 가지고 있지 않고 정의만 가짐. 질의에서 뷰가 사용되면 뷰 정의를 참조하여 DBMS 내부적으로 질의를 재작성 하여 질의 수행. 
#### 뷰 사용의 장점
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_221.jpg)
#### 뷰 생성
```
CREATE VIEW V_PLAYER_TEAM AS SELECT P.PLAYER_NAME, P.POSITION, P.BACK_NO, P.TEAM_ID, T.TEAM_NAME FROM PLAYER P, TEAM T WHERE P.TEAM_ID = T.TEAM_ID;
```

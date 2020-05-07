# 2-2. SQL 기본 및 활용 - SQL기본(2)
## ORDER BY 절 
### 1. ORDER BY  정렬 
#### [가. ORDER BY 절]
ORDER BY 절 : SQL 문장으로 조회된 데이터들을 다양한 목적에 맞게 특정 칼럼을 기준으로 정렬하여 출력하는데 사용되는 절. (순서정렬가능), 기본적으로 오름차순(ASC)으로 적용됨. 
(cf. 내림차순(DESC))
```
SELECT 칼럼명 [ALIAS명] FROM 테이블명 [WHERE 조건식] [GROUP BY 칼럼(Column)이나 표현식] [HAVING 그룹조건식] [ORDER BY 칼럼(Column)이나 표현식 [ASC 또는 DESC]] ; ASC(Ascending) : 조회한 데이터를 오름차순으로 정렬한다.(기본 값이므로 생략 가능) DESC(Descending) : 조회한 데이터를 내림차순으로 정렬한다.
```

[예제] ORDER BY 절의 예로 선수 테이블에서 선수들의 이름, 포지션, 백넘버를 출력하는데 사람 이름을 내림차순으로 정렬하여 출력한다.
```
[예제] SELECT PLAYER_NAME 선수명, POSITION 포지션, BACK_NO 백넘버 FROM PLAYER ORDER BY PLAYER_NAME DESC;
```
#### [나. ORDER BY 절 특징]
- 기본정렬 순서는 오름차순(ASC)이다.
- 숫자형 데이터 타입은 오름차순으로 정렬했을 때 가장 작은 값부터 출력된다.
- 날짜형 데이터 타입은 오름차순으로 정렬 했을 경우 날짜 값이 가장 빠른 값이 먼저 출력된다.
- Oracle 에서는 null 값을 가장 큰 값으로 간주하여 오름차순으로 정렬했을 경우에는 가장 마지막에, 내림차순으로 정렬했을 경우에는 가장 먼저 위치한다.
- 반면 SQL server 에서는  null값을 가장 작은 값으로 간주하여 오름차순으로 정렬 했을 경우, 가장먼저 내림차순으로 정렬 했을 경우, 마지막에 위치한다.  

Oracle에서는 NULL을 가장 큰 값으로 취급하며 SQL Server에서는 NULL을 가장 작은 값으로 취급한다.

### 2.  SELECT 문장 실행 순서
#### SELECT 수행단계
1. FROM 테이블명
2. WHERE 조건식
3. GROUP BY 칼럼 이나 표현식
4. HAVING 그룹조건식
5. SELECT 칼럼명[ALIAS 명]
6. ORDER BY 칼럼(Column)이나 표현식
### 3. Top N 쿼리
#### [가. ROWNUM]
순위가 높은 N개의 로우를 추출하기 위해 ORDER BY 절과 WHERE 절의 ROWNUM 조건을 같이 사용하는 경우가 있음, BUT 이 두조건으로 원하는 결과x 


- ROWNUM : WHERE 절에서 행의 개수를 제한하는 목적으로 사용, ORDER BY 절보다 먼저 처리되는 WHERE 절에서 처리됨. 

한 행만 가져오고 싶을 때
```
 WHERE ROWNUM = 1; WHERE ROWNUM <= 1; WHERE ROWNUM < 2; 
```
 두건 이상의 N행을 가져오고 싶을 때  
 ```
WHERE ROWNUM = N; (X) WHERE ROWNUM <= N; (O) WHERE ROWNUM < N+1; (O)
```

[예제] 사원 테이블에서 급여가 높은 3명만 내림차순으로 출력
```
[예제] SELECT ENAME, SAL FROM (SELECT ENAME, SAL FROM EMP ORDER BY SAL DESC) WHERE ROWNUM < 4 ;
```
```
[실행 결과] ENAME SAL ------ ---- KING 5000 SCOTT 3000 FORD 3000 3개의 행이 선택되었다
```
인라인 뷰를 사용하여 추출하고자 하는 집합을 정렬 한 후 ROWNUM 을 적용시킴으로써, Top N 쿼리결과를 출력한 사례. 
c.f) sql server 
```
SELECT TOP(2) WITH TIES ENAME, SAL
```
 (1위 한명, 공동2위가 2명있을 때 with ties 조건 추가하면 결과 3건 출력됨. with ties 없으면 결과 2건 출력됨)

#### [나. TOP()]
반면 SQL server 에서는TOP 조건을 사용하면, 별도 처리 없이 데이터 정렬 후 원하는 결과를 얻을 수 있음.
```
TOP (Expression) [PERCENT] [WITH TIES]
```
- TOP 절 : 결과집합으로 반환되는 행수를 제한. WITH TIES 조건은 ORDER BY 조건 기준으로, TOP N 마지막 행으로 표시되는 추가 행 데이터가 같은 경우, 동일 정렬 순서 데이터를 추가 반환하는 옵션.

[예제] 사원 테이블에서 급여가 높은 2명을 내림차순으로 출력하는데 같은 급여를 받는 사원이 있으면 같이 출력한다.
```
[예제] SELECT TOP(2) WITH TIES ENAME, SAL FROM EMP ORDER BY SAL DESC;
```
```
[실행 결과] ENAME SAL ----- --- KING 5000 SCOTT 3000 FORD 3000 3개의 행이 선택되었다.
```
 급여가 높은 2명을 내림차순으로 출력하는데 같은 급여를 받는 사원은 같이 출력한다(WITH TIES)
```
SELECT TOP(2) WITH TIES ENAME, SAL
FROM EMP
ORDER BY SAL DESC;
```

## 조인(JOIN)
### 1. JOIN
JOIN : 두 개 이상의 테이블과 연결 또는 결합하여 필요한 데이터를 출력하는 것, 단 두개의 집합 간에만 조인이 일어남. 5가지 테이블을 JOIN 하기 위해서는 최소 4번의 JOIN 과정이 필요하다. (N-1)  
JOIN 을 이용하여 데이터를 검색하는 과정 : 선수라는 테이블과 팀이라는 테이블이 있을 경우, 선수 테이블을 기준으로 필요한 데이터를 검색하고 이 데이터와 연관된 팀 테이블의 특정 행을 찾아오는 과정.
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_194.jpg)
### 2. EQUI JOIN
EQUI(등가) JOIN : 두 개 테이블 간에 칼럼 값들이 정확하게 일치하는 경우 사용되는 방법으로, 대부분 PK↔FK 의 관계를 이반으로 한다. (반드시 성립X)
JOIN 조건은 WHERE 절에  "=" 연산을 사용해서 표현
```
SELECT 테이블1.칼럼명, 테이블2.칼럼명, ... FROM 테이블1, 테이블2 WHERE 테이블1.칼럼명1 = 테이블2.칼럼명2; → WHERE 절에 JOIN 조건을 넣는다.
```
SQL처럼 컬럼명 앞에 테이블 명을 기술해줘야 함
```
SELECT PLAYER.PLAYER_NAME
FROM PLAYER
```
#### [가. 선수-팀 EQUI JOIN 사례 ]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_195.jpg)  

선수(PLAYER) 테이블에 있는 소속팀코드(TEAM_ID) 칼럼이 팀(TEAM) 테이블의 팀코드(TEAM_ID)와 PK(팀 테이블의 팀코드)와 FK(선수 테이블의 소속팀 코드)의 관계. 

선수들과 선수들이 소속해 있는 팀명 및 연고지를 알아보기 위해서 선수 테이블의 소속팀코드를 기준으로 팀 테이블에 들어 있는 데이터를 다음과 같이 순서를 바꾸어야함.
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_196.jpg)  
[그림 Ⅱ-1-14]의 실바 선수를 예로 들면 백넘버는 45번이고, 소속팀코드는 K07번이다. K07번 팀코드의 팀명은 드래곤즈이고 연고지는 전남이라는 결과를 얻을 수 있게 된다.

[예제] [그림 Ⅱ-1-14]의 데이터를 출력하기 위한 SELECT SQL 문장을 작성한다.
```
[예제] SELECT PLAYER.PLAYER_NAME, PLAYER.BACK_NO, PLAYER.TEAM_ID, TEAM.TEAM_NAME, TEAM.REGION_NAME FROM PLAYER, TEAM WHERE PLAYER.TEAM_ID = TEAM.TEAM_ID; 또는 INNER JOIN을 명시하여 사용할 수도 있다. SELECT PLAYER.PLAYER_NAME, PLAYER.BACK_NO, PLAYER.TEAM_ID, TEAM.TEAM_NAME, TEAM.REGION_NAME FROM PLAYER INNER JOIN TEAM ON PLAYER.TEAM_ID = TEAM.TEAM_ID;
```
```
[실행 결과] PLAYER_NAME BACK_NO TEAM_ID TEAM_NAME REGION_NAME ----------- ------- ------- ---------- ----------- 이고르 21 K06 아이파크 부산 오비나 26 K10 시티즌 대전 윤원일 45 K02 삼성블루윙즈 수원 페르난도 44 K04 유나이티드 인천 레오 45 K03 스틸러스 포항 실바 45 K07 드래곤즈 전남 무스타파 77 K04 유나이티드 인천 에디 7 K01 울산현대 울산 알리송 14 K01 울산현대 울산 쟈스민 33 K08 일화천마 성남 디디 8 K06 아이파크 부산 480개 항이 선택되었다.
```
SQL 문장 복잡시 에러 발생 가능성이 높으므로, ALIAS 를 적용하여 SQL 수정
```
[예제] SELECT P.PLAYER_NAME 선수명, P.BACK_NO 백넘버, P.TEAM_ID 팀코드, T.TEAM_NAME 팀명, T.REGION_NAME 연고지 FROM PLAYER P, TEAM T WHERE P.TEAM_ID = T.TEAM_ID; 또는 INNER JOIN을 명시하여 사용할 수도 있다. SELECT P.PLAYER_NAME 선수명, P.BACK_NO 백넘버, P.TEAM_ID 팀코드, T.TEAM_NAME 팀명, T.REGION_NAME 연고지 FROM PLAYER P INNER JOIN TEAM T ON P.TEAM_ID = T.TEAM_ID;
```
```
[실행 결과] 선수명 백넘버 팀코드 팀명 연고지 -------- ----- ----- -------- ----- 이고르 21 K06 아이파크 부산 오비나 26 K10 시티즌 대전 윤원일 45 K02 삼성블루윙즈 수원 페르난도 44 K04 유나이티드 인천 레오 45 K03 스틸러스 포항 실바 45 K07 드래곤즈 전남 무스타파 77 K04 유나이티드 인천 에디 7 K01 울산현대 울산 알리송 14 K01 울산현대 울산 쟈스민 33 K08 일화천마 성남 디디 8 K06 아이파크 부산 … … … … … 480개 항이 선택되었다
```
#### [나. 선수-팀WHERE 절 검색조건 사례]
EQUI JOIN의 최소한의 연관 관계를 위해서 테이블 개수 - 1개의 JOIN 조건을 WHERE 절에 명시하고, 부수적인 제한 조건을 논리 연산자를 통하여 추가로 입력 가능.

[예제] 위 SQL 문장의 WHERE 절에 포지션이 골키퍼인(골키퍼에 대한 포지션 코드는 ‘GK’임) 선수들에 대한 데이터만을 백넘버 순으로 출력하는 SQL문을 만들어 본다.
```
[예제] SELECT P.PLAYER_NAME 선수명, P.BACK_NO 백넘버, T.REGION_NAME 연고지, T.TEAM_NAME 팀명 FROM PLAYER P, TEAM T WHERE P.TEAM_ID = T.TEAM_ID AND P.POSITION = 'GK' ORDER BY P.BACK_NO; 또는 INNER JOIN을 명시하여 사용할 수도 있다. SELECT P.PLAYER_NAME 선수명, P.BACK_NO 백넘버, T.REGION_NAME 연고지, T.TEAM_NAME 팀명 FROM PLAYER P INNER JOIN TEAM T ON P.TEAM_ID = T.TEAM_ID WHERE P.POSITION = 'GK' ORDER BY P.BACK_NO;
```
```
[실행 결과] 선수명 백넘버 연고지 팀명 ------- ---- ----- ---- 최종문 1 전남 드래곤즈 정병지 1 포항 스틸러스 박유석 1 부산 아이파크 김승준 1 대전 시티즌 이현 1 인천 유나이티드 김운재 1 수원 삼성블루윙즈 정해운 1 성남 일화천마 권정혁 1 울산 울산현대 최동석 1 서울 FC서울 김창민 1 전북 현대모터스 김용발 18 전북 현대모터스 한동진 21 인천 유나이티드 이은성 21 대전 시티즌 김준호 21 포항 스틸러스 조범철 21 수원 삼성블루윙즈 백민철 21 서울 FC서울 권찬수 21 성남 일화천마 서동명 21 울산 울산현대 강성일 30 대전 시티즌 김대희 31 포항 스틸러스 남현우 31 인천 유나이티드 정지혁 31 부산 아이파크 양영민 31 성남 일화천마 염동균 31 전남 드래곤즈 이무림 31 울산 울산현대 최관민 31 전북 현대모터스 최호진 31 수원 삼성블루윙즈 우태식 31 서울 FC서울 김정래 33 전남 드래곤즈 최창주 40 울산 울산현대 정용대 40 부산 아이파크 정경진 41 부산 아이파크 정광수 41 수원 삼성블루윙즈 정경두 41 성남 일화천마 허인무 41 포항 스틸러스 정영광 41 전남 드래곤즈 조의손 44 서울 FC서울 정이섭 45 전북 현대모터스 양지원 45 울산 울산현대 선원길 46 강원 강원FC 최주호 51 포항 스틸러스 최동우 60 전북 현대모터스 김충호 60 인천 유나이티드 43개 항이 선택되었다.
```
JOIN 조건 기술 시 WHERE 절과 SELECT 절에는 테이블 명이 아닌 테이블에 대한 ALIAS 를 사용해야한다. 
#### [다. 팀-운동장 EQUI JOIN  사례]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_197.jpg)  
[예제] 이번에는 [그림 Ⅱ-1-15]에 나와 있는 팀(TEAM) 테이블과 구장(STADIUM) 테이블의 관계를 이용해서 소속팀이 가지고 있는 전용구장의 정보를 팀의 정보와 함께 출력하는 SQL문을 작성한다.
```
[예제] SELECT TEAM.REGION_NAME, TEAM.TEAM_NAME, TEAM.STADIUM_ID, STADIUM.STADIUM_NAME, STADIUM.SEAT_COUNT FROM TEAM, STADIUM WHERE TEAM.STADIUM_ID = STADIUM.STADIUM_ID; 또는 INNER JOIN을 명시하여 사용할 수도 있다. SELECT TEAM.REGION_NAME, TEAM.TEAM_NAME, TEAM.STADIUM_ID, STADIUM.STADIUM_NAME, STADIUM.SEAT_COUNT FROM TEAM INNER JOIN STADIUM ON TEAM.STADIUM_ID = STADIUM.STADIUM_ID; 위 SQL문과 ALIAS를 사용한 아래 SQL문은 같은 결과를 얻을 수 있다. SELECT T.REGION_NAME, T.TEAM_NAME, T.STADIUM_ID, S.STADIUM_NAME, S.SEAT_COUNT FROM TEAM T, STADIUM S WHERE T.STADIUM_ID = S.STADIUM_ID; 또는 INNER JOIN을 명시하여 사용할 수도 있다. SELECT T.REGION_NAME, T.TEAM_NAME, T.STADIUM_ID, S.STADIUM_NAME, S.SEAT_COUNT FROM TEAM T INNER JOIN STADIUM S ON T.STADIUM_ID = S.STADIUM_ID; 중복이 되지 않는 칼럼의 경우 ALIAS를 사용하지 않아도 되므로, 아래 SQL 문은 위 SQL문과 같은 결과를 얻을 수 있다. 그러나 같은 이름을 가진 중복 칼럼의 경우는 테이블명이나 ALIAS가 필수 조건이다. SELECT REGION_NAME, TEAM_NAME, T.STADIUM_ID, STADIUM_NAME, SEAT_COUNT FROM TEAM T, STADIUM S WHERE T.STADIUM_ID = S.STADIUM_ID;
```
```
[실행 결과] REGION_NAME TEAM_NAME STADIUM_ID STADIUM_NAME SEAT_COUNT ----------- ------------ --------- ------------ ---------- 전북 현대모터스 D03 전주월드컵경기장 28000 성남 일화천마 B02 성남종합운동장 27000 포항 스틸러스 C06 포항스틸야드 25000 전남 드래곤즈 D01 광양전용경기장 20009 서울 FC서울 B05 서울월드컵경기장 66806 인천 유나이티드 B01 인천월드컵경기장 35000 경남 경남FC C05 창원종합운동장 27085 울산 울산현대 C04 울산문수경기장 46102 대전 시티즌 D02 대전월드컵경기장 41000 수원 삼성블루윙즈 B04 수원월드컵경기장 50000 광주 광주상무 A02 광주월드컵경기장 40245 부산 아이파크 C02 부산아시아드경기장 30000 강원 강원FC A03 강릉종합경기장 33000 제주 제주유나이티드FC A04 제주월드컵경기장 42256 대구 대구FC A05 대구월드컵경기장 66422 15개의 행이 선택되었다.
```
### 3. Non EQUI JOIN
NON EQUI JOIN(비등가조인) : 두 개 테이블 간 칼럼 값들이 서로 정확히 일치하지 않은 경우 사용됨. 
#### 비등가조인 특징 
- "=" 연산자가 아닌 다른(Between, >, >=, <, <=등) 연산자들을 사용하여 JOIN 수행. 
- 두개의 테이블이 PK-FK로 연관관계를 가지거나 논리적으로 같은 값이 존재하면, "="연산자를 이용하여  EQUI JOIN을 사용
- 두개의 테이블 간에 칼럼 값들이 정확히 일치하지 않으면 EQUI JOIN 사용X
```
SELECT 테이블1.칼럼명, 테이블2.칼럼명, ... FROM 테이블1, 테이블2 WHERE 테이블1.칼럼명1 BETWEEN 테이블2.칼럼명1 AND 테이블2.칼럼명2;
```
```
SELECT E.ENAME, E.JOB, E.SAL, S.GRADE
FROM EMP E, SALGRADE S
WHERE E.SAL BETWEEN S.LOSAL AND S.HSAL;
```
위는 E의 SAL의 값을 S의 LOSAL과 HSAL 범위에서 찾는 것이다.
### 4. 3개 이상 TABLE JOIN
```
[예제] SELECT P.PLAYER_NAME 선수명, P.POSITION 포지션, T.REGION_NAME 연고지, T.TEAM_NAME 팀명, S.STADIUM_NAME 구장명 FROM PLAYER P, TEAM T, STADIUM S WHERE P.TEAM_ID = T.TEAM_ID AND T.STADIUM_ID = S.STADIUM_ID ORDER BY 선수명; 또는 INNER JOIN을 명시하여 사용할 수도 있다. SELECT P.PLAYER_NAME 선수명, P.POSITION 포지션, T.REGION_NAME 연고지, T.TEAM_NAME 팀명, S.STADIUM_NAME 구장명 FROM PLAYER P INNER JOIN TEAM T ON P.TEAM_ID = T.TEAM_ID INNER JOIN STADIUM S ON T.STADIUM_ID = S.STADIUM_ID ORDER BY 선수명;
```
```
[실행 결과] 선수명 포지션 연고지 팀명 구장명 ------ ----- ----- --------- ------------- 가비 MF 수원 삼성블루윙즈 수원월드컵경기장 가이모토 DF 성남 일화천마 성남종합운동장 강대희 MF 수원 삼성블루윙즈 수원월드컵경기장 강성일 GK 대전 시티즌 대전월드컵경기장 강용 DF 포항 스틸러스 포항스틸야드 강정훈 MF 대전 시티즌 대전월드컵경기장 강철 DF 전남 드래곤즈 광양전용경기장 고관영 MF 전북 현대모터스 전주월드컵경기장 고규억 DF 광주 광주상무 광주월드컵경기장 고민기 FW 전북 현대모터스 전주월드컵경기장 고병운 DF 포항 스틸러스 포항스틸야드 고종수 MF 수원 삼성블루윙즈 수원월드컵경기장 고창현 MF 수원 삼성블루윙즈 수원월드컵경기장 공오균 MF 대전 시티즌 대전월드컵경기장 곽경근 FW 인천 유나이티드 인천월드컵경기장 곽기훈 FW 울산 울산현대 울산문수경기장 곽기훈 FW 울산 울산현대 울산문수경기장 곽치국 MF 성남 일화천마 성남종합운동장 … … … … … 480개의 행이 선택되었다.
```

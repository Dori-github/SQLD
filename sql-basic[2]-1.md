# 2-2. SQL 기본 및 활용 - SQL기본(2)
## 함수(FUNCTION)
### 1. 내장함수(BULT-IN FUNCTION)개요 
#### [가. 내장함수]
내장함수(데이터 값을 간편하게 조작)
- 단일행 함수 : 단일행 값 입력(문자형함수, 숫자형함수, 날짜형함수, 변환형함수, NULL관련함수)
- 다중행 함수 : 여러 행 값 입력(집계함수, 그룹함수, 윈도우함수)
#### [나. 단일행 함수]
단일행 함수 : 함수는 입력값이 많아도 출력은 하나만 된다는 M:1 관계라는 중요한 특징을 가짐. 단일행 함수의 경우 하나값 또는 여러 값이 입력인수로 표현됨. 
```
함수명 (칼럼이나 표현식 [, Arg1, Arg2, ... ])
```
#### 단일행 함수 종류
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_181.jpg)
#### 단일행 함수 특징 
- SELECT, WHERE, ORDER BY 절에 사용 가능하다. 
- 각 행(Row)들에 대해 개별적으로 작용하여 데이터 값들을 조작, 각 행에 대해 조작 결과를 리턴한다. 
- 여러 인자(Argument)를 입력해도 단 하나의 결과만 리턴한다. 
- 함수의 인자(Arguments)로 상수, 변수, 표현식이 사용 가능하고, 하나의 인수를 가지는 경우도 있지만 여러 개의 인수를 가질 수도 있다. 
- 특별한 경우가 아니면 함수의 인자(Arguments)로 함수를 사용하는 함수의 중첩이 가능하다.
### 2. 문자형 함수
문자형함수 : 문자 데이터를 매개 변수로 받아들여서 문자나 숫자의 값의 결과를 돌려주는 함수 
#### [가. 단일행 문자형 함수의 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_182.jpg)
#### [나. 단일행 문자형 함수 사례]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_183.jpg)
#### [다.Oracle server 사례]
[예제] ‘SQL Expert’라는 문자형 데이터의 길이를 구하는 문자형 함수를 사용한다.
```
[예제 및 실행 결과] Oracle SELECT LENGTH('SQL Expert') FROM DUAL; LENGTH('SQL Expert') --------------- 10
```
Oracle 절은 SELECT 와 FROM 절을 사용
SQL문장은 DUAL 테이블을 FROM 절에 지정.
#### DUAL 테이블 특성
- 사용자 SYS가 소유하며, 모든 사용자가 엑세스 가능한 테이블
- SELECT-FROM 형식을 갖추기 위한 일종의 DUMMY 테이블
- DUMMY 문자열 유형에 칼럼 X 라는 값이 들어있는 행을 1건 포함함.
```
[예제 및 실행 결과] Oracle DESC DUAL; 칼럼 NULL 가능 데이터 유형 ---------------- -------- ----------- DUMMY VARCHAR2(1)
```
```
[예제 및 실행 결과] Oracle SELECT * FROM DUAL; DUMMY ----- X 1개의 행이 선택되었다.
```
#### [라.SQL server 사례]
SELECT 절 만으로도 SQL 문장 수행 가능 . 그러나 사용자 테이블 칼럼 사용시 FROM 절이 필수적으로 사용되어야함.
[예제] ‘SQL Expert’라는 문자형 데이터의 길이를 구하는 문자형 함수를 사용한다.
```
[예제 및 실행 결과] Oracle SELECT LEN('SQL Expert') AS ColumnLength; ColumnLength ---------- 10
```
[예제] 선수 테이블에서 CONCAT 문자형 함수를 이용해 축구선수란 문구를 추가한다.
```
[예제] SELECT CONCAT(PLAYER_NAME, ' 축구선수') 선수명 FROM PLAYER; CONCAT 함수는 Oracle의 '||' 합성 연산자와 같은 기능이다. SELECT PLAYER_NAME || ' 축구선수' AS 선수명 FROM PLAYER;
```
### 3. 숫자형 함수
숫자형 함수 : 숫자 데이터를 입력 받아 처리하고 숫자를 리턴하는 함수
#### [가. 단일행 숫자형 함수 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_184.jpg)
#### [나. 단일행 숫자형 함수 사례]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_185.jpg)
[예제] 정수 기준으로 반올림 및 올림하여 출력한다.
```
[예제] SQL Server SELECT ENAME, ROUND(SAL/12), CEILING(SAL/12) FROM EMP;
```
```
[실행 결과] ENAME ROUND(SAL/12) CEILING(SAL/12) -------- ------------ -------------- SMITH 67 67 ALLEN 133 134 WARD 104 105 JONES 248 248 MARTIN 104 105 BLAKE 238 238 CLARK 204 205 SCOTT 250 250 KING 417 417 TURNER 125 125 ADAMS 92 92 JAMES 79 80 FORD 250 250 MILLER 108 109 14개의 행이 선택되었다.
```
### 4. 날짜형 함수
날짜형 함수 : DATE 타입의 값을 연산하는 함수. 
#### [가. 단일행 날짜형 함수 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_186.jpg)
#### [나. 단일행 날짜형 데이터 연산]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_187.jpg)
[예제] Oracle의 SYSDATE 함수와 SQL Server의 GETDATE( ) 함수를 사용하여 데이터베이스에서 사용하는 현재의 날짜 데이터를 확인한다. 날짜 데이터는 시스템 구성에 따라 다양하게 표현될 수 있으므로 사용자마다 다른 결과가 나올 수 있다.
```
[예제 및 실행 결과] Oracle SELECT SYSDATE FROM DUAL; SYSDATE -------- 12/07/18
```
```
[예제 및 실행 결과] SQL Server SELECT GETDATE() AS CURRENTTIME; CURRENTTIME ----------------------- 2012-07-18 13:10:02.047
```
### 5. 변환형 함수 
변환형 함수 : 특정 데이터 타입을 다양한 형식으로 출력하고 싶을 때 사용되는 함수
#### [가. 데이터 유형 변환의 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_188.jpg)
암시적 데이터 유형 변환의 경우 성능 저하가 발생 할 수 읶음. 그러므로 명시적 데이터 유형 변환 방법을 사용 하는 것이 바람직함.
#### [나. 단일행 변환형 함수의 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_189.jpg)
[예제] 날짜를 정해진 문자 형태로 변형한다.
```
[예제 및 실행 결과] Oracle SELECT TO_CHAR(SYSDATE, 'YYYY/MM/DD') 날짜, TO_CHAR(SYSDATE, 'YYYY. MON, DAY') 문자형 FROM DUAL; 날자 문자형 --------- ---------------- 2012-07-19 2012. 7월 , 월요일
```
```
[예제 및 실행 결과] SQL Server SELECT CONVERT(VARCHAR(10),GETDATE(),111) AS CURRENTDATE CURRNETDATE ---------- 2012/07/19
```
### 6. CASE 표현 
CASE 표현 : IF-THEN-ELSE 논리와 유사한 방식으로 표현식을 작성하여 비교연산 기능을 보완하는 역할을 함.
####  단일행 CASE 표현의 종류
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_190.jpg)
### 7. NULL 관련 함수
- NVL(식1,식2)/ISNULL(식1,식2) : 식1의 값이 NULL 이면 식2 출력
- NULLIF(식1,식2) : 식1이 식2와 같으면 NULL을 아니면 식1을 출력
- COALESCE(식1,식2) : 임의의 개수표현식에서 NULL이 아닌 최초의 표현식, 모두 NULL이면 NULL 반환

ex)COALESCE(NULL,NULL,‘abc’) -> ‘abc’
#### [가. NVL/ISNULL함수]
#### NULL 특성 정리
- 널 값은 아직 정의되지 않은 값으로 0 또는 공백과 다르다. 0은 숫자이고, 공백은 하나의 문자이다. 
- 테이블을 생성할 때 NOT NULL 또는 PRIMARY KEY로 정의되지 않은 모든 데이터 유형은 널 값을 포함할 수 있다. 
- 널 값을 포함하는 연산의 경우 결과 값도 널 값이다. 
 - 결과값을 NULL이 아닌 다른 값을 얻고자 할 때 NVL/ISNULL 함수를 사용한다.
#### [나. NULL 포함 연산의 결과]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_191.jpg)
#### [다. 단일행 NULL 관련 함수의 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_192.jpg)
1) NVL(표현식1,표현식2) / ISNULL(표현식1,표현식2) 
표현식1값이 NULL이면 표현식2값 출력한다. --- NVL(NULL판단대상,‘NULL일 때 대체값’)
2) NULLIF(표현식1,표현식2) 
표현식1값이 표현식2와 같으면 NULL, 같지 않으면 표현식1값 출력한다 
3) COALESCE(표현식1, 표현식2, ...)
 임의의 개수 표현식에서 NULL이 아닌 최초의 표현식을 나타낸다. 모든 표현식이 NULL이라면 
 NULL값 리턴 (NULL과 관련된 함수가 아닌 것? 1.ISNULL 2.NULLIF 3.COALESCE 4.IS NOT NULL) ※ IS NULL, IS NOT NULL은 함수가 아니라 연산자이다.
#### 예제 
Oracle의 경우 NVL 함수를 사용한다.
```
NVL (NULL 판단 대상,‘NULL일 때 대체값’)
```
```
[예제 및 실행 결과] Oracle SELECT NVL(NULL, 'NVL-OK') NVL_TEST FROM DUAL; NVL_TEST ------- NVL-OK 1개의 행이 선택되었다.
```
```
[예제 및 실행 결과] Oracle SELECT NVL('Not-Null', 'NVL-OK') NVL_TEST FROM DUAL; NVL_TEST ------- Not-Null 1개의 행이 선택되었다.
```
```
SQL Server의 경우 ISNULL 함수를 사용한다.
```
```
ISNULL (NULL 판단 대상,‘NULL일 때 대체값’)
```
```
[예제 및 실행 결과] SQL Server SELECT ISNULL(NULL, 'NVL-OK') ISNULL_TEST ; ISNULL_TEST --------- NVL-OK 1개의 행이 선택되었다.
```
```
[예제 및 실행 결과] SQL Server SELECT ISNULL('Not-Null', 'NVL-OK') ISNULL_TEST ; ISNULL_TEST --------- Not-Null 1개의 행이 선택되었다.
```
#### [라. NULL과 공집합]
#### NVL / ISNULL 함수를 이용해 공집합을 9999로 바꾸기
```
 SELECT NVL(MGR, 9999) MGR FROM EMP WHERE ENAME = 'JSC'; 데이터를 찾을 수 없다.
 ```
 → NVL / ISNULL 함수는 NULL 값을 대상으로 다른 값으로 바꾸는 함수이지 공집합을 대상으로 하지 않는다. 
 ```
  SELECT NVL(MAX(MGR), 9999) MGR FROM EMP WHERE ENAME = 'JSC';
```
 → 집계함수를 인수로 한 NVL / ISNULL 함수를 이용해서 공집합인 경우에도 빈칸이 아닌 999로 출력하게 한다. 다른 함수와 달리 집계함수나 SCALAR SUBQUERY의 경우는 인수의 결과 값이 공 집합인 경우에도 NULL을 출력한다.
#### [마. 기타 NULL 관련 함수(COALESCE)]
COALESCE 함수는 인수의 숫자가 한정되어 있지 않으며, 임의의 개수 EXPR에서 NULL이 아닌 최초의 EXPR을 나타낸다. 만일 모든 EXPR이 NULL이라면 NULL을 리턴한다.
```
COALESCE (EXPR1, EXPR2, …)
```
[예제] 사원 테이블에서 커미션을 1차 선택값으로, 급여를 2차 선택값으로 선택하되 두 칼럼 모두 NULL인 경우는 NULL로 표시한다.
```
[예제] SELECT ENAME, COMM, SAL, COALESCE(COMM, SAL) COAL FROM EMP;
```
```
[예제] COALESCE 함수는 두개의 중첩된 CASE 문장으로 표현할 수 있다. SELECT ENAME, COMM, SAL, CASE WHEN COMM IS NOT NULL THEN COMM ELSE (CASE WHEN SAL IS NOT NULL THEN SAL ELSE NULL END) END COAL FROM EMP;
```
```
[실행 결과] ENAME COMM SAL COAL ------- ------- ------ ------ SMITH 800 800 ALLEN 300 1600 300 WARD 500 1250 500 JONES 2975 2975 MARTIN 1400 1250 1400 BLAKE 2850 2850 CLARK 2450 2450 SCOTT 3000 3000 KING 5000 5000 TURNER 0 1500 0 ADAMS 1100 1100 JAMES 950 950 FORD 3000 3000 MILLER 1300 1300 14개의 행이 선택되었다.
```
## GROUP BY, HAVING 절 
### 1. 집계함수
#### 집계함수의 특성 
- 여러 행들의 그룹이 모여서 그룹당 단 하나의 결과를 돌려주는 함수
- GROUP BY 절은 행들을 소그룹화 함
- SELECT 절, HAVING 절, ORDER BY 절에 사용 가능
```
집계 함수명 ( [DISTINCT | ALL] 칼럼이나 표현식 )
 - ALL : Default 옵션이므로 생략 가능함 
 - DISTINCT : 같은 값을 하나의 데이터로 간주할 때 사용하는 옵션임
```
#### [가. 집계함수의 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_193.jpg)
[예제] 일반적으로 집계 함수는 GROUP BY 절과 같이 사용되지만 아래와 같이 테이블 전체가 하나의 그룹이 되는 경우에는 GROUP BY 절 없이 단독으로도 사용 가능하다.
```
[예제] SELECT COUNT(*) "전체 행수", COUNT(HEIGHT) "키 건수", MAX(HEIGHT) 최대키, MIN(HEIGHT) 최소키, ROUND(AVG(HEIGHT),2) 평균키 FROM PLAYER;
```
```
[실행 결과] 전체 행수 키 건수 최대키 최소키 평균키 ------ ----- ---- ---- ----- 480 447 196 165 179.31 1개의 행이 선택되었다.
```
### 2. GROUP BY 절 
GROUP BY : SQL 문에서 FROM 절과 WHERE 절 뒤에 오며, 데이터들을 작은 그룹으로 분류하여 소그룹에 대한 항목별로 통계 정보를 얻을 때 추가로 사용함.
```
SELECT [DISTINCT] 칼럼명 [ALIAS명] FROM 테이블명 [WHERE 조건식] [GROUP BY 칼럼(Column)이나 표현식] [HAVING 그룹조건식] ;
```
#### GROUP BY 절과 HAVING 절 특성 
- GROUP BY 절을 통해 소그룹별 기준을 정한 후, SELECT 절에 집계 함수를 사용한다. 
- 집계 함수의 통계 정보는 NULL 값을 가진 행을 제외하고 수행한다. 
- GROUP BY 절에서는 SELECT 절과는 달리 ALIAS 명을 사용할 수 없다. 
- 집계 함수는 WHERE 절에는 올 수 없다. (집계 함수를 사용할 수 있는 GROUP BY 절보다 WHERE 절이 먼저 수행된다) - WHERE 절은 전체 데이터를 GROUP으로 나누기 전에 행들을 미리 제거시킨다. 
- HAVING 절은 GROUP BY 절의 기준 항목이나 소그룹의 집계 함수를 이용한 조건을 표시할 수 있다. 
- GROUP BY 절에 의한 소그룹별로 만들어진 집계 데이터 중, HAVING 절에서 제한 조건을 두어 조건을 만족하는 내용만 출력한다. 
- HAVING 절은 일반적으로 GROUP BY 절 뒤에 위치한다
※ ORDER BY 절을 SELECT 절에 정의되지 않은 컬럼 사용 가능
```
SEARCHED_CASE_EXPRESSION
CASE WHEN LOC = ‘a’ THEN ‘b’
```
```
SIMPLE_CASE_EXPRESSION
CASE LOC WHEN ‘a’ THEN ‘b’
```
이 2문장은 같은 의미이다.

[예제] 포지션별 최대키, 최소키, 평균키를 출력한다. (포지션별이란 소그룹의 조건을 제시하였기 때문에 GROUP BY 절을 사용한다.)
```
[예제] SELECT POSITION 포지션, COUNT(*) 인원수, COUNT(HEIGHT) 키대상, MAX(HEIGHT) 최대키, MIN(HEIGHT) 최소키, ROUND(AVG(HEIGHT),2) 평균키 FROM PLAYER GROUP BY POSITION;
```
```
[실행 결과] 포지션 인원수 키대상 최대키 43 43 196 174 186.26 DF 172 142 190 170 180.21 FW 100 100 194 168 179.91 MF 162 162 189 165 176.31 5개의 행이 선택되었다.
```
#### Order by 특징 
기본적인 정렬순서는 오름차순 (ASC)이다. cf. 내림차순(DESC) 
숫자 오름차순 - 가장 작은 값부터 출력 
날짜 오름차순 - 가장 날짜값 빠른 값이 먼저 출력
Oracle - NULL 가장 큰 값으로 간주. 오름차순 -> 가장 마지막. 내림차순 -> 가장 먼저 
SQL Server -NULL 가장 작은 값. 오름차순 -> 가장 먼저, 내림차순 -> 가장 마지막 위치 
ORDER BY 절에서는 칼럼명, ALIAS명, 칼럼순서 같이 혼용해서 사용 가능

※ Oracle에서는 NULL을 가장 큰 값으로 취급하며 SQL Server에서는 NULL을 가장 작은 값으로 취급한다.

<![if !supportEmptyParas]> <![endif]>
### 3. HAVING 절 (조건절)
[예제] HAVING 조건절에는 GROUP BY 절에서 정의한 소그룹의 집계 함수를 이용한 조건을 표시할 수 있으므로, HAVING 절을 이용해 평균키가 180 센티미터 이상인 정보만 표시한다.
```
[예제] SELECT POSITION 포지션, ROUND(AVG(HEIGHT),2) 평균키 FROM PLAYER GROUP BY POSITION HAVING AVG(HEIGHT) >= 180;
```
```
[예제] 포지션 평균키 ------ ------ GK 186.26 DF 180.21 2개의 행이 선택되었다.
```
GROUP BY 소그룹의 데이터 중 일부만 필요한 경우, 
1. GROUP BY 연산 전 WHERE 절에서 조건을 적용하여 필요한 데이터만 추출하여 GROUP BY 연산을 하는 방법
2. GROUP BY 연산 후 HAVING 절에서 필요한 데이터만 필터링 하는 두 가지 방법
### 4. CASE 표현을 활용한 월별 데이터 집계
1. 개별 데이터 확인 
[예제] 먼저 개별 입사정보에서 월별 데이터를 추출하는 작업을 진행한다. 이 단계는 월별 정보가 있다면 생략 가능하다.
```
[예제] Oracle SELECT ENAME, DEPTNO, EXTRACT(MONTH FROM HIREDATE) 입사월, SAL FROM EMP;
```
```
[예제] SQL Server SELECT ENAME, DEPTNO, DATEPART(MONTH, HIREDATE) 입사월, SAL FROM EMP;
```
2. 월별 데이터 구분
[예제] 추출된 MONTH 데이터를 Simple Case Expression을 이용해서 12개의 월별 칼럼으로 구분한다. 실행 결과에서 보여 주는 ENAME 칼럼은 최종 리포트에서 요구되는 데이터는 아니지만, 정보의 흐름을 이해하기 위해 부가적으로 보여 주는 임시 정보이다. FROM 절에서 사용된 인라인 뷰는 2장 4절에서 설명한다.
```
[예제] SELECT ENAME, DEPTNO, CASE MONTH WHEN 1 THEN SAL END M01, CASE MONTH WHEN 2 THEN SAL END M02, CASE MONTH WHEN 3 THEN SAL END M03, CASE MONTH WHEN 4 THEN SAL END M04, CASE MONTH WHEN 5 THEN SAL END M05, CASE MONTH WHEN 6 THEN SAL END M06, CASE MONTH WHEN 7 THEN SAL END M07, CASE MONTH WHEN 8 THEN SAL END M08, CASE MONTH WHEN 9 THEN SAL END M09, CASE MONTH WHEN 10 THEN SAL END M10, CASE MONTH WHEN 11 THEN SAL END M11, CASE MONTH WHEN 12 THEN SAL END M12 FROM (SELECT ENAME, DEPTNO, EXTRACT(MONTH FROM HIREDATE) MONTH, SAL FROM EMP);
```
3. 부서별 데이터 집계
[예제] 최종적으로 보여주는 리포트는 부서별로 월별 입사자의 평균 급여를 알고 싶다는 요구사항이므로 부서별 평균값을 구하기 위해 GROUP BY 절과 AVG 집계 함수를 사용한다. 
```
[예제] SELECT DEPTNO, AVG(CASE MONTH WHEN 1 THEN SAL END) M01, AVG(CASE MONTH WHEN 2 THEN SAL END) M02, AVG(CASE MONTH WHEN 3 THEN SAL END) M03, AVG(CASE MONTH WHEN 4 THEN SAL END) M04, AVG(CASE MONTH WHEN 5 THEN SAL END) M05, AVG(CASE MONTH WHEN 6 THEN SAL END) M06, AVG(CASE MONTH WHEN 7 THEN SAL END) M07, AVG(CASE MONTH WHEN 8 THEN SAL END) M08, AVG(CASE MONTH WHEN 9 THEN SAL END) M09, AVG(CASE MONTH WHEN 10 THEN SAL END) M10, AVG(CASE MONTH WHEN 11 THEN SAL END) M11, AVG(CASE MONTH WHEN 12 THEN SAL END) M12 FROM (SELECT ENAME, DEPTNO, EXTRACT(MONTH FROM HIREDATE) MONTH, SAL FROM EMP) GROUP BY DEPTNO ;
```
### SELECT 문장 실행 순서 
FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY 
예시)
```
SELECT POSITION 포지션, ROUND(AVG(HEIGHT),2) 평균키 FROM PLAYER 
GROUP BY POSITION 
HAVING MAX(HEIGHT) >= 190 
ORDER BY 1, 평균키 ;
```

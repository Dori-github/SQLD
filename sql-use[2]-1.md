# 3-1.SQL 기본 및 활용-SQL활용(2)
##  그룹함수
### 1. 데이터 분석 개요
####  ANSI/ISO SQL 세가지 함수 정의
1. AGGREGATE FUNCTION
GROUP FUNCTION의 한 부분으로 분류가능. COUNT, SUM, AVG,MAX,MIN 외 각종 집계 함수 포함됨.
2. GROUP FUNCTION 
그룹함수를 사용하면 하나의 테이블을 묶어서 재정렬 하지 않아도 하나의 SQL 로 테이블을 한번만 읽어서 원하는 리포트 작성 가능 , 소계/합계를 표시하기 위해 GROUPING , CASE 함수를 이용하여 원하는 보고서 작성 가능. ROLLING UP 함수, CUBE 함수, GROUPING SETS 함수가 포함됨. 정렬이 필요한 경우 ORDER BY 절에 정렬 칼럼 명시.
3. WINDOW FUNCTION
분석 함수(ANALYTIC FUNCTION) 나 순위함수(RANK FUNTION) 로도 알려져 있는 윈도우 함수는 데이터웨어하우스에서 발전한 기능임.
### 2. ROLLUP 함수 
ROLLUP : ROLLUP에 지정된 Grouping Columns 의 List 는 Subtotal 을 생성하기 위해 사용되며,  n+1 level 의 subtotal 이 생성됨. 인수 계층구조, 인수 순서 변경 시 수행결과도 변경됨. GROUP BY ROLLUP (DNAME, JOB); ROLLUP이나 CUBE에 의한 소계가 계산된 결과에는 GROUPING(EXPR) = 1, 그렇지 않으면 GROUPING(EXPR)=0
#### [STEP1. 일반적인 GROUP BY 절 사용]
[예제] 부서명과 업무명을 기준으로 사원수와 급여 합을 집계한 일반적인 GROUP BY SQL 문장을 수행한다.
```
[예제] SELECT DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY DNAME, JOB;
```
```
[실행 결과] DNAME JOB Total Empl Total Sal --------- -------- ------- ------- SALES MANAGER 1 2850 SALES CLERK 1 950 ACCOUNTING MANAGER 1 2450 RESEARCH ANALYST 2 6000 ACCOUNTING CLERK 1 1300 SALES SALESMAN 4 5600 RESEARCH MANAGER 1 2975 ACCOUNTING PRESIDENT 1 5000 RESEARCH CLERK 2 1900 9개의 행이 선택되었다.
```
정렬이 필요한 경우 ORDER BY 에 명시적으로 정렬 칼럼 표시 필요.
#### [STEP1-2. GROUP BY 절+ ORDER BY 절 사용]
[예제] 부서명과 업무명을 기준으로 집계한 일반적인 GROUP BY SQL 문장에 ORDER BY 절을 사용함으로써 부서, 업무별로 정렬이 이루어진다.
```
[예제] SELECT DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY DNAME, JOB ORDER BY DNAME, JOB;
```
#### [STEP 2. ROLLUP 함수 사용]
```
[예제] 부서명과 업무명을 기준으로 집계한 일반적인 GROUP BY SQL 문장에 ROLLUP 함수를 사용한다.
```
```
[예제] SELECT DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY ROLLUP (DNAME, JOB);
```
```
[실행 결과] DNAME JOB Total Empl Total Sal ---------- -------- --------- -------- SALES CLERK 1 950 SALES MANAGER 1 2850 SALES SALESMAN 4 5600 SALES 6 9400 RESEARCH CLERK 2 1900 RESEARCH ANALYST 2 6000 RESEARCH MANAGER 1 2975 RESEARCH 5 10875 ACCOUNTING CLERK 1 1300 ACCOUNTING MANAGER 1 2450 ACCOUNTING PRESIDENT 1 5000 ACCOUNTING 3 8750 14 29025 13개의 행이 선택되었다.
```
실행 결과에서 2개의 GROUPING COLUMNS에 대하여 다음과 같은 추가 LEVEL  집계가 생성 된 것을 볼 수 있음. 
(L1 - GROUP BY 수행시 생성되는 표준 집계 (9건) L2 - DNAME 별 모든 JOB의 SUBTOTAL (3건) L3 - GRAND TOTAL (마지막 행, 1건))

추가로  L1, L2, L3 계층 내 정렬을 위해서는 별도의 ORDER BY 절을 사용.
#### [STEP 3. GROUPING 함수 사용]
GROUPING 함수 : ROLLUP, CUBE, GROUPING SETS 등을 지원하기 위해 추가된 함수. 
- ROLLUP이나 CUBE에 의한 소계가 계산된 결과에는 GROUPING(EXPR) = 1 이 표시.
 - 그 외의 결과에는 GROUPING(EXPR) = 0 이 표시.

GROUPING 함수와 CASE/DECODE를 이용해, 소계를 나타내는 필드에 원하는 문자열을 지정할 수 있어, 보고서 작성시 유용하게 사용가능.
[예제] ROLLUP 함수를 추가한 집계 보고서에서 집계 레코드를 구분할 수 있는 GROUPING 함수가 추가된 SQL 문장이다.
```
[예제] SELECT DNAME, GROUPING(DNAME), JOB, GROUPING(JOB), COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY ROLLUP (DNAME, JOB);
```
```
[실행 결과] DNAME ------ GROUPING(DNAME) -------------- JOB --- GROUPING(JOB) ----------- Total Empl -------- Total Sal ------ SALES 0 CLERK 0 1 950 SALES 0 MANAGER 0 1 2850 SALES 0 SALESMAN 0 4 5600 SALES 0 1 6 9400 RESEARCH 0 CLERK 0 2 1900 RESEARCH 0 ANALYST 0 2 6000 RESEARCH 0 MANAGER 0 1 2975 RESEARCH 0 1 5 10875 ACCOUNTING 0 CLERK 0 1 1300 ACCOUNTING 0 MANAGER 0 1 2450 ACCOUNTING 0 PRESIDENT 0 1 5000 ACCOUNTING 0 1 3 8750 1 1 14 29025 13개의 행이 선택되었다.
```
부서별, 업무별과 전체 집계를 표시한 레코드에서는 GROUPING 함수가 1을 리턴함. 그리고 전체 합계를 나타내는 결과 라인에서는 부서별 GROUPING 함수와 업무별 GROUPING 함수가 둘 다 1인 것을 알 수 있음.
#### [STEP 4. GROUPING+CASE]
[예제] ROLLUP 함수를 추가한 집계 보고서에서 집계 레코드를 구분할 수 있는 GROUPING 함수와 CASE 함수를 함께 사용한 SQL 문장을 작성한다.
```
[예제] SELECT CASE GROUPING(DNAME) WHEN 1 THEN 'All Departments' ELSE DNAME END AS DNAME, CASE GROUPING(JOB) WHEN 1 THEN 'All Jobs' ELSE JOB END AS JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY ROLLUP (DNAME, JOB); Oracle의 경우는 DECODE 함수를 사용해서 좀더 짧게 표현할 수 있다. SELECT DECODE(GROUPING(DNAME), 1, 'All Departments', DNAME) AS DNAME, DECODE(GROUPING(JOB), 1, 'All Jobs', JOB) AS JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY ROLLUP (DNAME, JOB);
```
```
[실행 결과] DNAME JOB Total Empl Total Sal ----------- -------- -------- -------- SALES CLERK 1 950 SALES MANAGER 1 2850 SALES SALESMAN 4 5600 SALES All Jobs 6 9400 RESEARCH CLERK 2 1900 RESEARCH ANALYST 2 6000 RESEARCH MANAGER 1 2975 RESEARCH All Jobs 5 10875 ACCOUNTING CLERK 1 1300 ACCOUNTING MANAGER 1 2450 ACCOUNTING PRESIDENT 1 5000 ACCOUNTING All Jobs 3 8750 All Departments All Jobs 14 29025 13개의 행이 선택되었다.
```
부서별과 전체 집계를 표시한 레코드에서 ‘ALL JOBS’와 ‘ALL DEPARTMENTS’라는 사용자 정의 텍스트를 확인가능.
#### [STEP 4-2. ROLLUP 함수 일부 사용]
[예제] GROUP BY ROLLUP (DNAME, JOB) 조건에서 GROUP BY DNAME, ROLLUP(JOB) 조건으로 변경한 경우.
```
[예제] SELECT CASE GROUPING(DNAME) WHEN 1 THEN 'All Departments' ELSE DNAME END AS DNAME, CASE GROUPING(JOB) WHEN 1 THEN 'All Jobs' ELSE JOB END AS JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY DNAME, ROLLUP(JOB)
```
```
[실행 결과] DNAME JOB Total Empl Total Sal ----------- -------- -------- -------- SALES CLERK 1 950 SALES MANAGER 1 2850 SALES SALESMAN 4 5600 SALES All Jobs 6 9400 RESEARCH CLERK 2 1900 RESEARCH ANALYST 2 6000 RESEARCH MANAGER 1 2975 RESEARCH All Jobs 5 10875 ACCOUNTING CLERK 1 1300 ACCOUNTING MANAGER 1 2450 ACCOUNTING PRESIDENT 1 5000 ACCOUNTING All Jobs 3 8750 12개의 행이 선택되었다.
```
결과 마지막 ALL DEPARTMENTS & ALL JOBS 줄만 계산이 되지 않았다. ROLLUP이 JOB 칼럼에만 사용되었기 때문에 DNAME에 대한 집계는 필요하지 않기 때문.

![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_222.jpg)
#### [STEP 4-3. ROLLUP 함수 결합 칼럼 사용]
[예제] JOB과 MGR는 하나의 집합으로 간주하고, 부서별, JOB & MGR에 대한 ROLLUP 결과를 출력한다.
```
[예제] SELECT DNAME, JOB, MGR, SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY ROLLUP (DNAME, (JOB, MGR)); ☞ JOB, MGR을 소계시 하나의 집합으로 간주하여 구분하지 않음
```
```
[실행 결과] DNAME JOB MGR Total Sal --------- --------- ---- ------ SALES CLERK 7698 950 SALES MANAGER 7839 2850 SALES SALESMAN 7698 5600 SALES 9400 RESEARCH CLERK 7788 1100 RESEARCH CLERK 7902 800 RESEARCH ANALYST 7566 6000 RESEARCH MANAGER 7839 2975 RESEARCH 10875 ACCOUNTING CLERK 7782 1300 ACCOUNTING MANAGER 7839 2450 ACCOUNTING PRESIDENT 5000 ACCOUNTING 8750 29025 14개의 행이 선택되었다.
```
ROLLUP 함수 사용시 괄호로 묶은 JOB과 MGR의 경우 하나의 집합(JOB+MGR) 칼럼으로 간주하여 괄호 내 각 칼럼별 집계를 구하지 않음.
### 3. CUBE 함수
CUBE : 결합 가능한 모든 값에 대하여 다차원 집계 생성. CUBE 사용시 Grouping Columns 의 순서를 변경하여 한번의 쿼리 더 추가. 인수들 간 평등한 관계. 순서 바뀌어도 상관 없음. 2의 N승 LEVEL의 Subtotal 생성. 정렬시 ORDER BY 절에 명시적으로 정렬 칼럼이 표시
#### [STEP 5. CUBE 함수 이용]
[예제] GROUP BY ROLLUP (DNAME, JOB) 조건에서 GROUP BY CUBE (DNAME, JOB) 조건으로 변경해서 수행한다.
```
[예제] SELECT CASE GROUPING(DNAME) WHEN 1 THEN 'All Departments' ELSE DNAME END AS DNAME, CASE GROUPING(JOB) WHEN 1 THEN 'All Jobs' ELSE JOB END AS JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY CUBE (DNAME, JOB) ;
```
```
[실행 결과] DNAME JOB Total Empl Total Sal ------------- --------- --------- -------- All Departments All Jobs 14 29025 All Departments CLERK 4 4150 All Departments ANALYST 2 6000 All Departments MANAGER 3 8275 All Departments SALESMAN 4 5600 All Departments PRESIDENT 1 5000 SALES All Jobs 6 9400 SALES CLERK 1 950 SALES MANAGER 1 2850 SALES SALESMAN 4 5600 RESEARCH All Jobs 5 10875 RESEARCH CLERK 2 1900 RESEARCH ANALYST 2 6000 RESEARCH MANAGER 1 2975 ACCOUNTING All Jobs 3 8750 ACCOUNTING CLERK 1 1300 ACCOUNTING MANAGER 1 2450 ACCOUNTING PRESIDENT 1 5000 18개의 행이 선택되었다.
```
CUBE는  GROUPING COLUMNS의 수가 N이라고 가정하면, 2의 N승 LEVEL의 Subtotal을 생성하게 된다.    

실행 결과에서 CUBE 함수 사용으로 ROLLUP 함수의 결과에다 업무별 집계까지 추가해서 출력할 수 있는데, ROLLUP 함수에 비해 업무별 집계를 표시한 5건의 레코드가 추가된 것을 확인가능. (All Departments - CLERK, ANALYST, MANAGER, SALESMAN, PRESIDENT 별 집계가 5건 추가)
#### [STEP 5-2. UNION ALL 사용 SQL]

UNION ALL은 Set Operation 내용으로, 여러 SQL 문장을 연결하는 역할을 할 수 있음. 
```
[예제] SELECT DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY DNAME, JOB UNION ALL SELECT DNAME, 'All Jobs', COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY DNAME UNION ALL SELECT 'All Departments', JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY JOB UNION ALL SELECT 'All Departments', 'All Jobs', COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO ;
```
CUBE 함수를 사용하면서 가장 크게 개선되는 부분은 CUBE 사용 전 SQL에서 EMP, DEPT 테이블을 네 번이나 반복 액세스하는 부분을 CUBE 사용 SQL에서는 한 번으로 줄일 수 있음.
### 4. GROUPING SETS 함수
GROUPING SETS : GROUP BY SQL 문장을 여러번 반복하지 않아도 원하는 결과를 쉽게 얻을 수 있음. 인수의 순서가 바뀌어도 결과는 같음. 정렬 시 ORDER BY 절에 명시적으로 표시필요.
#### [가.일반 그룹함수를 이용한 SQL]
[예제] 일반 그룹함수를 이용하여 부서별, JOB별 인원수와 급여 합을 구하라.
```
[예제] SELECT DNAME, 'All Jobs' JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY DNAME UNION ALL SELECT 'All Departments' DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY JOB ;
```
```
[실행 결과] DNAME JOB Total Empl Total Sal ---------- ------- -------- ------ ACCOUNTING All Jobs 3 8750 RESEARCH All Jobs 5 10875 SALES All Jobs 6 9400 All Departments CLERK 4 4150 All Departments SALESMAN 4 5600 All Departments PRESIDENT 1 5000 All Departments MANAGER 3 8275 All Departments ANALYST 2 6000 8개의 행이 선택되었다.
```
#### [나.GROUPING SETS 사용 SQL]
[예제] 일반 그룹함수를 GROUPING SETS 함수로 변경하여 부서별, JOB별 인원수와 급여 합을 구하라.
```
[예제] SELECT DECODE(GROUPING(DNAME), 1, 'All Departments', DNAME) AS DNAME, DECODE(GROUPING(JOB), 1, 'All Jobs', JOB) AS JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY GROUPING SETS (DNAME, JOB);
```
```
[실행 결과] DNAME JOB Total Empl Total Sal ---------------- ---------- -------- ------- All Departments CLERK 4 4150 All Departments SALESMAN 4 5600 All Departments PRESIDENT 1 5000 All Departments MANAGER 3 8275 All Departments ANALYST 2 6000 ACCOUNTING All Jobs 3 8750 RESEARCH All Jobs 5 10875 SALES All Jobs 6 9400 8개의 행이 선택되었다.
```
GROUPING SETS 함수 사용시 UNION ALL을 사용한 일반 그룹함수를 사용한 SQL과 같은 결과를 얻을 수 있으며, 괄호로 묶은 집합 별로 집계를 구할 수 있음.
#### [다. GROUPING SETS 사용 SQL - 순서 변경]
[예제] 일반 그룹함수를 GROUPING SETS 함수로 변경하여 부서별, JOB별 인원수와 급여 합을 구하는데 GROUPING SETS의 인수들의 순서를 바꾸어 본다.
```
[예제] SELECT DECODE(GROUPING(DNAME), 1, 'All Departments', DNAME) AS DNAME, DECODE(GROUPING(JOB), 1, 'All Jobs', JOB) AS JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY GROUPING SETS (JOB, DNAME);
```
JOB과 DNAME의 순서가 바뀌었지만 결과는 같음.
#### [라. 3개의 인수를 이용한 GROUPING SETS 이용]
[예제] 부서-JOB-매니저 별 집계와, 부서-JOB 별 집계와, JOB-매니저 별 집계를 GROUPING SETS 함수를 이용해서 구해본다.
```
[예제] SELECT DNAME, JOB, MGR, SUM(SAL) "Total Sal" FROM EMP, DEPT WHERE DEPT.DEPTNO = EMP.DEPTNO GROUP BY GROUPING SETS ((DNAME, JOB, MGR), (DNAME, JOB), (JOB, MGR)); GROUPING SETS 함수 사용시 괄호로 묶은 집합별로(괄호 내는 계층구조가 아닌 하나의 데이터로 간주함) 집계를 구할 수 있다.
```
```
[실행 결과] DNAME JOB MGR Total Sal ----------- ---------- ------- ------- SALES CLERK 7698 950 ACCOUNTING CLERK 7782 1300 RESEARCH CLERK 7788 1100 RESEARCH CLERK 7902 800 RESEARCH ANALYST 7566 6000 SALES MANAGER 7839 2850 RESEARCH MANAGER 7839 2975 ACCOUNTING MANAGER 7839 2450 SALES SALESMAN 7698 5600 ACCOUNTING PRESIDENT 5000 CLERK 7698 950 CLERK 7782 1300 CLERK 7788 1100 CLERK 7902 800 ANALYST 7566 6000 MANAGER 7839 8275 SALESMAN 7698 5600 PRESIDENT 5000 SALES MANAGER 2850 SALES CLERK 950 ACCOUNTING CLERK 1300 ACCOUNTING MANAGER 2450 ACCOUNTING PRESIDENT 5000 RESEARCH MANAGER 2975 SALES SALESMAN 5600 RESEARCH ANALYST 6000 RESEARCH CLERK 1900 27개의 행이 선택되었다.
```
실행 결과에서 첫 번째 10건의 데이터는 (DNAME+JOB+MGR) 기준의 집계이며, 두 번째 8건의 데이터는 (JOB+MGR) 기준의 집계이며, 세 번째 9건의 데이터는 (DNAME+JOB) 기준의 집계. 
## 윈도우 함수 
### 1. WINDOW FUNCTION 개요 
WINDOW 함수 : 복잡한 SQL 문을 작성해야 하는 것을 부분적이나마 행과 행간의 관계를 쉽게 정의하기 위해 만든 함수, 중첩해서 사용하지 못하지만 서브쿼리에서는 사용 가능. 
#### WINDOW FUNCTION 종류
- 그룹 내 순위 함수 : RANK, DENSE_RANK,ROW_NUMBER 
RANK : 특정 항목(칼럼)에 대한 순위를 구하는 함수. 동일한 값에 대해서는 동일한 순위.  
 DENSE_RANK : 동일한 순위를 하나의 건수로 취급.   ROW_NUMBER : 동일한 값이라도 고유한 순위를 부여
- 그룹 내 집계 함수 : SUM,MAX,MIN,AVG,COUNT (SQL서버 경우 ODERBY 구문 지원X)
- 그룹 내 행 순서 관련 함수 : FIRST_VALUE,LAST_VALUE,LAG,LEAD 
- 그룹 내 비율 관련 함수 : CUME_DIST, PERCENT_RANK, NTILE, RATIO_TO_REPORT 
#### WINDOW FUNCTION SYNTAX 
WINDOW 함수에는 OVER 문구가 키워드로 필수 포함됨.
```
SELECT WINDOW_FUNCTION (ARGUMENTS) OVER ( [PARTITION BY 칼럼] [ORDER BY 절] [WINDOWING 절] ) FROM 테이블 명
```
- WINDOW_FUNCTION : 기존에 사용하던 함수, 새롭게 WINDOW 함수용으로 추가된 함수도 있음.
- ARGUMENTS (인수) : 함수에 따라 0~N개의 인수가 지정 될 수 있음.
- PARTITION BY 절 : 전체 집합을 기준에 의해 소그룹으로 나눌 수 있음.
- ORDER BY 절 : 어떤 항목에 따라 순위지정.
- WINDOWING 절 : 함수에 대상이 되는 행 기준 범위를 강력히 지정.
- ROWS : 물리적인 결과 행의 수
- RANGE : 논리적인 값에 의한 범위를 나타냄 
### 2. 그룹 내 순위 함수
RANK 함수 : ORDER BY 를 포함한 QUERY 문에서 특정 항목에 대한 순위를 구하는 함수. 
DENSE_RANK 함수 : 동일한 순위를 하나의 건수로 취급하는 점에서 RANK 함수와 다름. 
ROW_NUMBER 함수 : 동일한 값이라도 고유한 순위를 부여.
```
SELECT JOB, ENAME, SAL,
             RANK() OVER (ORDER BY SAL DESC) ALL RANK,
             RANK() OVER (PARTTION BY JOB ORDER BY DESC) GOB_RANK
FROM EMP :
```
```
RANK : 1 2 2 4 
DENSE_RANK : 1 2 2 3
ROW_NUMBER : 1 2 3 4
```
SELECT DENSE_RANK() OVER (ORDER BY SAL DESC) DENSE_RANK 
SELECT ROW_NUMBER() OVER (ORDER BY SAL DESC) ROW_NUMBER
### 3. 일반 집계 함수
#### [가. SUM 함수]
SUM 함수를 통해 파티션 별 윈도우 의 합을 구할 수 있음.
[예제] 사원들의 급여와 같은 매니저를 두고 있는 사원들의 SALARY 합을 구한다.
```
SELECT MGR, ENAME, SAL, SUM(SAL) OVER ( PARTITION BY MGR ORDER BY SAL RANGE UNBOUNDED PRECEDING ) AS MGR_SUM   FROM   EMP ; 
```
#### [나. MAX 함수]
MAX 함수를 사용하여 파티션 별 윈도우의 최대값을 구함.
[예제] 사원들의 급여와 같은 매니저를 두고 있는 사원들의 SALARY 중 최대값을 같이 구한다.
```
[예제] SELECT MGR, ENAME, SAL, MAX(SAL) OVER (PARTITION BY MGR) as MGR_MAX FROM EMP;
```
실행시 파티션 내 최대값을 모든 행에서 MRG_MAX 라는 칼럼 값을 가질 수 있음.
#### [다. MIN 함수]
MIN 함수를 사용하여 파티션별 윈도우 최소값을 구함.

[예제] 사원들의 급여와 같은 매니저를 두고 있는 사원들을 입사일자를 기준으로 정렬하고, SALARY 최소값을 같이 구한다.
```
[예제] SELECT MGR, ENAME, HIREDATE, SAL, MIN(SAL) OVER(PARTITION BY MGR ORDER BY HIREDATE) as MGR_MIN FROM EMP;
```
#### [라. AVG 함수]
조건에 맞는 데이터 통계 값을 구할 수 있음.

[예제] EMP 테이블에서 같은 매니저를 두고 있는 사원들의 평균 SALARY를 구하는데, 조건은 같은 매니저 내에서 자기 바로 앞의 사번과 바로 뒤의 사번인 직원만을 대상으로 한다.
```
[예제] SELECT MGR, ENAME, HIREDATE, SAL, ROUND (AVG(SAL) OVER (PARTITION BY MGR ORDER BY HIREDATE ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING)) as MGR_AVG FROM EMP; ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING : 현재 행을 기준으로 파티션 내에서 앞의 한 건, 현재 행, 뒤의 한 건을 범위로 지정한다. (ROWS는 현재 행의 앞뒤 건수를 말하는 것임)
```
결과적으로 평균값 집계 대상은 본인의 데이터와 뒤의 한 건으로 평균값을 구한다. (1600 + 1250) / 2 = 1425의 값을 가진다.  
 TURNER: 앞의 한건과, 본인의 데이터와, 뒤의 한 건으로 평균값을 구한다. (1250 + 1500 + 1250) / 3 = 1333의 값을 가진다.   
 JAMES:  파티션 내에서 마지막 데이터이므로 뒤의 한 건을 제외한, 앞의 한 건과 본인의 데이터를 가지고 평균값을 구한다. (1250 + 950) / 2 = 1100의 값을 가진다.

#### [마. COUNT 함수]
COUNT 함수와 파티션별 ROWS 윈도우를 통해 이용하는 조건에 맞는 데이터 통계값을 구할 수 있다. 

[예제] 사원들을 급여 기준으로 정렬하고, 본인의 급여보다 50 이하가 적거나 150 이하로 많은 급여를 받는 인원수를 출력하라.
```
[예제] SELECT ENAME, SAL, COUNT(*) OVER (ORDER BY SAL RANGE BETWEEN 50 PRECEDING AND 150 FOLLOWING) as SIM_CNT FROM EMP; RANGE BETWEEN 50 PRECEDING AND 150 FOLLOWING : 현재 행의 급여값을 기준으로 급여가 -50에서 +150의 범위 내에 포함된 모든 행이 대상이 된다. (RANGE는 현재 행의 데이터 값을 기준으로 앞뒤 데이터 값의 범위를 표시하는 것임)
```
### 4. 그룹 내 행 순서함수
#### [가. FIRST_VALUE 함수]
파티션별 윈도우에서 가장 먼저 나온 값을 구한다. (SQL server 에서는 지원하지 않음) MIN 함수를 이용하여 같은결과를 구할 수 있음.  
[예제] 부서별 직원들을 연봉이 높은 순서부터 정렬하고, 파티션 내에서 가장 먼저 나온 값을 출력한다.
```
[예제] SELECT DEPTNO, ENAME, SAL, FIRST_VALUE(ENAME) OVER (PARTITION BY DEPTNO ORDER BY SAL DESC ROWS UNBOUNDED PRECEDING) as DEPT_RICH FROM EMP; RANGE UNBOUNDED PRECEDING : 현재 행을 기준으로 파티션 내의 첫 번째 행까지의 범위를 지정한다.
```
#### [나. LAST_VALUE 함수]
LAST_VALUE 함수를 이용해 파티션별 윈도우에서 가장 나중에 나온 값을 구한다. MAX 함수를 활용하여 같은 결과 얻을 수 있음.  

[예제] 부서별 직원들을 연봉이 높은 순서부터 정렬하고, 파티션 내에서 가장 마지막에 나온 값을 출력한다.
```
[예제] SELECT DEPTNO, ENAME, SAL, LAST_VALUE(ENAME) OVER (PARTITION BY DEPTNO ORDER BY SAL DESC ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) as DEPT_POOR FROM EMP; ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING: 현재 행을 포함해서 파티션 내의 마지막 행까지의 범위를 지정한다.
```
만일 공동 등수가 있을 경우를 의도적으로 정렬하고 싶다면 별도의 정렬 조건을 가진 INLINE VIEW를 사용하거나, OVER () 내의 ORDER BY 조건에 칼럼을 추가해야 함.
#### [다. LAG 함수]
LAG 함수를 이용하여 파티션별 윈도우에서 이전 몇번째 행의 값을 가져 올 수 있다. (SQL server 지원 X)    

[예제] 직원들을 입사일자가 빠른 기준으로 정렬을 하고, 본인보다 입사일자가 한 명 앞선 사원의 급여를 본인의 급여와 함께 출력한다.
```
[예제] SELECT ENAME, HIREDATE, SAL, LAG(SAL) OVER (ORDER BY HIREDATE) as PREV_SAL FROM EMP WHERE JOB = 'SALESMAN' ;
```
NVL이나 ISNULL 기능과 같다.
#### [라. LEAD 함수]
LEAD 함수를 이용해 파티션별 윈도우에서 이후 몇 번째 행의 값을 가져올 수 있다.( SQL Server에서는 지원X)    

[예제] 직원들을 입사일자가 빠른 기준으로 정렬을 하고, 바로 다음에 입사한 인력의 입사일자를 함께 출력한다.
```
[예제] SELECT ENAME, HIREDATE, LEAD(HIREDATE, 1) OVER (ORDER BY HIREDATE) as "NEXTHIRED" FROM EMP;
```
### 5. 그룹 내 비율 함수
#### [가. RATIO_TO_REPORT 함수]
RATIO_TO_REPORT 함수를 이용해 파티션 내 전체 SUM(칼럼)값에 대한 행별 칼럼 값의 백분율을 소수점으로 구할 수 있음. 결과 값은 > 0 & <= 1 의 범위를 가짐. 그리고 개별 RATIO의 합을 구하면 10이됨    
[예제] JOB이 SALESMAN인 사원들을 대상으로 전체 급여에서 본인이 차지하는 비율을 출력한다.
```
[예제] SELECT ENAME, SAL, ROUND(RATIO_TO_REPORT(SAL) OVER (), 2) as R_R FROM EMP WHERE JOB = 'SALESMAN';
```
실행 결과에서 전체 값은 1650 + 1250 + 1250 + 1500 = 5600이 되고, RATIO_TO_REPORT 함수 연산의 분모로 사용된다. 그리고 개별 RATIO의 전체 합을 구하면 1이 되는 것을 확인할 수 있다.
#### [나. PERCENT_RANK 함수]
PERCENT_RANK 함수를 이용해 파티션별 윈도우에서 제일 먼저 나오는 것을 0으로, 제일 늦게 나오는 것을 1로 하여, 값이 아닌 행의 순서별 백분율을 구한다. 결과 값은 >= 0 & <= 1 의 범위를 가진다.   

[예제] 같은 부서 소속 사원들의 집합에서 본인의 급여가 순서상 몇 번째 위치쯤에 있는지 0과 1 사이의 값으로 출력한다.
```
[예제] SELECT DEPTNO, ENAME, SAL, PERCENT_RANK() OVER (PARTITION BY DEPTNO ORDER BY SAL DESC) as P_R FROM EMP;
```
#### [다. CUME_DIST 함수]
CUME_DIST 함수를 이용해 파티션별 윈도우의 전체건수에서 현재 행보다 작거나 같은 건수에 대한 누적백분율을 구한다. 결과 값은 > 0 & <= 1 의 범위를 가진다. 참고로 SQL Server에서는 지원하지 않는 함수이다.  

[예제] 같은 부서 소속 사원들의 집합에서 본인의 급여가 누적 순서상 몇 번째 위치쯤에 있는지 0과 1 사이의 값으로 출력한다.
```
[예제] SELECT DEPTNO, ENAME, SAL, CUME_DIST() OVER (PARTITION BY DEPTNO ORDER BY SAL DESC) as CUME_DIST FROM EMP;
```
#### [라. NTILE 함수]
NTILE 함수를 이용해 파티션별 전체 건수를 ARGUMENT 값으로 N 등분한 결과를 구할 수 있다.  

[예제] 전체 사원을 급여가 높은 순서로 정렬하고, 급여를 기준으로 4개의 그룹으로 분류한다.
```
[예제] SELECT ENAME, SAL, NTILE(4) OVER (ORDER BY SAL DESC) as QUAR_TILE FROM EMP
```

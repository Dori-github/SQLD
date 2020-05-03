# 2-1. SQL 기본 및 활용 - SQL기본
## WHWERE 절 
### 1. WHERE 조건절 개요 
WHERE 절 : 원하는 자료를 검색하기 위하여 자료들을 제한하는 것. 
```
SELECT [DISTINCT/ALL] 칼럼명 [ALIAS명] FROM 테이블명 WHERE 조건식;
```
#### 조건식 
- 칼럼명 (보통 조건식 좌측에 위치)
- 비교연산자
- 문자,숫자 표현식(보통 조건식 우측에 위치)
- 비교칼럼명(JOIN사용시)
### 2. 연산자의 종류
#### [가. 연산자의 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_172.jpg)
#### [나. 연산자의 우선순위]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_173.jpg)
```
SELECT PLAYER_NAME 선수명   
FROM PLAYER  
WHERE TEAM_ID = ‘K2’; -> 팀ID가 K2인 사람   
WHERE TEAM_ID IN (‘K2’,‘K7’); -> K2,K7인 사람  
WHERE HEIGHT BETWEEN 170 AND 180;  
-> 키가 170 ~ 180인 사람  
WHERE POSITION IS NULL; -> 포지션 없는 사람.
```
### 3. 비교 연산자
#### [가. 비교연산자 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_174.jpg)
[요구사항] 소속팀이 삼성블루윙즈이거나 전남드래곤즈에 소속된 선수들이어야 하고, 포지션이 미드필더(MF:Midfielder)이어야 한다. 키는 170 센티미터 이상이고 180 이하여야 한다.

```
[예제 ]1) 소속팀코드 = 삼성블루윙즈팀 코드(K02) 2) 소속팀코드 = 전남드래곤즈팀 코드(K07) 3) 포지션 = 미드필더 코드(MF) 4) 키 >= 170 센티미터 5) 키 <= 180 센티미터
```
CHAR /VARCHAR2와 같은 문자형 타입을 가진 칼럼을 특정 값과 비교하려면 인용 부호(작은따옴표/ 큰따옴표)로 묶어서 처리.
```
[예제] SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE TEAM_ID = 'K02' ;
```
```
[실행 결과] 선수이름 포지션 백넘버 키 ------ ---- ---- ---- 김성환 DF 5 183 가비 MF 10 177 강대희 MF 26 174 고종수 MF 22 176 고창F 4 175 정준 MF 44 170 정진우 DF 7 179 데니스 FW 11 176 서정원 FW 14 173 ：：：： 49개의 행이 선택되었다.
```
#### [나. 문자 유형 비교방법]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_175.jpg)

[예제] "키가 170 센티미터 이상"인 조건도 WHERE 절로 옮겨서 SQL 문장을 완성하여 실행한다.  
```
[예제] SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE HEIGHT >= 170;
```
```
[실행 결과] 선수이름 포지션 백넘버 키 ------ ---- ---- --- 김성환 DF 5 183 가비 MF 10 177 강대희 MF 26 174 고종수 MF 22 176 고창현 MF 8 170 정기범 MF 28 173 정동현 MF 25 175 정두현 MF 4 175 정준 MF 44 170 정진우 DF 7 179 데니스 FW 11 176 ：：：： 439개의 행이 선택되었다.
```

### 4. SQL 연산자
#### [가. SQL 연산자의 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_176.jpg)
앞의 요구사항을 비교연산자 로 SQL 연산자를 적용하여 표현하면, 
```
1) 소속팀코드 IN (삼성블루윙즈 코드(K02), 전남드래곤즈 코드(K07)) 2) 포지션 LIKE 미드필더(MF) 3) 키 BETWEEN 170 센티미터 AND 180 센티미터
```
#### [나.IN 연산자]
[예제] 소속팀 코드와 관련된 IN (list) 형태의 SQL 비교 연산자를 사용하여 WHERE 절에 사용한다.
```
[예제] SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE TEAM_ID IN ('K02','K07');
```
```
[실행 결과] 선수이름 포지션 백넘버 키 ------ ---- ---- --- 데니스 FW 11 176 서정원 FW 14 173 손대호 DF 17 186 오규찬 MF 24 178 윤원일 MF 45 176 김동욱 MF 40 176 김회택 DF 서현옥 DF 정상호 DF 최철우 DF 정영광 GK 41 185 ：：：： 100개의 행이 선택되었다.
```
#### [다. LIKE 연산자]
```
[예제] SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE POSITION LIKE 'MF';
```
```
[예제] SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE POSITION LIKE 'MF';
```
```
[실행 결과] 선수이름 포지션 백넘버 키 ------ ----- ----- --- 가비 MF 10 177 강대희 MF 26 174 고종수 MF 22 176 고창현 MF 8 170 정기범 MF 28 173 정동현 MF 25 175 정두현 MF 4 175 정준 MF 44 170 ：：：： 162개의 행이 선택되었다
```
#### 와일드카드 
와일드카드 : 한개 혹은 0개 이상의 문자를 대신해서 사용하기 위한 특수문자. STRING 값으로 용이하게 사용.
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_177.jpg)
[예제] “장”씨 성을 가진 선수들의 정보를 조회하는 WHERE 절을 작성한다.
```
[예제] SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE PLAYER_NAME LIKE '장%';
```
```
[실행 결과] 선수이름 포지션 백넘버 키 ------ ---- ---- --- 장성철 MF 27 176 장윤정 DF 17 173 장서연 FW 7 180 장재우 FW 12 172 장대일 DF 7 184 장기봉 FW 12 180 장철우 DF 7 172 장형석 DF 36 181 장경진 DF 34 184 장성욱 MF 19 174 장철민 MF 24 179 장경호 MF 39 174 장동현 FW 39 178 13개의 행이 선택되었다.
```
#### [라.BETWEEN a AND b 연산자]
[예제] 세 번째로 키가 170 센티미터 이상 180센티미터 이하인 선수들의 정보를 BETWEEN a AND b 연산자를 사용하여 WHERE 절을 완성한다.
```
[예제] SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE HEIGHT BETWEEN 170 AND 180; BETWEEN a AND b는 범위에서 'a'와 'b'의 값을 포함하는 범위를 말하는 것이다.
```
```
[실행 결과] 선수이름 포지션 백넘버 키 ------ ---- ---- --- 장철우 DF 7 172 홍광철 DF 4 172 강정훈 MF 38 175 공오균 MF 22 177 정국진 MF 16 172 정동선 MF 9 170 최경규 MF 10 177 최내철 MF 24 177 배성재 MF 28 178 샴 MF 25 174 김관우 MF 8 175 ：：：： 259개의 행이 선택되었다.
```
#### [마. IS NULL 연산자]
NULL(ASCII 00) : 값이 존재하지 않는 것으로 확정되지 않는 값을 표현 할 때 사용. 공백이나 0과 달리 비교 자체가 불가능한 값.
#### NULL 값 특징
- NULL 값과의 수치연산은 NULL 값을 리턴한다. 
- NULL 값과의 비교연산은 거짓(FALSE)을 리턴한다. 
- 어떤 값과 비교할 수도 없으며, 특정 값보다 크다, 적다라고 표현할 수 없다
- 비교연산자를 통해서 비교 할 수 없다.
- NULL의 비교연산은 IS NULL, IS NOT NULL 이라는 문구를 사용해야 결과를 얻을 수 있다.

[예제] POSITION 칼럼(Column) 값이 NULL 값인지를 판단하기 위해서는 IS NULL을 사용하여 다음과 같이 SQL 문장을 수정하여 실행한다.
```
[예제] SELECT PLAYER_NAME 선수이름, POSITION 포지션, TEAM_ID FROM PLAYER WHERE POSITION IS NULL;
```
```
[실행 결과] 선수이름 포지션 TEAM_ID ------ ----- ------- 정학범 K08 안익수 K08 차상광 K08 3개의 행이 선택되었다.
```
### 5. 논리 연산자
논리연산자 : SQL 비교연산자들로 이루어진 여러개의 조건들을 논리적으로 연결시키기 위해서 사용되는 연산자.
#### [가. 논리 연산자의 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_178.jpg)
[예제] 예를 들어 “소속이 삼성블루윙즈”인 조건과 “키가 170 센티미터 이상”인 조건을 연결해 보면 “소속이 삼성블루윙즈이고 키가 170 센티미터 이상인 조건을 가진 선수들의 자료를 조회”하는 것이 되는 것이다.
```
[예제] SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE TEAM_ID = 'K02' AND HEIGHT >= 170;
```
```
[실행 결과] 선수이름 포지션 백넘버 키 ------ ---- ---- --- 김반코비 MF 47 185 김선우 FW 33 174 김여성 MF 36 179 김용우 FW 27 175 김종민 MF 30 174 박용훈 MF 9 175 김만근 FW 34 177 김재민 MF 35 180 김현두 MF 12 176 이성용 DF 20 173 하태근 MF 29 182 ：：：： 45개의 행이 선택되었다
```
[예제] 요구 사항을 하나씩 하나씩 AND, OR 같은 논리 연산자를 사용하여 DBMS가 이해할 수 있는 SQL 형식으로 질문을 변경한다. 요구 사항을 순서대로 논리적인 조건을 적용한다.
```
소속팀이 삼성블루윙즈이거나 전남드래곤즈에 소속된 선수들이어야 하고, 포지션이 미드필더(MF:Midfielder)이어야 한다. 키는 170 센티미터 이상이고 180 이하여야 한다. 1) 소속팀이 삼성블루윙즈 OR 소속팀이 전남드래곤즈 2) AND 포지션이 미드필더 3) AND 키는 170 센티미터 이상 4) AND 키는 180 센티미터 이하
```
```
[예제] SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE TEAM_ID = 'K02' OR TEAM_ID = 'K07' AND POSITION = 'MF' AND HEIGHT >= 170 AND HEIGHT <= 180;
```
```
[실행 결과] 선수이름 포지션 백넘버 키 ------ ----- ---- ---- 김성환 DF 5 183 가비 MF 10 177 강대희 MF 26 174 고종수 MF 22 176 고창현 MF 8 170 정기범 MF 28 173 정동현 MF 25 175 정두현 MF 4 175 정준 MF 44 170 정진우 DF 7 179 데니스 FW 11 176 ：：：： 66개의 행이 선택되었다.
```
#### [나. 논리 연산자의 순서 사례]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_179.jpg)
[예제] 잘못된 결과를 보여 준 SQL 문장을 괄호를 사용하여 다시 적용한다.
```
[예제] SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE (TEAM_ID = 'K02' OR TEAM_ID = 'K07') AND POSITION = 'MF' AND HEIGHT >= 170 AND HEIGHT <= 180;
```
```
[실행 결과] 선수이름 포지션 백넘버 키 ------ ---- ---- --- 가비 MF 10 177 강대희 MF 26 174 고종수 MF 22 176 고창현 MF 8 170 정기범 MF 28 173 정동현 MF 25 175 정두현 MF 4 175 정준 MF 44 170 오규찬 MF 24 178 윤원일 MF 45 176 김동욱 MF 40 176 ：：：： 33개의 행이 선택되었다.
```
### 6. 부정연산자
#### [가. 부정연산자의 종류]
![sql가이드](http://www.dbguide.net/publishing/img/knowledge/SQL_180.jpg)
[예제] 삼성블루윙즈 소속인 선수들 중에서 포지션이 미드필더(MF:Midfielder)가 아니고, 키가 175 센티미터 이상 185 센티미터 이하가 아닌 선수들의 자료를 찾아본다.
```
[예제] SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE TEAM_ID = 'K02' AND NOT POSITION = 'MF' AND NOT HEIGHT BETWEEN 175 AND 185;
```
```
[예제] Oracle 위의 SQL과 아래 SQL은 같은 내용을 나타내는 SQL이다. SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키 FROM PLAYER WHERE TEAM_ID = 'K02' AND POSITION <> 'MF' AND HEIGHT NOT BETWEEN 175 AND 185;
```
```
[실행 결과] 선수이름 포지션 백넘버 키 ------ ----- ----- --- 서정원 FW 14 173 손대호 DF 17 186 김선우 FW 33 174 이성용 DF 20 173 미트로 FW 19 192 최호진 GK 31 190 정유진 DF 37 188 손승준 DF 32 186 8개의 행이 선택되었다.
```
### 7. ROWNUM, TOP 사용
#### [가. ROWNUM]
ROWNUM : SQL 처리 결과 집합의 각 행에 대해 임시로 부여되는 일련번호이며, 테이블이나 집합에서 원하는 만큼의 행만 가져오고 싶을 때 WHERE 절에서 행의 개수를 제한하는 목적으로 사용. 
#### 행만 가져오고 싶을 때 
- SELECT PLAYER_NAME FROM PLAYER WHERE ROWNUM = 1;
- SELECT PLAYER_NAME FROM PLAYER WHERE ROWNUM <= 1; 이나 - SELECT PLAYER_NAME FROM PLAYER WHERE ROWNUM < 2;
#### 두건 이상의 N행
- SELECT PLAYER_NAME FROM PLAYER WHERE ROWNUM <= N;
- SELECT PLAYER_NAME FROM PLAYER WHERE ROWNUM
#### 테이블 내 고유한 키나 인덱스값 만들 때 
- UPDATE MY_TABLE SET COLUMN1 = ROWNUM;
#### [나. TOP 절 ]
TOP절 : 결과 집합으로 출력되는 행의 수 제한 
```
TOP (Expression) [PERCENT] [WITH TIES]
```
- Expression : 반환할 행의 수를 지정하는 숫자이다.
 - PERCENT : 쿼리 결과 집합에서 처음 Expression%의 행만 반환됨을 나타낸다.
  - WITH TIES : ORDER BY 절이 지정된 경우에만 사용할 수 있으며, TOP N(PERCENT)의 마지막 행과 같은 값이 있는 경우 추가 행이 출력되도록 지정할 수 있다.
#### 한건의 행만 
- SELECT TOP(1) PLAYER_NAME FROM PLAYER;
#### 두건의 이상 N행
- SELECT TOP(N) PLAYER_NAME FROM PLAYER;

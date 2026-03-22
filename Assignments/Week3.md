# SQL_ADVANCED 3주차 정규 과제 

📌SQL_ADVANCED 정규과제는 매주 정해진 분량의 『*혼자 공부하는 SQL*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **SQL_ADVANCED_3rd_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=1YmWy-7-OhQ&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=10
https://www.youtube.com/watch?v=tuQFkzjqEGw&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=11
https://www.youtube.com/watch?v=IOCsreDYqFE&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=12
-->

**교재 실습 예제 파일은 07_SQL_ADVANCED_Template 레포지토리의 src 폴더에 업로드되어 있습니다. market_db 파일도 해당 폴더에 함께 포함되어 있으니 참고하시기 바랍니다.**

**👀(수행 인증샷은 필수입니다.)** 

## SQL_ADVANCED_3rd_TIL

### 4장 SQL 고급 문법
#### 01. MySQL의 데이터 형식
#### 02. 두 테이블을 묶는 조인
#### 03. SQL 프로그래밍 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~99    | ✅         |
| 2주차 | p.102~155   | ✅         |
| 3주차 | p.158~213  | ✅         |
| 4주차 | p.216~271 | 🍽️         |
| 5주차 | p.274~327 | 🍽️         |
| 6주차 | p.330~369 | 🍽️         |
| 7주차 | p.372~407 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. MySQL의 데이터 형식

**각 데이터에 맞는 형식을 지정함으로써 효율적으로 데이터를 저장할 수 있다!**

### 1-1. 데이터 형식
#### 정수형
- 소수점이 없는 숫자를 저장
- 바이트 수에 따라 TINYINT < SMALLINT < INT < BIGINT -> 각 변수의 범위를 고려하여 효율적으로 선택하기
- 정수형에 UNSIGNED를 붙이면 범위가 0부터 시작되어 양수값만 가지지만, 바이트 수는 같게 유지됨
  - e.g., TINYINT: -128~+127의 값을 가지고 TINYINT UNSIGNED: 0~255의 값을 가짐
- SIGNED는 부호가 있는, UNSIGNED는 부호가 없는 정수형을 의미
#### 문자형
- 글자를 저장

| 데이터 형식 | 바이트 수 | 설명 |
|---|---|---|
| CHAR() | 1~255 | 고정길이 문자형 |   
| VARCHAR() | 1~16383 |가변길이 문자형 |
- CHAR()
  - 글자 크기가 고정되어있을때
  - 속도가 더 빠름
- VARCHAR()
  - 글자의 개수가 변동될 때
  - 최대 글자 크기가 정해져있으나 데이터의 길이에 따라 짧아질 수 있음
  - 공간 효율적으로 운영 가능 
- 숫자인데 문자형으로 저장하는 경우
  - 0으로 시작하는 데이터
  - 숫자로서의 의미를 가지지 않는(연산이나 순서의 의미가 없는) 데이터

#### 대량의 데이터 형식

| 데이터 형식 | 바이트 수 | 설명 |
|---|---|---|
| TEXT | 1~65535 | 대량의 텍스트 저장 |
| LONGTEXT | 1~4294967295 | 대량의 텍스트 저장|
| BLOB | 1~65535 | 대용량 이진 데이터(이미지, 동영상) 저장 |
| LONGBLOB | 1~4294967295 | 대용량 이진 데이터(이미지, 동영상) 저장 |

#### 실수형
- 소수점이 있는 숫자를 저장

| 데이터 형식 | 바이트 수 | 설명 |
|---|---|---|
| FLOAT | 4 | 소수점 아래 7자리까지 표현 |
| DOUBLE | 8 | 소수점 아래 15자리까지 표현 |

#### 날짜형
- 날짜 및 시간 저장

| 데이터 형식 | 바이트 수 | 설명 |
|---|---|---|
| DATE | 3 | **날짜만** 저장. YYYY-MM-DD 형식으로 사용 |
| TIME | 3 | **시간만** 저장. HH:MM:SS 형식으로 사용 |
| DATETIME | 8 | **날짜 및 시간**을 저장. YYYY-MM-DD HH:MM:SS 형식으로 사용 |

### 1-2. 변수의 사용
```sql
SET @변수이름 = 변수의 값 ; -- 변수의 선언 및 값 대입
SELECT @변수이름 ; -- 변수의 값 출력
```

- 변수는 워크벤치를 종료하면 없어짐 -> **임시적이다!**

e.g.,) 변수의 사용
```sql
SET @txt = '가수 이름==> ';
SET @height = 166 ;
SELECT @txt, mem_name FROM member WHERE height > @height ;
```
-> 가수 이름이 `SELECT`되고, height이 166보다 큰 데이터가 `WHERE`로 필터링됨.

e.g.,) LIMIT에서의 변수의 사용 - PREPARE과 EXECUTE
```sql
SET @count = 3;
PREPARE mySQL FROM 'SELECT mem_name, height FROM member ORDER BY height LIMIT ?';
EXECUTE mySQL USING @count; 
```
-> `LIMIT`에서는 변수 사용이 불가능함. 대신 `PREPARE`을 이용하여 mySQL이라는 이름으로 준비시켜놓고, `USING`으로 물음표값에 3을 대입하여 실행할 수 있음

### 1-3. 데이터 형 변환
#### 함수를 이용한 명시적인 변환
`CAST`와 `CONVERT`는 형식만 다를 뿐 동일한 기능을 함
```sql
CAST (값 AS 데이터 형식 [(길이)])
CONVERT (값, 데이터_형식[(길이)])
```
e.g.,) 정수형으로 바꾸기
```sql
SELECT CAST(AVG(price) AS SIGNED) '평균 가격' FROM buy;
```
-> 여기서 `SIGNED`는 부호있는 정수형을 의미함

e.g.,) 날짜형으로 바꾸기
```sql
SELECT CAST('2022$12$12' AS DATE);
```
-> $ 말고도 /, %, @ 등 다양한 구분자를 날짜형으로 변경 가능

e.g.,) `CONCAT` 이용하여 결과를 원하는 형태로 표기하기
```sql
SELECT num, CONCAT(CAST(price AS CHAR), 'X', CAST(amount AS CHAR), '='
'가격수량', price*amount '구매액
FROM buy;
```
-> 문자를 이어주는 함수인 `CONCAT`을 이용하여 숫자를 문자형으로 변환하여 표기

#### 암시적인 변환
```sql
SELECT '100' + '200'; -- 문자와 문자를 더함 -> 정수로 변환되어 연산됨
SELECT CONCAT('100', '200'); -- 문자와 문자를 연결 -> 문자로 처리
SELECT CONCAT(100, '200'); -- 정수와 문자를 연결 -> 정수가 문자로 변환되어 처리
```


> **확인문제: 다음 보기에서 데이터 형식의 변환에 사용되는 함수를 2개 고르세요.**

보기는 아래와 같습니다.
```
CONVERT() / DATA() / CAST() / MOVE() / TYPE() / SUM() / AVG() / CURRENT_DATE()
```

```
CONVERT(), CAST()
```


## 2. 두 테이블을 묶는 조인

### 2-1. (내부) 조인
#### 일대다 관계의 이해
- 분리된 테이블은 서로 **관계**를 맺고있음
- **기본 키(PK)**: 중복 불가, NULL 불가, 테이블당 하나만 존재
- **외래 키(FK)**: 중복 가능, NULL 가능, 테이블당 여러개 존재 가능
- 일대다 관계는 기본 키와 외래 키로 이루어진 'PK-FK 관계'임
#### 내부 조인의 기본
```sql
SELECT <열 목록>
FROM <첫 번째 테이블>
    INNER JOIN <두 번째 테이블> -- 그냥 JOIN이라 써도 INNER JOIN이라 인식함
    ON <조인될 조건>
[WHERE 검색 조건]
```

#### 내부 조인의 간결한 표현
테이블 밝히기의 필요성
```sql
SELECT mem_id, mem_name, prod_name, addr, CONCAT(phone1, phone2) '연락처'
-- mem_id -> buy.mem_id로 수정해야 에러 안뜸
FROM buy
    INNER JOIN member
    ON buy.mem_id = member.mem_id;
```
-> `mem_id`는 JOIN KEY로, 두 테이블에 모두 존재하니 어느 테이블의 mem_id를 추출할지 정확하게 작성해야함

별칭 만들기
```sql
SELECT B.mem_id, M.mem_name, B.prod_name, M.addr, CONCAT(M.phone1, M.phone2) '연락처'
FROM buy В
    INNER JOIN member M -- 테이블 이름에 별칭 붙임
    ON B.mem_id = M.mem_id;
```
-> 어느 테이블에서 온 칼럼인지 간결하게 밝히기 가능

### 2-2. 외부 조인
**내부 조인은 두 테이블에 모두 존재하는 데이터만 출력되나, 외부 조인은 한쪽에만 데이터가 있어도 그 결과가 출력됨!**
#### 외부 조인의 기본
```sql
SELECT <열 목록>
FROM <첫 번째 테이블(LEFT 테이블)>
    <LEFT RIGHT FULL> OUTER JOIN <두 번째 테이블(RIGHT 테이블)>
    ON <조인될 조건>
[WHERE 검색 조건];
```
- `LEFT OUTER JOIN`: 왼쪽 테이블에 있는 내용은 모두 출력됨
- `RIGHT OUTER JOIN`: 오른쪽 테이블에 있는 내용은 모두 출력됨
- `FULL OUTER JOIN`: 왼쪽&오른쪽 테이블에 있는 내용이 모두 출력됨

### 2-3. 기타 조인
#### 상호 조인
- 한쪽 테이블의 모든 행과 다른 쪽 테이블의 모든 행을 조인시킴
```sql
SELECT *
FROM buy
    CROSS JOIN member;
```
- 결과의 내용은 딱히 의미가 없음
- 대용량의 테스트용 데이터를 생성할 때 주로 사용
#### 자체 조인
- 하나의 테이블을 자기 자신과 조인 -> 테이블은 하나인데 그 안의 데이터들끼리 서로 연관성이 있을 때 사용
- 활용 예시: 조직도
```sql
SELECT A.emp "직원", B.emp "직속상관", B.phone "직속상관연락처"
FROM emp_table A
    INNER JOIN emp_table B -- 같은 테이블을 서로 다른 별칭으로 지정
    ON A.manager = B.emp
WHERE A.emp = '경리부장';
```


> **확인문제: 다음 SQL은 회원으로 가입만 하고, 한 번도 구매한 적이 없는 회원의 목록을 조회하는 쿼리입니다. 빈칸에 들어갈 가장 적절한 구문을 고르세요..**

```sql
SELECT DISTINCT M.mem_id, B.prod_name, M.mem_name, M.addr
  FROM member M
    LEFT OUTER JOIN buy B
    ON M.mem_id = B.mem_id
  __________
  ORDER BY M.mem_id;
```
보기는 아래와 같습니다.
```
1. JOIN B.prod_name IS NULL
2. LIMIT B.prod_name IS NULL
3. HAVING B.prod_name IS NULL
4. WHERE B.prod_name IS NULL
```
```
4. WHERE B.prod_name IS NULL
```

## 3. SQL 프로그래밍 
**스토어드 프로시저**란?
- MySQL에서 프로그래밍 기능이 필요할 때 사용하는 데이터베이스 개체 -> SQL 프로그래밍은 기본적으로 스토어드 프로시저 안에 만듦
- 스토어드 프로시저의 구조
    ```sql
    DELIMITER $$
    CREATE PROCEDURE 스토어드_프로시저_이름()
    BEGIN
    (이 부분에 SQL 프로그래밍 코딩)
    END $$ -- 스토어드 프로시저 종료
    DELIMITER ; -- 종료 문자를 다시 세미콜론(;)으로 변경
    CALL 스토어드_프로시저_이름(); -- 스토어드 프로시저 실행
    ```

### 3-1. IF문
#### IF문의 기본 형식
```sql
IF <조건식> THEN
    SQL문장들
END IF;
```
- 조건식이 참일 때만 SQL문장들을 실행
- SQL문장들이 두 문장 이상일 때는 BEGIN~END로 묶어줘야함
    ```sql
    DROP PROCEDURE IF EXISTS ifProс1; -- 만약 기존에 ifProc1 ()을 만든 적이 있다면 삭제
    DELIMITER $$
    CREATE PROCEDURE ifProc1() 
    BEGIN
        IF 100 = 100 THEN 2
            SELECT '100은 100과 같습니다.';
        END IF;
    END $$
    DELIMITER ;
    CALL ifProc1();  
    ```

#### IF ELSE 문
```sql
IF <조건식> THEN
    SQL문장들1
ELSE
    SQL문장들2
END IF;
```
- 조건식이 참이라면 'SQL문장들1'을 실행, 그렇지 않으면 'SQL문장들2'를 실행

#### IF문의 활용
아이디가 APN (에이핑크)인 회원의 데뷔 일자가 5년이 넘었는지 확인해보고 5년이 넘었으면 축하 메시지를 출력하기:
```sql
DROP PROCEDURE IF EXISTS ifProc3;
DELIMITER $$
CREATE PROCEDURE ifProc3()
BEGIN
    DECLARE debutDate DATE; -- 데뷔 일자
    DECLARE curDate DATE; -- 오늘
    DECLARE days INT; -- 활동한 일수

    SELECT debut_date INTO debutDate 
    FROM market_db.member
    WHERE mem_id = 'APN';

    SET curDate = CURRENT_DATE(); -- 현재 날짜
    SET days = DATEDIFF(curDate, debutDate); -- 날짜의 차이, 일 단위

    IF (days/365) > 5 THEN -- 5년이 지났다면
        SELECT CONCAT('데뷔한 지', days, '일이나 지났습니다. 핑순이들 축하합니다!');
    ELSE
        SELECT '데뷔한 지' + days + '일밖에 안되었네요. 핑순이들 화이팅~';
    END IF;
END $$
DELIMITER;
CALL ifProc3();
```
- `DECLARE`: 프로시저 안에서 변수를 정식으로 선언(변수 타입 지정해야함)
- `INTO`: `debut_date`를 출력하지 않고 `debutDate` 변수 안에 저장
- `CURRENT_DATE()`: 현재날짜
- `DATEDIFF(날짜1, 날짜2)`: 날짜2부터 날짜1까지 일수로 몇일인지 알려줌

### 3-2. CASE문
#### CASE 문의 기본 형식
```sql
CASE
    WHEN 조건1 THEN
        SQL문장들1
    WHEN 조건2 THEN
        SQL문장들2
    WHEN 조건3 THEN
        SQL문장들3
    ELSE -- 모든 조건에 해당하지 않을 때
        SQL문장들4
END CASE;
```
- IF 문은 참 아니면 거짓 두 가지만 있으나, CASE문은 2가지 이상의 여러가지 경우일 때 처리 가능

#### CASE 문의 활용
총 구매액에 따라 회원 등급을 구분하기
```sql

SELECT M.mem_id, M.mem_name, SUM(price*amount) "총구매액",
    CASE
        WHEN (SUM(price*amount) >= 1500) THEN '최우수고객'
        WHEN (SUM(price*amount) >= 1000) THEN '우수고객'
        WHEN (SUM(price*amount) >= 1 ) THEN '일반고객
        ELSE '유령고객
    END "회원등급" -- 열 이름을 "회원등급"으로 지정
FROM buy В
    RIGHT OUTER JOIN member M -- 구매수가 0인 회원도 출력
    ON B.mem id = M.mem id
GROUP BY M.mem id
ORDER BY SUM(price*amount) DESC
```
### 3-3. WHILE 문
#### WHILE 문의 기본 형식
```sql
WHILE <조건식> DO
    SQL 문장들
END WHILE;
```
- **조건식이 참인 동안**에 'SQL문장들'을 계속 반복

#### WHILE 문의 응용
- `ITERATE [레이블]`: 지정한 레이블로 가서 계속 진행
- `LEAVE [레이블]`: 지정한 레이블을 빠져나감. 즉 WHILE 문이 종료됨.
- e.g.,) 1에서 100까지의 합계에서 4의 배수를 제외시키고, 1000이 넘는 순간 그 숫자를 출력한 후 프로그램 종료시키기
    ```sql
    DROP PROCEDURE IF EXISTS whileProc2;
    DELIMITER $$
    CREATE PROCEDURE whileProc2()
    BEGIN
        DECLARE i INT; -- 1에서 100까지 증가할 변수
        DECLARE hap INT; -- 더한 값을 누적할 변수
        SET i = 1;
        SET hap = 0;

        myWhile: -- WHILE문의 이름을 지정
        WHILE (i <= 100) DO
            IF (i%4 = 0) THEN
                SET i = i + 1;
                ITERATE myWhile; -- 지정한 label 문으로 가서 계속 진행
            END IF; -- 4의 배수가 아니면 IF문을 바로 빠져나와 hap + i가 실행
            SET hap = hap + i;
            IF (hap > 1000) THEN
                LEAVE myWhile; -- 지정한 label 문을 떠남. 즉 WHILE 종료
            END IF; 
            SET i = i + 1;
        END WHILE;
    
        SELECT '1부터 100까지의 합(4의 배수 제외), 1000 넘으면 종료 ==>', hap;
    END $$
    DELIMITER;
    CALL whileProc2();
    ```

### 3-4. 동적 SQL
 **동적 SQL**: 미리 SQL을 준비한 후에 나중에 실행하는 것
#### PREPARE와 EXECUTE
- `PREPARE`: SQL 문을 실행하지는 않고 미리 준비만 해둠
- `EXECUTE`: 준비한 SQL 문을 실행
- `DEALLOCATE PREPARE`: 실행 후 문장을 해제
##### `PREPARE`문에서는 ?로 향후 입력될 값을 비워 놓고, `EXECUTE`에서는 `USING`으로 ?에 값을 전달함

> **확인문제: 다음은 CASE 문의 형식입니다. 빈칸에 들어갈 가장 적절한 명령어를 보기에서 고르세요..**

```sql
CASE
    (1) 조건 THEN
        SQL문장들1
    ELSE
        SQL문장들4
END (2);
```

보기는 아래와 같습니다.
```
WHEN / THEN / CURRENT / DATE / TIME / IF / END IF / CASE
```

```
(1) IF
(2) CASE
```


---

# 2️⃣ 실습과제

## 1. 데이터베이스 구축

아래 코드를 MySQL Workbench에 붙여넣은 후,  
**전체 드래그 → 실행 (Ctrl + shift + Enter)** 하여 데이터베이스를 구축하세요.

```sql
-- 1. 데이터베이스 생성
CREATE DATABASE IF NOT EXISTS week3_db;

-- 2. 사용할 데이터베이스 선택
USE week3_db;

-- 3. 기존 테이블 삭제 (초기화용)
DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS customers;

-- 4. 테이블 생성 (조인 실습용)
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(20),
    signup_date_str VARCHAR(8) 
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,           
    order_date_str VARCHAR(8), 
    amount_str VARCHAR(10)     
);

-- 5. 데이터 삽입
INSERT INTO customers VALUES
(1, '진아', '20240218'),
(2, '혜인', '20230302'),
(3, '규서', '20220315'),
(4, '규영', '20210401'),
(5, '철원', '20230909'),
(6, '예운', '20240201'),
(7, '민서', '20220320'),
(8, '광윤', '20240105'); -- 주문 없는 고객(외부 조인용)

INSERT INTO orders VALUES
(101, 1, '20240220', '12000'),
(102, 1, '20240303', '30000'),
(103, 2, '20240111', '15000'),
(104, 3, '20221201', '9000'),
(105, 5, '20231111', '20000'),
(106, 7, '20220707', '5000'),
(107, 99, '20240210', '7000'); -- 고객 테이블에 없는 customer_id (외부 조인용)
```

## 2. 실습 문제

다음 SQL 문을 작성하고 실행 결과를 확인 후 인증 사진을 아래에 업로드하세요.

1. **데이터 형식 변환**
   - orders 테이블의 `order_date_str`을 DATE 형식으로 변환하여 조회하시오.
   (힌트: STR_TO_DATE 사용)

2. **데이터 형식 변환**
   - orders 테이블의 `amount_str`을 숫자형으로 변환하여 조회하시오.

3. **내부 조인 (INNER JOIN)**
   - customers와 orders를 customer_id 기준으로 내부 조인하여
     고객 이름(name)과 주문 번호(order_id)를 함께 조회하시오.

4. **외부 조인 (LEFT JOIN)**
   - customers를 기준으로 LEFT JOIN을 수행하여,
     주문이 없는 고객도 함께 조회하시오.

5. **스토어드 프로시저 (IF문 사용)**
   - 입력받은 금액이 10000 이상이면 '고액 주문',
     그렇지 않으면 '일반 주문'을 출력하는
     프로시저를 생성하시오.
   - 생성 후 CALL로 실행 결과를 확인하시오.


![alt text](image-17.png)


### 🎉 수고하셨습니다.







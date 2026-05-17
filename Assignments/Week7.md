# SQL_ADVANCED 7주차 정규 과제 

📌SQL_ADVANCED 정규과제는 매주 정해진 분량의 『*혼자 공부하는 SQL*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **SQL_ADVANCED_7th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=KZmW6VaY5BU&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=16
https://www.youtube.com/watch?v=vWTDuoSG-YQ&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=17
https://www.youtube.com/watch?v=aiMSluMNzI8&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=18
-->

**교재 실습 예제 파일은 07_SQL_ADVANCED_Template 레포지토리의 src 폴더에 업로드되어 있습니다. market_db 파일도 해당 폴더에 함께 포함되어 있으니 참고하시기 바랍니다.**

**👀(수행 인증샷은 필수입니다.)** 

## SQL_ADVANCED_7th_TIL

### 8장 SQL과 파이썬 연결 
#### 01. 파이썬 개발 환경 준비
#### 02. 파이썬과 MySQL의 연동
#### 03. GUI 응용 프로그램 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~99    | ✅         |
| 2주차 | p.102~155   | ✅         |
| 3주차 | p.158~213  | ✅         |
| 4주차 | p.216~271 | ✅         |
| 5주차 | p.274~327 | ✅         |
| 6주차 | p.330~369 | ✅         |
| 7주차 | p.372~407 | ✅         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 파이썬 개발 환경 준비
- 파이썬 자체에는 MySQL을 인식하는 기능이 없음 -> 파이썬 코드에서 MySQL을 활용하기 위해 외부 라이브러리인 pymysql을 설치해야함
- 명령 프롬프트에서 다음과 같이 설치
    ```
    pip install pymysql
    ```

<!-- 이번 챕터에서 제시된 실습을 흐름에 맞게 진행한 후, 실습 과정이 보일 수 있도록 인증 사진을 2장 이상 제출해 주세요. -->

<!-- 이 부분을 지우고 인증사진을 제출해주세요.-->


## 2. 파이썬과 MySQL의 연동

### 연동 프로그래밍 기본
#### 연동 프로그램을 위한 쇼핑몰 생성
- MySQL 워크벤치를 실행해서 '혼공 쇼핑몰 DB (soloDB)'를 생성하기
    ```sql
    DROP DATABASE IF EXISTS soloDB;
    CREATE DATABASE soloDB;
    ```
#### 파이썬에서 데이터 입력

1. MySQL 연결하기
    - pymysql.connect()로 데이터베이스와 연동하기
    - `연결자 = pymysql.connect(연결 옵션)`
2. 커서 생성하기
    - 커서는 데이터베이스에 SQL 문을 실행하거나 실행된 결과를 돌려받는 통로
    - `커서이름 = 연결자.cursor()`
3. 테이블 만들기
    - 테이블을 만드는 SQL 문을 `커서이름.execute()` 함수의 매개변수로 넘겨주면 SQL 문이 데이터베이스에 실행됨
    - 파이썬에서도 MySQL 워크벤치에서 사용한 것과 동일한 SQL 문을 사용
    - `커서이름.execute("CREATE TABLE 문장")`
4. 데이터 입력하기
    - 필요한 만큼 반복해서 입력하기
    - 데이터 입력도 SQL 문을 사용해야 하므로 `커서이름.execute()` 함수를 사용
    - `커서이름.execute("INSERT 문장")`
5. 입력한 데이터 저장하기
    - 데이터 확실하게 저장하기 위해 커밋(commit)하기
    - `연결자.commit()`
6. MySQL 연결 종료하기
    - 과정1에서 연결한 데이터베이스 닫기
    - `연결자.close()`
    ```python
    # 1. MySQL 연결하기
    import pymysql
    conn = pymysql.connect(host='127.0.0.1', user='root', password='1019', db='soloDB', charset='utf8')

    # 2. 커서 생성하기
    cur = conn.cursor()

    # 3. 테이블 만들기
    cur.execute("CREATE TABLE userTable (id char(4), userName char(15), email char(20), birthYear int)")

    # 4. 데이터 입력하기
    cur.execute("INSERT INTO userTable VALUES('hong', '홍지윤', 'hong@naver.com', 1996)")
    cur.execute("INSERT INTO userTable VALUES( 'kim', '김태연', 'kim@daum.net', 2011)")
    cur.execute("INSERT INTO userTable VALUES('star', '별사랑', 'star@paran.com', 1990)")
    cur.execute("INSERT INTO userTable VALUES( 'yang', '양지은', 'yang@gmail.com', 1993)")

    # 5. 입력한 데이터 저장하기
    conn.commit()

    # 6. MySQL 연결 종료하기
    conn.close()
    ```

### 연동 프로그래밍 활용
#### 완전한 데이터 입력 프로그램의 완성
- 사용자가 반복해서 데이터를 입력하는 코드 -> 더 이상 입력할 데이터가 없으면 Enter 키를 입력하여 종료하도록
    ```python
    import pymysql

    # 전역변수 선언부
    conn, cur = None, None
    data1, data2, data3, data4 = "", "", "", ""
    sql=""

    # 메인 코드
    conn = pymysql.connect(host='127.0.0.1', user='root', password='1019', db='soloDB', charset='utf8')
    cur = conn.cursor()

    while (True) : 
        data1 = input("사용자 ID ==>")
        if data1 == "": 
            break;
        data2 = input("사용자 이름 ==>")
        data3 = input("사용자 이메일 ==> ")
        data4 = input("사용자 출생연도 ==> ")
        sql = "INSERT INTO userTable VALUES('" + data1 + "', '" + data2 + "', '" + data3 + "'," + data4 + ")"
        cur.execute(sql)

    conn.commit()
    conn.close()
    ```

#### MySQL의 데이터 조회를 위한 파이썬 코딩 순서
1. MySQL 연결하기
2. 커서 생성하기
3. 데이터 조회하기
    - 커서에 SELECT로 조회한 결과를 한꺼번에 저장해 놓음
4. 조회한 데이터 출력하기
    - 조회한 데이터를 `fetchone()` 함수로 한 행씩 접근한 후 출력
5. MySQL 연결 종료하기(데이터를 입력하거나 변경하는 것이 아니니 커밋과정 불필요)

#### 완전한 데이터 조회 프로그램의 완성
```python
import pymysql

# 전역변수 선언부
con, cur = None, None
data1, data2, data3, data4 = "", "", "", ""
row=None

#메인 코드
conn = pymysql.connect(host='127.0.0.1', user='root', password='1019', db='soloDB', charset='utf8')
cur = conn.cursor()

cur.execute ("SELECT * FROM userTable")

print("사용자ID 사용자이름 이메일 출생연도")
print("---------------------------------")

while (True) :
    row = cur.fetchone()
    if row== None :
        break
    data1 = row[0]
    data2 = row[1]
    data3 = row[2]
    data4 = row[3]
    print("%5s  %15s    %20s    %d" % (data1, data2, data3, data4))

conn.close()
```

## 3. GUI 응용 프로그램 
### GUI 기본 프로그래밍
#### 기본 윈도의 구성
```python
from tkinter import *
# tkinter: 파이썬에서 GUI 관련 모듈을 제공해주는 표준 윈도 라이브러리

root = Tk()
# Tk(): 기본이 되는 윈도 반환(루트 윈도)
# 이 부분에서 코딩을 추가해서 화면을 구성하고 처리

root.mainloop()
# 키보드 누르기, 마우스 클릭 등의 다양한 작업이 일어날 때 이벤트를 처리하기 위해 필요
```

---

이번 주는 마지막 주로 별도의 실습 과제는 진행하지 않습니다.

한 학기 동안 다소 부족한 커리큘럼이었음에도 불구하고 성실하게, 그리고 열심히 과제를 수행해주셔서 진심으로 감사드립니다.

이번 과정을 통해 배운 내용이 앞으로 여러분이 SQL을 다루는 데에 조금이나마 도움이 되기를 바랍니다.

여러분의 앞날을 진심으로 응원합니다. 😊


### 🎉 수고하셨습니다.







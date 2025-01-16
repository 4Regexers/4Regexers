## DB 연동 및 정규표현식 실습 Guide

## 💡 목표 
**1. 여러 사용자가 하나의 데이터베이스를 활용할 수 있는 환경 구축**
   - DB 계정 생성
   - Ubuntu 기반 MySQL 환경 설정 및 공유
   - DBeaver를 활용한 원격 데이터베이스 접속 설정
     
**2. 정규표현식을 활용하여 원하는 데이터 추출 및 분석**
   - 정규표현식 기본 문법 학습 및 활용
   - SQL문 기반의 쿼리를 정규표현식으로 변환


## 🚀 프로젝트 소개

### 📘 **입사 첫날, 정규표현식과 마주하다**
> 지훈이는 증권사에 입사한 첫날이다. 첫 업무로 주식 거래 데이터베이스를 검증하고 필요한 정보를 추출하는 일을 맡게 되었다.

> 아래 7가지 업무들을 해결하기 위해 주어진 정보들을 참고하여 공유 데이터베이스에서 **정규표현식을 활용한 select 문**을 구성해보자.   
## 🛠️ 기술 스택
<div style="text-align: left;"> 
  <img src="https://img.shields.io/badge/MySQL-F29111?style=for-the-badge&logo=MySQL&logoColor=white">
  <img src="https://img.shields.io/badge/DBeaver-5C6BC0?style=for-the-badge&logo=DBeaver&logoColor=white">
  <img src="https://img.shields.io/badge/VirtualBox-42A5F5?style=for-the-badge&logo=VirtualBox&logoColor=white">
</div>

## 📋 실습 가이드 

### 1️⃣ 환경 구축

#### 🖥️ 1. VirtualBox Network 설정
- **설정 > 네트워크 > 어댑터를 브릿지 모드로 설정**
- **브릿지 모드(Adaptor Bridge Mode)란?**
  - 공유기로부터 가상머신이 IP 주소를 할당받아 호스트 PC와 동일한 네트워크 대역의 IP 주소로 통신 가능
  - 가상머신이 독립된 장치처럼 동작하며 물리적 네트워크 장치와 직접 통신 가능
  <p align="center">
    <img src="https://github.com/user-attachments/assets/258ffda3-7d76-4c73-81cf-2873e6d44d7f" alt="VirtualBox Network 설정" width="70%">
</p>

#### 🛠️ 2. 데이터베이스 구축

#### 2.1. 공동 계정 생성
- **Root 계정에서 새로운 계정 생성**
- `%`로 설정하면 모든 호스트에서 접근 가능
    ```sql
    CREATE USER '4Regexers'@'%' IDENTIFIED BY '4Regexers';
    ```

#### 2.2. 특정 데이터베이스(main) 접근 권한 부여
- 특정 데이터베이스(main)에만 접근 권한 부여
    ```sql
    GRANT ALL PRIVILEGES ON main.* TO '4Regexers'@'%';
    FLUSH PRIVILEGES; -- 변경사항 적용
    ```

#### 2.3. DBeaver 연결
- 위에서 생성한 계정을 사용하여 **DBeaver**로 원격 연결
<p align="center">
    <img src="https://github.com/user-attachments/assets/94a06751-e04f-469b-9617-f3ddad253e1e" alt="DBeaver 연결" width="70%">
</p>

### 2️⃣ 정규표현식 사용 목적 및 활용법
#### 📌 1. 정의
- **정규표현식(Regular Expression, Regex)** : 특정 규칙을 가진 문자열의 집합을 정의하고 이를 기반으로 텍스트 데이터를 검색, 추출, 검증, 변환, 치환하는 데 사용하는 형식 언어
- **활용 목적** : 다양한 환경(SQL, Python, Java 등)에서 주로 문자열 데이터의 패턴을 정의하여 효율적으로 처리할 수 있도록 도와줌

#### 📌 2. 자주 활용되는 표현식 및 활용법
**2.1 기본 문법**
<p align="center">
    <img src="https://github.com/user-attachments/assets/df0b137a-1fee-4683-b407-440da83e938f" alt="기본 문법" width="80%">
</p>

**2.2 유용한 패턴**
<p align="center">
    <img src="https://github.com/user-attachments/assets/3d27c0b4-fe4f-488f-83de-267e785ac57c" alt="유용한 패턴" width="80%">
</p>

**2.3 수량연산자**
<p align="center">
    <img src="https://github.com/user-attachments/assets/f23910d2-87c4-4a55-aa86-f2e9594561f5" alt="수량 연산자" width="80%">
</p>

### 3️⃣ 주식 거래 DB 
#### 🗂️ 1. Table(stock_trades) 정의
<table style="border-collapse: collapse; width: 100%; text-align: left;">
  <thead>
    <tr>
      <th style="width: 50%; border-bottom: 2px solid #ddd; padding: 8px;">ERD</th>
      <th style="width: 50%; border-bottom: 2px solid #ddd; padding: 8px;">DDL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 8px; vertical-align: top;">
        <img src="https://github.com/user-attachments/assets/cdb334c0-5542-4c66-ba5f-7063b3d54706" alt="Table Structure" style="width: 100%;">
      </td>
      <td style="padding: 8px; vertical-align: top;">
        <pre>
<code>
CREATE TABLE stock_trades (
    trade_id INT PRIMARY KEY AUTO_INCREMENT, -- 거래 고유 ID, `INT`, 기본 키, 자동 증가
    user_id INT NOT NULL, -- 사용자 ID, `INT`
    stock_symbol VARCHAR(10) NOT NULL, -- 주식 심볼, `VARCHAR(10)`
    trade_date DATETIME NOT NULL, -- 거래 날짜 및 시간, `DATETIME`
    trade_type ENUM('BUY', 'SELL') NOT NULL, -- 거래 유형 (`BUY` 또는 `SELL`)
    quantity INT NOT NULL CHECK (quantity > 0), -- 거래 수량, `INT`, 양수만 허용
    price DECIMAL(10, 2) NOT NULL CHECK (price > 0), -- 주당 거래 가격, 양수만 허용
    transaction_id VARCHAR(15) NOT NULL -- 거래 식별자, `VARCHAR(15)`
);
</code>
        </pre>
      </td>
    </tr>
  </tbody>
</table>

#### 💾 2. 30개의 Dummy 데이터를 INSERT 문으로 삽입
- 트랜잭션 id(transaction_id)에 유효하지 않은 값 3개가 존재
- 주식 심볼(stock_symbol)에 유효하지 않은 값 11개
- 2024년 12월 중순부터 2025년 1월 중순까지의 데이터
```sql
INSERT INTO stock_trades (user_id, stock_symbol, trade_date, trade_type, quantity, price, transaction_id) VALUES
(101, 'AAPL', '2024-12-16 09:32:10', 'BUY', 10, 175.50, 'T-0001'),
(102, 'TS@LA', '2024-12-17 10:15:45', 'SELL', 5, 275.30, 'T-0002'),
(103, 'GOOGL', '2024-12-18 13:22:17', 'BUY', 15, 125.75, 'INVALID_TXN'),
(104, 'MSFT', '2024-12-19 11:02:33', 'BUY', 20, 310.00, 'T-0004'),
(105, 'AMZN123', '2024-12-20 15:45:50', 'SELL', 7, 135.25, 'T-0005'),
(106, 'AAP-L', '2024-12-21 10:00:00', 'BUY', 25, 180.00, 'T-0006'),
(107, 'TSLA', '2024-12-22 14:30:45', 'SELL', 12, 290.75, 'T-0007'),
(108, 'GO-GL', '2024-12-23 09:15:20', 'BUY', 8, 126.00, 'T-0008'),
(109, 'MSFT', '2024-12-24 16:10:50', 'SELL', 30, 312.50, 'T-0009'),
(110, 'AMZN', '2024-12-25 11:25:35', 'BUY', 5, 133.00, 'INVALID_TXN'),
(111, 'AAPL', '2024-12-26 14:50:00', 'SELL', 6, 178.25, 'T-0011'),
(112, 'T2SLA', '2024-12-27 09:45:00', 'BUY', 3, 275.00, 'T-0012'),
(113, 'GOOGL', '2024-12-28 12:00:00', 'SELL', 4, 128.00, 'T-0013'),
(114, 'M$FT', '2024-12-29 15:15:15', 'BUY', 18, 320.00, 'T-0014'),
(115, 'AMZN', '2024-12-30 10:45:10', 'SELL', 11, 140.00, 'T-0015'),
(116, 'AAPL', '2024-12-31 13:50:50', 'BUY', 20, 185.50, 'T-0016'),
(117, 'TSLA', '2025-01-01 09:30:00', 'SELL', 7, 280.25, 'T-0017'),
(118, 'GOOGL', '2025-01-02 11:15:25', 'BUY', 10, 130.00, 'T-0018'),
(119, 'M@SFT', '2025-01-03 14:40:10', 'SELL', 25, 315.00, 'T-0019'),
(120, 'AMZN', '2025-01-04 10:10:30', 'BUY', 2, 145.00, 'INVALID_TXN'),
(121, 'AAPL', '2025-01-05 12:30:00', 'SELL', 4, 200.00, 'T-0021'),
(122, 'TSLA', '2025-01-06 09:00:00', 'BUY', 1, 295.00, 'T-0022'),
(123, 'GO0GL', '2025-01-07 11:45:50', 'SELL', 9, 135.50, 'T-0023'),
(124, 'MSFT', '2025-01-08 16:20:15', 'BUY', 12, 325.00, 'T-0024'),
(125, 'AMZ#N', '2025-01-09 09:35:30', 'SELL', 8, 150.00, 'T-0025'),
(126, 'AAPL', '2025-01-10 11:50:00', 'BUY', 16, 195.00, 'T-0026'),
(127, 'TSL-A', '2025-01-11 10:15:10', 'SELL', 6, 300.50, 'T-0027'),
(128, 'GOOGL', '2025-01-12 13:20:45', 'BUY', 5, 140.75, 'T-0028'),
(129, 'M5FT', '2025-01-13 15:30:10', 'SELL', 14, 330.00, 'T-0029'),
(130, 'AMZN', '2025-01-14 10:00:00', 'BUY', 9, 155.00, 'T-0030');
```

## ⚔️ 주어진 7가지 업무 
### 업무 1. 잘못된 거래 ID 탐지
- **모든 거래 ID는 T-로 시작하고 뒤에 4자리 숫자**가 와야 한다. **거래 ID가 잘못 기입**된 것이 있는지 확인하도록 한다.
<p align="center">
    <img src="https://github.com/user-attachments/assets/629e6040-8ca8-4db8-b1e7-3b66bfe2bdbc" alt="image">
</p>

```sql
-- 정규표현식을 사용하지 않았을 경우 SQL문 
SELECT trade_id, transaction_id
FROM stock_trades
WHERE LEFT(transaction_id, 2) != 'T-' 
   OR LENGTH(transaction_id) != 6 
   OR NOT (
       SUBSTRING(transaction_id, 3, 1) BETWEEN '0' AND '9' AND
       SUBSTRING(transaction_id, 4, 1) BETWEEN '0' AND '9' AND
       SUBSTRING(transaction_id, 5, 1) BETWEEN '0' AND '9' AND
       SUBSTRING(transaction_id, 6, 1) BETWEEN '0' AND '9'
   ); 
```
<br>
<details>
  <summary>🔍 풀이</summary>

```sql
SELECT trade_id, transaction_id -- trade_id와 transaction_id를 선택
FROM stock_trades 
WHERE transaction_id NOT REGEXP '^T-[0-9]{4}$'; -- T- 로 시작하며 0~9사이의 4자리수 값을 가지는 항목이 아닌 것을 선택
```
- **`NOT REGEXP`** :  값이 정규표현식 패턴과 일치하지 않는 경우 필터링
- **`^`** : 문자열 시작
- **`T-`** : T-로 시작하는 문자열을 반환
- **`[0-9]{4}`** : 0부터 9까지의 문자열을 `{4}`를 사용해 4개만 반환
- **`$`** : 문자열의 끝을 의미

<p align="center">
    <img src="https://github.com/user-attachments/assets/bf00be7a-3260-4229-89ae-50f31c19fdb8" alt="image">
</p>
</details>


------------------------------------------------------
### 업무 2. 특정 날짜 범위의 거래 필터링
- **2024년 12월에 이루어진 거래**를 찾아야 한다.
<p align="center">
    <img src="https://github.com/user-attachments/assets/9d931285-359b-4803-afdc-a37a809ac1fb" alt="image">
</p>

```sql
-- 정규표현식을 사용하지 않았을 경우 SQL문 
SELECT trade_id, trade_date, stock_symbol
FROM stock_trades
WHERE trade_date LIKE '2024-12%';
```
<br>
<details>
  <summary>🔍 풀이</summary>
   
```sql
SELECT trade_id, trade_date, stock_symbol
FROM stock_trades
WHERE trade_date REGEXP '^2024-12';
```

- **`REGEXP`** : 정규표현식을 활용한 문자열 검색을 필터링 가능하게 하는 조건문
- **`^`** : 문자열 시작
- **`2024-12`** : 고정된 텍스트로 대상 문자열이 “2024-12”로 시작하는 경우 일치
- **`$`** : 문자열의 끝을 의미

<p align="center">
    <img src="https://github.com/user-attachments/assets/1a36ae43-be28-4166-8814-b7c04e74c3bb" alt="image">
</p>
</details>

---------------------------------------------------------------------------------------------
### 업무 3. 주식 심볼 형식 필터링
- **주식 심볼은 1~5자 사이의 영문 대문자로만 구성**되어야 한다. **잘못된 형식의 주식 심볼**을 찾아내야 한다.
<p align="center">
    <img src="https://github.com/user-attachments/assets/0628daeb-ccbd-494e-ab9b-0e89651d1c8a" alt="image">
</p>

```sql
-- 정규표현식을 사용하지 않았을 경우 SQL문 
SELECT trade_id, stock_symbol
FROM stock_trades
WHERE LENGTH(stock_symbol) < 1
   OR LENGTH(stock_symbol) > 5
   OR NOT (
       SUBSTRING(stock_symbol, 1, 1) BETWEEN 'A' AND 'Z'
       AND (LENGTH(stock_symbol) < 2 OR SUBSTRING(stock_symbol, 2, 1) BETWEEN 'A' AND 'Z')
       AND (LENGTH(stock_symbol) < 3 OR SUBSTRING(stock_symbol, 3, 1) BETWEEN 'A' AND 'Z')
       AND (LENGTH(stock_symbol) < 4 OR SUBSTRING(stock_symbol, 4, 1) BETWEEN 'A' AND 'Z')
       AND (LENGTH(stock_symbol) < 5 OR SUBSTRING(stock_symbol, 5, 1) BETWEEN 'A' AND 'Z')
   );
```
<br>
<details>
  <summary>🔍 풀이</summary>
   
```sql
SELECT trade_id, stock_symbol
FROM stock_trades
WHERE stock_symbol NOT REGEXP '^[A-Z]{1,5}$';
```

- **`^`** : 문자열 시작
- **`[A-Z]`** : 대문자 알파벳(A~Z)의 하나의 문자와 일치
- **`{1,5}`** : 바로 앞의 **`[A-Z]`** 대문자 범위에 대해 1개 이상, 최대 5개까지의 반복을 허용, 따라서 문자열이 1~5개의 대문자로 구성
- **`$`** : 문자열의 끝
- **`^[A-Z]{1,5}$`** : 문자열은 반드시 1~5개의 대문자 알파벳만 포함되어야 하고, 그 외의 문자(숫자, 소문자, 특수문자 등)가 포함되거나 대문자 알파벳이 5개를 초과하는 경우는 허용하지 않음


<p align="center">
    <img src="https://github.com/user-attachments/assets/604718bf-894e-4b35-87a1-296428483af3" alt="image">
</p>
</details>

---------------------------------------------------------------------------------------------

## 업무 4. 거래 유형 탐지 
까다로운 부장님이 특별한 업무를 지시하였다. "BUY" 거래 중에서 거래 가격(price)이 소수점 1자리만 있는 거래를 찾아야 한다.

```sql
--SQL문
SELECT * 
FROM stock_trades
WHERE trade_type = 'BUY' 
  AND price - FLOOR(price) BETWEEN 0.1 AND 0.9
  AND (price - FLOOR(price)) * 10 = FLOOR((price - FLOOR(price)) * 10);
```

<details>
  <summary>풀이</summary>

```sql
SELECT * FROM stock_trades
WHERE trade_type = 'BUY' AND price REGEXP '^[0-9]+\\.[1-9]{1}0$';
```
**REGEXP**: 정규표현식을 활용한 문자열 검색을 필터링 가능하게 하는 조건문

**^** : 문자열 시작

**[0-9]+**: 0부터 9까지의 문자열을 반환, 뒤에 +를 붙여서 한번 이상 반복하는 연속적인 문자열을 전부 반환

**\\.** : 소수점을 그대로 반환

**[1-9]{1}0** : 1부터 9까지의 문자열을 {1}를 사용해 1개만 반환 후 0 출력

**$** : 문자열의 끝을 의미

![image](https://github.com/user-attachments/assets/c511179d-6310-4a6e-8335-3e8ef728b880)


</details>

  
![image](https://github.com/user-attachments/assets/baafa0b7-5663-40e1-8fd0-27696e623aaa)


---------------------------------------------------------------------------------------------

## 업무 5. 특정 가격 범위 거래
   가진 재산이 $200.00인 고객이 $100.00이상의 주식 중 자신이 구매 가능한 종목을 문의 하였다. 거래기록을 기반으로 매수 가능 거래의 주식 심볼, 거래일자, 가격을 확인하여라.

```sql
--SQL문
SELECT stock_symbol, trade_date, price
FROM stock_trades
WHERE price >= 100.00 AND price <= 200.00
```

<details>
  <summary>풀이</summary>

```sql
SELECT stock_symbol, trade_date, price
FROM stock_trades
WHERE price REGEXP '^1[0-9]{2}\\.\\d{2}$|^200\\.00$';
```
**REGEXP** : 정규표현식을 활용한 문자열 검색을 필터링 가능하게 하는 조건문

**^** : 문자열 시작

**1[0-9]{2}**: 1 반환, 0부터 9까지의 숫자 중 뒤에 {2}를 사용해 2개만 반환

**\\.** : 소수점을 그대로 반환

**\\d{2}** : 0부터 9까지의 숫자 중 뒤에 {2}를 사용해 2개만 반환

**200\\.00** : 200.00 을 표현

$ : 문자열의 끝을 의미

![image](https://github.com/user-attachments/assets/ef8ec8d9-5448-4250-a122-9e69242c6959)



</details>

  
![image](https://github.com/user-attachments/assets/58f0787c-1146-4557-90b4-9ed72a5e2ffb)


---------------------------------------------------------------------------------------------

## 업무 6. 가격 범위에 따른 거래 필터링
더욱 까다로운 과장님이 다음 거래에서는 1달러 이하의 금액(센트)을 남기고 싶다고 하신다.  가격이 소수점으로 끝나는 거래만 선택해보자.

```sql

SELECT trade_id, price
FROM stock_trades
WHERE price - FLOOR(price) != 0;
```

<details>
  <summary>풀이</summary>

```sql
SELECT trade_id, price
FROM stock_trades
WHERE price NOT REGEXP '^[0-9]+\\.(00)$';
```

  
**NOT REGEXP** : 값이 정규표현식 패턴과 일치하지 않는 경우
   

**^** : 문자열 시작

**[0-9]+**: 0부터 9까지의 문자열을 반환, 뒤에 +를 붙여서 한번 이상 반복하는 연속적인 문자열을 전부 반환

**\\.** : 소수점을 그대로 반환

**(00)** : 00을 그룹화

**$** : 문자열의 끝을 의미

![image](https://github.com/user-attachments/assets/03c218e6-a5e0-475d-b46c-4be89dfaacc1)






</details>


![image](https://github.com/user-attachments/assets/f6c437ec-4c5d-4cbb-a31a-2d2d8257497d)



## 업무 7. 오전/오후 거래 구분
   오전 주식시장의 동향을 파악하기 위해 오전(09:00 ~ 12:00) 시간대에 거래된 데이터만 출력하도록 하자.

```sql
SELECT trade_id, trade_date, stock_symbol
FROM stock_trades
WHERE HOUR(trade_date) BETWEEN 9 AND 11
  OR (HOUR(trade_date) = 12 AND MINUTE(trade_date) = 0 AND SECOND(trade_date) = 0);
```

<details>
  <summary>풀이</summary>

```sql
SELECT trade_id, trade_date, stock_symbol
FROM stock_trades
WHERE trade_date REGEXP ' (09|1[0-1]):[0-5][0-9]:[0-5][0-9]$'
  OR trade_date REGEXP ' 12:00:00$';
```

  
 **REGEXP** : 정규표현식을 활용한 문자열 검색을 필터링 가능하게 하는 조건문

**[0-N]**: 0부터 N까지의 문자열을 반환

**$** : 문자열의 끝을 의미

오전 시간대는 00:00:00 부터 12:00:00 까지 이다. 따라서 hh부분은(09|1[0-1])를 이용해 9시부터 11시까지를, mm 부분은 [0-5][0-9]를 이용해 00분부터 59까지를 ss부분은 00초부터 59초까지를 출력하고, 여기서 추가로 12:00:00까지 필터링하도록 한다.

![image](https://github.com/user-attachments/assets/720170bd-64e0-45ee-a5d3-bfdbd62637c9)





</details>

![image2](https://github.com/user-attachments/assets/7ba67f37-7d48-4a2f-827f-f57cfe0992b0)


## 🤸‍♂️ 팀원 소개
<table>
  <tbody>
    <tr>
      <td align="center">
        <a href="https://github.com/parkjhhh">
          <img src="https://avatars.githubusercontent.com/u/153366521?v=4" width="150px;" alt="박지혜"/>
          <br /><sub><b> 박지혜 </b></sub>
        </a>
        <br />
      </td>
      <td align="center">
        <a href="https://github.com/JaeHee-devSpace">
          <img src="https://avatars.githubusercontent.com/u/193316939?v=4" width="150px;" alt="박재희"/>
          <br /><sub><b> 박재희 </b></sub>
        </a>
        <br />
      </td>
      <td align="center">
        <a href="https://github.com/wild-turkey">
          <img src="https://github.com/user-attachments/assets/0596f1ff-ab90-49f0-9dd4-c306c63a397b" width="150px;" alt="김지훈"/>
          <br /><sub><b> 김지훈 </b></sub>
        </a>
        <br />
      </td>
      <td align="center">
        <a href="https://github.com/riyeong0916">
          <img src="https://avatars.githubusercontent.com/u/193798531?v=4" width="150px;" alt="김리영"/>
          <br /><sub><b> 김리영 </b></sub>
        </a>
        <br />
      </td>
    </tr>
  </tbody>
</table>







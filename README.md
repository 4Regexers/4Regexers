# 정규표현식 이해 및 MySQL 구축
## 🤸‍♂️목표 



## 🤸‍♂️팀원소개
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

## 🚀 프로젝트 이름

> **간단한 프로젝트 소개**  
> SQL 정규표현식을 이해하고 예제를 만들어 풀이하였습니다. 


## 🚀 기술 스택

- **Backend**: MySQL

---

## 💡 주요 기능

✔️ **주요기능1**  
✔️ **주요기능2**  
✔️ **주요기능3**   

---

## 📂 프로젝트 구조

1. 데이터 테이블 설명
- DDL

![image](https://github.com/user-attachments/assets/b6eee4b0-b243-425c-9081-4c978239e28b)

```sql
CREATE TABLE stock_trades (
    trade_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    stock_symbol VARCHAR(10) NOT NULL,
    trade_date DATETIME NOT NULL,
    trade_type ENUM('BUY', 'SELL') NOT NULL,
    quantity INT NOT NULL CHECK (quantity > 0),
    price DECIMAL(10, 2) NOT NULL CHECK (price > 0),
    transaction_id VARCHAR(15) NOT NULL
);
```

모든 열은 `NOT NULL` 을 활용하여 필수로 값을 받도록 설정

- **`trade_id`**: 거래 고유 ID, `INT`, 기본 키, 자동 증가.
- **`user_id`**: 사용자 ID, `INT`
- **`stock_symbol`**: 주식 심볼, `VARCHAR(10)`
- **`trade_date`**: 거래 날짜 및 시간, `DATETIME`
- **`trade_type`**: 거래 유형 (`BUY` 또는 `SELL`), `ENUM` 을 활용하여 buy 혹은 sell 값만 입력되도록 설정
- **`quantity`**: 거래 수량, `INT`, 양수만 허용
- **`price`**: 주당 거래 가격, `DECIMAL(10, 2)`, 양수만 허용
- **`transaction_id`**: 거래 식별자, `VARCHAR(15)`

- DML
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
(121, 'AAPL', '2025-01-05 12:30:00', 'SELL', 4, 190.00, 'T-0021'),
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
30개의 더미 데이터를 DML을 이용하여 삽입한다.

특이점: 

- 트랜잭션 id에 유효하지 않은 값 3개가 존재한다
- 주식 심볼에 유효하지 않은 값 11개
- 2024년 12월 중순부터 2025년 1월 중순까지의 데이터


---------------------------------------------------------------------------------------------

2. 특정 날짜 범위의 거래 필터링
- `2024-12`로 시작하는 날짜만 포함한 거래 찾기

```sql
--SQL문
SELECT trade_id, trade_date, stock_symbol
FROM stock_trades
WHERE trade_date LIKE '2024-12%';
```

<details>
  <summary>풀이</summary>

```sql
SELECT trade_id, trade_date, stock_symbol
FROM stock_trades
WHERE trade_date REGEXP '^2024-12';
```

**REGEXP** : 정규표현식을 활용한 문자열 검색을 필터링 가능하게 하는 조건문

`^` : 문자열의 시작

`2024-12` : 고정된 텍스트로 대상 문자열이 “2024-12”로 시작하는 경우 일치

![image](https://github.com/user-attachments/assets/1a36ae43-be28-4166-8814-b7c04e74c3bb)

</details>


- 출력 결과
 ![image](https://github.com/user-attachments/assets/9d931285-359b-4803-afdc-a37a809ac1fb)

---------------------------------------------------------------------------------------------

3. 주식 심볼 형식 필터링 
- 주식 심볼이 1~5자의 영문 대문자로만 구성되었는지 확인

```sql
--SQL문
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

<details>
  <summary>풀이</summary>

```sql
SELECT trade_id, stock_symbol
FROM stock_trades
WHERE stock_symbol NOT REGEXP '^[A-Z]{1,5}$';
```
- ^ : 문자열의 시작
- [A-Z] : 대문자 알파벳(A~Z)의 하나의 문자와 일치
- {1,5} : 바로 앞의 [A-Z] 대문자 범위에 대해 **1개 이상, 최대 5개까지**의 반복을 허용한다. 따라서 문자열이 1~5개의 대문자로 구성되어야한다.
- $ ****: 문자열의 끝
- ^[A-Z]{1,5}$ : 문자열은 반드시 1~5개의 대문자 알파벳만 포함되어야하고, 그외의 문자(숫자, 소문자, 특수문자 등)가 포함되거나 대문자 알파벳이 5개를 초과하는 경우는 허용 하지 않는다.

![image](https://github.com/user-attachments/assets/604718bf-894e-4b35-87a1-296428483af3)
</details>


- 출력 결과
  
![image](https://github.com/user-attachments/assets/0628daeb-ccbd-494e-ab9b-0e89651d1c8a)

---------------------------------------------------------------------------------------------

4. 잘못된 거래 유형 탐지 
- "BUY" 거래 중에서 거래 가격(price)이 소수점 1자리만 있는 거래를 찾으세요.

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
REGEXP ****: 정규표현식을 활용한 문자열 검색을 필터링 가능하게 하는 조건문

^ : 문자열 시작

[0-9]+: 0부터 9까지의 문자열을 반환, 뒤에 +를 붙여서 한번 이상 반복하는 연속적인 문자열을 전부 반환

\\. : 소수점을 그대로 반환

[1-9]{1}0 : 1부터 9까지의 문자열을 {1}를 사용해 1개만 반환 후 0 출력

$ : 문자열의 끝을 의미한다.

![image](https://github.com/user-attachments/assets/1cad1f61-d7e1-44f6-b202-74ca90880307)

</details>


- 출력 결과
  
![image](https://github.com/user-attachments/assets/baafa0b7-5663-40e1-8fd0-27696e623aaa)


---------------------------------------------------------------------------------------------

5. 특정 가격 범위 거래 탐지
- 가격이 $100.00에서 $200.00 사이인 거래만 선택.

```sql
--SQL문
SELECT trade_id, price
FROM stock_trades
WHERE price >= 100.00 AND price <= 200.00
```

<details>
  <summary>풀이</summary>

```sql
SELECT trade_id, price
FROM stock_trades
WHERE price REGEXP '^1[0-9]{2}\\.\\d{2}$|^200\\.00$';
```
REGEXP : 정규표현식을 활용한 문자열 검색을 필터링 가능하게 하는 조건문

^ : 문자열 시작

1[0-9]{2}: 1 반환, 0부터 9까지의 숫자 중 뒤에 {2}를 사용해 2개만 반환

\\. : 소수점을 그대로 반환한다.

\\d{2} : 0부터 9까지의 숫자 중 뒤에 {2}를 사용해 2개만 반환

200\\.00 : 200.00 을 표현

$ : 문자열의 끝을 의미한다.

![image](https://github.com/user-attachments/assets/396200ea-aedf-4bba-b16e-837b0016d442)

</details>


- 출력 결과
  
![image](https://github.com/user-attachments/assets/c4cb04c9-c090-4efb-8faa-ab6b729f8073)












<details>
  <summary>클릭하여 더보기</summary>
  
  여기에 토글할 내용이 들어갑니다.
  
  예시:
  - 항목 1
  - 항목 2
  - 항목 3
  
</details>

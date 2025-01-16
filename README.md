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

1. 잘못된 거래 ID 탐지
   거래 ID가 T-로 시작하고 뒤에 4자리 숫자가 있는지 확인.

```sql
--SQL문
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

<details>
  <summary>풀이</summary>

```sql
SELECT trade_id, transaction_id -- trade_id와 transaction_id를 선택한다
FROM stock_trades -- stock_trades 테이블에서
WHERE transaction_id NOT REGEXP '^T-[0-9]{4}$'; -- T- 로 시작하며 0~9사이의 4자리수 값을 가지는 항목이 아닌 것을 선택
```

  
  **REGEXP** : 정규표현식을 활용한 문자열 검색을 필터링 가능하게 하는 조건문

   

**^** : 문자열 시작

T-:  T- 로 시작하는 문자열을 반환한다

**[0-9]{4}** : 0부터 9까지의 문자열을 {4}를 사용해 4개만 반환한다.

**$** : 문자열의 끝을 의미한다.

![image](https://github.com/user-attachments/assets/bec9ae52-5aae-46d3-b797-56268ae07e43)

</details>














<details>
  <summary>클릭하여 더보기</summary>
  
  여기에 토글할 내용이 들어갑니다.
  
  예시:
  - 항목 1
  - 항목 2
  - 항목 3
  
</details>

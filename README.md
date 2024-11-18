# PostgreSQL 資料庫學習紀錄

主要根據六角學院 「2024 後端工程師 - 資料庫體驗營」整理的學習筆記，方便查閱。

[課程簡報](https://gamma.app/docs/SQL--23heyix4r93u5u0?mode=doc#card-zbtn6kfn3th4gpr)

## 目錄

- 資料庫簡介

  - [資料庫的形式](#資料庫的形式)

  - [如何操作資料庫？](#如何操作資料庫)

  - [資料庫設計順序](#資料庫設計順序)

  - [Data Type 資料類型](#data-type-資料類型)

  - [新增一張資料表](#新增一張資料表)

  - [新增資料 ( insert )](#新增資料-insert)

  - [SELECT 查詢](#select-查詢)

    - [AS 新增別名](#as-新增別名)

  - [WHERE 篩選資料](#where-篩選資料)

    - [比較運算子](#比較運算子)

    - [邏輯運算子](#邏輯運算子)

    - [集合與範圍運算子](#集合與範圍運算子)

  - [更新資料](#更新資料)

  - [刪除資料](#刪除資料)

  - [小節作業：家具店的菜雞銷售奇遇記](#小節作業家具店的菜雞銷售奇遇記)

- [資料庫與 Docker 環境建立](#資料庫與-docker-環境建立)

  - [安裝 DBeaver](#安裝-dbeaver)

  - [安裝 Docker Desktop](#安裝-docker-desktop)

## 資料庫的形式

- 資料表 ( Table )

  整體資料，包含了所有相關的資料。

- 欄位 ( Columns )

  定義資料的屬性和類型 ( Data Type )。

- 資料列 ( Rows )

  橫向的資料集合，代表一筆完整資料。

## 如何操作資料庫？

可以透過 **SQL** 指令操作資料庫，全名是 "Structured Query Language"，中文是「結構化查詢語言」。

練習平台：[Temporary Postgres Database](https://pg-sql.com/)

## 資料庫設計順序

![圖片01](./images/01.PNG)

## Data Type 資料類型

| 資料型態     | 描述         | 使用情境       | 範例                  | 範例欄位               |
| :----------- | :----------- | :------------- | :-------------------- | :--------------------- |
| INTEGER      | 整數         | ID、年齡、數量 | 1, 42, -15            | `age INTEGER`          |
| VARCHAR(n)   | 可變長度字串 | 名稱、描述     | "John Doe"            | `name VARCHAR(50)`     |
| TIMESTAMP    | 日期時間     | 建立/更新時間  | '2024-03-20 15:30:00' | `created_at TIMESTAMP` |
| DECIMAL(p,s) | 精確小數     | 金額計算       | 199.99                | `price DECIMAL(10,2)`  |
| BOOLEAN      | 真/假值      | 狀態、開關     | true, false           | `is_active BOOLEAN`    |
| JSON         | JSON 資料    | 彈性資料結構   | '{"name": "John"}'    | `settings JSON`        |

## 新增一張資料表

使用 `CREATE TABLE` 來指定 `資料表名稱`及定義 `資料的屬性和類型`。

> 記得結尾加上 `;` 來結束整個 SQL 指令。

```sql
CREATE TABLE users (
  name VARCHAR(50),
  email VARCHAR(100),
  age INTEGER
);
```

## 新增資料 (insert)

使用 `INSERT INTO` 來指定要新增資料的 `資料表名稱`及 `資料欄位`，`VALUES` 後面 使用 `()` 來添加對應欄位的一筆資料。

```sql
INSERT INTO users (name, email, age)
VALUES ('張小明', 'kdjkl@gmail.com', 25);
```

若要一次新增多筆資料則使用 `,` 分隔多筆 `()` 資料即可。

```sql
INSERT INTO users (name, email, age)
VALUES
  ('張小美', 'k2djkl@gmail.com', 21),
  ('張小立', 'kdj3kl@gmail.com', 34),
  ('張小花', 'kdjk4l@gmail.com', 28);
```

## SELECT 查詢

使用 `SELECT` 可以查詢指定資料表 ( `FROM` 後指定資料表名稱 ) 的特定欄位的所有資料。

```sql
-- 查詢所有欄位
SELECT * FROM users;
```

![圖片02](./images/02.PNG)

```sql
-- 查詢特定欄位 (只要 name 和 age)
SELECT name, age FROM users;
```

![圖片03](./images/03.PNG)

### AS 新增別名

可以使用 `AS` 設定查詢資料的**欄位別名**，並且也可設定查詢計算的資料欄位名稱。

```sql
SELECT
  name AS 姓名,
  age AS 現在年齡,
  65 - age AS 距離退休年數
FROM users;
```

![圖片04](./images/04.PNG)

## WHERE 篩選資料

使用 `WHERE` 可以篩選指定的資料。

### 練習用資料庫環境

```sql
-- 商品資料表
CREATE TABLE products (
    name VARCHAR(100),           -- 商品名稱
    price INTEGER,              -- 原價
    discount_price INTEGER,      -- 折扣價
    stock INTEGER,              -- 庫存數量
    category VARCHAR(50),        -- 商品類別
    status VARCHAR(20)          -- 商品狀態（上架、下架）
);

INSERT INTO products (name, price, discount_price, stock, category, status) VALUES
   ('iPhone 16', 25000, 23000, 50, '3C', 'active'),
   ('AirPods', 5000, 5000, 5, '3C', 'inactive'),
   ('iPad', 18000, 16000, 25, '3C', 'active'),
   ('充電線', 500, 500, 100, '配件', 'active'),
   ('手機殼', 300, 250, 30, '配件', 'active'),
   ('螢幕保護貼', 200, 180, 150, '配件', 'active'),
   ('手機支架', 400, 400, 0, '配件', 'inactive');
```

![圖片05](./images/05.PNG)

#### 查找 3C 類別的產品：

```sql
SELECT name, price
FROM products
WHERE category = '3C';
```

![圖片06](./images/06.PNG)

![圖片07](./images/07.PNG)

### 比較運算子

| 運算子 | 意義     | 範例                 |
| :----- | :------- | :------------------- |
| =      | 等於     | `WHERE price = 100`  |
| >      | 大於     | `WHERE price > 100`  |
| <      | 小於     | `WHERE price < 100`  |
| >=     | 大於等於 | `WHERE price >= 100` |
| <=>    | 小於等於 | `WHERE price <= 100` |

#### 查找庫存小於 50 的產品：

```sql
SELECT name, stock
FROM products
WHERE stock < 50;
```

![圖片08](./images/08.PNG)

### 邏輯運算子

| 運算子 | 意義                 | 範例                                         |
| :----- | :------------------- | :------------------------------------------- |
| AND    | 且，兩個條件都要成立 | `WHERE category = '3C' AND price > 1000`     |
| OR     | 或，其中一個條件成立 | `WHERE category = '3C' OR category = '配件'` |

#### 已上架的 3C 商品：

```sql
SELECT name, price, stock
FROM products
WHERE status = 'active'
AND category = '3C';
```

![圖片09](./images/09.PNG)

#### 下架或無庫存商品：

```sql
SELECT name, status, stock
FROM products
WHERE status = 'inactive'
  OR stock = 0;
```

![圖片10](./images/10.PNG)

### 集合與範圍運算子

| 運算子  | 意義               | 範例                               |
| :------ | :----------------- | :--------------------------------- |
| BETWEEN | 在指定的範圍內     | `WHERE price BETWEEN 100 AND 500`  |
| IN      | 包含在列出的值中   | `WHERE category IN ('3C', '配件')` |
| NOT IN  | 不包含在列出的值中 | `WHERE category NOT IN ('3C')`     |

#### 想幫家人買生日禮物，預算在 500~1000 元：

```sql
SELECT * FROM products
WHERE discount_price BETWEEN 500 AND 1000;
```

![圖片11](./images/11.PNG)

#### 找出特定類別商品：

```sql
SELECT * FROM products
WHERE category IN ('3C');
```

![圖片12](./images/12.PNG)

#### 排除特定商品 (充電線、手機殼、螢幕保護貼)：

```sql
SELECT * FROM products
WHERE name NOT IN ('充電線', '手機殼', '螢幕保護貼');
```

![圖片13](./images/13.PNG)

## 更新資料

![圖片14](./images/14.PNG)

#### 調整特定商品的價格：

```sql
UPDATE products
SET price = 28000
WHERE name = 'iPhone 16';
```

#### 更新庫存數量：

```sql
UPDATE products
SET stock = stock + 50
WHERE name = '手機殼';
```

#### 同時修改多個欄位：

```sql
UPDATE products
SET stock = stock + 50, name = '充電線-修正'
WHERE name = '充電線';
```

## 刪除資料

![圖片15](./images/15.PNG)

#### 單筆刪除 - 刪除特定商品：

```sql
DELETE FROM products
WHERE name = 'iPhone 16';
```

#### 條件刪除 - 刪除類別為 3C 的商品：

```sql
DELETE FROM products
WHERE category = '3C';
```

#### 多重條件刪除 - 刪除沒庫存且已下架的商品 ：

```sql
DELETE FROM products
WHERE stock = 0 AND status = 'inactive';
```

## 小節作業：家具店的菜雞銷售奇遇記

[小節作業](./homework/work1/homework_1.md)

## 資料庫與 Docker 環境建立

以下皆使用 Windows 環境。

### 安裝 DBeaver

[DBeaver](https://dbeaver.io/) 是一個 SQL 客戶端和資料庫管理工具。

安裝 Windows 版本：

![圖片16](./images/16.PNG)

安裝完成：

![圖片17](./images/17.PNG)

### 安裝 Docker Desktop

[Docker](https://www.docker.com/) 是一種軟體平台，可讓你快速地建置、測試和部署應用程式。

Docker 將軟體封裝到名為**容器**的標準化單位，其中包含程式庫、系統工具、程式碼和執行時間等執行軟體所需的所有項目。

![圖片18](./images/18.PNG)

安裝 Windows 版本：

> [Windows 排錯教學](https://chalk-freedom-ec6.notion.site/Windows-89848c476c6b44928bd9ead5046fa06b)

![圖片19](./images/19.PNG)

安裝完成：

![圖片20](./images/20.PNG)

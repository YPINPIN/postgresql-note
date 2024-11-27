# PostgreSQL 資料庫學習紀錄

主要根據六角學院 「2024 後端工程師 - 資料庫體驗營」整理的學習筆記，方便查閱。

- [課程簡報](https://gamma.app/docs/SQL--23heyix4r93u5u0?mode=doc#card-zbtn6kfn3th4gpr)

- [每日任務紀錄](./task/task.md)

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

- [Tables 資料表管理](#tables-資料表管理)

  - [從單張資料表升級到多張資料表的管理流程](#從單張資料表升級到多張資料表的管理流程)

  - [主鍵和外來鍵的差異](#主鍵和外來鍵的差異)

  - [如何規劃外來鍵與資料表拆分](#如何規劃外來鍵與資料表拆分)

  - [主鍵設定方式、ID 自動遞增方法](#主鍵設定方式id-自動遞增方法)

  - [建立完整資料庫流程](#建立完整資料庫流程)

  - [合併資料表查詢](#合併資料表查詢)

  - [主鍵、外來鍵 constraint 約束提醒](#主鍵外來鍵-constraint-約束提醒)

  - [小節作業：主鍵、外來鍵、INNER JOIN](#小節作業主鍵外來鍵inner-join)

- [postgres 函式](#postgres-函式)

  - [NULL 介紹](#null-介紹)

  - [COALESCE 函數](#coalesce-函數)

  - [DISTINCT 不重複函數](#distinct-不重複函數)

  - [COUNT 函數](#count-函數)

  - [AVG、SUM、MAX、MIN 函數](#avgsummaxmin-函數)

  - [UUID 介紹](#uuid-介紹)

- [JOIN 資料關聯](#join-資料關聯)

  - [INNER JOIN](#inner-join)

  - [LEFT JOIN](#left-join)

  - [FULL JOIN](#full-join)

  - [JOIN 搭配 COALESCE 設計](#join-搭配-coalesce-設計)

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

## Tables 資料表管理

[miro 簡報](https://miro.com/app/board/uXjVLMcM_hQ=/)

### 從單張資料表升級到多張資料表的管理流程

![圖片21](./images/21.PNG)

當我們只有單張資料表時，想要修改部門名稱的開發部為資訊部時，就會**需要花較多的時間去針對所有開發部的員工資料去進行寫入修改**，對資料庫的效能來說較差。

實務上則是會拆分成多張資料表來進行管理，如下圖：

![圖片22](./images/22.PNG)

多了一個部門編號 (team_id) 的欄位，並且其會對應到新的部門名稱資料表 (teams) 中的部門編號 (id) 的欄位。

這時候如果想要修改開發部為資訊部時，就只需要對部門名稱資料表 (teams) 進行寫入修改即可。

### 主鍵和外來鍵的差異

當拆分成多張資料表進行管理時，會將資料表的指定欄位設定為主鍵和外來鍵來進行多張資料表之間的關聯。

![圖片23](./images/23.PNG)

- 主鍵 ( Primary Key，簡稱 PK )

  - 1.每個資料表都要有**一個主鍵** (只能有一個)。

  - 2.主鍵的值必須是**唯一的**，**不能重複**。

  - 3.主鍵不能是 `Null` 值，但外來鍵可以是 `Null` 值。

  - 4.大部分情況會命名為 `'id'`，使用整數或 UUID 。

  - 5.設定後就不應該再更動。

- 外來鍵 ( Foreign Key，簡稱 FK )

  - 1.當資料**需要關聯到其他表格**時才會用到。

  - 2.同一個資料表中，可以有多個外來鍵。

  - 3.命名通常會用 `'參考資料表_id'` 的格式。

  - 4.外來鍵必須**對應到被參考資料表的主鍵**。

剛剛的範例中就是將員工資料表 (users) 的員工編號 (id) 設為主鍵 (PK)，而外來鍵 (FK) 部門編號 (team_id) 則是會對應到部門名稱資料表 (teams) 中的主鍵 (PK) 部門編號 (id)。

### 如何規劃外來鍵與資料表拆分

![圖片24](./images/24.PNG)

### 主鍵設定方式、ID 自動遞增方法

可以在新增資料表時，使用 `PRIMARY KEY` 設定主鍵，使用 `SERIAL` 則是可以設定欄位為 ID 自動遞增。

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY, -- 員工編號，主鍵
  name VARCHAR(50) -- 員工姓名
);
```

現在新增資料只需要設定員工姓名 ( name ) 欄位，員工編號 ( id ) 則會自己遞增。

```sql
INSERT INTO users (name)
VALUES
  ('張小美'),
  ('張小立'),
  ('張小花');
```

![圖片25](./images/25.PNG)

### 建立完整資料庫流程

根據以下結構建立完整的資料表。

![圖片26](./images/26.PNG)

- 建立 teams 資料表

  ```sql
  CREATE TABLE teams (
    id SERIAL PRIMARY KEY, -- 部門編號，主鍵
    name VARCHAR(50)  -- 部門名稱
  );
  ```

- 建立 users 資料表

  使用 `FOREIGN KEY` (未來鍵欄位) `REFERENCES` 資料表 (主鍵欄位) 語法來設定外來鍵並關聯資料表主鍵。

  ```sql
  CREATE TABLE users (
    id SERIAL PRIMARY KEY, -- 員工編號，主鍵
    name VARCHAR(50), -- 姓名
    salary INTEGER, -- 薪資
    team_id INTEGER,  -- 部門編號，外來鍵
    FOREIGN KEY (team_id) REFERENCES teams (id) -- 設定外來鍵關聯
  );
  ```

- 新增部門 ( teams ) 資料

  ```sql
  INSERT INTO teams (name)
  VALUES
    ('開發部'),
    ('人事部');
  ```

  ![圖片27](./images/27.PNG)

- 新增員工 ( users ) 資料

  ```sql
  INSERT INTO users (name, salary, team_id)
  VALUES
    ('張小明', 45000, 1),
    ('王大明', 48000, 1),
    ('李小華', 52000, 2),
    ('陳小玉', 55000, 2),
    ('林小豪', 47000, 1);
  ```

  ![圖片28](./images/28.PNG)

### 合併資料表查詢

- 搭配 `WHERE` 條件。

  同時查詢兩張表的指定欄位，並搭配 `WHERE` 根據部門 id 查詢符合的資料。

  > `users.name` 和 `teams.name` 欄位名稱相同，可能導致錯誤，因此可以使用 `AS` 指定欄位別名。

  ```sql
  SELECT
    users.id,
    users.name,
    users.salary,
    teams.name AS 部門名稱
  FROM users,teams
  WHERE users.team_id = teams.id;
  ```

  ![圖片29](./images/29.PNG)

- 使用 `INNER JOIN`。

  實際結果與 `WHERE` 相同，主流建議使用這種方式。

  `INNER JOIN` 要合併的資料表 `ON` 條件。

  > `FROM` 後面只需要主要查詢的資料表。

  ```sql
  SELECT
    users.id,
    users.name,
    users.salary,
    teams.name AS 部門名稱
  FROM users
  INNER JOIN teams ON users.team_id = teams.id;
  ```

  ![圖片30](./images/30.PNG)

### 主鍵、外來鍵 constraint 約束提醒

- 當新增資料時主鍵重複，會出現錯誤提示。

  ```sql
  INSERT INTO users (id, name, salary, team_id)
  VALUES (4, '李小花', 48000, 2);
  ```

  ![圖片31](./images/31.PNG)

- 當新增資料時外來鍵不存在對應資料，會出現錯誤提示。

  ```sql
  INSERT INTO users (name, salary, team_id)
  VALUES ('李小花', 48000, 3);
  ```

  ![圖片32](./images/32.PNG)

### 小節作業：主鍵、外來鍵、INNER JOIN

[小節作業](./homework/work2/homework_2.md)

## postgres 函式

### NULL 介紹

`NULL` 在資料庫中表示「未知」或「沒有值」。和零 (0) 或空字串 ('') 是不同的概念。

- 使用時機

  用來表示資料庫中某個欄位尚未填入資料。

  在實際應用中，並不是所有資料在一開始就具備，像是表單的某些欄位可能會被使用者選擇性填寫。例如：

  - 社群網站的個人大頭照

  - 新員工尚未被指派部門，需先顯示為 `null`

- 使用方法

  可以設置 `NOT NULL` 表示欄位必填，`NULL` 則表示欄位可為空。

  ```sql
  -- 建立員工資料表
  CREATE TABLE users (
      id SERIAL PRIMARY KEY,
      name VARCHAR(50) NOT NULL,
      email VARCHAR(100) NOT NULL,
      salary INTEGER NULL
  );

  -- 新增員工資料
  INSERT INTO users (name, email)
  VALUES ('王小明', 'dfksfdh@gamil.com');
  ```

  ![圖片33](./images/33.PNG)

  若是未正確設置必填欄位，則會出現錯誤提示。

  ```sql
  -- 新增員工資料 (未正確設置必填欄位)
  INSERT INTO users (name)
  VALUES ('王小美');
  ```

  ![圖片34](./images/34.PNG)

### COALESCE 函數

用來檢查並處理 `NULL` 結果。

會依序檢查傳入的參數，**回傳參數列表中第一個不是 `NULL` 的值**。

- users 資料表

  ```sql
  -- 建立員工資料表
  CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    team_name VARCHAR(50) NULL,  -- 可能還沒分配部門
    salary INTEGER NULL         -- 可能還沒設定薪水
  );
  ```

- 模擬員工資料

  ```sql
  -- 插入員工資料，展示不同情況
  INSERT INTO users (name, email, team_name, salary) VALUES
    -- 完整資料的員工
    ('張小明', 'zhang@example.com', '開發部', 45000),
    ('王大明', 'wang@example.com', '行銷部', 48000),

    -- 還沒分配部門，但已有薪水
    ('李小華', 'lee@example.com', NULL, 42000),
    ('陳小玲', 'chen@example.com', NULL, 44000),

    -- 已分配部門，但還沒定薪水
    ('林小美', 'lin@example.com', '人事部', NULL),
    ('周小威', 'zhou@example.com', '開發部', NULL),

    -- 新進員工，部門和薪水都還沒設定
    ('劉小安', 'liu@example.com', NULL, NULL),
    ('黃小凱', 'huang@example.com', NULL, NULL);
  ```

- 原始資料表

  ![圖片35](./images/35.PNG)

- 使用 COALESCE 函數整理查詢結果

  ```sql
  SELECT
    id,
    name,
    email,
    COALESCE(team_name, '待分配') as team_name,
    COALESCE(salary, 0) as salary
  FROM users;
  ```

  ![圖片36](./images/36.PNG)

### DISTINCT 不重複函數

SQL 中用於**去除重複資料**的關鍵字。

- users 資料表

  ```sql
  -- 建立員工資料表
  CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    team_name VARCHAR(50) NULL,
    salary INTEGER NULL
  );
  ```

- 模擬員工資料

  ```sql
  INSERT INTO users (name, email, team_name, salary)
  VALUES
      ('陳志明', 'chihming.chen@company.com', '開發部', 68000),
      ('林雅婷', 'yating.lin@company.com', '開發部', 55000),
      ('王建宏', 'jianhong.wang@company.com', '開發部', 46000),
      ('張美玲', 'meiling.chang@company.com', '人事部', 42000),
      ('黃佳琳', 'jialin.huang@company.com', '人事部', 48000);
  ```

- 原始資料表

  ![圖片37](./images/37.PNG)

- 使用 `DISTINCT` 查詢有哪些部門

  ```sql
  SELECT DISTINCT team_name FROM users;
  ```

  ![圖片38](./images/38.PNG)

### COUNT 函數

COUNT 是一個計數函數，用來**計算資料表中的資料筆數**
。是最常用的集合函數（Aggregate Function）之一。

- 資料表

  ![圖片39](./images/39.PNG)

- 計算總共有幾個員工

  ```sql
  SELECT COUNT(*) AS 員工總數
  FROM users;
  ```

  ![圖片40](./images/40.PNG)

- 計算開發部有幾個員工

  ```sql
  SELECT COUNT(*) AS 開發部人數
  FROM users
  WHERE team_name = '開發部';
  ```

  ![圖片41](./images/41.PNG)

- 計算薪水大於 45000 的員工人數

  ```sql
  SELECT COUNT(*) AS 高薪員工數
  FROM users
  WHERE salary > 45000;
  ```

  ![圖片42](./images/42.PNG)

### AVG、SUM、MAX、MIN 函數

AVG（Average）：計算一組數值的「平均值」。

SUM（Summary）：計算一組數值的「總和」。

MAX（Maximum）：找出一組數值中的「最大值」。

MIN（Minimum）：找出一組數值中的「最小值」。

- 資料表

  ![圖片39](./images/39.PNG)

- 公司整體統計

  ```sql
  SELECT
    AVG(salary) AS 平均薪資,
    SUM(salary) AS 總薪資
  FROM users;
  ```

  ![圖片43](./images/43.PNG)

- 開發部統計

  ```sql
  SELECT
    AVG(salary) AS 開發部平均薪資,
    SUM(salary) AS 開發部總薪資
  FROM users
  WHERE team_name = '開發部';
  ```

  ![圖片44](./images/44.PNG)

- 完整統計資料

  ```sql
  SELECT
    COUNT(*) AS 員工數,
    AVG(salary) AS 平均薪資,
    SUM(salary) AS 總薪資,
    MAX(salary) AS 最高薪資,
    MIN(salary) AS 最低薪資
  FROM users;
  ```

  ![圖片45](./images/45.PNG)

### UUID 介紹

資料庫開啟 UUID 功能 (postgresql 13 版本以上不用)：

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
```

SERIAL 跟 UUID 差異：

| 特性     | SERIAL                  | UUID                                                       |
| :------- | :---------------------- | :--------------------------------------------------------- |
| 格式     | 整數 (1, 2, 3...)       | 32 位元十六進制 (如：550e8400-e29b-41d4-a716-446655440000) |
| 生成方式 | 自動遞增                | 隨機生成                                                   |
| 空間使用 | 小                      | 大                                                         |
| 優點     | 簡單、易讀、節省空間    | 全球唯一、安全性高                                         |
| 建立語法 | `id SERIAL PRIMARY KEY` | `id UUID PRIMARY KEY DEFAULT gen_random_uuid()`            |
| 可預測性 | 容易預測下一個值        | 完全隨機，無法預測                                         |

- users 資料表 (使用 UUID)

  ```sql
  -- 建立員工資料表
  CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    team_name VARCHAR(50) NULL,
    salary INTEGER NULL
  );
  ```

- 模擬員工資料

  ```sql
  INSERT INTO users (name, email, team_name, salary)
  VALUES
      ('陳志明', 'chihming.chen@company.com', '開發部', 68000),
      ('林雅婷', 'yating.lin@company.com', '開發部', 55000),
      ('王建宏', 'jianhong.wang@company.com', '開發部', 46000),
      ('張美玲', 'meiling.chang@company.com', '人事部', 42000),
      ('黃佳琳', 'jialin.huang@company.com', '人事部', 48000);
  ```

- 資料表結果

  ![圖片46](./images/46.PNG)

## JOIN 資料關聯

[miro 簡報](https://miro.com/app/board/uXjVLKVNa80=/)

可將兩個資料表 (Table) 組合在一起。

JOIN 語法種類：

- inner join (最常使用)

- outer join

  - left join

  - right join

  - full join

模擬資料：

```sql
-- 建立部門資料表
CREATE TABLE teams (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50)
);

-- 建立員工資料表
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    salary INTEGER,
    team_id INTEGER,  -- 允許 NULL，代表未分配部門的員工
    FOREIGN KEY (team_id) REFERENCES teams(id)
);

-- 新增部門資料
INSERT INTO teams (name)
VALUES
    ('開發部'),
    ('人事部'),
    ('行銷部'), -- 新成立還沒有人的部門
    ('研發部'); -- 新成立還沒有人的部門

-- 新增員工資料
INSERT INTO users (name, salary, team_id)
VALUES
    ('張小明', 45000, 1),
    ('王大明', 48000, 1),
    ('李小華', 52000, 2),
    ('陳小玉', 55000, 2),
    ('林小豪', 47000, 1),
    ('陳小明', 42000, NULL),  -- 新進員工，還沒分配部門
    ('王小美', 41000, NULL);  -- 新進員工，還沒分配部門
```

![圖片47](./images/47.PNG)

![圖片48](./images/48.PNG)

### INNER JOIN

`INNER JOIN` 為只顯示條件符合的合併資料。

![圖片49](./images/49.PNG)

#### 情境：查詢有部門的員工

```sql
SELECT
  users.id,
  users.name AS users_name,
  users.salary,
  teams.name AS teams_name
FROM users
INNER JOIN teams ON users.team_id = teams.id;
```

![圖片50](./images/50.PNG)

### LEFT JOIN

`LEFT JOIN` 為包含 `INNER JOIN` 的合併特性，但是同時也保留顯示未符合條件的的**左側資料**，未符合的資料則會顯示 `NULL`。

![圖片51](./images/51.PNG)

#### 情境：未分配部門的員工資料處理

```sql
SELECT
  users.name AS users_name,
  teams.name AS teams_name
FROM users
LEFT JOIN teams ON users.team_id = teams.id;
```

![圖片52](./images/52.PNG)

### RIGHT JOIN

`RIGHT JOIN` 為包含 `INNER JOIN` 的合併特性，但是與 `LEFT JOIN` 相反會保留顯示未符合條件的的**右側資料**，未符合的資料則會顯示 `NULL`。

![圖片53](./images/53.PNG)

#### 情境：查詢空部門的報表

```sql
SELECT
  teams.name AS teams_name,
  users.name AS users_name
FROM users
RIGHT JOIN teams ON users.team_id = teams.id;
```

![圖片54](./images/54.PNG)

同時也可以搭配 `WHERE` 篩選查詢：

```sql
SELECT
  teams.name AS teams_name,
  users.name AS users_name
FROM users
RIGHT JOIN teams ON users.team_id = teams.id
WHERE users.name IS NULL;
```

![圖片55](./images/55.PNG)

### FULL JOIN

`FULL JOIN` 其實就是 `LEFT JOIN` 和 `RIGHT JOIN` 的組合。

![圖片56](./images/56.PNG)

#### 情境：部門配置與新進人員總覽

```sql
SELECT
	teams.name AS team_name,
  users.name AS user_name
FROM users
FULL JOIN teams ON users.team_id = teams.id;
```

![圖片57](./images/57.PNG)

### JOIN 搭配 COALESCE 設計

可以搭配 `COALESCE` 來處理 `NULL` 資料顯示。

```sql
SELECT
	COALESCE(teams.name, '未分配') AS team_name,
  COALESCE(users.name, '無員工') AS user_name
FROM users
FULL JOIN teams ON users.team_id = teams.id;
```

![圖片58](./images/58.PNG)

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

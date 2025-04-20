## 連線與資料庫操作

```bash
# 連線資料庫
psql -U [user] -d [database]
```

```sql
-- 查看所有資料庫
\l

-- 建立資料庫
CREATE DATABASE dbname;

-- 切換資料庫
\c dbname

-- 刪除資料庫
DROP DATABASE dbname;
```

---

## 資料表操作

```sql
-- 查看所有資料表
\dt

-- 建立資料表
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT UNIQUE
);

-- 查看資料表結構
\d users

-- 刪除資料表
DROP TABLE users;

-- 修改資料表
ALTER TABLE users ADD COLUMN age INT;

-- 刪除資料表欄位
ALTER TABLE users DROP COLUMN age;
```

---

## 資料操作

```sql
-- 新增資料
INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');

-- 查詢資料
SELECT * FROM users;
SELECT name FROM users WHERE id = 1;

-- 更新資料
UPDATE users SET name = 'Bob' WHERE id = 1;

-- 刪除資料
DELETE FROM users WHERE id = 1;
```

---

## 查詢進階技巧

```sql
-- 排序
SELECT * FROM users ORDER BY name DESC;

-- 限制筆數
SELECT * FROM users LIMIT 5 OFFSET 10;

-- JOIN
SELECT users.name, orders.total
FROM users
JOIN orders ON users.id = orders.user_id;

-- GROUP BY / 聚合
SELECT country, COUNT(*) FROM users GROUP BY country;
```

## 聚合函式

| 函式 | 說明 | 範例 |
| --- | --- | --- |
| COUNT() | 計算筆數 | COUNT(*)、COUNT(column) |
| SUM() | 加總 | SUM(amount) |
| AVG() | 平均值 | AVG(score) |
| MIN() | 最小值 | MIN(price) |
| MAX() | 最大值 | MAX(price) |
| GROUP_CONCAT() 或 STRING_AGG() | 將多筆資料合併為字串 | STRING_AGG(name, ', ') |

## 字串處理函式

| 函式 | 說明 | 範例 |
| --- | --- | --- |
| UPPER() | 全轉大寫 | UPPER(name) |
| LOWER() | 全轉小寫 | LOWER(name) |
| LENGTH() | 字串長度 | LENGTH(email) |
| SUBSTRING() | 取部分字串 | SUBSTRING(name, 1, 3) |
| TRIM() | 去除首尾空白或特定字元 | TRIM(name) |
| CONCAT() | 字串合併 | CONCAT(first, ' ', last) |

## 日期時間函式

| 函式 | 說明 | 範例 |
| --- | --- | --- |
| NOW() | 目前系統時間 | NOW() |
| CURRENT_DATE | 當天日期 | CURRENT_DATE |
| DATE_TRUNC() | 截斷時間單位（PostgreSQL） | DATE_TRUNC('month', created_at) |
| EXTRACT() | 擷取年/月/日等 | EXTRACT(YEAR FROM created_at) |
| AGE() | 計算兩日期差（PostgreSQL） | AGE(NOW(), birth_date) |

## 條件函式 / 邏輯函式

| 函式 | 說明 | 範例 |
| --- | --- | --- |
| COALESCE() | 回傳第一個非 NULL 的值 | COALESCE(nickname, username) |
| NULLIF() | 如果兩值相等，回傳 NULL，否則回傳第一個 | NULLIF(score, 0) |
| CASE | 條件式處理 | CASE WHEN score > 60 THEN 'Pass' ELSE 'Fail' END |
# 2. Thiết kế & truy vấn nâng cao

## Mục tiêu

Viết truy vấn nhiều bảng, tổng hợp dữ liệu, và dùng CTE / window functions.

## JOIN

- `INNER JOIN`: chỉ hàng khớp ở cả hai phía.
- `LEFT JOIN`: giữ tất cả hàng bên trái, bên phải thiếu thì NULL.
- `RIGHT JOIN` / `FULL OUTER JOIN`: tương tự, đủ cả hai phía hoặc đầy đủ.
- Lưu ý: điều kiện lọc nên đặt rõ trong `ON` vs `WHERE` để tránh kết quả không mong muốn với outer join.

```sql
SELECT o.id, c.name
FROM orders o
LEFT JOIN customers c ON c.id = o.customer_id;
```

## Aggregate & GROUP BY

```sql
SELECT customer_id, COUNT(*) AS order_count, SUM(total)::NUMERIC(12,2)
FROM orders
GROUP BY customer_id
HAVING COUNT(*) >= 2;
```

`FILTER (WHERE ...)` dùng trong aggregate để đếm/tổng có điều kiện:

```sql
SELECT
  COUNT(*) FILTER (WHERE status = 'paid') AS paid_orders
FROM orders;
```

## CTE (`WITH`)

Tách bước truy vấn, dễ đọc và tái sử dụng tên trong cùng câu lệnh.

```sql
WITH heavy AS (
  SELECT id FROM orders WHERE total > 1000
)
SELECT c.name
FROM customers c
JOIN heavy h ON h.id IN (SELECT id FROM orders WHERE customer_id = c.id);
```

(Có thể viết lại tối ưu hơn tùy schema; mục đích là minh họa cú pháp CTE.)

## Window functions

Tính toán trên “cửa sổ” các hàng liên quan mà không gộp thành một hàng như `GROUP BY`.

```sql
SELECT
  id,
  total,
  ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY created_at) AS seq,
  SUM(total) OVER (PARTITION BY customer_id) AS customer_total
FROM orders;
```

Hữu ích: xếp hạng, tổng lũy kế, so sánh với trung bình nhóm.

## Set operations

`UNION` / `UNION ALL`, `INTERSECT`, `EXCEPT` — kết hợp kết quả nhiều `SELECT` (cùng số cột, kiểu tương thích).

## Bài tập gợi ý

1. Top 3 khách hàng theo doanh thu trong 30 ngày gần nhất (dùng `JOIN` + aggregate).
2. Với mỗi khách hàng, gán `ROW_NUMBER()` theo ngày đặt hàng mới nhất.
3. Viết CTE: bước 1 lọc đơn “paid”, bước 2 join sang chi tiết.

## Tài liệu chính thức

- [Queries](https://www.postgresql.org/docs/current/queries.html)
- [Window functions](https://www.postgresql.org/docs/current/tutorial-window.html)

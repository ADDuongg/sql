# 5. Index & tối ưu truy vấn

## Mục tiêu

Chọn loại index phù hợp và đọc được kế hoạch thực thi cơ bản.

## Các loại index (tổng quan)

| Loại | Khi nào nghĩ tới |
|------|------------------|
| **B-tree** (mặc định) | So sánh `=`, `<`, `>`, `BETWEEN`, `ORDER BY` |
| **Hash** | Chỉ equality (ít dùng trực tiếp) |
| **GIN** | `JSONB`, full-text, mảng, nhiều giá trị trong một cột |
| **GiST** | Geometry, full-text, một số kiểu đặc biệt |
| **BRIN** | Bảng rất lớn, dữ liệu tương đối theo thứ tự vật lý (ví dụ theo thời gian) |

## Index đặc biệt

- **Partial**: chỉ index tập con hàng (`WHERE active = true`).
- **Expression**: index trên biểu thức (`lower(email)`).
- **INCLUDE**: cột “đi kèm” trong index để **index-only scan** (covering).

```sql
CREATE INDEX idx_orders_created ON orders (created_at);
CREATE INDEX idx_orders_open ON orders (customer_id) WHERE status = 'open';
```

## EXPLAIN

```sql
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT * FROM orders WHERE created_at > now() - interval '7 days';
```

- **Seq Scan**: quét cả bảng — có thể chấp nhận được với bảng nhỏ hoặc đọc gần hết.
- **Index Scan / Bitmap Index Scan**: dùng index.
- `ANALYZE` chạy thật — chỉ dùng trên môi trường thử, cẩn thận tải production.

## Thống kê cho planner

Sau thay đổi dữ liệu lớn: `ANALYZE tên_bảng;` (hoặc autovacuum analyze).

## Bài tập gợi ý

1. So sánh `EXPLAIN` cùng một truy vấn trước và sau khi thêm index.
2. Thử partial index cho truy vấn lọc theo `status` cố định.

## Tài liệu chính thức

- [Indexes](https://www.postgresql.org/docs/current/indexes.html)
- [EXPLAIN](https://www.postgresql.org/docs/current/sql-explain.html)
- [Planner stats](https://www.postgresql.org/docs/current/planner-stats.html)

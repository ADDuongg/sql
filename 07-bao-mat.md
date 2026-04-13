# 7. Bảo mật: role, quyền & RLS

## Mục tiêu

Phân quyền tối thiểu và (tuỳ nhu cầu) kiểm soát theo từng hàng.

## Role

Trong PostgreSQL, **user** và **group** đều là **role**.

```sql
CREATE ROLE app_reader LOGIN PASSWORD '...';
GRANT CONNECT ON DATABASE mydb TO app_reader;
GRANT USAGE ON SCHEMA public TO app_reader;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO app_reader;
```

Nguyên tắc: quyền tối thiểu; tách role đọc/ghi.

## Row Level Security (RLS)

Bật trên bảng; chỉ các hàng thỏa **policy** mới thấy được (theo `current_user` hoặc biểu thức).

```sql
ALTER TABLE orders ENABLE ROW LEVEL SECURITY;

CREATE POLICY orders_own ON orders
  FOR ALL
  USING (customer_id = current_setting('app.customer_id')::bigint);
```

Cần thiết kế cách set `app.customer_id` (session / JWT mapping) — phần ứng dụng phải phối hợp.

## Ghi nhớ

- `pg_hba.conf`: ai kết nối bằng phương thức nào, từ đâu.
- SSL/TLS cho kết nối production.

## Bài tập gợi ý

1. Tạo hai role: chỉ `SELECT` và `SELECT, INSERT, UPDATE`.
2. Đọc doc policy `FOR SELECT` vs `FOR ALL` và thử một ví dụ đơn giản.

## Tài liệu chính thức

- [Database roles](https://www.postgresql.org/docs/current/user-manag.html)
- [Row security policies](https://www.postgresql.org/docs/current/ddl-rowsecurity.html)

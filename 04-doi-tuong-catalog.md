# 4. View, sequence, function, trigger

## Mục tiêu

Dùng các đối tượng trong catalog để tái sử dụng logic và tự động hóa.

## View

Bảng ảo từ câu `SELECT`; có thể `INSERT`/`UPDATE`/`DELETE` nếu thỏa điều kiện (updatable view).

```sql
CREATE VIEW active_customers AS
SELECT id, name, email FROM customers WHERE active = true;
```

## Materialized view

Lưu **kết quả** vật lý; cần `REFRESH` khi dữ liệu nguồn đổi — phù hợp báo cáo nặng, đọc nhiều.

```sql
CREATE MATERIALIZED VIEW sales_summary AS
SELECT date_trunc('day', created_at) AS day, SUM(total) AS revenue
FROM orders GROUP BY 1;
REFRESH MATERIALIZED VIEW sales_summary;
```

## Sequence & identity

- `SERIAL` / `BIGSERIAL` là shortcut cho sequence + default.
- PostgreSQL 10+: `GENERATED ALWAYS AS IDENTITY` / `BY DEFAULT AS IDENTITY`.

## Function & procedure

```sql
CREATE OR REPLACE FUNCTION add_tax(amount NUMERIC)
RETURNS NUMERIC LANGUAGE sql IMMUTABLE AS
$$ SELECT round(amount * 1.1, 2); $$;
```

**PL/pgSQL** cho logic có nhánh, vòng lặp, exception. **PROCEDURE** (`CALL`) từ PG11, có thể `COMMIT` bên trong (khác function).

## Trigger

Thực thi function trước/sau `INSERT`/`UPDATE`/`DELETE`.

```sql
CREATE FUNCTION set_updated_at() RETURNS trigger AS $$
BEGIN NEW.updated_at := now(); RETURN NEW; END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER tr_products_updated
BEFORE UPDATE ON products
FOR EACH ROW EXECUTE PROCEDURE set_updated_at();
```

*(PostgreSQL 14+: có thể dùng `EXECUTE FUNCTION` thay cho `EXECUTE PROCEDURE`.)*

## Bài tập gợi ý

1. Tạo view join 2 bảng dùng cho dashboard.
2. Viết trigger ghi audit log vào bảng `audit_log` khi `UPDATE`.

## Tài liệu chính thức

- [Views](https://www.postgresql.org/docs/current/sql-createview.html)
- [Triggers](https://www.postgresql.org/docs/current/trigger-definition.html)
- [PL/pgSQL](https://www.postgresql.org/docs/current/plpgsql.html)

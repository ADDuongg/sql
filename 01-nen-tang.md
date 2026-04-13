# 1. Nền tảng PostgreSQL

## Mục tiêu

Hiểu mô hình dữ liệu quan hệ trong PostgreSQL và viết được SQL CRUD cơ bản.

## Khái niệm

- **ORDBMS**: PostgreSQL là hệ quản trị cơ sở dữ liệu quan hệ–đối tượng; dữ liệu chính nằm trong **bảng** (relation): hàng (row) và cột (column).
- **Cluster**: một tiến trình server quản lý một **cluster** PostgreSQL.
- **Database**: trong cluster có nhiều database độc lập (schema + dữ liệu riêng).
- **Schema**: namespace trong database (`public` là mặc định); nhóm bảng, function, v.v.

## Kiểu dữ liệu thường gặp

- Số: `SMALLINT`, `INTEGER`, `BIGINT`, `NUMERIC(p,s)`, `REAL`, `DOUBLE PRECISION`
- Chuỗi: `TEXT`, `CHAR(n)`, `VARCHAR(n)`
- Thời gian: `DATE`, `TIME`, `TIMESTAMP`, `TIMESTAMPTZ`
- Khác: `BOOLEAN`, `UUID`, `BYTEA`

## SQL cơ bản

```sql
-- Tạo bảng
CREATE TABLE products (
  id          BIGSERIAL PRIMARY KEY,
  name        TEXT NOT NULL,
  price       NUMERIC(10,2) NOT NULL CHECK (price >= 0),
  created_at  TIMESTAMPTZ NOT NULL DEFAULT now()
);

INSERT INTO products (name, price) VALUES ('Notebook', 29.99);
SELECT id, name, price FROM products WHERE price < 50 ORDER BY name;
UPDATE products SET price = 24.99 WHERE id = 1;
DELETE FROM products WHERE id = 1;
```

## Ràng buộc (constraints)

| Ràng buộc | Ý nghĩa |
|-----------|---------|
| `PRIMARY KEY` | Duy nhất, không NULL, định danh hàng |
| `FOREIGN KEY` | Tham chiếu tới khóa ở bảng khác |
| `UNIQUE` | Giá trị (hoặc tổ hợp cột) không trùng |
| `NOT NULL` | Bắt buộc có giá trị |
| `CHECK` | Điều kiện logic trên cột/hàng |
| `DEFAULT` | Giá trị mặc định khi không INSERT |

## Index nhập môn

```sql
CREATE INDEX idx_products_name ON products (name);
```

Index giúp tăng tốc truy vấn lọc/sắp theo cột được index (đổi lại: chi phí ghi và dung lượng).

## Bài tập gợi ý

1. Tạo bảng `customers` và `orders` với khóa ngoại.
2. Thêm `UNIQUE` cho email khách hàng.
3. Tạo index trên cột hay dùng trong `WHERE`.

## Tài liệu chính thức

- [Concepts](https://www.postgresql.org/docs/current/tutorial-concepts.html)
- [Data types](https://www.postgresql.org/docs/current/datatype.html)

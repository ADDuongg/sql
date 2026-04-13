# 6. JSON/JSONB, full-text search & extension

## Mục tiêu

Lưu và truy vấn dữ liệu bán cấu trúc, tìm kiếm văn bản, và cài extension chuẩn.

## JSON vs JSONB

- **JSON**: lưu text gốc, parse lại mỗi lần đọc.
- **JSONB**: nhị phân đã parse; hỗ trợ index GIN; thường **nên dùng JSONB** cho truy vấn.

```sql
CREATE TABLE events (id BIGSERIAL PRIMARY KEY, payload JSONB NOT NULL);
INSERT INTO events (payload) VALUES ('{"user": 1, "action": "login"}');

SELECT * FROM events WHERE payload->>'action' = 'login';
SELECT * FROM events WHERE payload @> '{"user": 1}';

CREATE INDEX idx_events_payload ON events USING GIN (payload);
```

Toán tử hữu ích: `->`, `->>`, `@>`, `?`, `?|`, `?&`.

## Full-text search

```sql
SELECT to_tsvector('english', 'PostgreSQL is powerful') @@ to_tsquery('english', 'postgres');
```

Cột kiểu `tsvector` (có thể lưu + index GIN) và truy vấn `tsquery`.

## Extension

```sql
CREATE EXTENSION IF NOT EXISTS pg_trgm;   -- tìm gần đúng chuỗi
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";  -- UUID (hoặc dùng gen_random_uuid() built-in)
```

Mỗi extension có tài liệu riêng trong PostgreSQL docs.

## Bài tập gợi ý

1. Lưu metadata linh hoạt dạng JSONB và viết 3 kiểu truy vấn khác nhau.
2. Thử `to_tsvector` trên cột `TEXT` và tạo index GIN.

## Tài liệu chính thức

- [JSON types](https://www.postgresql.org/docs/current/datatype-json.html)
- [Full text search](https://www.postgresql.org/docs/current/textsearch.html)
- [Additional supplied modules](https://www.postgresql.org/docs/current/contrib.html)

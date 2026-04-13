# 10. Nội bộ & mở rộng nâng cao

## Mục tiêu

Định hướng đọc sâu: đường đi của câu truy vấn, FDW, và mở rộng bằng C.

## Đường đi của một query (internals)

Tóm tắt: **parse** → **rewrite** (rules) → **plan** (optimizer) → **execute**.

Hiểu giúp debug “vì sao planner chọn plan này” và khi nào cần hint/thống kê.

## Foreign Data Wrapper (FDW)

Truy vấn nguồn ngoài (file, DB khác, API qua extension) như bảng ngoại.

```sql
-- Ví dụ khái niệm; cần extension postgres_fdw và SERVER/USER MAPPING
CREATE EXTENSION postgres_fdw;
-- CREATE SERVER ... ; IMPORT FOREIGN SCHEMA ...
```

## Mở rộng bằng C

Định nghĩa kiểu dữ liệu, operator, index method tùy chỉnh — đọc phần **Server Programming** trong docs khi thực sự cần.

## Khi nào đọc phần này

Sau khi thành thạo SQL, EXPLAIN, và vận hành cơ bản; dùng khi tối ưu hệ thống lớn hoặc tích hợp đặc thù.

## Tài liệu chính thức

- [Overview of PostgreSQL internals](https://www.postgresql.org/docs/current/overview.html)
- [Foreign data](https://www.postgresql.org/docs/current/fdwhandler.html)
- [Server programming](https://www.postgresql.org/docs/current/server-programming.html)

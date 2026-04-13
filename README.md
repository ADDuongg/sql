# Lộ trình học PostgreSQL

Thư mục này gồm các file theo thứ tự từ cơ bản đến nâng cao. Nên đọc lần lượt; mỗi file có phần **Tài liệu chính thức** để tra cứu sâu hơn.

## Thứ tự đọc

| Thứ tự | File | Nội dung |
|--------|------|----------|
| 1 | [01-nen-tang.md](./01-nen-tang.md) | Khái niệm cluster/database, SQL cơ bản, ràng buộc, index nhập môn |
| 2 | [02-thiet-ke-truy-van.md](./02-thiet-ke-truy-van.md) | JOIN, aggregate, CTE, window functions |
| 3 | [03-giao-dich-dong-thoi.md](./03-giao-dich-dong-thoi.md) | Transaction, ACID, isolation, MVCC (khái niệm) |
| 4 | [04-doi-tuong-catalog.md](./04-doi-tuong-catalog.md) | View, sequence, function/procedure, trigger |
| 5 | [05-index-toi-uu.md](./05-index-toi-uu.md) | Các loại index, EXPLAIN, thống kê |
| 6 | [06-json-fulltext-mo-rong.md](./06-json-fulltext-mo-rong.md) | JSON/JSONB, full-text, extension |
| 7 | [07-bao-mat.md](./07-bao-mat.md) | Role, GRANT, RLS |
| 8 | [08-van-hanh-hieu-nang.md](./08-van-hanh-hieu-nang.md) | VACUUM, WAL, backup, partition, pooling |
| 9 | [09-sao-chep-ha.md](./09-sao-chep-ha.md) | Replication, failover (tổng quan) |
| 10 | [10-noi-bo-mo-rong-sau.md](./10-noi-bo-mo-rong-sau.md) | Internals, FDW, mở rộng nâng cao |

## Tài liệu gốc

- [PostgreSQL Documentation (current)](https://www.postgresql.org/docs/current/)
- [Tutorial / Concepts](https://www.postgresql.org/docs/current/tutorial-concepts.html)

## Gợi ý thực hành

Cài PostgreSQL cục bộ hoặc dùng Docker, tạo database thử và chạy từng ví dụ `SQL` trong các file (hoặc biến thể của chúng).

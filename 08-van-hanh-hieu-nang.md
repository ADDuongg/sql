# 8. Vận hành & hiệu năng

## Mục tiêu

Hiểu VACUUM, WAL, backup cơ bản, partition và pooling — đủ để vận hành dev/small prod.

## VACUUM & AUTOVACUUM

- Sau MVCC, các bản ghi “chết” cần được dọn; không dọn có thể **bloat** bảng/index.
- **VACUUM** (thường tự động qua autovacuum) thu hồi không gian để tái sử dụng và cập nhật thống kê.
- **VACUUM FULL** khóa mạnh, chỉ khi hiểu rõ hậu quả.

Theo dõi: bloat, delay autovacuum, `dead_tuple` ratio.

## WAL (Write-Ahead Log)

Ghi log trước khi dữ liệu được coi là commit — đảm bảo durability và là nền cho replication/backup.

## Backup

- **Logical**: `pg_dump` / `pg_dumpall` — portable, schema + data.
- **Physical** + archive WAL: phục vụ **PITR** (point-in-time recovery).

Luôn thử restore định kỳ.

## Partitioning

Chia bảng lớn theo RANGE / LIST / HASH; planner có thể **partition pruning** bỏ partition không liên quan.

```sql
-- Khái niệm; cú pháp chi tiết xem doc Declarative Partitioning
CREATE TABLE measurements (
  log_date date not null,
  peak_temp int
) PARTITION BY RANGE (log_date);
```

## Connection pooling

PostgreSQL fork process mỗi connection — quá nhiều connection tốn RAM. Dùng **PgBouncer** (hoặc pooling ở app) cho production.

## Tham số hay gặp

`shared_buffers`, `work_mem`, `maintenance_work_mem`, `effective_cache_size` — chỉnh theo RAM và workload (đọc doc tuning).

## Bài tập gợi ý

1. Chạy `pg_dump` một database thử và restore sang tên mới.
2. Xem `pg_stat_user_tables` (dead tuples, last autovacuum).

## Tài liệu chính thức

- [Routine vacuuming](https://www.postgresql.org/docs/current/routine-vacuuming.html)
- [Backup and restore](https://www.postgresql.org/docs/current/backup.html)
- [Partitioning](https://www.postgresql.org/docs/current/ddl-partitioning.html)

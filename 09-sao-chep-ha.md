# 9. Sao chép & high availability (tổng quan)

## Mục tiêu

Nắm khái niệm primary/standby và khi nào dùng logical replication — chi tiết triển khai từng môi trường khác nhau.

## Streaming replication

- **Primary** ghi WAL; **standby** nhận và áp dụng (physical replica).
- **Synchronous**: chờ standby xác nhận mới commit (an toàn hơn, latency cao hơn).
- **Asynchronous**: commit nhanh hơn; có cửa sổ mất dữ liệu nếu primary chết đột ngột.

## Logical replication

Publication / subscription ở mức logical changes — hữu ích cho upgrade nối tiếp, tích hợp, một phần bảng.

## Failover & orchestration

Chuyển standby thành primary thường cần quy trình/công cụ (Patroni, repmgr, v.v.) — không chi tiết trong file này; khi học tới đây nên đọc doc + lab riêng.

## Bài tập gợi ý

1. Đọc [High availability](https://www.postgresql.org/docs/current/high-availability.html) mục lục và ghi lại 5 thuật ngữ chưa rõ.
2. (Lab) Hai máy/ container: cấu hình streaming replication tối giản theo tutorial chính thức.

## Tài liệu chính thức

- [High availability, load balancing, replication](https://www.postgresql.org/docs/current/high-availability.html)
- [Logical replication](https://www.postgresql.org/docs/current/logical-replication.html)

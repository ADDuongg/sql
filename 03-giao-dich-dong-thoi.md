# 3. Giao dịch & đồng thời

## Mục tiêu

Hiểu transaction, mức cô lập, và ý tưởng MVCC — nền tảng để đọc tài liệu về VACUUM và locking sau này.

## Transaction & ACID

- **A**tomicity: hoặc toàn bộ commit hoặc toàn bộ rollback.
- **C**onsistency: ràng buộc dữ liệu vẫn đúng sau giao dịch.
- **I**solation: các giao dịch đồng thời không “lẫn” kết quả trung gian không mong muốn (tùy mức cô lập).
- **D**urability: sau commit, dữ liệu được ghi bền (WAL).

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;  -- hoặc ROLLBACK;
```

## Mức cô lập (isolation)

PostgreSQL mặc định thường là **READ COMMITTED**. Có thể set:

```sql
BEGIN ISOLATION LEVEL REPEATABLE READ;
-- hoặc SERIALIZABLE
```

Ý nghĩa ngắn gọn:

- **READ COMMITTED**: mỗi câu lệnh thấy snapshot đã commit tại thời điểm bắt đầu câu lệnh.
- **REPEATABLE READ**: snapshot ổn định suốt transaction (đọc lặp lại cho cùng kết quả logic).
- **SERIALIZABLE**: mô phỏng thực thi tuần tự; có thể báo lỗi serialization, cần retry.

## MVCC (Multi-Version Concurrency Control)

- Mỗi thay đổi tạo **phiên bản** hàng; reader không chặn writer và ngược lại như lock hạng nặng.
- Hệ quả: có **dead tuple** (bản cũ) cần dọn — liên quan **VACUUM** (file `08-van-hanh-hieu-nang.md`).

## Locking (khái niệm)

- Khóa hàng / bảng có thể xảy ra khi `UPDATE`/`DELETE` hoặc `SELECT ... FOR UPDATE`.
- Deadlock: hai transaction chờ lock lẫn nhau — PostgreSQL hủy một transaction.

## Bài tập gợi ý

1. Mở hai session: một `BEGIN` + `UPDATE` chưa commit; session kia đọc/ cập nhật cùng hàng — quan sát hành vi.
2. Thử `SERIALIZABLE` với hai transaction cập nhật cùng tài nguyên logic.

## Tài liệu chính thức

- [Transaction isolation](https://www.postgresql.org/docs/current/transaction-iso.html)
- [MVCC](https://www.postgresql.org/docs/current/mvcc.html)

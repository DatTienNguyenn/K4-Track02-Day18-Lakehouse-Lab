# Reflection: Anti-Pattern Lakehouse

**Anti-pattern:** Bỏ quên bảo trì định kỳ (Unmanaged Maintenance) & Vấn nạn Small Files.

**Lý do dễ gặp:**
Trong hệ thống AI logging và streaming, dữ liệu liên tục ghi qua các micro-batch vài KB khiến bảng tích tụ hàng nghìn file nhỏ và commit log phình to. Điều này làm bùng nổ chi phí S3 requests, giảm tốc độ query do engine phải mở nhiều file và replay hàng trăm JSON commits khi cold-start. Đồng thời, snapshot cũ và file rác do writer crash âm thầm tiêu tốn chi phí lưu trữ.

**Cách phòng tránh:**
1. **Compaction & Z-Order:** Tự động gộp file nhỏ về kích thước chuẩn (128–512 MB) và gom cụm theo cột lọc.
2. **Vacuum & Expiry:** Lập lịch dọn snapshot cũ và xóa file tombstone quá hạn retention.
3. **Orphan Cleanup & Checkpoint:** Quét dọn file mồ côi ngoài log và định kỳ ghi checkpoint rút ngắn thời gian nạp metadata.

**Phạm vi sử dụng AI:** AI hỗ trợ phân tích, giải thích các luồng hoạt động trong các notebook; mã nguồn và số liệu kiểm thử do học viên tự thực thi.

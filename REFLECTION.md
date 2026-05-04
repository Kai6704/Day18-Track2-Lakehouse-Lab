# Reflection: Anti-pattern

Anti-pattern trong slide §5 mà team tôi dễ mắc phải nhất là **"Small-file problem" (Vấn đề file nhỏ)**.

**Lý do:** Hệ thống hiện tại của chúng tôi nhận luồng dữ liệu (streaming) liên tục với dung lượng nhỏ mỗi lần (micro-batches). Nếu ghi trực tiếp dữ liệu thô này xuống Data Lake mà không có cơ chế quản lý, hệ thống sẽ sinh ra hàng ngàn file vật lý rất nhỏ. Điều này sẽ làm giảm nghiêm trọng hiệu suất truy vấn ở các lớp trên do engine phải tốn rất nhiều thời gian mở file và đọc metadata (giống như những gì đã quan sát ở Notebook 2). Để khắc phục, chúng tôi cần thiết lập các job chạy `OPTIMIZE` định kỳ để gom nhóm (compact) dữ liệu.

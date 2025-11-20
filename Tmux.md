## Cách sử dụng tmux
Từ bây giờ, mọi người hãy luôn khởi động tmux trước khi làm việc trên server, để tránh mất tiến trình khi mất kết nối SSH hoặc tắt máy.
Mỗi thành viên sẽ có một phiên làm việc (session) riêng đã được tạo sẵn.

### Tạo phiên làm việc mới
Tại cửa sổ terminal trên máy tính cá nhân, chạy lệnh:
```bash 
    tmux new -s <tên phiên> 
```
Khi tạo phiên, bạn đang làm việc trực tiếp trên server, không phụ thuộc vào máy tính cá nhân.
Nếu bạn vô tình đóng terminal hoặc tắt máy, phiên tmux vẫn sẽ tiếp tục chạy bình thường trên server.

### Thoát (detach) khỏi phiên làm việc
Nhấn tổ hợp phím:
```bash 
    Ctrl + b, d
```
Lệnh này giúp bạn rời khỏi phiên tmux mà không dừng chương trình đang chạy.
Ví dụ: bạn có thể đang train model hoặc chạy script dài mà vẫn thoát ra an toàn.
### Quay lại (attach) vào phiên làm việc
Khi muốn vào lại phiên của mình:
```bash
    tmux attach -t <tên phiên> 
```
Lệnh này cho phép bạn tiếp tục công việc đúng nơi đã dừng lại trước đó.

### Xem các phiên làm việc đang chạy 
```bash 
    tmux -ls 
```
Dùng lệnh này để kiểm tra danh sách tất cả phiên tmux đang hoạt động trên server.
Bạn có thể xem tên phiên để xác định phiên của mình.

### Tắt (kill) một phiên làm việc
Khi bạn đã hoàn thành công việc và không cần giữ phiên nữa, có thể xóa (kill) phiên để giải phóng tài nguyên trên server.
Nếu bạn đang ở bên trong phiên tmux, chỉ cần gõ:
```bash
    exit
```
Nếu bạn đang ở ngoài tmux (chưa attach vào), dùng lệnh
```bash 
    tmux kill-session -t <tên_phiên>
```
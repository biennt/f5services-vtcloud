# Hướng dẫn nhanh

## Tạo Cloud Server
1. Từ trang chủ Viettel Cloud, chọn **Dịch vụ** >> **Compute** >> **Server**.
2. Tại màn hình danh sách server, chọn vào `Tạo server`.
3. Chọn thông tin chung bao gồm: `Region`, `Zone`, nhập tên server.
4. Chọn các loại máy chủ: Basic, Premium, Enterprise và chọn `Flavor` (cấu hình cho máy chủ) phù hợp.
5. Tại mục **Image** chọn `Hệ điều hành` và phiên bản của hệ điều hành.
6. Tùy chọn thêm cấu hình ổ cứng.
7. Tại mục `Authentication` chọn `Keypair` cho server. Nếu chọn hệ điều hành là `Windows` thì không cần chọn `Keypair`.
8. Tại mục **Cấu hình mạng** cho phép cấu hình mạng cho server. Bạn cần chọn ít nhất 1 loại IP.
- Nếu tích chọn `Enable Elastic IP` mà không chọn Elastic IP cụ thể, hệ thống sẽ tự cấp phát 1 Elastic IP.
- Nếu chọn `Enable Private IP` bạn cần chọn `VPC` và `Subnet` mong muốn. Khi không chọn `Interface` cụ thể, hệ thống tự động cấp phát cho server 1 interface thuộc subnet mà bạn đã chọn.
9. Tạo mục **Thông tin khác** điền thông tin của `Placement group`, `Security Group` và `Userdata`.
- Bạn có thể chọn 1 `Placement Group` cho server.
- Server cần phải thuộc ít nhất 1 `Security Group`.
- Userdata được sử dụng để viết kịch bản cho server ngay sau khi server được khởi tạo và sử dụng định dạng **cloud-init**.
10. Chọn **Xem trước** để kiểm tra lại thông tin trước khi tạo.
11. Sau khi tạo trạng thái server là `pending` và có thể mất vài phút để server chuyển sang trạng thái `running`.

## Kết nối tới Cloud Server
1. Khi trạng thái server là `running` tại icon (...), chọn **Console** để kết nối tới server.
2. Sau khi server hiện màn hình đăng nhập bạn có thể sử dụng `keypair` qua giao thức SSH để truy cập vào server.

> Nếu không thể truy cập vào server, cần kiểm tra kết nối từ mạng của bạn đến server và kiểm tra lại `Security Group`, đảm bảo port 22 đã được mở.

- Ngoài cách sử dụng `keypair` và giao thức SSH, bạn có thể đăng nhập vào server với tên user là `root` và mật khẩu. Nếu là  lần đầu đăng nhập, thực hiện quay lại màn hình Danh sách server, tại icon (...), chọn `Đổi mật khẩu` và thực hiện đăng nhập.

**Lưu ý**: Thực hiện đổi lại mật khẩu ngay sau khi đăng nhập lần đầu.
## Xóa Cloud Server
Khi thực hiện xóa Cloud Server, các tài nguyên liên quan tới Cloud Server có thể bị xóa: bao gồm ổ cứng Root, ổ cứng gắn ngoài, các lịch backup...
1. Từ trang chủ Viettel Cloud, chọn **Dịch vụ** >> **Compute** >> **Server**.
2. Tại màn hình danh sách server, chọn server muốn xóa.
3. Chọn các tài nguyên liên quan muốn xóa và xác nhận **Xóa**.


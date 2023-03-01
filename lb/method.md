# CÁC PHƯƠNG THỨC CÂN BẰNG TẢI

## Giới thiệu tổng quan

Để đạt được tối ưu hiệu quả, việc lựa chọn phương thức cân bằng tải phù hợp lựa chọn vào nhiều yếu tố như ứng dụng cần chạy, khả năng xử lý của từng máy chủ hay hệ thống mạng của chúng.

Có 4 phương thức cân bằng tải phổ biến, theo mặc định, BIG-IP sẽ sử dụng phương thức **Round Robin**.

- **Round Robin:** Hệ thống lần lượt chuyển từng kết nối đến từng máy chủ theo hàng nhằm đảm bảo phân phối kết nối đồng đều cho tất cả các máy trong dãy máy được cân bằng tải
_<br>Round Robin thường được sử dụng khi các thiết bị cần cân bằng tải có có khả năng xử lý tương đương nhau._

- **Ratio:** Số lượng kết nối cho mỗi máy theo thời gian phân phối theo tính theo tỷ lệ được cấu hình trước cho từng máy.
_<br>Ratio thường được sử dụng khi khả năng xử lý của các máy chủ trong nhóm khác nhau và bạn muốn chỉ định trọng số tỷ lệ thuận với tài nguyên và khả năng của từng máy._

- **Least Connections:** Thuật toán cân bằng tải động phân phối kết nối tới máy chủ hiện đang duy trì ít kết nối nhất
_<br>Least Connections thường được sử dụng khi các thiết bị cần cân bằng tải có khả năng xử lý tương đương nhau._

- **Fastest:** Thuật toán cân bằng tải động hoạt động theo cơ chế đếm số lượng request chưa được xử lý (chưa có phản hồi) và phân phối tới máy chủ hiện còn ít số lượng request chưa xử lý nhất - máy chủ có khả năng xử lý nhanh nhất.
_<br>Fastest được sử dụng nhằm tối ưu thời gian nhận được phản hồi cho người dùng, đặc biệt hữu ích khi các máy cần cân bằng tải được đặt tại các mạng logic khác nhau._

## Hướng dẫn cấu hình
### Phương thức Round Robin

Để cấu hình phương thức cân bằng tải
1. Đăng nhập vào giao diện cấu hình
2. Lựa chọn `Local Traffic` > `Pools` > `Pool lists`
3. Chọn pool cần cấu hình trong danh sách pool
4. Lựa chọn tab `Members`
5. Tại mục **Load Balancing Method** lựa chọn phương thức `Round Robin`

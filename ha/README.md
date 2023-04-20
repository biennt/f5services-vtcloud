# Cấu hình sẵn sàng cao (HA) cho F5 BIG-IP trên nền tảng Viettel Cloud

## Giới thiệu tổng quan

Mô hình minh hoạ khi thiết lập cặp máy ảo F5 BIG-IP tương tự như sau:

![](./topo.png "")

Về mặt giao diện mạng, dù không bắt buộc nhưng khuyến nghị có tối thiểu 3 giao diện (và tương ứng với nó trong ví dụ này là VLAN):

-  public_vlan (interface 1.0): để đón nhận lưu lượng mạng của người dùng cũng như các giao thức quản trị ban đầu (HTTPS, SSH). Nó cũng là VLAN chứa địa chỉ Virtual IP - VIP để client kết nối đến trong chế độ HA
- private_vlan (interface 1.1): để kết nối với server backend bên trong hệ thống
- sync_vlan (interface 1.2): là một giao diện dành riêng cho việc đồng bộ cấu hình, trao đổi heart-beat để failover cũng như đồng bộ phiên kết nối của client nếu cấu hình

## Hướng dẫn cấu hình

Trước hết, cần cấu hình một số chính sách bảo mật mạng cơ bản như dưới đây để cặp máy ảo F5 BIG-IP có thể đồng bộ được cấu hình, trao đổi được thông tin heart-beat và failover cho nhau.

Trong Security Group được áp dụng cho 2 máy ảo F5 BIG-IP, cấu hình các luật sau:

![](./security-group.png "")

Trên cả 2 máy ảo F5 BIG-IP, kiểm tra lại việc đồng bộ thời gian với máy chủ NTP, có thể tham khảo server public như minh hoạ dưới đây:

![](./ntp.png "")

Rà soát lại danh sách VLAN so với mô hình trong phần giới thiệu bên trên, tương tự như sau:

![](./vlan-list.png "")

Trong môi trường Viettel Cloud, cần thay đổi giá trị MTU của sync_vlan và private_vlan về 1450:

![](./mtu.png "")

Trong phần cấu hình self-ip cho phần sync_ip và private_ip, đặt chế độ Port Lockdown là `Allow All`

![](./port-lockdown.png "")

Trong phần `Device Management` > `Devices`, chọn device đang thao tác (Self):

Kiểm tra Device Name, nếu chưa đúng thì đổi lại bằng cách bấm vào nút `Change Device Name`

![](./device-name.png "")

Trong tab `ConfigSync`, chọn địa chỉ IP thuộc sync_vlan:

![](./config-sync.png "")

Trong tab `Failover Network`, chọn 2 địa chỉ IP thuộc 2 vlan tương ứng là private_vlan và sync_vlan cho cơ chế *Failover Unicast Configuration*

![](./failover-network.png "")

Trong tab `Mirroring`, có thể config địa chỉ IP thuộc sync_vlan để trao đổi thông tin đồng bộ phiên. Phần này là tuỳ chọn, không bắt buộc.

![](./connection-mirroring.png "")

Trên một máy ảo F5 BIG-IP, giả sử chọn bigip1, thêm thiết bị bigip2 vào bằng cách truy cập vào phần `Device Management` > `Device Trust : Device Trust Members`, sau đó click vào nút `Add`.

Nhập vào địa chỉ IP và tài khoản quản trị của máy ảo thứ 2

![](./add-device-trust.png "")

Kiểm tra thông tin certificate của máy ảo đó đã đúng chưa. Nếu đúng thì click vào nút `Device Certificate Matches`

![](./add-device-trust-confirm.png "")

Cuối cùng, click vào nút `Add Device`

![](./add-device.png "")

Kiểm tra lại danh sách các device xem các thông tin về hostname, version, độ lệch thời gian.. Việc này nên kiểm tra trên tất cả các máy ảo F5 BIG-IP

![](./device-list.png "")

Vào `Device Management` > `Device Groups` để tạo Device Group với các thông tin tương tự như hình minh hoạ dưới đây:

![](./create-device-group.png "")

Khởi tạo việc đồng bộ cấu hình lần đầu bằng cách vào `Device Management` > `Overview`: chọn Device Group vừa tạo, đồng bộ cấu hình của máy ảo đang thao tác vào group (*Push the selected device configuration to the group*)

Bấm vào nút `Sync`.

![](./first-sync.png "")

Kiểm tra tình trạng đồng bộ và chế độ sẵn sàng của các máy ảo F5 BIG-IP. Ví dụ trường hợp điển hình có 1 traffic-group thì sẽ có 1 máy ảo active, một máy ảo standby

Máy ảo Active sẽ có thông tin tương tự như dưới đây ở phần góc trên bên trái:

![](./active-sync.png "")

Và đây là thông tin về máy ảo Standby:

![](./standby-sync.png "")

Tạo một virtual server với địa chỉ Virtual IP được cung cấp để thử hoạt động:

![](./test-vs.png "")

Phần Virtual Addresses, chúng ta sẽ thấy là địa chỉ này thuộc về **traffic-group-1**, nghĩa là nó là địa chỉ floating, trôi nổi giữa 2 máy ảo tuỳ thuộc vào máy nào đang active
![](./test-virtual-address.png "")

## Liên hệ hỗ trợ
Yêu cầu hỗ trợ kỹ thuật xin gửi đến địa chỉ: techsupport@viettelcloud.vn

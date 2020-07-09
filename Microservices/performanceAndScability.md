# Hiệu năng và khả năng mở rộng 
Nếu bạn gặp vấn đề về hiệu năng, hệ thống của bạn sẽ chậm đối với một người dùng.
Nếu bạn gặp vấn đề về khả năng mở rộng, hệ thống của bạn sẽ nhanh cho một người dùng nhưng chậm khi tải nặng.

# Độ trễ so với thông lượng
Độ trễ là thời gian để thực hiện một số hành động hoặc tạo ra một số kết quả.
Thông lượng là số lượng hành động hoặc kết quả như vậy trên mỗi đơn vị thời gian. (Khác với băng thông là số lượng hành động hoặc kết quả như vậy trên mỗi đơn vị thời gian trên lý thuyết)

# Tính khả dụng và tính nhất quán
Trong hệ thống máy tính phân tán, bạn chỉ có thể hỗ trợ hai trong số các đảm bảo sau:

![Khả dụng và tính nhất quán](images/image.png)

* **Tính nhất quán** - Mỗi lần đọc đều nhận được "ghi gần đây nhất" hoặc một lỗi. 
* **Tính khả dụng** - Mọi yêu cầu đều nhận được phản hồi mà "không đảm bảo rằng nó chứa phiên bản mới nhất" của thông tin
* **Dung sai phân vùng** - Hệ thống tiếp tục hoạt động mặc dù phân vùng tùy ý do lỗi mạng

Chú ý: Mạng không đáng tin cậy, vì vậy bạn sẽ cần hỗ trợ dung sai phân vùng. Bạn sẽ cần phải đánh đổi phần mềm giữa tính nhất quán và tính sẵn có.

## Tính nhất quán và dung sai phân vùng
Chờ phản hồi từ nút được phân vùng có thể dẫn đến lỗi hết thời gian. Tính nhất quán và dung sai phân vùnglà một lựa chọn tốt nếu nhu cầu kinh doanh của bạn yêu cầu "tự động đọc và viết".

## Tính khả dụng và dung sai phân vùng
Phản hồi trả về phiên bản có sẵn nhất của dữ liệu có sẵn trên bất kỳ nút nào, có thể không phải là phiên bản mới nhất. Người viết có thể mất một thời gian để tuyên truyền khi phân vùng được giải quyết.

Tính khả dụng và dung sai phân vùng là một lựa chọn tốt nếu nhu cầu kinh doanh cho phép tính nhất quán cuối cùng hoặc khi hệ thống cần tiếp tục hoạt động mặc dù có lỗi bên ngoài. 

# Các mẫu nhất quán

Với nhiều bản sao của cùng một dữ liệu, chúng tôi phải đối mặt với các tùy chọn về cách đồng bộ hóa chúng để khách hàng có một cái nhìn nhất quán về dữ liệu. Nhắc lại định nghĩa về tính nhất quán từ [Định lý CAP] (định lý # cap) - Mỗi lần đọc đều nhận được ghi hoặc lỗi gần đây nhất.

## Tính nhất quán yếu

Sau khi viết, đọc có thể hoặc không thể nhìn thấy nó. Một cách tiếp cận nỗ lực tốt nhất được thực hiện.

Cách tiếp cận này được nhìn thấy trong các hệ thống như memcached. Tính nhất quán yếu hoạt động tốt trong các trường hợp sử dụng thời gian thực như VoIP, trò chuyện video và trò chơi nhiều người chơi trong thời gian thực. Ví dụ: nếu bạn đang thực hiện một cuộc gọi điện thoại và mất liên lạc trong vài giây, khi bạn lấy lại kết nối, bạn không nghe thấy những gì được nói trong khi mất kết nối.

## Tính nhất quán cuối cùng

Sau khi viết, cuối cùng các lần đọc sẽ thấy nó (thường trong vòng một phần nghìn giây). Dữ liệu được nhân rộng không đồng bộ.

Cách tiếp cận này được nhìn thấy trong các hệ thống như DNS và email. Tính nhất quán cuối cùng hoạt động tốt trong các hệ thống có sẵn cao.

## Tính nhất quán mạnh mẽ

Sau khi viết, đọc sẽ thấy nó. Dữ liệu được nhân rộng đồng bộ.

Cách tiếp cận này được nhìn thấy trong các hệ thống tệp và RDBMSes. Tính nhất quán mạnh mẽ hoạt động tốt trong các hệ thống cần giao dịch.

# Các mẫu khả dụng 

Có hai mẫu bổ sung để hỗ trợ tính sẵn sàng cao: ** fail-over ** và ** sao chép **.

## Fail-over
### Thụ động tích cực
Với thất bại thụ động chủ động, nhịp tim được gửi giữa máy chủ hoạt động và máy chủ thụ động ở chế độ chờ. Nếu nhịp tim bị gián đoạn, máy chủ thụ động sẽ chiếm địa chỉ IP của hoạt động và tiếp tục dịch vụ.

Thời gian ngừng hoạt động được xác định bởi liệu máy chủ thụ động đã chạy ở chế độ chờ 'nóng' hay liệu nó có cần khởi động từ chế độ chờ 'lạnh' hay không. Chỉ có máy chủ hoạt động xử lý lưu lượng.

Chuyển đổi dự phòng thụ động chủ động cũng có thể được gọi là chuyển đổi dự phòng chủ-nô lệ.

### Chủ động tích cực
Trong active-active, cả hai máy chủ đều quản lý lưu lượng, phân tán tải giữa chúng.

Nếu các máy chủ đối diện công khai, DNS sẽ cần biết về IP công khai của cả hai máy chủ. Nếu các máy chủ đối diện nội bộ, logic ứng dụng sẽ cần biết về cả hai máy chủ.

Chuyển đổi dự phòng hoạt động tích cực cũng có thể được gọi là chuyển đổi dự phòng chính chủ.

## Nhược điểm: chuyển đổi dự phòng
* Fail-over thêm phần cứng và độ phức tạp bổ sung.
* Có khả dụng mất dữ liệu nếu hệ thống hoạt động bị lỗi trước khi bất kỳ dữ liệu mới được ghi nào có thể được sao chép sang thụ động.

# Nhân rộng

## Chủ nô lệ và chủ-chủ

Chủ đề này được thảo luận thêm trong phần [Cơ sở dữ liệu] (# cơ sở dữ liệu):

* [Sao chép chính-nô lệ] (# sao chép chính-nô lệ)
* [Sao chép chính chủ] (# nhân bản chính chủ)

## Tính khả dụng của số

Tính khả dụng thường được định lượng theo thời gian hoạt động (hoặc thời gian chết) theo phần trăm thời gian dịch vụ có sẵn. Tính khả dụng thường được đo bằng số lượng 9 giây - một dịch vụ có 99,99% khả dụng được mô tả là có bốn số 9.

## 99,9% khả dụng - ba 9 giây

| Thời lượng | Thời gian chết chấp nhận được |
| --------------------- | -------------------- |
| Thời gian chết mỗi năm | 8h 45 phút 57s |
| Thời gian chết mỗi tháng | 43m 49,7s |
| Thời gian chết mỗi tuần | 10m 4,8s |
| Thời gian chết mỗi ngày | 1m 26,4s |

### 99,99% khả dụng - bốn 9 giây

| Thời lượng | Thời gian chết chấp nhận được |
| --------------------- | -------------------- |
| Thời gian chết mỗi năm | 52 phút 35,7 giây |
| Thời gian chết mỗi tháng | 4m 23s |
| Thời gian chết mỗi tuần | 1m 5s |
| Thời gian chết mỗi ngày | 8,6s |

### Tính khả dụng song song so với trình tự

Nếu một dịch vụ bao gồm nhiều thành phần dễ bị lỗi, tính khả dụng chung của dịch vụ phụ thuộc vào việc các thành phần đó theo trình tự hay song song.

##### Tuần tự

Tính khả dụng chung giảm khi hai thành phần có sẵn <100% theo thứ tự:

`` `
Sẵn có (Tổng cộng) = Sẵn có (Foo) * Sẵn có (Thanh)
`` `

Nếu cả `Foo` và` Bar` đều có sẵn 99,9%, tổng số khả dụng của chúng theo thứ tự sẽ là 99,8%.

##### Song song

Tính khả dụng chung tăng khi hai thành phần có sẵn <100% song song:

`` `
Sẵn có (Tổng cộng) = 1 - (1 - Sẵn có (Foo)) * (1 - Sẵn có (Bar))
`` `

Nếu cả `Foo` và` Bar` đều có sẵn 99,9%, thì tổng số khả dụng của chúng song song sẽ là 99.9999%.

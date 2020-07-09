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
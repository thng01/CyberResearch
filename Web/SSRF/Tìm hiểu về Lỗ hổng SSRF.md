# 				Tìm hiểu về Lỗ hổng SSRF 

**SSRF (Server-Side Request Forgery)** là một loại lỗ hổng bảo mật phổ biến trong các ứng dụng web, cho phép kẻ tấn công tạo ra các yêu cầu từ phía máy chủ và có thể tương tác với các tài nguyên mạng nội bộ hoặc bên ngoài mà không cần sự cho phép của máy chủ. Lỗ hổng SSRF thường xảy ra khi ứng dụng web không kiểm tra hoặc kiểm soát chính xác đối tượng của các yêu cầu mạng mà nó tạo ra.

## Cách SSRF Hoạt Động

Lỗ hổng SSRF thường xuất hiện khi một ứng dụng web cho phép người dùng kiểm soát một phần hoặc toàn bộ URL mà nó sẽ gửi yêu cầu. Khi người dùng nhập một URL, ứng dụng web sẽ sử dụng giá trị này để tạo yêu cầu đến máy chủ mà URL chỉ định. Kẻ tấn công có thể tận dụng điều này bằng cách cung cấp một URL đặc biệt chứa các thông tin bí mật hoặc máy chủ nội bộ.

Dưới đây là một ví dụ về cách một lỗ hổng SSRF có thể được khai thác:

1. Kẻ tấn công truy cập một ứng dụng web có lỗ hổng SSRF.

2. Kẻ tấn công nhập một URL được tạo để gửi yêu cầu đến máy chủ nội bộ (ví dụ: `http://internal-server/private-data`).

3. Ứng dụng web sẽ sử dụng URL được cung cấp để tạo yêu cầu và trả về kết quả cho người dùng.

4. Khi ứng dụng web gửi yêu cầu đến `internal-server`, nó có thể nhận được truy cập vào các tài nguyên nội bộ mà kẻ tấn công muốn xâm nhập.

   ![img](https://images.viblo.asia/b0ae85ac-13ea-465d-a4a6-9e825c4c3fbd.jpg)

   ​        1. [Điển hình của một cuộc tấn công yêu cầu giả mạo từ phía máy chủ](https://viblo.asia/p/ssrf-la-gi-cach-phat-hien-va-ngan-chan-tan-cong-yeu-cau-gia-mao-tu-phia-may-chu-Eb85op08K2G)

## Tác Hại của SSRF

Lỗ hổng SSRF có thể gây ra nhiều vấn đề bảo mật nghiêm trọng, bao gồm:

1. **Truy cập vào Tài nguyên Nội bộ**: Kẻ tấn công có thể truy cập và thu thập thông tin từ các máy chủ nội bộ, cơ sở dữ liệu, hoặc các tài nguyên khác mà không được phép.
2. **Tấn Công vào Hệ thống Nội bộ**: Kẻ tấn công có thể sử dụng SSRF để tấn công các hệ thống nội bộ, thậm chí có thể khai thác các lỗ hổng khác trên mạng nội bộ.
3. **Phương tiện để Bypass Firewall và Filter**: SSRF có thể được sử dụng để bypass các tường lửa (firewall) và bộ lọc (filter) bằng cách sử dụng máy chủ web như một proxy để tạo yêu cầu đến các máy chủ khác.
4. Truy xuất thông tin nhạy cảm như địa chỉ IP của máy chủ web sau proxy ngược.

## Các kiểu tấn công

### 	1. SSRF tấn công chính máy chủ

- SSRF có thể giả mạo yêu cầu để truy xuất đến các miền khác nhau trong nội bộ hệ thống và nó bao gồm luôn cả chính máy chủ của nó, chính vì vậy cuộc tấn công SSRF có thể nhắm vào chính máy chủ và khai thác dữ liệu.
- Ví dụ nha, một website có chức năng kiểm tra xem liệu trong kho còn bao nhiêu sản phẩm
  `stockApi=http://stock.alexshop.com:8080/product/stock/check%3FproductId%3D6%26storeId%3D1`
- Trong trường hợp này ta có thể sửa đổi thành `stockApi=http://localhost:8080/admin`. Tại đây máy chủ sẽ trả về nội dung của thư mục admin

### 	2. SSRF tấn công các hệ thống back-end khác

Sau khi tìm được địa chỉ của domain muốn tấn công hacker có khả năng khai thác các domain khác trong hệ thống



## Cách Ngăn Chặn SSRF

Để ngăn chặn lỗ hổng SSRF, các nhà phát triển và quản trị hệ thống có thể thực hiện các biện pháp sau:

1. **Kiểm tra và kiểm soát URL đầu vào**: Ứng dụng web nên kiểm tra và hạn chế các URL đầu vào từ người dùng để đảm bảo rằng chúng chỉ có thể gửi yêu cầu đến các tài nguyên được phép.
2. **Sử dụng White-lists**: Thay vì cho phép bất kỳ URL nào, ứng dụng nên sử dụng danh sách trắng (white-list) các tài nguyên mà nó có thể truy cập.
3. **Tắt chức năng Proxy**: Nếu không cần thiết, tắt tính năng proxy trên máy chủ web để ngăn chặn việc sử dụng ứng dụng như một proxy.
4. **Cập nhật ứng dụng**: Đảm bảo rằng ứng dụng web và tất cả các phần mềm liên quan đều được cập nhật để vá các lỗ hổng SSRF đã biết.
5. **Kiểm tra bảo mật**: Thực hiện kiểm tra bảo mật định kỳ để phát hiện và sửa lỗi SSRF và các lỗ hổng bảo mật khác.


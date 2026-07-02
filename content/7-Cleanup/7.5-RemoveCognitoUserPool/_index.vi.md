+++
title = "Xóa Cognito User Pool"
date = 2025
weight = 5
chapter = false
pre = "<b>7.5 </b>"
+++

#### Tổng quan

Trong bước này, bạn sẽ xóa **Amazon Cognito User Pool** đã được tạo cho hệ thống **Serverless AI Invoice Scanner**.

Amazon Cognito được sử dụng để xác thực người dùng cho ứng dụng React frontend. Sau khi hoàn thành bài lab, bạn nên xóa Cognito User Pool để tránh giữ lại các tài nguyên xác thực không còn sử dụng trong tài khoản AWS.

{{% notice warning %}}
Việc xóa Cognito User Pool sẽ xóa vĩnh viễn toàn bộ người dùng, app client, cấu hình đăng nhập, cấu hình xác thực và các thiết lập liên quan trong user pool đó. Hãy đảm bảo bạn không còn cần các tài khoản người dùng trước khi xóa.
{{% /notice %}}

---

#### Tài nguyên cần xóa

Trong dự án này, tài nguyên Cognito cần xóa là:

```txt
invoice-scanner-user-pool
```

User Pool này được frontend sử dụng thông qua các biến môi trường sau:

```env
REACT_APP_USER_POOL_ID=ap-southeast-1_xxxxxxxxx
REACT_APP_USER_POOL_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
```

Sau khi User Pool bị xóa, chức năng đăng nhập và đăng ký tài khoản trên frontend sẽ không còn hoạt động.

---

#### Các bước thực hiện

- Mở **AWS Management Console**.

- Tìm kiếm và truy cập dịch vụ **Amazon Cognito**.

![Remove Cognito User Pool](/images/7/7.5/Screenshot_1.png)

- Trong thanh điều hướng bên trái, chọn **User pools**.

![Remove Cognito User Pool](/images/7/7.5/Screenshot_2.png)

- Trong danh sách User Pool, chọn User Pool được sử dụng trong dự án:

```txt
{User pool name}
```

![Remove Cognito User Pool](/images/7/7.5/Screenshot_3.png)

- Sau khi mở User Pool, chọn **Delete user pool**.

![Remove Cognito User Pool](/images/7/7.5/Screenshot_4.png)

- Hộp thoại xác nhận xóa sẽ xuất hiện.

- Nhập nội dung xác nhận theo yêu cầu của AWS Console. Tùy giao diện, bạn có thể cần nhập:

```txt
delete
```

hoặc nhập đúng tên User Pool:

```txt
invoice-scanner-user-pool
```

![Remove Cognito User Pool](/images/7/7.5/Screenshot_5.png)

- Chọn **Delete** để xác nhận xóa Cognito User Pool.

![Remove Cognito User Pool](/images/7/7.5/Screenshot_6.png)

---

#### Kiểm tra sau khi xóa

Sau khi xóa User Pool, thực hiện kiểm tra như sau:

- Quay lại danh sách **User pools**.
- Kiểm tra rằng `invoice-scanner-user-pool` không còn xuất hiện trong danh sách.
- Mở lại website frontend đã triển khai.
- Màn hình đăng nhập Cognito sẽ không còn hoạt động vì User Pool ID và App Client ID đã không còn hợp lệ.

---

#### Ảnh hưởng đến hệ thống

Sau khi Cognito User Pool bị xóa:

| Thành phần | Ảnh hưởng |
|---|---|
| React frontend | Người dùng không thể đăng nhập hoặc đăng ký tài khoản mới. |
| Cognito Authenticator | Màn hình đăng nhập sẽ bị lỗi vì User Pool không còn tồn tại. |
| App Client ID | App client bị xóa cùng với User Pool. |
| Người dùng đã đăng ký | Toàn bộ tài khoản người dùng trong User Pool bị xóa vĩnh viễn. |
| API Gateway | Không bị ảnh hưởng trực tiếp nếu API chưa cấu hình Cognito Authorizer. |
| Lambda Functions | Không bị ảnh hưởng trực tiếp. |
| DynamoDB | Dữ liệu hóa đơn vẫn còn nếu bảng DynamoDB chưa bị xóa. |

---

#### Lưu ý quan trọng

{{% notice info %}}
Nếu API Gateway của bạn có cấu hình Cognito Authorizer, hãy xóa hoặc gỡ liên kết authorizer trước khi xóa Cognito User Pool. Nếu không, các API route phụ thuộc vào authorizer có thể bị lỗi xác thực.
{{% /notice %}}

{{% notice warning %}}
Không nên xóa Cognito User Pool nếu bạn vẫn muốn tiếp tục kiểm thử chức năng đăng nhập của frontend. Sau khi xóa, User Pool và các tài khoản người dùng bên trong sẽ không thể khôi phục.
{{% /notice %}}

---

#### Kết luận

Bạn đã xóa thành công Amazon Cognito User Pool được sử dụng trong hệ thống **Serverless AI Invoice Scanner**.

- User Pool `invoice-scanner-user-pool` đã được xóa.
- App client được React frontend sử dụng cũng đã bị xóa.
- Chức năng đăng ký và đăng nhập người dùng không còn khả dụng.
- Cấu hình xác thực trong frontend không còn hợp lệ.
- Bước dọn dẹp này giúp loại bỏ tài nguyên xác thực không còn sử dụng trong tài khoản AWS.

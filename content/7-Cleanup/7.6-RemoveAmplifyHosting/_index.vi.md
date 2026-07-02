+++
title = "Xóa Amplify Hosting App"
date = 2025
weight = 6
chapter = false
pre = "<b>7.6 </b>"
+++

#### Tổng quan

Trong bước này, bạn sẽ xóa **AWS Amplify Hosting App** đã được tạo để triển khai giao diện frontend React của hệ thống **Serverless AI Invoice Scanner**.

AWS Amplify Hosting được sử dụng để build, deploy và host ứng dụng React frontend. Sau khi hoàn thành bài lab, bạn nên xóa Amplify Hosting App nếu không còn sử dụng website để tránh giữ lại tài nguyên triển khai không cần thiết.

{{% notice warning %}}
Sau khi xóa Amplify Hosting App, URL public của frontend sẽ không còn truy cập được. Người dùng sẽ không thể mở giao diện web đã deploy trên Amplify.
{{% /notice %}}

---

#### Tài nguyên cần xóa

Trong dự án này, tài nguyên cần xóa là ứng dụng frontend đã triển khai bằng **AWS Amplify Hosting**.

Ứng dụng này có thể có tên tương tự:

```txt
invoice-scanner-frontend
```

hoặc tên bạn đã đặt khi tạo app trên AWS Amplify.

Amplify Hosting App này được dùng để:

| Thành phần | Mô tả |
|---|---|
| Frontend React App | Giao diện quản lý hóa đơn. |
| Amplify Hosting Domain | URL public dạng `https://main.xxxxx.amplifyapp.com`. |
| Build Settings | Cấu hình build React app bằng `npm install` và `npm run build`. |
| Environment Variables | Các biến môi trường như API Gateway endpoint, Cognito User Pool ID và App Client ID. |
| GitHub Connection | Kết nối repository GitHub với Amplify để tự động deploy. |

---

#### Các bước thực hiện

- Mở **AWS Management Console**.

- Tìm kiếm và truy cập dịch vụ **AWS Amplify**.

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_1.png)

- Trong danh sách ứng dụng Amplify, chọn app frontend của dự án.

Ví dụ:

```txt
invoice-scanner-frontend
```

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_2.png)

- Sau khi mở app, chọn **App settings** ở thanh điều hướng bên trái.

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_3.png)

- Chọn mục **General settings**.

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_4.png)

- Kéo xuống cuối trang và tìm phần **Delete app**.

- Chọn **Delete app**.

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_5.png)

- Hộp thoại xác nhận xóa sẽ xuất hiện.

- Nhập tên app theo yêu cầu của AWS Console để xác nhận.

Ví dụ:

```txt
invoice-scanner-frontend
```

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_6.png)

- Chọn **Delete** để xóa Amplify Hosting App.

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_7.png)

---

#### Kiểm tra sau khi xóa

Sau khi xóa Amplify Hosting App:

- Quay lại danh sách ứng dụng trong **AWS Amplify**.
- Kiểm tra rằng app frontend của dự án không còn xuất hiện.
- Mở lại URL Amplify Hosting trước đó.

Ví dụ:

```txt
https://main.xxxxx.amplifyapp.com
```

- Website sẽ không còn truy cập được.
- Nếu app có kết nối GitHub, các lần push code mới lên repository sẽ không còn tự động deploy lên Amplify nữa.

---

#### Ảnh hưởng đến hệ thống

Sau khi xóa Amplify Hosting App, giao diện frontend đã deploy sẽ không còn hoạt động.

| Thành phần | Ảnh hưởng |
|---|---|
| React frontend | Website đã deploy không còn truy cập được. |
| Amplify domain | URL `amplifyapp.com` bị gỡ bỏ. |
| GitHub auto deploy | Không còn tự động build và deploy khi push code. |
| Environment variables | Các biến môi trường trong Amplify Hosting bị xóa cùng với app. |
| Amazon Cognito | Không bị xóa trực tiếp, nhưng callback URL trỏ đến Amplify sẽ không còn sử dụng được. |
| API Gateway | Không bị ảnh hưởng trực tiếp. |
| Lambda Functions | Không bị ảnh hưởng trực tiếp. |
| Amazon S3 | Không bị ảnh hưởng trực tiếp. |
| Amazon DynamoDB | Không bị ảnh hưởng trực tiếp. |

---

#### Lưu ý về Cognito Callback URL

Nếu bạn đã thêm URL Amplify Hosting vào Cognito, ví dụ:

```txt
https://main.xxxxx.amplifyapp.com/
```

trong:

```txt
Allowed callback URLs
Allowed sign-out URLs
```

thì sau khi xóa Amplify App, URL này không còn sử dụng được nữa.

Nếu bạn vẫn giữ Cognito User Pool để kiểm thử local, có thể giữ lại URL:

```txt
http://localhost:3000/
```

và xóa URL Amplify Hosting khỏi cấu hình Cognito.

{{% notice info %}}
Nếu Cognito User Pool cũng sẽ bị xóa ở bước cleanup, bạn không cần chỉnh lại callback URL nữa.
{{% /notice %}}

---

#### Lưu ý về GitHub Repository

Việc xóa Amplify Hosting App **không xóa repository GitHub** của bạn.

Sau khi xóa Amplify App:

- Source code trên GitHub vẫn còn.
- Repository vẫn có thể được dùng lại để deploy sau này.
- Nếu muốn xóa luôn source code, bạn cần xóa repository trực tiếp trong GitHub.

{{% notice info %}}
Amplify Hosting chỉ xóa ứng dụng triển khai trên AWS. Nó không tự động xóa source code trong GitHub.
{{% /notice %}}

---

#### Kết luận

Bạn đã xóa thành công AWS Amplify Hosting App được sử dụng trong hệ thống **Serverless AI Invoice Scanner**.

- Ứng dụng frontend React đã deploy trên Amplify Hosting đã được xóa.
- URL public của frontend không còn truy cập được.
- Cấu hình build, environment variables và kết nối GitHub trong Amplify Hosting đã bị xóa.
- Cognito, API Gateway, Lambda, S3 và DynamoDB không bị xóa tự động bởi bước này.
- Bước dọn dẹp này giúp loại bỏ tài nguyên hosting frontend không còn sử dụng trong tài khoản AWS.

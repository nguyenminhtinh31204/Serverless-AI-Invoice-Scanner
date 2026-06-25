---
title: "Đăng nhập IAM User"
weight: 3
chapter: false
pre: " <b> 2.3 </b> "
---

#### Mục tiêu

Trong bước này, bạn sẽ đăng nhập vào **IAM User** được cấp quyền để sử dụng AWS Console. Đây là bước bắt buộc trước khi triển khai bất kỳ tài nguyên nào.

---

#### Bước 1: Mở trình duyệt ở chế độ ẩn danh

-   Mở Google Chrome (hoặc trình duyệt bạn đang sử dụng) ở **chế độ ẩn danh**.
-   Cách nhanh nhất: Nhấn tổ hợp phím **Ctrl + Shift + N**.
-   Việc này giúp bạn tránh các xung đột phiên đăng nhập nếu đang sử dụng nhiều tài khoản AWS.

![IAM User](/images/2.environmentsetup/2.3-loginwithiamuser/001-loginwithiamuser.png)

---

#### Bước 2: Mở file Excel chứa thông tin IAM User

-   Mở file Excel đã được tải về từ người quản trị hệ thống.
-   Trong file này, bạn sẽ thấy các thông tin sau:

    -   **Account ID**
    -   **IAM User name**
    -   **Password**

![IAM User](/images/2.environmentsetup/2.3-loginwithiamuser/002-loginwithiamuser.png)

{{% notice info %}}
Thông tin này thường được cung cấp cho bạn qua email hoặc file đính kèm từ quản lý dự án.
{{% /notice %}}

---

#### Bước 3: Truy cập trang đăng nhập IAM

-   Truy cập đường dẫn đăng nhập IAM có dạng:

    ```
    https://<ACCOUNT_ID>.signin.aws.amazon.com/console
    ```

    Thay `<ACCOUNT_ID>` bằng ID tài khoản AWS đã được cung cấp (ví dụ: `123456789012`).

---

#### Bước 4: Nhập thông tin đăng nhập

-   **IAM user name**: Nhập đúng tên người dùng IAM từ file Excel.
-   **Password**: Nhập mật khẩu tương ứng.

![IAM User](/images/2.environmentsetup/2.3-loginwithiamuser/003-loginwithiamuser.png)

-   Sau đó nhấn nút **Sign in** để vào hệ thống.

![IAM User](/images/2.environmentsetup/2.3-loginwithiamuser/005-loginwithiamuser.png)

---

#### Bước 5: Chỉnh lại Region về N. Virginia

-   Sau khi đăng nhập thành công vào AWS Management Console:

    -   Nhìn lên **góc trên bên phải** màn hình.
    -   Nhấn vào tên Region hiện tại.
    -   Chọn lại **N. Virginia** (us-east-1).

![IAM User](/images/2.environmentsetup/2.3-loginwithiamuser/004-loginwithiamuser.png)

{{% notice info %}}
Toàn bộ tài nguyên của hệ thống sẽ được triển khai tại Region **us-east-1 (N. Virginia)**. Hãy đảm bảo bạn đang thao tác tại đúng khu vực này.
{{% /notice %}}

---

#### Kết luận

Bạn đã hoàn tất việc đăng nhập vào IAM User. Ở bước tiếp theo, bạn sẽ tiến hành tạo S3 Bucket tại Region **us-east-1**.

+++
title = "Xóa IAM User và Policy"
date = 2025
weight = 6
chapter = false
pre = "<b>7.6 </b>"
+++

{{% notice warning %}}
⚠️ **Lưu ý**: Hãy đăng nhập bằng **root user** để xóa dịch vụ IAM và Policy.
{{% /notice %}}

#### Các bước thực hiện

-   Mở **AWS Management Console** → Tìm kiếm dịch vụ **IAM**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_1.png)

-   Ở thanh điều hướng bên trái, chọn **Users**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_2.png)

-   Tìm và chọn user `ai-invoice-scanner-user` để vào trang thông tin chi tiết của user.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_3.png)

-   Cuộn xuống phần **Permissions policies**, chọn tất cả các policy đang gán cho user, sau đó nhấn nút **Remove**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_4.png)

-   Nhấn **Remove policies** để xác nhận gỡ các policy khỏi user.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_5.png)

-   Truy cập tab **Security credentials**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_6.png)

-   Cuộn xuống phần **Access keys**, nhấn **Actions** → **Delete** để xóa access key.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_7.png)

-   Sau khi xóa access key, cuộn lên đầu trang → Nhấn **Delete** để xóa IAM User.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_8.png)

-   Trong quá trình xóa, nhấn **Deactivate access key** nếu được yêu cầu.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_9.png)

-   Trong cửa sổ xác nhận, nhập `confirm` → Nhấn **Delete user**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_10.png)

-   Sau khi xóa user xong, ở thanh điều hướng bên trái, chọn **Policies**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_11.png)

-   Tìm policy `AIInvoiceScannerFullPolicy` → Nhấn **Delete**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_12.png)

-   Nhập lại tên policy để xác nhận → Nhấn **Delete**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_13.png)

-   Lặp lại thao tác trên để xóa policy `AmplifyAdminPolicy`.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_14.png)

-   Nhập lại tên policy để xác nhận → Nhấn **Delete**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_15.png)

{{% notice tip %}}
ℹ️ Để cẩn thận, bạn hãy kiểm tra các dịch vụ có liên quan đến dự án một lần nữa để tránh **phát sinh chi phí** sau này nhé!
{{% /notice %}}

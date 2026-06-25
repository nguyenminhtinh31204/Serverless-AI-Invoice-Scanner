---
title: "Tạo IAM User và cấp quyền"
weight: 2
chapter: false
pre: " <b> 2.2 </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ tạo một IAM User có quyền truy cập vào các dịch vụ AWS thông qua giao diện dòng lệnh (CLI) hoặc SDK. Người dùng này sẽ được gán 2 policy vừa tạo để có thể tương tác với backend và frontend của hệ thống Serverless Invoices Scanner.

---

#### Bước 1: Truy cập IAM Console

1. Truy cập [AWS Console](https://console.aws.amazon.com/), tìm **IAM**, sau đó chọn **IAM** trong kết quả.

![IAM User](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/001-createiamuser.png)

2. Ở menu bên trái, chọn **Users**.

![IAM User](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/002-createiamuser.png)

3. Nhấp vào nút **Create user** để bắt đầu tạo IAM User mới.

![IAM User](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/003-createiamuser.png)

---

#### Bước 2: Cấu hình IAM User

1. **User name**: `ai-invoice-scanner-user`
2. **Tích chọn ô**: Provide user access to the AWS Management Console.
3. **Chọn mục**: I want to create an IAM user.

![IAM User](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/004-createiamuser.png)

4. Trong phần **Console password**:

    - **Chọn mục**: Custom password.
    - **Đặt mật khẩu**: `Admin@123`
    - **Bỏ tick chọn**: Users must create a new password at next sign-in.

{{% notice info %}}
Việc bỏ chọn tùy chọn này giúp người dùng không bị yêu cầu thay đổi mật khẩu khi đăng nhập lần đầu.
{{% /notice %}}

5. Nhấn **Next** để sang bước gán quyền.

![IAM User](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/005-createiamuser.png)

> 💡 _Bạn có thể chọn mật khẩu khác theo chính sách bảo mật nội bộ._

---

#### Bước 3: Gán policy cho IAM User

1. Trong phần **Set permissions**, chọn **Attach policies directly**.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/006-attachpolicy.png)

2. Tìm và chọn các policy sau:

    - `AIInvoiceScannerFullPolicy`
    - `AmplifyAdminPolicy`

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/007-attachpolicy.png)
![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/008-attachpolicy.png)

3. Nhấn **Next** để tiếp tục.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/009-attachpolicy.png)

4. Nhấn **Create user** để hoàn tất việc tạo IAM User.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/010-UPDATEattachpolicy.png)

---

#### Bước 4: Lưu thông tin đăng nhập

1. Nhấn **Download .csv file** để lưu **Access Key ID** và mật khẩu console.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/012-savefileiamuser.png)

2. Tệp sẽ được tải về dưới dạng Excel như hình sau — hãy **lưu tệp vào máy tính để sử dụng sau**.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/013-savefileiamuser.png)

---

#### Bước 5: Kiểm tra thông tin IAM User

1. Bấm **Return to users list** để quay về danh sách Users.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/014-checkiamuser.png)

2. IAM User vừa tạo sẽ hiển thị trong danh sách như sau:

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/015-checkiamuser.png)

3. Nhấp vào để xem thông tin chi tiết của IAM User vừa tạo.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/016-checkiamuser.png)

---

#### Bước 6: Tạo Access Key

1. Trong trang thông tin chi tiết của IAM User, chọn **Create access key**.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/017-createaccesskey.png)

2. Chọn **Command Line Interface (CLI)**.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/018-createaccesskey.png)

3. **Tích chọn ô**: I understand the above recommendation and want to proceed to create an access key.
4. Nhấn **Next** để tiếp tục.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/019-createaccesskey.png)

---

#### Bước 7: Đặt mô tả cho Access Key

1. **Description tag value**: `AI Invoice Scanner Project`
2. Bấm **Create access key** để tạo.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/020-createaccesskey.png)

---

#### Bước 8: Sao lưu Access Key

1. Sau khi Access Key được tạo thành công, AWS sẽ cung cấp:

    - **Access Key ID** ✅
    - **Secret Access Key** 🔐

{{% notice warning %}}
⚠️ **Lưu ý**: Đây là lần duy nhất bạn thấy được **Secret Access Key**. Hãy lưu trữ cẩn thận và tuyệt đối không chia sẻ thông tin này trên GitHub hay bất kỳ nơi công khai nào.
{{% /notice %}}

2. Nhấn **Download .csv file** và lưu tệp vào máy tính để sử dụng sau.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/021-saveaccesskey.png)

3. Nhấn **Done** để hoàn tất.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/022-saveaccesskey.png)

---

#### Bước 9: Kiểm tra lại Access Key

1. Trở lại tab **Security credentials**, bạn sẽ thấy **Access Key ID** đã được tạo.
2. Kiểm tra xem Access Key vừa tạo đã có trạng thái **Active**.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/023-checkaccesskey.png)

> 💡 **Lưu ý**: Hãy đảm bảo bạn đã lưu **Secret Access Key** từ bước trước. Nếu chưa lưu, bạn cần xóa và tạo lại Access Key mới.
>
> -   Có thể **deactivate** hoặc **xóa** key này nếu không còn sử dụng.
> -   Tuyệt đối **không commit Access Key vào GitHub** hoặc chia sẻ công khai.
> -   Nếu bạn để lộ key, hãy vào IAM > User > Access Keys > Deactivate rồi Delete.

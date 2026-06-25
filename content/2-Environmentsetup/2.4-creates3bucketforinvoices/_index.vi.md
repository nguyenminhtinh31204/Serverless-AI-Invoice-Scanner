---
title: "Tạo S3 Bucket"
weight: 4
chapter: false
pre: " <b> 2.4 </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ tạo một **S3 bucket** để lưu trữ các tệp hóa đơn mà người dùng tải lên. Bucket này sẽ được tạo trong vùng **us-east-1**, đồng thời bạn sẽ thiết lập một sự kiện (event notification) để kích hoạt **Lambda function** mỗi khi có tệp mới được upload vào thư mục.

---

#### Bước 1: Truy cập S3 Console

1. Đăng nhập vào [AWS Console](https://console.aws.amazon.com/), tìm **S3**, sau đó chọn **S3** trong kết quả.

![S3 Console](/images/2.environmentsetup/2.4-creates3bucket/001-opens3console.png)

{{% notice info %}}
💡 **Lưu ý:** Trước khi tạo, hãy đảm bảo bạn đã chọn đúng region là **us-east-1** ở góc trên bên phải màn hình AWS Console.  
Việc tạo S3 bucket ở đúng region là rất quan trọng để các dịch vụ như Lambda hoặc Textract hoạt động đồng bộ.
{{% /notice %}}

2. Nhấn **Create bucket** để bắt đầu tạo mới.

![Create Bucket](/images/2.environmentsetup/2.4-creates3bucket/002-createbucket.png)

---

#### Bước 2: Cấu hình bucket

1. **Bucket name**: `invoice-upload-s3-bucket`

![Configure Bucket](/images/2.environmentsetup/2.4-creates3bucket/003-bucketname-region.png)

{{% notice info %}}
💡 **Lưu ý:** Tên bucket phải duy nhất trên toàn cầu. Bạn có thể thêm hậu tố nếu tên bị trùng, ví dụ: **invoice-upload-s3-bucket-123456**.
{{% /notice %}}

2. Trong phần **Object Ownership**, chọn **ACLs disabled**.

3. **Block Public Access settings**: giữ nguyên mặc định (bật toàn bộ) để đảm bảo dữ liệu không công khai.

![Block Public Access](/images/2.environmentsetup/2.4-creates3bucket/004-publicaccess.png)

4. Trong phần **Bucket Versioning**, chọn **Disabled**.

![Versioning](/images/2.environmentsetup/2.4-creates3bucket/006-versioning.png)

5. Trong phần **Default encryption**, chọn **Amazon S3 managed keys (SSE-S3)**.

6. Nhấn **Create bucket** để hoàn tất.

![Encryption](/images/2.environmentsetup/2.4-creates3bucket/005-encryption.png)

7. Kiểm tra **Bucket** đã tạo.

![Bucket](/images/2.environmentsetup/2.4-creates3bucket/007-check-bucket.png)

---

#### Bước 3: Tạo thư mục uploads/

1.  Trong bucket vừa tạo, nhấn **Create folder**.

![Create Folder](/images/2.environmentsetup/2.4-creates3bucket/008-create-folder.png)

2.  Đặt tên folder là: `uploads`

![Create Folder](/images/2.environmentsetup/2.4-creates3bucket/009-folder-name.png)

3.  Nhấn **Create folder** để xác nhận.

![Create Folder](/images/2.environmentsetup/2.4-creates3bucket/010-createuploads.png)

4.  Kiểm tra sau khi tạo folder.

![Create Folder](/images/2.environmentsetup/2.4-creates3bucket/011-checkfolder.png)

---

#### Bước 7: Thiết lập Event Notification

1. Vào tab **Properties** của bucket.

![Event Notifications](/images/2.environmentsetup/2.4-creates3bucket/012-properties.png)

2. Kéo xuống phần **Event notifications** → chọn **Create event notification**.

![Event Notifications](/images/2.environmentsetup/2.4-creates3bucket/013-createevent.png)

3. Cấu hình như sau:

    - **Name**: `TriggerLambdaOnUpload`
    - **Prefix**: `uploads/`
    - **Suffix**: _(để trống)_
    - **Event types**: chọn `PUT` (All object create events)
    - **Destination**: chọn **Lambda function**
    - **Lambda function**: chọn `UploadInvoiceFileFunction`

![Event Settings](/images/2.environmentsetup/2.4-creates3bucket/014-eventsettings.png)

![Event Settings](/images/2.environmentsetup/2.4-creates3bucket/015-eventsettings.png)

![Event Settings](/images/2.environmentsetup/2.4-creates3bucket/016-eventsettings.png)

4. Nhấn **Save changes** để hoàn tất.

![Event Settings](/images/2.environmentsetup/2.4-creates3bucket/017-savechanges.png)

{{% notice info %}}
Nếu chưa thấy Lambda function, hãy đảm bảo bạn đã tạo đúng vùng **(us-east-1)** và IAM role của Lambda có quyền **s3:PutBucketNotification**.
{{% /notice %}}

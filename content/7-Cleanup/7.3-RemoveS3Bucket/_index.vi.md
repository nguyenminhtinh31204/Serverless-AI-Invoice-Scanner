+++
title = "Xóa S3 Bucket"
date = 2025
weight = 3
chapter = false
pre = "<b>7.3 </b>"
+++

#### Tổng quan

Trong bước này, bạn sẽ xóa **Amazon S3 Bucket** đã được tạo để lưu trữ các tệp hóa đơn trong hệ thống **Serverless AI Invoice Scanner**.

Amazon S3 được sử dụng để lưu các file hóa đơn do người dùng upload từ frontend. Các file này thường được lưu trong thư mục:

```txt
uploads/
```

Sau khi hoàn thành bài lab, bạn nên xóa S3 Bucket và toàn bộ object bên trong để tránh phát sinh chi phí lưu trữ không cần thiết.

{{% notice warning %}}
Khi xóa S3 Bucket, toàn bộ file hóa đơn đã upload sẽ bị xóa vĩnh viễn nếu bạn không sao lưu trước. Hãy đảm bảo rằng bạn không còn cần các file này trước khi tiếp tục.
{{% /notice %}}

---

#### Tài nguyên cần xóa

Trong dự án này, S3 Bucket được sử dụng để lưu trữ:

| Thành phần | Mô tả |
|---|---|
| S3 Bucket | Lưu trữ file hóa đơn được upload từ frontend |
| Thư mục `uploads/` | Chứa các file PDF, PNG, JPG hoặc JPEG của hóa đơn |
| S3 Event Trigger | Kích hoạt Lambda xử lý hóa đơn sau khi file được upload |
| Object metadata | Có thể chứa thông tin như content type, file name hoặc thời gian upload |

Tên bucket của bạn có thể có dạng tương tự:

```txt
invoice-scanner-upload-bucket
```

hoặc tên bucket bạn đã đặt trong quá trình tạo tài nguyên.

{{% notice info %}}
Tên S3 Bucket phải là duy nhất toàn cầu, vì vậy tên bucket thực tế trong tài khoản AWS của bạn có thể khác với ví dụ trong tài liệu.
{{% /notice %}}

---

#### Kiểm tra S3 Event Trigger trước khi xóa

Trước khi xóa bucket, bạn nên kiểm tra xem bucket có đang cấu hình event trigger đến Lambda hay không.

Trong hệ thống này, S3 Bucket có thể đang trigger Lambda xử lý hóa đơn:

```txt
S3 Bucket → Processing Lambda
```

Trigger này được dùng để tự động xử lý file sau khi người dùng upload hóa đơn.

Nếu Lambda Function đã được xóa ở bước trước, trigger này không còn cần thiết nữa.

---

#### Các bước thực hiện

- Mở **AWS Management Console**.

- Tìm kiếm và truy cập dịch vụ **S3**.

![Remove S3 Bucket](/images/7/7.3/Screenshot_1.png)

- Trong danh sách bucket, chọn S3 Bucket được sử dụng cho dự án **Serverless AI Invoice Scanner**.

Ví dụ:

```txt
invoice-scanner-upload-bucket
```

![Remove S3 Bucket](/images/7/7.3/Screenshot_2.png)

- Mở bucket và kiểm tra các object bên trong.

Thông thường, các file hóa đơn được lưu trong thư mục:

```txt
uploads/
```

![Remove S3 Bucket](/images/7/7.3/Screenshot_3.png)

---

#### Xóa toàn bộ object trong bucket

Trước khi xóa bucket, bạn cần làm trống bucket.

- Chọn toàn bộ object hoặc thư mục trong bucket.

- Chọn **Delete**.

![Remove S3 Bucket](/images/7/7.3/Screenshot_4.png)

- Hộp thoại xác nhận sẽ xuất hiện.

- Nhập nội dung xác nhận theo yêu cầu của AWS Console, thường là:

```txt
permanently delete
```

![Remove S3 Bucket](/images/7/7.3/Screenshot_5.png)

- Chọn **Delete objects** để xóa toàn bộ object.

{{% notice info %}}
S3 Bucket chỉ có thể bị xóa khi bucket đã trống. Nếu bucket vẫn còn object, AWS sẽ không cho phép xóa bucket.
{{% /notice %}}

---

#### Xóa S3 Bucket

Sau khi bucket đã trống:

- Quay lại danh sách bucket trong Amazon S3.

- Chọn bucket cần xóa.

![Remove S3 Bucket](/images/7/7.3/Screenshot_7.png)

- Chọn **Delete**.

![Remove S3 Bucket](/images/7/7.3/Screenshot_8.png)

- Nhập tên bucket để xác nhận xóa.

Ví dụ:

```txt
invoice-scanner-upload-bucket
```

![Remove S3 Bucket](/images/7/7.3/Screenshot_9.png)

- Chọn **Delete bucket** để hoàn tất.

![Remove S3 Bucket](/images/7/7.3/Screenshot_10.png)

---

#### Trường hợp bucket bật Versioning

Nếu bucket của bạn đã bật **S3 Versioning**, việc xóa object thông thường có thể chưa làm bucket trống hoàn toàn.

Trong trường hợp này, bạn cần xóa thêm:

- Object versions.
- Delete markers.

Các bước thực hiện:

- Mở bucket.
- Bật tùy chọn hiển thị **Show versions**.
- Chọn toàn bộ object versions và delete markers.
- Chọn **Delete**.
- Sau khi bucket thật sự trống, quay lại danh sách bucket và xóa bucket.

{{% notice warning %}}
Nếu S3 Versioning được bật, bucket có thể vẫn không xóa được dù bạn đã xóa các object hiện tại. Hãy kiểm tra object versions và delete markers trước khi xóa bucket.
{{% /notice %}}

---

#### Kiểm tra sau khi xóa

Sau khi xóa S3 Bucket:

- Quay lại danh sách bucket trong Amazon S3.
- Kiểm tra rằng bucket của dự án không còn xuất hiện.
- Thử upload hóa đơn từ frontend.
- Chức năng upload sẽ không còn hoạt động vì bucket lưu trữ đã bị xóa.
- Nếu còn S3 trigger cũ, hãy kiểm tra lại Lambda hoặc Event Notifications để đảm bảo không còn cấu hình dư thừa.

---

#### Ảnh hưởng đến hệ thống

Sau khi xóa S3 Bucket, hệ thống sẽ không còn nơi lưu trữ file hóa đơn được upload.

| Thành phần | Ảnh hưởng |
|---|---|
| React frontend | Không thể upload file hóa đơn thành công. |
| Upload Lambda | Không thể lưu file vào S3 Bucket đã bị xóa. |
| Processing Lambda | Không còn nhận sự kiện từ S3 để xử lý hóa đơn mới. |
| Amazon Textract | Không còn file đầu vào từ S3 để trích xuất văn bản. |
| OpenAI API | Không còn dữ liệu hóa đơn mới để chuẩn hóa. |
| Amazon DynamoDB | Dữ liệu hóa đơn cũ vẫn còn nếu bảng chưa bị xóa. |
| Amazon CloudWatch | Log cũ vẫn còn nếu Log Groups chưa bị xóa. |

---

#### Lưu ý quan trọng

{{% notice info %}}
Việc xóa S3 Bucket không tự động xóa dữ liệu trong DynamoDB. Nếu hóa đơn đã được xử lý và lưu vào DynamoDB, bạn cần xóa bảng DynamoDB ở bước cleanup tiếp theo.
{{% /notice %}}

{{% notice warning %}}
Nếu Lambda Function vẫn còn tồn tại và đang sử dụng biến môi trường trỏ đến S3 Bucket đã xóa, các lần gọi Lambda sau đó sẽ bị lỗi vì bucket không tồn tại.
{{% /notice %}}

{{% notice info %}}
Nếu bucket có cấu hình lifecycle rules, event notifications hoặc bucket policy, các cấu hình này sẽ bị xóa cùng với bucket.
{{% /notice %}}

---

#### Kết luận

Bạn đã xóa thành công Amazon S3 Bucket được sử dụng trong hệ thống **Serverless AI Invoice Scanner**.

- Các file hóa đơn trong thư mục `uploads/` đã được xóa.
- S3 Bucket dùng để lưu trữ file upload đã được xóa.
- S3 Event Trigger đến Lambda xử lý hóa đơn không còn hoạt động.
- Hệ thống không còn lưu trữ file hóa đơn mới trên Amazon S3.
- Bước dọn dẹp này giúp giải phóng tài nguyên lưu trữ và tránh phát sinh chi phí không cần thiết.

+++
title = "Xóa DynamoDB Table"
date = 2025
weight = 4
chapter = false
pre = "<b>7.4 </b>"
+++

#### Tổng quan

Trong bước này, bạn sẽ xóa **Amazon DynamoDB Table** đã được tạo để lưu trữ dữ liệu hóa đơn trong hệ thống **Serverless AI Invoice Scanner**.

Amazon DynamoDB được sử dụng để lưu các thông tin hóa đơn sau khi file được xử lý bởi Lambda, Amazon Textract và OpenAI API. Dữ liệu trong bảng có thể bao gồm thông tin khách hàng, mã hóa đơn, ngày hóa đơn, tổng tiền, loại tiền tệ, tags và trạng thái đánh dấu quan trọng.

Sau khi hoàn thành bài lab, bạn nên xóa bảng DynamoDB để tránh giữ lại dữ liệu thử nghiệm và tránh phát sinh chi phí không cần thiết.

{{% notice warning %}}
Khi xóa DynamoDB Table, toàn bộ dữ liệu bên trong bảng sẽ bị xóa vĩnh viễn. Hãy đảm bảo rằng bạn đã sao lưu dữ liệu cần thiết trước khi thực hiện thao tác này.
{{% /notice %}}

---

#### Tài nguyên cần xóa

Trong dự án này, DynamoDB Table cần xóa là:

```txt
InvoiceData
```

Bảng này được sử dụng để lưu trữ dữ liệu hóa đơn đã được trích xuất và chuẩn hóa.

Một số dữ liệu thường được lưu trong bảng bao gồm:

| Trường dữ liệu | Mô tả |
|---|---|
| `InvoiceId` | Mã định danh duy nhất của hóa đơn. |
| `CustomerName` | Tên khách hàng hoặc nhà cung cấp được trích xuất từ hóa đơn. |
| `InvoiceNumber` | Số hóa đơn. |
| `InvoiceDate` | Ngày lập hóa đơn. |
| `TotalAmount` | Tổng tiền trên hóa đơn. |
| `Currency` | Loại tiền tệ, ví dụ `USD`, `VND`, `EUR`. |
| `Tags` | Danh sách nhãn được thêm cho hóa đơn. |
| `Starred` | Trạng thái đánh dấu hóa đơn quan trọng. |
| `ExtractedData` | Dữ liệu đã trích xuất và chuẩn hóa từ nội dung hóa đơn. |

{{% notice info %}}
Tên bảng trong tài khoản AWS của bạn có thể khác nếu bạn đã đặt tên khác trong quá trình triển khai. Hãy kiểm tra đúng bảng được Lambda sử dụng trong biến môi trường hoặc trong mã nguồn backend.
{{% /notice %}}

---

#### Kiểm tra dữ liệu trước khi xóa

Trước khi xóa bảng, bạn có thể kiểm tra nhanh dữ liệu trong bảng.

- Mở **AWS Management Console**.
- Tìm kiếm và truy cập dịch vụ **DynamoDB**.
- Chọn **Tables**.
- Chọn bảng:

```txt
InvoiceData
```

- Chọn **Explore table items** để xem các item đã được lưu.

![Remove DynamoDB Table](/images/7/7.4/Screenshot_1.png)

Nếu cần giữ lại dữ liệu phục vụ báo cáo hoặc kiểm thử, hãy export hoặc chụp lại dữ liệu trước khi xóa.

---

#### Các bước thực hiện

- Mở **AWS Management Console**.

- Tìm kiếm và truy cập dịch vụ **DynamoDB**.

![Remove DynamoDB Table](/images/7/7.4/Screenshot_2.png)

- Trong thanh điều hướng bên trái, chọn **Tables**.

![Remove DynamoDB Table](/images/7/7.4/Screenshot_3.png)

- Trong danh sách bảng, chọn bảng được sử dụng cho dự án:

```txt
InvoiceData
```

![Remove DynamoDB Table](/images/7/7.4/Screenshot_4.png)

- Sau khi mở bảng, chọn **Actions**.

- Chọn **Delete table**.

![Remove DynamoDB Table](/images/7/7.4/Screenshot_5.png)

- Hộp thoại xác nhận xóa sẽ xuất hiện.

- Đọc kỹ thông báo xác nhận để đảm bảo bạn đang xóa đúng bảng.

- Nhập nội dung xác nhận theo yêu cầu của AWS Console nếu có.

![Remove DynamoDB Table](/images/7/7.4/Screenshot_6.png)

- Chọn **Delete table** để xác nhận xóa bảng.

![Remove DynamoDB Table](/images/7/7.4/Screenshot_7.png)

---

#### Xóa Global Secondary Indexes

Nếu bảng `InvoiceData` có tạo thêm **Global Secondary Indexes**, các index này sẽ được xóa cùng với bảng.

Trong dự án này, bảng có thể sử dụng các index như:

```txt
CustomerName-index
StarredInvoicesIndex
```

Các index này được dùng để hỗ trợ:

- Tìm kiếm hóa đơn theo tên khách hàng.
- Lọc danh sách hóa đơn đã đánh dấu quan trọng.

{{% notice info %}}
Bạn không cần xóa Global Secondary Indexes riêng nếu xóa toàn bộ DynamoDB Table. Khi bảng bị xóa, các index thuộc bảng đó cũng bị xóa theo.
{{% /notice %}}

---

#### Kiểm tra sau khi xóa

Sau khi xóa bảng DynamoDB:

- Quay lại danh sách **Tables** trong DynamoDB.
- Kiểm tra rằng bảng `InvoiceData` không còn xuất hiện.
- Nếu bảng đang ở trạng thái `Deleting`, chờ vài phút cho đến khi quá trình xóa hoàn tất.
- Thử gọi lại các API lấy danh sách hóa đơn hoặc tìm kiếm hóa đơn.
- Các API liên quan đến dữ liệu hóa đơn sẽ không còn trả về dữ liệu vì bảng đã bị xóa.

---

#### Ảnh hưởng đến hệ thống

Sau khi xóa DynamoDB Table, toàn bộ dữ liệu hóa đơn trong hệ thống sẽ bị mất.

| Thành phần | Ảnh hưởng |
|---|---|
| React frontend | Không còn hiển thị được danh sách, chi tiết hoặc kết quả tìm kiếm hóa đơn. |
| Invoice Management Lambda | Không thể truy vấn hoặc cập nhật dữ liệu hóa đơn. |
| Processing Lambda | Không thể lưu kết quả xử lý hóa đơn mới vào DynamoDB. |
| API Gateway | API vẫn có thể tồn tại nhưng các request liên quan đến dữ liệu hóa đơn sẽ bị lỗi. |
| Amazon S3 | File hóa đơn trong S3 không bị ảnh hưởng nếu bucket chưa bị xóa. |
| Amazon Textract | Không bị ảnh hưởng trực tiếp. |
| OpenAI API | Không bị ảnh hưởng trực tiếp. |
| CloudWatch Logs | Log cũ vẫn còn nếu chưa xóa Log Groups. |

---

#### Lưu ý quan trọng

{{% notice warning %}}
Dữ liệu trong DynamoDB Table sẽ bị xóa vĩnh viễn. Nếu bạn cần giữ lại dữ liệu hóa đơn để làm minh chứng báo cáo, hãy export hoặc chụp màn hình trước khi xóa.
{{% /notice %}}

{{% notice info %}}
Việc xóa DynamoDB Table không tự động xóa Lambda Functions, API Gateway, S3 Bucket, Cognito User Pool hoặc CloudWatch Logs. Các tài nguyên này cần được xóa riêng ở các bước cleanup tương ứng.
{{% /notice %}}

{{% notice warning %}}
Nếu Lambda Function vẫn còn tồn tại và đang trỏ đến bảng `InvoiceData`, các lần gọi Lambda sau khi bảng bị xóa có thể gặp lỗi như `ResourceNotFoundException`.
{{% /notice %}}

---

#### Kết luận

Bạn đã xóa thành công DynamoDB Table được sử dụng trong hệ thống **Serverless AI Invoice Scanner**.

- Bảng `InvoiceData` đã được xóa.
- Dữ liệu hóa đơn đã trích xuất và chuẩn hóa không còn được lưu trong DynamoDB.
- Các index như `CustomerName-index` hoặc `StarredInvoicesIndex` cũng được xóa cùng với bảng.
- Các chức năng xem danh sách, xem chi tiết, tìm kiếm, cập nhật tags và đánh dấu hóa đơn quan trọng sẽ không còn hoạt động.
- Bước dọn dẹp này giúp xóa dữ liệu thử nghiệm và tránh phát sinh chi phí lưu trữ không cần thiết.

#### Important Note

{{% notice warning %}}
Data in the DynamoDB Table will be permanently deleted. If you need to retain invoice data for reporting purposes, please export or take a screenshot before deleting.

{{% /notice %}}

{{% notice info %}}
Deleting a DynamoDB Table does not automatically delete Lambda Functions, API Gateway, S3 Bucket, Cognito User Pool, or CloudWatch Logs. These resources need to be deleted separately in their respective cleanup steps.

{{% /notice %}}

{{% notice warning %}}
If the Lambda Function still exists and is pointing to the `InvoiceData` table, Lambda calls after the table is deleted may result in errors such as `ResourceNotFoundException`.

{{% /notice %}}

---

#### Conclusion

You have successfully deleted the DynamoDB Table used in the **Serverless AI Invoice Scanner** system.

- The `InvoiceData` table has been deleted.

- The extracted and normalized invoice data is no longer stored in DynamoDB.

- Indexes such as `CustomerName-index` or `StarredInvoicesIndex` have also been deleted along with the table.

- The functions of viewing lists, details, searching, updating tags, and marking important invoices will no longer work.

- This cleanup step helps remove test data and avoids unnecessary storage costs.
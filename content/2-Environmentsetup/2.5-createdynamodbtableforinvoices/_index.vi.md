---
title: "Tạo DynamoDB Table"
weight: 5
chapter: false
pre: " <b> 2.5 </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ tạo một **DynamoDB Table** để lưu trữ thông tin hóa đơn sau khi được xử lý bởi Lambda function. Bảng sẽ sử dụng chế độ **on-demand** và có thêm 2 **Global Secondary Indexes (GSIs)** để hỗ trợ truy vấn theo tên khách hàng và các hóa đơn đánh dấu sao.

---

#### Bước 1: Truy cập DynamoDB Console

1. Đăng nhập vào [AWS Console](https://console.aws.amazon.com/), tìm **DynamoDB**, sau đó chọn **DynamoDB** trong kết quả.

![Open DynamoDB](/images/2.environmentsetup/2.5-createdynamodb/001-opendynamodb.png)

{{% notice info %}}
💡 **Lưu ý:** Trước khi nhấn **Create table**, hãy đảm bảo bạn đã chọn đúng **region là ap-southeast-1(Singapore)** ở góc trên bên phải màn hình AWS Console.  
{{% /notice %}}

2. Nhấn **Create table** để bắt đầu tạo bảng mới.

![Create Table](/images/2.environmentsetup/2.5-createdynamodb/002-createtable.png)

---

#### Bước 2: Cấu hình bảng DynamoDB

1. **Table name**: `InvoiceData`

2. **Partition key**:

    - **Name**: `InvoiceId`
    - **Type**: `String`

3. Bỏ qua phần **Sort key** (không cần thiết).

![Table Key](/images/2.environmentsetup/2.5-createdynamodb/003-tablekeys.png)

4. Trong phần **Table settings**, Chọn **Default settings** để chuyển sang chế độ **On-demand capacity**.

![Billing Mode](/images/2.environmentsetup/2.5-createdynamodb/004-ondemand.png)

5. Nhấn **Create table** để hoàn tất.

![Create Table](/images/2.environmentsetup/2.5-createdynamodb/005-finishcreate.png)

---

#### Bước 3: Thêm Global Secondary Index (GSI)

Sau khi bảng **InvoiceData** được tạo thành công, bạn sẽ thêm hai chỉ mục phụ:

---

##### GSI #1: CustomerName-index

1. Trong trang chi tiết bảng, chọn tab **Indexes**.

![Create Index](/images/2.environmentsetup/2.5-createdynamodb/006-indexes.png)

2. Nhấn **Create index**.

![Create Index](/images/2.environmentsetup/2.5-createdynamodb/007-createindex.png)

3. Cấu hình:

    - **Partition key**: `CustomerName`
    - **Data type**: String
    - **Sort key**: _(để trống)_
    - **Projected attributes**: chọn **All**

![GSI 1](/images/2.environmentsetup/2.5-createdynamodb/008-gsi1.png)

![GSI 1](/images/2.environmentsetup/2.5-createdynamodb/009-gsi1.png)

4. Nhấn **Create index**.

![GSI 1](/images/2.environmentsetup/2.5-createdynamodb/010-gsi1.png)

---

##### GSI #2: StarredInvoicesIndex

1. Nhấn **Create index** lần nữa để tạo GSI thứ hai.

![GSI 2](/images/2.environmentsetup/2.5-createdynamodb/011-gsi2.png)

2. Cấu hình:

    - **Partition key**: `IsStarred` (kiểu **String**)
    - **Sort key**: `CreatedAt` (kiểu **String**)
    - **Index name**: `StarredInvoicesIndex`
    - **Projected attributes**: chọn **All**

![GSI 2](/images/2.environmentsetup/2.5-createdynamodb/012-gsi2.png)

![GSI 2](/images/2.environmentsetup/2.5-createdynamodb/013-gsi2.png)

3. Nhấn **Create index**.

![GSI 2](/images/2.environmentsetup/2.5-createdynamodb/014-gsi2.png)

4. Đảm bảo cả hai GSI **CustomerName-index** và **StarredInvoicesIndex** đều có trạng thái **ACTIVE** trước khi tiếp tục cấu hình Lambda function.

![Check GSI Status](/images/2.environmentsetup/2.5-createdynamodb/015-gsistatus.png)

{{% notice warning %}}
⚠️ Nếu trạng thái của GSI vẫn là **Creating**, bạn cần đợi vài phút đến khi chuyển sang **Active** trước khi thực hiện truy vấn hoặc triển khai Lambda truy cập GSI.
{{% /notice %}}

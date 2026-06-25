---
title: "Kích hoạt Nova Pro"
weight: 6
chapter: false
pre: " <b> 2.6 </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ kích hoạt model **Nova Pro** trên Amazon Bedrock để sử dụng trong dự án Serverless Invoices Scanner. Đây là một trong những model ngôn ngữ mạnh mẽ do chính Amazon phát triển, phù hợp cho tác vụ trích xuất, phân tích và phân loại thông tin từ hóa đơn.

{{% notice tip %}}
Bạn chỉ cần kích hoạt model một lần duy nhất cho mỗi region. Trong dự án này, hãy đảm bảo đang thao tác ở **us-east-1 (N. Virginia)** để đồng bộ với Lambda, S3 và các dịch vụ khác.
{{% /notice %}}

#### Bước 1: Truy cập Amazon Bedrock Console

1. Đăng nhập vào [AWS Console](https://console.aws.amazon.com/), tìm **Bedrock**, sau đó chọn **Amazon Bedrock**.

![Open Bedrock](/images/2.environmentsetup/2.6-enablebedrock/001-openbedrock.png)

2. Trong giao diện Amazon Bedrock, ở thanh điều hướng bên trái, chọn **Model access**.

![Model Access](/images/2.environmentsetup/2.6-enablebedrock/002-modelaccess.png)

---

#### Bước 2: Kích hoạt mô hình Nova Pro

1. Tại phần **Base models**, cuộn để tìm dòng có tên **Nova Pro**.

2. Nhấn vào **Available to request**, sau đó chọn **Request model access**.

![Enable Nova Pro](/images/2.environmentsetup/2.6-enablebedrock/003-enablenovapro.png)

3. Nhấn **Next** để tiếp tục.

![Enable Nova Pro](/images/2.environmentsetup/2.6-enablebedrock/004-enablenovapro.png)

4. Nhấn **Submit** để gửi yêu cầu truy cập model.

![Enable Nova Pro](/images/2.environmentsetup/2.6-enablebedrock/005-enablenovapro.png)

5. Sau khi gửi thành công, chờ vài giây đến vài phút, trạng thái model sẽ chuyển sang **Access granted**.

![Enable Access](/images/2.environmentsetup/2.6-enablebedrock/006-accessgranted.png)

---

#### Hoàn tất

Bạn đã kích hoạt thành công mô hình Nova Pro trên Amazon Bedrock. Model này giờ đây đã sẵn sàng để sử dụng trong Lambda Function để xử lý dữ liệu hóa đơn trong hệ thống Serverless Invoices Scanner.

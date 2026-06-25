---
title: "Triển khai Frontend"
weight: 6
chapter: false
pre: " <b> 6 </b> "
---

### Yêu cầu

-   Đảm bảo bạn đã tải **Visual Studio Code**. Nếu bạn chưa tải thì hãy tải tại liên kết sau: [https://code.visualstudio.com/download](https://code.visualstudio.com/download).

---

#### Bước 1: Tải xuống source code

-   Nhấn vào [Tải xuống source code](https://drive.google.com/uc?export=download&id=1pbOd59G4p2Z0K4T2nTDBZAqTRO1Ghksi).
-   Giải nén file zip vừa tải xuống.

#### Bước 2: Cập nhật API Endpoint

-   Mở **Visual Studio Code**.
-   Mở tệp `.env` và thay giá trị API endpoint bằng API ID tương ứng từ API Gateway:
    -   **REACT_APP_API_UPLOAD_URL** : Dùng **API ID** của `PostInvoiceAPI` (AWS API Gateway).
    -   **REACT_APP_API_INVOICE_URL** : Dùng **API ID** của `GetInvoiceAPI` (AWS API Gateway).
    -   **REACT_APP_API_UPDATE_TAGS_URL** : Dùng **API ID** của `GetInvoiceAPI` (AWS API Gateway).
    -   **REACT_APP_API_UPDATE_STARRED_URL** : Dùng **API ID** của `GetInvoiceAPI` (AWS API Gateway).

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_1.png)

#### Bước 3: Cấu hình Amplify

-   Mở **Terminal** bằng cách sử dụng phím `Ctrl + ~`.

-   Trỏ đến thư mục gốc của dự án bằng cách sử dụng:

```bash
cd <folder_name>
```

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_2.png)

-   Chạy câu lệnh sau để **cài đặt dependencies**:

```bash
npm install
```

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_3.png)

-   Chạy câu lệnh sau để cài đặt **AWS Amplify CLI**:

```bash
npm install -g @aws-amplify/cli
```

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_4.png)

-   Sau khi cài đặt xong, kiểm tra **AWS Amplify CLI** đã cài đặt thành công chưa bằng cách dùng câu lệnh sau:

```bash
amplify -v
```

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_5.png)

-   Sử dụng câu lệnh sau để cấu hình Amplify:

```bash
amplify configure
```

{{% notice warning %}}
⚠️ **Lưu ý**: Sau khi dùng câu lệnh `amplify configure` để cấu hình, lập tức sẽ xuất hiện tab trang mới. bạn có thể đóng tab và quay trở lại **Visual Studio Code** tiếp tục cấu hình nhé!
{{% /notice %}}

-   Trong **cấu hình Amplify**, bạn thực hiện như sau:

    -   **Region**: chọn `us-east-1`.
    -   **Access Key ID**: điền **access key** của IAM User mà bạn đã tạo.
    -   **Secret Access Key**: điền **secret key** của IAM User mà bạn đã tạo.
    -   **Profile Name**: chọn `default`.

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_6.png)

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_7.png)

-   Sử dụng câu lệnh sau để **khởi tạo Amplify**:

```bash
amplify init
```

-   Bạn hãy cấu hình như sau để khởi tạo Amplify:

    -   **? Do you want to use an existing environment**: chọn `No`.
    -   **? Enter a name for the environment**: `dev`.
    -   **? Select the authentication method you want to use**: `AWS access keys`.
    -   **? accessKeyId**: điền **access key** của IAM User mà bạn đã tạo.
    -   **? secretAccessKey**: điền **secret key** của IAM User mà bạn đã tạo.
    -   **? region**: `us-east-1`.

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_8.png)

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_9.png)

#### Bước 4: Cấu hình Cognito

-   Sử dụng câu lệnh sau để thêm **Auth** của **AWS Cognito**:

```bash
amplify add auth
```

-   Cấu hình thêm Auth như sau:

    -   **Do you want to use the default authentication and security configuration?**: `Default configuration`.
    -   **How do you want users to be able to sign in?**: `Email`.
    -   **Do you want to configure advanced settings?**: `No, I am done`.

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_10.png)

-   Sau đó triển khai các thay đổi:

```bash
amplify push
```

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_11.png)

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_12.png)

#### Bước 5: Chạy ứng dụng

-   Khởi chạy ứng dụng trong:

```bash
npm start
```

-   Bạn hãy thử thao tác các chức năng như:
    -   Tải tệp hóa đơn.
    -   Xem hóa đơn chi tiết.
    -   Xem tất cả hóa đơn.
    -   Tra cứu hóa đơn theo ID hoặc tên khách hàng.
    -   Xuất tệp Excel.
    -   Lọc tìm kiếm theo thời gian.
    -   Sắp xếp theo tổng số tiền.
    -   Sắp xếp theo ngày hóa đơn.
    -   Kéo - thả khi tải tệp hóa đơn.
    -   Xem tags của hóa đơn.
    -   Thêm tags hóa đơn.
    -   Chỉnh sửa tags hóa đơn.
    -   Lọc tags hóa đơn.
    -   Đánh dấu hóa đơn quan trọng.

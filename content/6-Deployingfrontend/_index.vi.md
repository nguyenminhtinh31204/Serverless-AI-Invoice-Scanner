---

title: "Triển khai Frontend"
weight: 6
chapter: false
pre: " <b> 6 </b> "
-------------------

### Yêu cầu

Trước khi triển khai frontend, bạn cần chuẩn bị:

* Visual Studio Code.
* Node.js và npm.
* Git và tài khoản GitHub.
* Tài khoản AWS có quyền sử dụng Amazon Cognito và AWS Amplify Hosting.
* Các API Gateway endpoint đã tạo ở các bước trước.
* Source code React frontend.

{{% notice info %}}
Trong dự án này, **AWS Amplify** được sử dụng với vai trò **Amplify Hosting** để triển khai React frontend. Amazon Cognito được cấu hình thủ công trên AWS Console thay vì dùng `amplify init`, `amplify add auth` và `amplify push`.
{{% /notice %}}

---

#### Bước 1: Tải và mở source code frontend

* Tải source code frontend.(https://github.com/nguyenminhtinh31204/frontend-invoice-scanner.git)
* Giải nén file zip vừa tải.
* Mở **Visual Studio Code**.
* Mở thư mục source code đã giải nén.

Cấu trúc dự án sẽ có dạng tương tự:

```txt
public/
src/
.env.example
package.json
README.md
```

Mở Terminal trong Visual Studio Code bằng phím:

```txt
Ctrl + ~
```

Di chuyển vào thư mục gốc của dự án:

```bash
cd <folder_name>
```

Cài đặt các thư viện cần thiết:

```bash
npm install
```

---

#### Bước 2: Cập nhật biến môi trường

Mở file `.env` trong thư mục gốc của dự án.

Nếu chưa có file `.env`, hãy tạo mới file:

```txt
.env
```

Sau đó cấu hình nội dung như sau:

```env
REACT_APP_SEND_AUTH_TOKEN=false

REACT_APP_AWS_REGION=ap-southeast-1

REACT_APP_USER_POOL_ID=ap-southeast-1_xxxxxxxxx
REACT_APP_USER_POOL_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx

REACT_APP_API_UPLOAD_URL=https://<PostInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/uploads

REACT_APP_API_INVOICE_URL=https://<GetInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/invoice

REACT_APP_API_UPDATE_TAGS_URL=https://<GetInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/invoice/tags

REACT_APP_API_UPDATE_STARRED_URL=https://<GetInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/invoice/starred
```

Trong đó:

* **REACT_APP_USER_POOL_ID**: User Pool ID lấy từ Amazon Cognito.
* **REACT_APP_USER_POOL_CLIENT_ID**: App Client ID lấy từ Amazon Cognito.
* **REACT_APP_API_UPLOAD_URL**: API Gateway endpoint dùng để upload hóa đơn.
* **REACT_APP_API_INVOICE_URL**: API Gateway endpoint dùng để lấy danh sách và chi tiết hóa đơn.
* **REACT_APP_API_UPDATE_TAGS_URL**: API Gateway endpoint dùng để cập nhật tags hóa đơn.
* **REACT_APP_API_UPDATE_STARRED_URL**: API Gateway endpoint dùng để đánh dấu hóa đơn quan trọng.

{{% notice warning %}}
Không lưu OpenAI API Key trong file `.env` của React frontend. OpenAI API Key phải được lưu ở phía backend, ví dụ trong Lambda environment variables hoặc AWS Secrets Manager.
{{% /notice %}}

---

#### Bước 3: Cấu hình Amazon Cognito

Vì dự án không còn sử dụng Amplify Gen 1 CLI để tạo Auth, Amazon Cognito sẽ được tạo thủ công trên AWS Console.

Truy cập:

```txt
AWS Console → Amazon Cognito → User pools → Create user pool
```

Cấu hình User Pool như sau:

* **Application type**: `Single-page application (SPA)`.
* **Application name**: `invoice-scanner-frontend`.
* **Sign-in identifiers**: Chọn `Email`.
* **Self-registration**: Bật `Enable self-registration`.
* **Required attributes**: Chọn `email`.
* **User pool name**: `invoice-scanner-user-pool`.
* **App client name**: `invoice-scanner-frontend-client`.
* **Client secret**: Không tạo client secret.

Với môi trường chạy local, thêm URL sau:

```txt
http://localhost:3000/
```

vào các mục:

* Allowed callback URLs.
* Allowed sign-out URLs.

Sau khi tạo User Pool xong, sao chép hai giá trị sau:

```txt
User Pool ID
App Client ID
```

Sau đó cập nhật vào file `.env`:

```env
REACT_APP_USER_POOL_ID=ap-southeast-1_xxxxxxxxx
REACT_APP_USER_POOL_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
```

---

#### Bước 4: Cấu hình Cognito trong React

Tạo file:

```txt
src/awsConfig.js
```

Thêm nội dung sau:

```javascript
const awsConfig = {
  Auth: {
    Cognito: {
      userPoolId: process.env.REACT_APP_USER_POOL_ID,
      userPoolClientId: process.env.REACT_APP_USER_POOL_CLIENT_ID,
      loginWith: {
        email: true
      }
    }
  }
};

export default awsConfig;
```

Cài đặt thư viện cần thiết cho Cognito và Amplify UI:

```bash
npm install aws-amplify @aws-amplify/ui-react
```

Trong file `src/App.js`, ứng dụng sử dụng `Authenticator` từ Amplify UI để hiển thị màn hình đăng nhập/đăng ký trước khi người dùng truy cập vào dashboard quản lý hóa đơn.

---

#### Bước 5: Chạy ứng dụng ở môi trường local

Chạy ứng dụng React:

```bash
npm start
```

Ứng dụng sẽ chạy tại:

```txt
http://localhost:3000
```

Khi mở ứng dụng, màn hình đăng nhập Cognito sẽ xuất hiện trước.

Người dùng có thể đăng ký tài khoản mới hoặc đăng nhập bằng tài khoản đã có.

Sau khi đăng nhập thành công, giao diện quản lý hóa đơn sẽ được hiển thị.

Kiểm tra các chức năng chính:

* Tải tệp hóa đơn.
* Kéo - thả tệp hóa đơn.
* Xem tất cả hóa đơn.
* Xem chi tiết hóa đơn.
* Tra cứu hóa đơn theo Invoice ID.
* Tra cứu hóa đơn theo tên khách hàng.
* Xuất dữ liệu hóa đơn ra Excel.
* Lọc hóa đơn theo ngày.
* Sắp xếp hóa đơn theo tổng tiền.
* Sắp xếp hóa đơn theo ngày hóa đơn.
* Xem tags của hóa đơn.
* Thêm tags hóa đơn.
* Chỉnh sửa tags hóa đơn.
* Lọc hóa đơn theo tags.
* Đánh dấu hóa đơn quan trọng.

{{% notice info %}}
Lệnh `npm start` chỉ dùng để chạy thử ở môi trường local. Khi triển khai thật, frontend sẽ được build và host bằng AWS Amplify Hosting.
{{% /notice %}}

---

#### Bước 6: Đưa source code lên GitHub

Để triển khai frontend mà không cần chạy server local liên tục, cần đưa source code React lên GitHub.

Khởi tạo Git trong thư mục dự án:

```bash
git init
```

Thêm toàn bộ file vào Git:

```bash
git add .
```

Commit source code:

```bash
git commit -m "Deploy invoice scanner frontend"
```

Đổi nhánh chính thành `main`:

```bash
git branch -M main
```

Thêm remote repository GitHub:

```bash
git remote add origin <your_github_repository_url>
```

Đẩy source code lên GitHub:

```bash
git push -u origin main
```

---

#### Bước 7: Triển khai frontend bằng AWS Amplify Hosting

Truy cập AWS Console:

```txt
AWS Console → AWS Amplify → Create new app → Host web app
```

Chọn:

```txt
GitHub
```

Sau đó chọn:

* Repository: repository frontend của bạn.
* Branch: `main`.

Amplify sẽ tự động nhận diện dự án React. Nếu cần cấu hình build thủ công, sử dụng cấu hình sau:

```yaml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - npm install
    build:
      commands:
        - npm run build
  artifacts:
    baseDirectory: build
    files:
      - "**/*"
  cache:
    paths:
      - node_modules/**/*
```

Sau đó chọn:

```txt
Save and deploy
```

Sau khi triển khai xong, AWS Amplify sẽ cung cấp một đường dẫn public tương tự:

```txt
https://main.xxxxx.amplifyapp.com
```

Đây là URL frontend sau khi deploy.

---

#### Bước 8: Thêm biến môi trường trong Amplify Hosting

Trong AWS Amplify Console, mở ứng dụng vừa deploy.

Truy cập:

```txt
App settings → Environment variables
```

Thêm các biến môi trường giống như trong file `.env` local:

```env
REACT_APP_SEND_AUTH_TOKEN=false

REACT_APP_AWS_REGION=ap-southeast-1

REACT_APP_USER_POOL_ID=ap-southeast-1_xxxxxxxxx
REACT_APP_USER_POOL_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx

REACT_APP_API_UPLOAD_URL=https://<PostInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/uploads

REACT_APP_API_INVOICE_URL=https://<GetInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/invoice

REACT_APP_API_UPDATE_TAGS_URL=https://<GetInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/invoice/tags

REACT_APP_API_UPDATE_STARRED_URL=https://<GetInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/invoice/starred
```

Sau khi thêm hoặc chỉnh sửa biến môi trường, cần redeploy lại ứng dụng.

{{% notice warning %}}
Các biến môi trường của React được đưa vào ứng dụng tại thời điểm build. Vì vậy, sau khi thay đổi biến môi trường trong Amplify Hosting, bạn phải redeploy ứng dụng để giá trị mới có hiệu lực.
{{% /notice %}}

---

#### Bước 9: Cập nhật Callback URL và Sign-out URL trong Cognito

Sau khi AWS Amplify Hosting deploy xong, sao chép URL frontend.

Ví dụ:

```txt
https://main.xxxxx.amplifyapp.com/
```

Quay lại Amazon Cognito:

```txt
Amazon Cognito → User pools → invoice-scanner-user-pool → App client
```

Thêm URL Amplify Hosting vào:

```txt
Allowed callback URLs
```

và:

```txt
Allowed sign-out URLs
```

Ví dụ:

```txt
http://localhost:3000/
https://main.xxxxx.amplifyapp.com/
```

Việc này giúp Cognito hoạt động được cả khi chạy local và khi chạy trên website đã deploy bằng Amplify Hosting.

---

#### Bước 10: Kiểm tra frontend sau khi deploy

Mở URL frontend đã deploy:

```txt
https://main.xxxxx.amplifyapp.com
```

Đăng nhập bằng Cognito.

Sau đó kiểm tra lại toàn bộ chức năng:

* Tải tệp hóa đơn.
* Kéo - thả tệp hóa đơn.
* Xem chi tiết hóa đơn.
* Xem tất cả hóa đơn.
* Tra cứu hóa đơn theo ID.
* Tra cứu hóa đơn theo tên khách hàng.
* Xuất dữ liệu hóa đơn ra Excel.
* Lọc hóa đơn theo ngày.
* Sắp xếp hóa đơn theo tổng tiền.
* Sắp xếp hóa đơn theo ngày hóa đơn.
* Xem tags của hóa đơn.
* Thêm tags hóa đơn.
* Chỉnh sửa tags hóa đơn.
* Lọc hóa đơn theo tags.
* Đánh dấu hóa đơn quan trọng.

---

### Xử lý lỗi thường gặp

#### Upload hóa đơn thất bại

Nếu upload hóa đơn bị lỗi, kiểm tra các phần sau:

* `REACT_APP_API_UPLOAD_URL` có đúng API Gateway endpoint hay không.
* API Gateway có route:

```txt
POST /uploads
```

* Lambda upload có trả CORS headers hay không.
* API Gateway có hỗ trợ method `OPTIONS` cho CORS preflight hay không.
* Request body phải có dạng:

```json
{
  "filename": "invoice.png",
  "file": "base64_string_here",
  "contentType": "image/png"
}
```

#### Lỗi Missing Authentication Token

Nếu API Gateway trả về:

```json
{
  "message": "Missing Authentication Token"
}
```

Cần kiểm tra:

* HTTP method có đúng không.
* Đường dẫn API có đúng không.
* Stage của API Gateway có đúng không.
* API đã được deploy lại sau khi chỉnh route hay chưa.

#### Cognito đăng nhập được ở local nhưng lỗi khi deploy

Kiểm tra:

* Đã thêm URL Amplify Hosting vào Cognito callback URLs chưa.
* Đã thêm URL Amplify Hosting vào Cognito sign-out URLs chưa.
* Đã thêm đầy đủ environment variables trong Amplify Hosting chưa.
* Đã redeploy sau khi cập nhật environment variables chưa.

#### API chạy được trong Postman nhưng lỗi trên trình duyệt

Nguyên nhân thường gặp là do CORS.

Đảm bảo Lambda hoặc API Gateway trả về headers:

```json
{
  "Access-Control-Allow-Origin": "*",
  "Access-Control-Allow-Headers": "Content-Type,Authorization",
  "Access-Control-Allow-Methods": "OPTIONS,GET,POST,PATCH"
}
```

---

### Kết quả

Sau khi hoàn thành bước này, frontend đã được triển khai thành công trên AWS Amplify Hosting.

Người dùng có thể truy cập hệ thống thông qua URL public mà không cần chạy `npm start` trên máy cá nhân.

Frontend kết nối với các dịch vụ:

* Amazon Cognito để xác thực người dùng.
* Amazon API Gateway để gọi các REST API.
* AWS Lambda để xử lý logic backend.
* Amazon S3 để lưu trữ file hóa đơn.
* Amazon Textract và OpenAI API để trích xuất văn bản và chuẩn hóa dữ liệu hóa đơn.
* Amazon DynamoDB để lưu trữ và truy vấn dữ liệu hóa đơn.

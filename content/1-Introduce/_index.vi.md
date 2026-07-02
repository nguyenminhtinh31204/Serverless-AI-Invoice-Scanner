---

title: "Giới thiệu"
weight: 1
chapter: false
pre: " <b> 1. </b> "
--------------------

#### Giới thiệu

Trong bài lab này, bạn sẽ xây dựng hệ thống **Serverless AI Invoice Scanner** sử dụng các dịch vụ AWS kết hợp với API AI bên ngoài. Hệ thống cho phép người dùng tải lên các tệp hóa đơn, trích xuất nội dung từ tài liệu, chuẩn hóa thông tin hóa đơn, lưu trữ dữ liệu và quản lý hóa đơn thông qua giao diện web.

Giải pháp bao gồm các thành phần chính sau:

* **AWS Amplify Hosting** – dùng để host và triển khai ứng dụng frontend React.
* **Amazon Cognito** – dùng để xử lý đăng ký, đăng nhập và xác thực người dùng ở phía frontend.
* **Amazon API Gateway** – dùng để cung cấp các REST API cho chức năng upload hóa đơn, lấy dữ liệu hóa đơn, cập nhật tags và đánh dấu hóa đơn quan trọng.
* **AWS Lambda** – dùng để xử lý các yêu cầu từ API Gateway, upload tệp hóa đơn, xử lý tài liệu và quản lý dữ liệu hóa đơn.
* **Amazon S3** – dùng để lưu trữ các tệp hóa đơn được tải lên như PDF, PNG, JPG và JPEG.
* **Amazon Textract** – dùng để trích xuất văn bản từ tài liệu hóa đơn bằng công nghệ OCR.
* **OpenAI API** – dùng để chuẩn hóa và cấu trúc dữ liệu hóa đơn đã trích xuất, chẳng hạn như tên khách hàng, số hóa đơn, ngày hóa đơn, tổng tiền và loại tiền tệ.
* **Amazon DynamoDB** – dùng để lưu trữ thông tin hóa đơn, dữ liệu đã trích xuất, tags, trạng thái đánh dấu quan trọng và trạng thái xử lý.
* **Amazon CloudWatch** – dùng để theo dõi log của Lambda, lỗi API và hỗ trợ quá trình kiểm tra, gỡ lỗi hệ thống.

{{% notice info %}}
Trong dự án này, Amazon Bedrock đã được thay thế bằng OpenAI API để thực hiện bước chuẩn hóa dữ liệu bằng AI. OpenAI API Key được lưu ở phía backend, ví dụ trong biến môi trường của Lambda hoặc AWS Secrets Manager, và không được đưa vào frontend.
{{% /notice %}}

#### Mục tiêu học tập

Sau khi hoàn thành bài lab này, bạn sẽ có kinh nghiệm thực hành trong các nội dung sau:

* Thiết kế và xây dựng kiến trúc xử lý hóa đơn theo mô hình serverless.
* Triển khai ứng dụng frontend React bằng AWS Amplify Hosting.
* Cấu hình Amazon Cognito để xác thực người dùng.
* Xây dựng REST API bằng Amazon API Gateway và AWS Lambda.
* Upload và lưu trữ tệp hóa đơn trên Amazon S3.
* Sử dụng Amazon Textract để trích xuất văn bản từ tài liệu hóa đơn.
* Tích hợp OpenAI API để chuẩn hóa và cấu trúc dữ liệu hóa đơn đã trích xuất.
* Lưu trữ và truy vấn dữ liệu hóa đơn bằng Amazon DynamoDB.
* Xây dựng các chức năng quản lý hóa đơn như xem danh sách, xem chi tiết, tìm kiếm, thêm tags, chỉnh sửa tags, lọc, sắp xếp, đánh dấu hóa đơn quan trọng và xuất dữ liệu ra Excel.
* Theo dõi và xử lý lỗi backend thông qua Amazon CloudWatch.
* Hiểu cách hoạt động của CORS, API Gateway routes, quyền Lambda và biến môi trường trong một ứng dụng serverless.

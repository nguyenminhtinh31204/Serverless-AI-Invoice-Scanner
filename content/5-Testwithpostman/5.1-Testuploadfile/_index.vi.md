---
title: "Kiểm thử tải tệp hóa đơn"
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

#### Yêu cầu chuẩn bị

-   Đã cài đặt Postman ([https://www.postman.com/downloads](https://www.postman.com/downloads)).

---

#### Chuẩn bị tài nguyên

Tải các tệp dưới đây trước khi kiểm thử API trên Postman:

-   [demo_invoice.png](https://drive.google.com/uc?export=download&id=1p_sRXNN9saZLPYJNATZNqV-NmydsUo3L)
-   [demo_invoice2.png](https://drive.google.com/uc?export=download&id=1D-5nvOvdM2Fo2hBTDICTgE8xY-kg7vvY)
-   [demo_invoice3.png](https://drive.google.com/uc?export=download&id=1h_4q-1Xy-MSuwAqXW7D4oTTsnjdn0USh)

---

#### Bước 1: Convert hình ảnh sang Base64

Chúng ta sẽ dùng công cụ online [base64-image.de](https://www.base64-image.de/):

1. Truy cập trang web và chọn file **demo_invoice.png**.

![Website base64-image.de](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/001-websitebase64.png)

2. Trang web sẽ tự động convert sang Base64.

![Convert to Base64](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/002-convert-to-base64.png)

3. Nhấn **</> show code** để lấy Base64.

![Click show code](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/003-click-show-code.png)

4. Sao chép đoạn mã Base64.

![Copy Base64](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/004-copy-base64.png)

5. Lưu vào Notepad tạm.

6. Tiếp tục lặp lại cho 2 file còn lại.

---

#### Bước 2: Tạo collection trong Postman

1.  Mở ứng dụng Postman.

![Postman](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/005-postman.png)

2. Nhấn vào dấu **"+"** để tạo collection.

![Create new collection](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/006-create-new-collection.png)

3.  Nhấn **Blank collection**.

![Blank collection](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/007-blank-collection.png)

4. Đặt tên: `InvoiceUploadAPI-Tests`

![Rename collection](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/008-rename-collection.png)

---

#### Bước 3: Tạo request

1. Trong Collection vừa tạo, nhấn dấu **"+"** để tạo request.

![Create request](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/009-create-request.png)

2. Đặt tên: `Upload Invoice`.

![Rename request](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/010-rename-request.png)

3. Chọn method **POST**.

![Choose method post](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/011-choose-method-post.png)

5. Truy cập API Gateway, chọn API: `PostInvoiceAPI`.

![PostInvoiceAPI](/images/4.deployingapigatewayandfrontend/4.4/image.png)

6. Chọn mục **Stages**.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(1).png>)

7. Bấm dấu **“+”** để mở ra đường dẫn đầy đủ.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(2).png>)

8. Chọn phương thức **POST** và sao chép **Invoke URL**.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(3).png>)

9. Dán **Invoke URL** vừa sao chép vào trong Postman như sau:

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(4).png>)

10. Chọn tab **Headers**, cấu hình như sau:

```bash
Content-Type: application/json
```

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(5).png>)

11. Chọn tab **Body** → chọn **raw** → chọn **JSON**.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(6).png>)

12. Dán đoạn **JSON** vào Postman:

```json
{
    "file": "",
    "filename": ""
}
```

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(7).png>)

13. Dán Base64 đã lưu tạm trong Notepad và viết tên file vào như sau:

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(8).png>)

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(9).png>)

14. Nhấn nút **Send** để xem kết quả.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(10).png>)

15. Kết quả trả về thành công như sau:

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(11).png>)

16. Truy cập vào **S3** → Vào thư mục `uploads/` → Xác nhận file đã được upload.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(12).png>)

17. Truy cập vào **DynamoDB** → Chọn **Explore items** → Chọn bảng **InvoiceData**.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(13).png>)

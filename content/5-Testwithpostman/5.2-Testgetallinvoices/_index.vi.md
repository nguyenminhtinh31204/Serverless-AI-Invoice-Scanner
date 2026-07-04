---
title: "Kiểm thử lấy tất cả hóa đơn"
weight: 2
chapter: false
pre: " <b> 5.2 </b> "
---

#### Yêu cầu chuẩn bị

> ⚠️ Vừa rồi chỉ hướng dẫn tải một tệp "demo_invoice.png". Bạn hãy **tải thêm 2 tệp hóa đơn** còn lại nữa nhé!

---

#### Bước 1: Tạo collection trong Postman

1.  Mở ứng dụng Postman và nhấn dấu **"+"** để tạo collection.

![Test get all invoices](/images/5/5.2/001.png)

2. Đặt tên: `InvoiceGetAPI-Tests`

![Test get all invoices](/images/5/5.2/002.png)

---

#### Bước 2: Tạo request

1. Trong Collection vừa tạo, nhấn dấu **"+"** để tạo request.

![Test get all invoices](/images/5/5.2/003.png)

2. Đặt tên: `Get All Invoices`.

![Test get all invoices](/images/5/5.2/004.png)

3. Chọn method **GET**.

![Test get all invoices](/images/5/5.2/005.png)

5. Truy cập API Gateway, chọn API: `GetInvoiceAPI`.

![Test get all invoices](/images/5/5.2/006.png)

6. Chọn mục **Stages**.

![Test get all invoices](/images/5/5.2/007.png)

7. Bấm dấu **“+”** để mở ra đường dẫn đến `/invoice` như dưới đây:

![Test get all invoices](/images/5/5.2/008.png)

8. Chọn phương thức **GET** và sao chép **Invoke URL**.

![Test get all invoices](/images/5/5.2/009.png)

9. Dán **Invoke URL** vừa sao chép vào trong Postman như sau:

![Test get all invoices](/images/5/5.2/010.png)

10. Nhấn nút **Send** để xem kết quả.

![Test get all invoices](/images/5/5.2/011.png)

14. Kết quả trả về thành công như sau:

![Test get all invoices](/images/5/5.2/012.png)

> Kiểm tra nếu đủ 3 hóa đơn tức là thành công ✅

---
title: "Kiểm thử lấy danh sách hóa đơn đánh dấu"
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

#### Bước 1: Tạo request

1. Trong Collection **InvoiceGetAPI-Tests**, nhấn dấu **"+"** để tạo request.

![Get All Invoices Starred](/images/5/5.4/1.png)

2. Đặt tên: `Get All Invoices Starred`.

![Get All Invoices Starred](/images/5/5.4/2.png)

3. Chọn method **GET**.

![Get All Invoices Starred](/images/5/5.4/3.png)

5. Truy cập API Gateway, chọn API: `GetInvoiceAPI`.

6. Chọn mục **Stages**.

7. Bấm dấu **“+”** để mở ra đường dẫn đến `/invoice/starred` như dưới đây:

![Get All Invoices Starred](/images/5/5.4/4.png)

8. Chọn phương thức **GET** và sao chép **Invoke URL**.

![Get All Invoices Starred](/images/5/5.4/5.png)

9. Dán **Invoke URL** vừa sao chép vào trong Postman như sau:

![Get All Invoices Starred](/images/5/5.4/6.png)

10. Nhấn nút **Send** để xem kết quả.

![Get All Invoices Starred](/images/5/5.4/7.png)

11. Kết quả trả về như sau:

![Get All Invoices Starred](/images/5/5.4/8.png)

> Khi trả về kết quả `[]` tức là hiện tại chưa có hóa đơn nào đánh dấu

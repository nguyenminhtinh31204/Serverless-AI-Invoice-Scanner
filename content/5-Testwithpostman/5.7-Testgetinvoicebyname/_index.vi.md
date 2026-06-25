---
title: "Kiểm thử lấy hóa đơn theo tên khách hàng"
weight: 7
chapter: false
pre: " <b> 5.7 </b> "
---

#### Bước 1: Tạo request

1. Trong Collection **InvoiceGetAPI-Tests**, nhấn dấu **"+"** để tạo request.

![Get Invoices By Name](/images/5/5.7/Screenshot_1.png)

2. Đặt tên: `Get Invoices By Name`.

![Get Invoices By Name](/images/5/5.7/Screenshot_2.png)

3. Chọn method **GET**.

![Get Invoices By Name](/images/5/5.7/Screenshot_3.png)

5. Truy cập API Gateway, chọn API: `GetInvoiceAPI`.

6. Chọn mục **Stages**.

7. Bấm dấu **“+”** để mở ra đường dẫn đến `/invoice` như dưới đây:

![Get Invoices By Name](/images/5/5.7/Screenshot_4.png)

8. Chọn phương thức **GET** và sao chép **Invoke URL**.

![Get Invoices By Name](/images/5/5.7/Screenshot_5.png)

9. Dán **Invoke URL** vừa sao chép vào trong Postman như sau:

![Get Invoices By Name](/images/5/5.7/Screenshot_6.png)

10. Thêm đoạn dưới đây vào phía sau API endpoint:

```bash
?name=<ten_khach_hang>
```

11. Nhập tên khách hàng cần tìm trong hóa đơn.

![Get Invoices By Name](/images/5/5.7/Screenshot_7.png)

10. Nhấn nút **Send** để xem kết quả.

![Get Invoices By Name](/images/5/5.7/Screenshot_8.png)

11. Kết quả trả về như sau:

![Get Invoices By Name](/images/5/5.7/Screenshot_9.png)

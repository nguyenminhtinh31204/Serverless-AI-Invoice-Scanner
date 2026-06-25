---
title: "Kiểm thử lấy invoice theo ID"
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

#### Bước 1: Tạo request

1. Trong Collection **InvoiceGetAPI-Tests**, nhấn dấu **"+"** để tạo request.

![Test get all invoices by ID](/images/5/5.3/Screenshot_1.png)

2. Đặt tên: `Get Invoices By ID`.

![Test get all invoices by ID](/images/5/5.3/Screenshot_2.png)

3. Chọn method **GET**.

![Test get all invoices by ID](/images/5/5.3/Screenshot_3.png)

5. Truy cập API Gateway, chọn API: `GetInvoiceAPI`.

6. Chọn mục **Stages**.

7. Bấm dấu **“+”** để mở ra đường dẫn đến `/invoice/{id}` như dưới đây:

![Test get all invoices by ID](/images/5/5.3/Screenshot_4.png)

8. Chọn phương thức **GET** và sao chép **Invoke URL**.

![Test get all invoices by ID](/images/5/5.3/Screenshot_5.png)

9. Dán **Invoke URL** vừa sao chép vào trong Postman như sau:

![Test get all invoices by ID](/images/5/5.3/Screenshot_6.png)

10. Xóa `{id}` trong API endpoint của Postman và thay bằng ID Invoice trong DynamoDB:

```bash
https://x4uqolxky6.execute-api.us-east-1.amazonaws.com/dev/invoice/<InvoiceId_trong_DynamoDB>
```

![Test get all invoices by ID](/images/5/5.3/Screenshot_7.png)

![Test get all invoices by ID](/images/5/5.3/Screenshot_8.png)

11. Nhấn nút **Send** để xem kết quả.

![Test get all invoices by ID](/images/5/5.3/Screenshot_9.png)

14. Kết quả trả về thành công như sau:

![Test get all invoices by ID](/images/5/5.3/Screenshot_10.png)

> Bạn có thể kiểm tra 2 tệp hóa đơn còn lại!

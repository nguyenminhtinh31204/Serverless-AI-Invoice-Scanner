---
title: "Kiểm thử cập nhật trạng thái đánh dấu hóa đơn"
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---

#### Bước 1: Tạo request

1. Trong Collection **InvoiceGetAPI-Tests**, nhấn dấu **"+"** để tạo request.

![Update Invoice Starred](/images/5/5.5/Screenshot_1.png)

2. Đặt tên: `Update Invoice Starred`.

![Update Invoice Starred](/images/5/5.5/Screenshot_2.png)

3. Chọn method **PATCH**.

![Update Invoice Starred](/images/5/5.5/Screenshot_3.png)

4. Truy cập API Gateway, chọn API: `GetInvoiceAPI`.

5. Chọn mục **Stages**.

6. Bấm dấu **“+”** để mở ra đường dẫn đến `/invoice/starred/{id}` như dưới đây:

![Update Invoice Starred](/images/5/5.5/Screenshot_4.png)

8. Chọn phương thức **PATCH** và sao chép **Invoke URL**.

![Update Invoice Starred](/images/5/5.5/Screenshot_5.png)

9. Dán **Invoke URL** vừa sao chép vào trong Postman như sau:

![Update Invoice Starred](/images/5/5.5/Screenshot_6.png)

10. Xóa `{id}` trong API endpoint của Postman và thay bằng ID Invoice trong DynamoDB:

```bash
https://x4uqolxky6.execute-api.us-east-1.amazonaws.com/dev/invoice/starred/<InvoiceId_trong_DynamoDB>
```

![Update Invoice Starred](/images/5/5.5/Screenshot_7.png)

11. Chọn tab **Body** → chọn **raw** → chọn **JSON**.

![Update Invoice Starred](/images/5/5.5/Screenshot_8.png)

12. Dán đoạn mã **JSON** sau vào Postman để đánh dấu hóa đơn:

```json
{
    "starred": true
}
```

![Update Invoice Starred](/images/5/5.5/Screenshot_9.png)

13. Nhấn nút **Send** để xem kết quả.

![Update Invoice Starred](/images/5/5.5/Screenshot_10.png)

14. Kết quả trả về như sau:

![Update Invoice Starred](/images/5/5.5/Screenshot_11.png)

15. Truy cập vào **DynamoDB** để kiểm tra trường `Starred`.

![Update Invoice Starred](/images/5/5.5/Screenshot_12.png)

---
title: "Kiểm thử cập nhật danh mục hóa đơn"
weight: 6
chapter: false
pre: " <b> 5.6 </b> "
---

#### Bước 1: Tạo request

1. Trong Collection **InvoiceGetAPI-Tests**, nhấn dấu **"+"** để tạo request.

![Update Invoice Tags](/images/5/5.6/Screenshot_1.png)

2. Đặt tên: `Update Invoice Tags`.

![Update Invoice Tags](/images/5/5.6/Screenshot_2.png)

3. Chọn method **PATCH**.

![Update Invoice Tags](/images/5/5.6/Screenshot_3.png)

4. Truy cập API Gateway, chọn API: `GetInvoiceAPI`.

5. Chọn mục **Stages**.

6. Bấm dấu **“+”** để mở ra đường dẫn đến `/invoice/tags/{id}` như dưới đây:

![Update Invoice Tags](/images/5/5.6/Screenshot_4.png)

8. Chọn phương thức **PATCH** và sao chép **Invoke URL**.

![Update Invoice Tags](/images/5/5.6/Screenshot_5.png)

9. Dán **Invoke URL** vừa sao chép vào trong Postman như sau:

![Update Invoice Tags](/images/5/5.6/Screenshot_6.png)

10. Xóa `{id}` trong API endpoint của Postman và thay bằng ID Invoice trong DynamoDB:

```bash
https://x4uqolxky6.execute-api.us-east-1.amazonaws.com/dev/invoice/tags/<InvoiceId_trong_DynamoDB>
```

![Update Invoice Tags](/images/5/5.6/Screenshot_7.png)

11. Chọn tab **Body** → chọn **raw** → chọn **JSON**.

![Update Invoice Tags](/images/5/5.6/Screenshot_8.png)

12. Dán đoạn mã **JSON** sau vào Postman để đánh dấu hóa đơn:

```json
{
    "tags": ["VIP", "Urgent"]
}
```

13. Nhấn nút **Send** để xem kết quả.

![Update Invoice Tags](/images/5/5.6/Screenshot_11.png)

14. Kết quả trả về như sau:

![Update Invoice Tags](/images/5/5.6/Screenshot_12.png)

15. Truy cập vào **DynamoDB** để kiểm tra trường `Tags`.

![Update Invoice Tags](/images/5/5.6/Screenshot_13.png)

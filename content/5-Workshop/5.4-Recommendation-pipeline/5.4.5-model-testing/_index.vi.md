---
title: "Kiểm thử mô hình gợi ý"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.4.5. </b> "
---

# Kiểm thử mô hình gợi ý

{{% notice info %}}
Web xem phim của nhóm chưa có nhiều dữ liệu lịch sử tương tác của người dùng như `like`, `watch progress`, `share`, ... Do đó, việc kiểm thử mô hình gợi ý dựa trên dữ liệu lịch sử của người dùng sẽ được thực hiện bằng cách tạo ra các dữ liệu giả lập đối với `user_id` trong tập dữ liệu có sẵn. Các `user_id` này đã có lịch sử tương tác `rating` với các bộ phim.
{{% /notice %}}

## Tính năng cần kiểm thử
- Gợi ý 5 phim tương đồng theo bộ phim mà người dùng nhập - **Đối với tất cả loại người dùng**.
- Gợi ý phim **Top-Rated** - **Đối với người dùng chưa đăng nhập**.
- Gợi ý phim dựa trên lịch sử tương tác của người dùng với các bộ phim - **Đối với người dùng đã đăng nhập và có lịch sử tương tác**.
- Gợi ý phim cho người dùng mới đăng nhập bằng cách khảo sát thể loại yêu thích - **Đối với người dùng mới đăng nhập và chưa có lịch sử tương tác**.

![test model 1](/images/5-Workshop/5.4.5-model-testing/test-model-1.png)

> Đây là demo kiểm thử hệ thống gợi ý phim

Khi đăng nhập bằng `user_id`, sẽ có 3 loại người dùng:

- **Người dùng chưa đăng nhập - Guest:** Bỏ trống, không nhập `user_id`.  
- **Người dùng mới đăng nhập và chưa có lịch sử tương tác - New user:** Nhập `user_id` từ id 270897 trở lên. Do đã có sẵn dữ liệu lịch sử rating phim của 270896 người dùng trước đó.
- **Người dùng đã đăng nhập và có lịch sử tương tác - Returning user:** Nhập `user_id` từ id 1 đến 270896. Do đã có sẵn dữ liệu lịch sử rating phim của 270896 người dùng trước đó. 

## Guest
### Gợi ý phim tương đồng theo phim mà người dùng nhập

![guest 1](/images/5-Workshop/5.4.5-model-testing/guest-1.png)

Người dùng nhập tên phim cần tìm kiếm, hệ thống sẽ hiển thị danh sách kết quả tìm kiếm. Sau đó, người dùng chọn một bộ phim cụ thể trong danh sách để hệ thống gợi ý 5 bộ phim tương đồng.

![guest 2](/images/5-Workshop/5.4.5-model-testing/guest-2.png)

{{% notice note %}} 
Tuy là người dùng khách nhưng vẫn có thể dùng kịch bản của `returning_user`. Vì hệ thống đã chọn mô hình **Content-based** (do người dùng đang là khách) không cần dữ liệu lịch sử tương tác của người dùng mà chỉ dựa vào nội dung, tên phim mà đã tìm kiếm.
{{% /notice %}}

### Gợi ý phim **Top-Rated**

![guest 3](/images/5-Workshop/5.4.5-model-testing/guest-3.png)

Mô hình gợi ý dựa trên **Collaborative Filtering** sẽ được sử dụng để gợi ý phim **Top-Rated** cho người dùng chưa đăng nhập.

## New user

{{% notice note %}}
Đối với người dùng mới đăng nhập, cũng có tính năng [**Gợi ý phim tương đồng theo phim mà người dùng nhập**](#gợi-ý-phim-tương-đồng-theo-phim-mà-người-dùng-nhập) như người dùng khách.
{{% /notice %}}

### Gợi ý phim theo thể loại yêu thích

![new user 1](/images/5-Workshop/5.4.5-model-testing/new-user-1.png)

Người dùng mới đăng nhập sẽ được khảo sát thể loại yêu thích. Sau khi chọn thể loại, hệ thống sẽ gợi ý các bộ phim thuộc thể loại đó.

**Ví dụ:** chọn thể loại `Music`, `Romance`, `Family`

![new user 2](/images/5-Workshop/5.4.5-model-testing/new-user-2.png)

{{% notice note %}} 
Do người dùng đã chọn được thể loại yêu thích nên tính năng **Gợi ý phim dựa trên lịch sử tương tác** sẽ đưa ra kết quả tương tự như **Gợi ý phim theo thể loại yêu thích**. 
{{% /notice %}}

## Returning user

{{% notice note %}}
Đối với người dùng đã đăng nhập, cũng có tính năng [**Gợi ý phim tương đồng theo phim mà người dùng nhập**](#gợi-ý-phim-tương-đồng-theo-phim-mà-người-dùng-nhập) như người dùng khách.
{{% /notice %}}

### Gợi ý phim dựa trên lịch sử tương tác

**Ví dụ:** `user_id = 1`. Trước hết, xem qua lịch sử tương tác `rating` có sẵn của người dùng này.

![return user 1](/images/5-Workshop/5.4.5-model-testing/return-user-1.png)

Sau đó, dùng tính năng **Gợi ý phim dựa trên lịch sử tương tác** để gợi ý các bộ phim dựa trên lịch sử `rating` trên.

![return user 2](/images/5-Workshop/5.4.5-model-testing/return-user-2.png)

Danh sách phim gợi ý có nhiều thể loại phổ biến như `Action`, `Adventure`, `Fantasy`, `Family`. Kiểm tra nếu người dùng tương tác nhiều với các phim thuộc thể loại `Music`, `Romance` thì danh sách có thay đổi không.

Tiến hành nạp một tập dữ liệu giả lập bao gồm các tương tác mới nhất của người dùng này. Cụ thể, người dùng đã thực hiện các hành vi có trọng số cao `rating: 5.0`, `watch: 1.0` đối với các bộ phim thuộc thể loại `Music` và `Romance`, đồng thời thực hiện một lượt `click` vào bộ phim **Harry Potter and the Half-Blood Prince** (thuộc thể loại `Adventure`, `Fantasy`) đã từng xem trong quá khứ.

![return user 3](/images/5-Workshop/5.4.5-model-testing/return-user-3.png)

Danh sách phim sau khi mô hình đưa ra gợi ý:

![return user 4](/images/5-Workshop/5.4.5-model-testing/return-user-4.png)

- **Bảo toàn sở thích dài hạn:** Các vị trí Top 1-4 đều thuộc về chuỗi phim **Harry Potter**. Hệ thống nhận diện sự liên kết giữa cú click ngắn hạn hiện tại và lịch sử đánh giá 5 sao trong quá khứ, từ đó đẩy các phim cùng series lên đầu. Điều này chứng minh hệ thống không bị mất trí nhớ khi người dùng có sở thích mới.

- **Thỏa hiệp với tương tác ngắn hạn:** Tại các vị trí kế tiếp, mô hình bắt đầu chèn vào các bộ phim mang đặc trưng `Music` và `Romance`. Hệ thống đã thành công trong việc mở rộng không gian gợi ý, đáp ứng tức thời nhu cầu mới phát sinh của người dùng mà không cần chờ huấn luyện lại toàn bộ mô hình.
---
title: "Bản đề xuất"
date: 2026--01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Movie Recommendation System
## Hệ thống đề xuất phim dựa trên dữ liệu người dùng sử dụng

### 1. Tóm tắt điều hành
Movie Recommendation System là một hệ thống gợi ý phim dựa trên dữ liệu người dùng, nhằm cung cấp trải nghiệm cá nhân hóa cho người dùng. Hệ thống sử dụng kết hợp Content-Base Filtering và Collaborative Filtering.

Duy trì các máy chủ Machine Learning chạy tính toán theo thời gian thực cực kỳ tốn kém. Để giải quyết vấn đề trên, toàn bộ quá trình huấn luyện mô hình, chấm điểm phim sẽ được xử lý ngầm định kỳ qua các SageMaker Processing Job và EC2. Kết quả được lưu trữ trên Amazon S3 và Amazon DynamoDB. Backend FastAPI chỉ việc nạp dữ liệu này vào bộ nhớ lúc khởi động để phục vụ người dùng tức thì.

### 2. Tuyên bố vấn đề
_Vấn đề hiện tại_
Hiện tại, các dịch vụ xem phim trực tuyến phát triển bùng nổ, số lượng phim và chương trình truyền hình sẵn có đã lên tới hàng chục, hàng trăm nghìn tác phẩm.

Vấn đề cốt lõi: 
- Khi có quá nhiều sự lựa chọn, người dùng thường bị quá tải thông tin và mất quá nhiều thời gian (trung bình 15–20 phút) chỉ để duyệt qua danh sách phim mà không chọn được phim phù hợp.

- Việc tìm kiếm thủ công phim và chương trình dựa theo từ khóa hay thể loại không phản ánh đúng sở thích đa dạng và thay đổi theo thời gian của người dùng dần đến trải nghiệm người dùng bị kém đi.

- Người dùng dễ chán nản và rời khỏi ứng dụng nếu không tìm thấy nội dung hấp dẫn trong vài phút đầu tiên.

Do đó, các dịch vụ xem phim cần một hệ thống gợi ý thông minh có khả năng thấu hiểu hành vi và sở thích của người dùng, từ đó cung cấp các đề xuất cá nhân hóa, giúp người dùng tiết kiệm thời gian và nâng cao trải nghiệm xem phim.

_Giải pháp_
Hệ thống sử dụng các thuật toán Machine Learning để thu thập, phân tích vết hành vi của người dùng, từ đó tìm ra các mẫu ẩn về sở thích và sự tương đồng giữa các nội dung. Nhằm đảm bảo khả năng mở rộng khi lượng người dùng và dữ liệu tăng cao, hệ thống không thể chỉ chạy trên một máy chủ cục bộ mà cần tận dụng sức mạnh của hạ tầng điện toán đám mây. Bằng cách kết hợp các dịch vụ của AWS, hệ thống có thể lưu trữ lượng lớn dữ liệu tương tác, lên lịch huấn luyện mô hình định kỳ, và phân phối kết quả gợi ý đến ứng dụng web với tốc độ cao.

### 3. Kiến trúc giải pháp

_Dịch vụ AWS sử dụng_
- **Amazon SageMaker**: Để huấn luyện và triển khai mô hình Machine Learning.
- **Amazon EC2**: Để chạy các tác vụ xử lý dữ liệu và tính toán.
- **Amazon S3**: Để lưu trữ dữ liệu lớn và kết quả mô hình.
- **Amazon DynamoDB**: Để lưu trữ dữ liệu cấu trúc với độ trễ thấp.

_Thiết kế thành phần_
- 

### 4. Triển khai kỹ thuật
_Các giai đoạn triển khai_
Dự án gồm 2 phần được triển khai song song: xây dựng Web xem phim và xây dựng mô hình Machine Learning gợi ý phim.

1. **Khởi tạo hạ tầng và Chuẩn bị dữ liệu:** Thiết lập nền tảng cơ bản cho cả hai phần Web và Machine Learning, chốt schema dữ liệu và xử lý xong nguồn dữ liệu thô.
    - _Phần Web:_
        - Thiết lập cấu trúc dự án, cấu hình môi trường Docker và các CI/CD cơ bản.  
        - Dựng khung Backend FastAPI, thiết kế schema cho DynamoDB và tạo các bảng trên AWS.  
        - Thiết kế UI/UX cơ bản, dựng luồng Đăng ký/Đăng nhập và Onboarding trên Vite. 
    - _Phần Machine Learning:_
        - Tiền xử lý tập The Movies Dataset.
        - Phát triển Data Pipeline xuất dữ liệu làm sạch ra các tập Train/Validation/Test.

2. **Tính toán chi phí:** Sử dụng AWS Pricing Calculator để ước tính và điều chỉnh

3. **Phát triển tính năng:** Hoàn thiện các tính năng, xây dựng kịch bản người dùng trên Web và huấn luyện thành công các mô hình gợi ý đầu tiên.
    - _Phần Web:_
        - Phát triển trang chi tiết phim, xây dựng API hiển thị metadata.
        - Xây dựng hoàn chỉnh Interaction Pipeline: bắt các sự kiện (như `click`, `watch`, `rate`, `like`) từ Frontend và lưu vào bảng Interactions trên DynamoDB.
    - _Phần Machine Learning:_
        - Xây dựng mô hình **Popularity Ranker** cho người dùng khách và **Content-Based Recommender** cho người dùng đã đăng nhập.
        - Phát triển mô hình cốt lõi **Collaborative Filtering**, chuyển đổi các sự kiện tương tác thành điểm trọng số.
        - Tích hợp mô hình vào SageMaker Endpoint để phục vụ dự đoán gợi ý phim.

4. **Tích hợp hệ thống và Triển khai Cloud:** Nối Backend với mô hình Machine Learning, đảm bảo hệ thống tuân thủ kiến trúc Batch-First trên hạ tầng AWS.
    - _Phần Web:_
        - Tích hợp mô hình Machine Learning vào tiến trình Backend. Xây dựng API POST để định tuyến yêu cầu từ Frontend xuống mô hình.
    - _Phần Machine Learning:_
        - Tự động hóa quy trình re-train mô hình.
        - Chạy thử nghiệm trên Amazon SageMaker.

5. **Kiểm thử:** Rà soát toàn bộ hệ thống, xử lý lỗi và đo lường hiệu năng thực tế. Tối ưu hóa thời gian tải trang và tốc độ truy vấn DynamoDB. Giám sát chi phí AWS để đảm bảo không có tài nguyên chạy ngầm vượt ngân sách. 

_Yêu cầu kỹ thuật_
    - _Frontend:_ Sử dụng Vite và có hiểu biết về EC2. Xây dựng giao diện hiển thị phim, quản lý trạng thái đăng nhập.
    - _Backend:_ Xây dựng bằng FastAPI. Xử lý chuẩn xác luồng xác thực, quản lý luồng thu thập tương tác, và định tuyến kịch bản gợi ý dựa trên trạng thái người dùng.
    - _Machine Learning:_ Phát triển bằng Python sử dụng các thư viện implicit, scikit-learn, numpy, pandas. Yêu cầu xây dựng mô hình Implicit ALS, cùng thuật toán lai ghép Weighted RRF.
    - _Cloud - AWS:_ Amazon S3 để lưu trữ tập dữ liệu thô và các file kết quả mô hình. Amazon DynamoDB làm cơ sở dữ liệu chính cho Web. Sử dụng Amazon SageMaker để chạy tự động quy trình Re-train.

### 5. Lộ trình & Mốc triển khai
- _Trước thực tập (Tháng 0):_ Lập kế hoạch, chuẩn bị dataset.
- _Thực tập (Tháng 1-3):_
    - Tháng 1: Tìm hiểu chung các dịch vụ AWS.
    - Tháng 2: Xây dựng web xem phim và hệ thống gợi ý phim.
    - Tháng 3: Kiểm thử hệ thống và chuẩn bị cho triển khai thực tế.
- _Sau triển khai:_ Theo dõi hiệu suất, tối ưu hóa mô hình và mở rộng tính năng.

### 6. Ước tính ngân sách
Có thể xem chi phí trên [AWS Pricing Calculator](https://github.com/tanghongai1704/movie-project-report)
Hoặc tải [tệp ước tính ngân sách](https://github.com/tanghongai1704/movie-project-report)

_Chi phí hạ tầng_

* AWS 1:
* AWS 2:
* AWS 3:

_Tổng:_ 

### 7. Đánh giá rủi ro

_Ma trận rủi ro_
* Rủi ro 1:
* Rủi ro 2:

_Chiến lược giảm thiểu_
* Chiến lược 1:
* Chiến lược 2:

_Kế hoạch dự phòng_
* Kế hoạch 1:
* Kế hoạch 2:

### 8. Kết quả kỳ vọng
_Cải tiến kỹ thuật:_ 
_Giá trị dài hạn:_
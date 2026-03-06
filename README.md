Báo cáo Kiểm thử Hiệu năng với JMeter
1. Mục tiêu
Thực hiện kiểm thử hiệu năng đối với trang web được chỉ định nhằm đánh giá khả năng chịu tải, thời gian phản hồi (Response Time) và thông lượng (Throughput) trong các kịch bản sử dụng khác nhau. Từ đó đưa ra đánh giá tổng quan về tính ổn định của hệ thống.

2. Kịch bản Kiểm thử
Quá trình kiểm thử được chia thành 3 kịch bản (Thread Group) cụ thể:

Thread Group 1 (Cơ bản): 10 người dùng ảo (Threads), mỗi người lặp 5 lần (tổng cộng 50 requests). Thực hiện gửi yêu cầu HTTP GET đến Trang chủ.
<img width="2008" height="1279" alt="Screenshot 2026-03-06 151913" src="https://github.com/user-attachments/assets/7099320f-628a-4c22-8120-f2d3cd081694" />

Thread Group 2 (Tải nặng): 50 người dùng ảo, thời gian Ramp-up 30 giây (Ramp-up từ từ để mô phỏng tải tăng dần). Thực hiện gửi yêu cầu HTTP GET đồng thời đến Trang chủ và Trang English.
![Uploading Screenshot 2026-03-06 152310.png…]()


Thread Group 3 (Tùy chỉnh): 20 người dùng ảo, chạy liên tục trong khoảng 60 giây. Thực hiện gửi yêu cầu HTTP GET liên tục tới 2 trang danh mục là Trang Công Nghệ và Trang Nghệ Thuật.
<img width="2206" height="1123" alt="Screenshot 2026-03-06 152317" src="https://github.com/user-attachments/assets/678625a1-24f6-472e-97fa-bd8cebd8329e" />

3. Phân tích Kết quả
3.1. Kết quả Kịch bản 1 (Cơ bản)
Tổng số requests (Samples): 50

Error Rate (Tỷ lệ lỗi): 0.00% (Tất cả các yêu cầu đều được xử lý thành công).

Response Time (Thời gian phản hồi):

Average (Trung bình): 4401 ms (~4.4 giây)

Min (Nhỏ nhất): 374 ms

Max (Lớn nhất): 15447 ms (~15.4 giây)

Throughput (Thông lượng): 1.4 requests/sec.

Nhận xét: Mặc dù không có lỗi phát sinh, nhưng thời gian phản hồi trung bình cho trang chủ ở mức khá cao (4.4 giây), có request bị nghẽn lên đến hơn 15 giây. Điều này cho thấy trang chủ tải khá nặng (có thể do nhiều tài nguyên hình ảnh/mã nguồn tĩnh).

3.2. Kết quả Kịch bản 2 (Tải nặng)
Tổng số requests (Samples): 200 (100 cho Trang chủ, 100 cho Trang English).

Error Rate (Tỷ lệ lỗi): 0.00%

Response Time (Thời gian phản hồi trung bình):

GET Trang Chu: 4798 ms (Max đạt tới 17856 ms).

GET Trang English: 1125 ms (Max đạt tới 3947 ms).

Tổng quan trung bình (TOTAL): 2961 ms.

Throughput (Thông lượng): 1.5 requests/sec.

Nhận xét: Khi số lượng người dùng tăng lên 50, thời gian tải trang chủ tiếp tục tăng nhẹ (lên 4.7 giây). Tuy nhiên, trang con (Trang English) lại có thời gian phản hồi tốt hơn rất nhiều (~1.1 giây). Hệ thống vẫn giữ được sự ổn định tuyệt đối khi tỷ lệ lỗi vẫn là 0%.

3.3. Kết quả Kịch bản 3 (Tùy chỉnh - Chạy liên tục 60 giây)
Tổng số requests (Samples): 1550 (779 cho Trang Công Nghệ, 771 cho Trang Nghệ Thuật).

Error Rate (Tỷ lệ lỗi): 0.00%

Response Time (Thời gian phản hồi trung bình):

GET Trang Cong Nghe: 751 ms

GET Trang Nghe Thuat: 737 ms

Tổng quan trung bình (TOTAL): 744 ms.

Throughput (Thông lượng): Đạt mức rất cao 25.7 requests/sec (Mỗi trang con xử lý khoảng 12.9 requests/sec).

Nhận xét: Đây là kịch bản có hiệu suất tốt nhất. Khi truy cập vào các trang danh mục cụ thể, máy chủ phản hồi rất nhanh (dưới 1 giây). Thông lượng đạt hơn 25 yêu cầu mỗi giây nhưng hệ thống vẫn không hề phát sinh lỗi (0.00%), chứng tỏ các trang con được tối ưu hóa tốt hoặc được cache hiệu quả.

4. Kết luận
Độ ổn định: Tuyệt vời. Xuyên suốt cả 3 kịch bản với các mức tải khác nhau, tỷ lệ lỗi (Error Rate) luôn ở mức 0%. Máy chủ không có dấu hiệu bị sập hay từ chối kết nối.

Hiệu suất: Hệ thống xử lý các trang con/trang danh mục rất mượt mà và nhanh chóng (trung bình dưới 1 giây). Tuy nhiên, Trang chủ đang là nút thắt cổ chai (bottleneck) về mặt hiệu suất khi thời gian phản hồi trung bình luôn ở mức trên 4 giây và có độ trễ lớn khi tải tăng cao. Cần xem xét tối ưu hóa tài nguyên tĩnh (dung lượng ảnh, file JS/CSS) hoặc kiểm tra lại các truy vấn cơ sở dữ liệu trên trang chủ để cải thiện trải nghiệm người dùng.

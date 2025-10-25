# Online-Food-Ordering-System-v1.0
This is my Personal Learning Project, created as part of my self-study journey to improve my Business Analysis skills. In this version 1.0, there are still many potential features that have not yet been implemented, numerous aspects that still need improvement. A more complete and refined version is expected to be released in the future.

# 🍜 Case Study (Giả định): Ứng dụng đặt món ăn trực tuyến – FoodieOrder
## 1. Introduction
_Problem Statement:_
Ứng dụng đặt đồ ăn FoodieOrder đang gặp tình trạng người dùng rời bỏ cao (35%) ở bước xác nhận đơn hàng, dẫn đến tỷ lệ hoàn tất đặt hàng thấp và ảnh hưởng trực tiếp đến doanh thu của nhà hàng đối tác. Nguyên nhân chính đến từ việc người dùng không theo dõi được tiến trình giao hàng hoặc gặp lỗi khi cập nhật trạng thái đơn.

_Solution Summary:_
Báo cáo này đề xuất thiết kế lại luồng đặt hàng với trạng thái đơn hàng rõ ràng, thông báo thời gian thực và giao diện quản lý nhà hàng dễ sử dụng hơn, nhằm giảm tỷ lệ rời bỏ và tăng sự minh bạch giữa nhà hàng, tài xế và khách hàng.

_Expected Outcomes:_
- Giảm tỷ lệ rời bỏ đơn hàng từ 35% xuống còn 15% trong 3 tháng.
- Tăng tỷ lệ hoàn tất giao dịch thành công lên 20%.
- Tăng chỉ số hài lòng của người dùng (CSAT) thêm 15 điểm phần trăm.

## 2. Background / Context 
_Về FoodieOrder:_
FoodieOrder là nền tảng trung gian giúp người dùng đặt đồ ăn từ các nhà hàng gần nhất, tương tự GrabFood hoặc ShopeeFood.

_Đối tượng người dùng chính:_
- Khách hàng cá nhân (đặt đồ ăn nhanh, tiết kiệm thời gian).
- Nhà hàng đối tác (quản lý menu, đơn hàng, doanh thu).
- Tài xế giao hàng (nhận và vận chuyển đơn).

_Mục tiêu kinh doanh:_
- Tăng tỷ lệ hoàn tất đơn hàng.
- Giảm thời gian xử lý đơn trung bình.
- Nâng cao trải nghiệm của cả khách hàng và nhà hàng.

_Dữ liệu hiện trạng (giả định):_
35% người dùng rời bỏ ở bước xác nhận thanh toán hoặc theo dõi đơn.
20% phản hồi tiêu cực liên quan đến “không biết đơn đang ở đâu” hoặc “quán chuẩn bị quá lâu”.
15% đơn bị hủy do không cập nhật trạng thái kịp thời từ phía nhà hàng.

_Phản hồi người dùng:_
“Tôi không biết tài xế đã đến lấy món chưa.”
“App không báo khi nhà hàng trễ giờ, tôi phải hủy đơn.”
“Phần quản lý của quán quá phức tạp, bật/tắt món rất rối.”

📊 (Biểu đồ phễu minh họa)
Người dùng truy cập → Chọn món → Xác nhận đơn → Rời bỏ 35% tại đây → Thanh toán → Hoàn tất.

## 3. Evaluation / Alternatives (Phân tích & Đánh giá)
### 🔍 Phân tích hiện trạng (As-is Analysis)
Hiện tại, luồng đặt hàng bao gồm các bước:
- Người dùng chọn món → thêm vào giỏ.
- Xác nhận thanh toán → nhà hàng nhận đơn.
- Nhà hàng cập nhật trạng thái → tài xế nhận đơn → giao hàng.

Vấn đề chính:
- Nhà hàng không có hệ thống thông báo tức thời khi tài xế đến sớm hoặc trễ.
- Ứng dụng khách hàng thiếu hiển thị tiến trình giao hàng (countdown, trạng thái “Đang chuẩn bị”, “Đang giao”).
- Giao diện quản lý món ăn chưa thân thiện: bật/tắt món thủ công, khó tìm.

### ⚖️ Đánh giá các phương án (Alternatives)
- Phương án	A: Thêm hệ thống đếm ngược & thông báo thời gian thực giữa nhà hàng – tài xế – khách hàng.
    Ưu điểm: Tăng minh bạch, cải thiện trải nghiệm
    Nhược điểm: Cần đồng bộ backend và push notification

- Phương án B: Xây dựng chatbot thông báo tiến trình giao hàng tự động.
    Ưu điểm: Tương tác thân thiện
    Nhược điểm: Khó kiểm soát dữ liệu thời gian thực
  
- Phương án C: Tối ưu UI quản lý đơn hàng, thêm trạng thái chi tiết (Chuẩn bị, Đang giao, Hoàn tất).
    Ưu điểm: Dễ triển khai, cải thiện nhận thức người dùng
    Nhược điểm: Không xử lý được tình huống trễ thực tế

Lý do chọn Phương án A (kết hợp với C): Đáp ứng cả hai nhóm người dùng (khách & nhà hàng), mang lại tác động lớn nhất đến tỷ lệ hoàn tất đơn hàng với chi phí hợp lý.

## 4. Proposed Solution
### 🎯 Mô tả chi tiết giải pháp
_- Tính năng đếm ngược và cảnh báo trễ:_ Khi đến hạn chuẩn bị món mà chưa hoàn thành → app hiển thị thông báo “Nhà hàng đang chuẩn bị lâu hơn dự kiến”.

_- Cập nhật trạng thái đơn hàng thời gian thực:_ Giao diện khách hàng có 4 bước: “Xác nhận” → “Chuẩn bị” → “Đang giao” → “Hoàn tất”.

_- Cải tiến giao diện nhà hàng:_ Quản lý món dễ hơn (bật/tắt nhanh, sắp xếp, thêm mô tả, thời gian bán). Hiển thị rõ “đơn sắp hết thời gian” để quán chủ động xử lý.

## 5. Recommendations (Kiến nghị & Lộ trình)
### Roadmap triển khai
_- Giai đoạn 1 (MVP)_
  Mục tiêu / Tính năng chính: Hiển thị trạng thái đơn hàng (4 bước), đếm ngược chuẩn bị món, thông báo trễ
  Thời gian (dự kiến): 1 tháng
  
_- Giai đoạn 2:_
  Mục tiêu / Tính năng chính: Cải tiến UI quản lý nhà hàng, bật/tắt món nhanh, cập nhật mô tả & hình ảnh món
  Thời gian: 2 tháng
  
_- Giai đoạn 3:_
  Mục tiêu / Tính năng chính: Thêm dashboard doanh thu & báo cáo hiệu suất nhà hàng
  Thời gian: 3 tháng
  
## Metrics đo lường hiệu quả

- Tỷ lệ hoàn tất đơn hàng (%)
- Tỷ lệ đơn bị hủy do chậm chuẩn bị (%)
- Thời gian trung bình từ xác nhận đến giao hàng thành công (minutes)
- CSAT – Customer Satisfaction Score
- Tỷ lệ người dùng quay lại đặt hàng lần 2+

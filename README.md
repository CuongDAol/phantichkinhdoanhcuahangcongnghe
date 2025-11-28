# BÁO CÁO PHÂN TÍCH HIỆU SUẤT KINH DOANH CỬA HÀNG CÔNG NGHỆ (NĂM 2019)

## 1. Tổng Quan Dự Án & Dữ Liệu
Báo cáo này tổng hợp kết quả phân tích từ bộ dữ liệu bán hàng của 12 tháng năm 2019 (tổng cộng 186,850 giao dịch). Mục tiêu là trả lời các câu hỏi kinh doanh cốt lõi và đề xuất chiến lược để tăng trưởng doanh thu.

### 1.1. Nguồn Dữ Liệu
Dữ liệu được tổng hợp từ 12 tệp CSV riêng lẻ (mỗi tệp đại diện cho một tháng bán hàng) trong thư mục `Sales_Data`.

### 1.2. Cấu Trúc Dữ Liệu
Mỗi dòng dữ liệu đại diện cho một chi tiết đơn hàng, bao gồm các thông tin chính sau:
* **Order ID:** Mã định danh duy nhất của đơn hàng.
* **Product:** Tên sản phẩm được đặt mua.
* **Quantity Ordered:** Số lượng sản phẩm trong đơn hàng.
* **Price Each:** Đơn giá của từng sản phẩm.
* **Order Date:** Thời gian đặt hàng (Ngày & Giờ).
* **Purchase Address:** Địa chỉ giao hàng của khách (bao gồm Số nhà, Đường, Thành phố, Bang, Mã bưu điện).

### 1.3. Quy Trình Xử Lý Dữ Liệu (Data Cleaning)
Trước khi phân tích, dữ liệu thô đã trải qua các bước làm sạch quan trọng:
* **Xử lý dữ liệu nhiễu:** Loại bỏ các dòng tiêu đề cột bị lặp lại trong nội dung dữ liệu (do quá trình gộp file).
* **Xử lý giá trị thiếu:** Loại bỏ 545 dòng chứa giá trị rỗng (NaN) để đảm bảo tính toàn vẹn.
* **Chuẩn hóa kiểu dữ liệu:** Chuyển đổi cột `Quantity Ordered` và `Price Each` từ dạng chuỗi (string) sang dạng số (numeric) để tính toán. Chuyển đổi `Order Date` sang định dạng thời gian chuẩn.
* **Tạo biến mới (Feature Engineering):** Tách thêm các cột `Month` (Tháng), `Hour` (Giờ), `City` (Thành phố) và tính cột `Sales` (Doanh thu = Số lượng * Đơn giá).

---

## 2. Các Phát Hiện Chính (Key Insights)

### 2.1. Thời điểm kinh doanh tốt nhất trong năm
* **Tháng doanh thu cao nhất:** Tháng 12 (đạt ~4.6 triệu USD).
* **Tháng doanh thu thấp nhất:** Tháng 1.
* **Xu hướng:** Doanh thu tăng mạnh vào Quý 4 (Tháng 10, 11, 12), phản ánh rõ nét thói quen mua sắm dịp lễ hội cuối năm.



![Image of Bar chart showing monthly sales revenue](https://github.com/CuongDAol/phantichkinhdoanhcuahangcongnghe/blob/304453324d07b2d798b18e080065cf8643448342/Q1.png)


### 2.2. Khu vực địa lý trọng điểm
* **Thành phố dẫn đầu:** **San Francisco (CA)** có sức mua vượt trội so với các thành phố khác.
* **Top 3 thành phố:** San Francisco, Los Angeles, và New York City đóng góp tỷ trọng lớn nhất vào tổng doanh thu toàn chuỗi.



![Image of Bar chart showing sales by city](https://github.com/CuongDAol/phantichkinhdoanhcuahangcongnghe/blob/304453324d07b2d798b18e080065cf8643448342/Q2.png)


### 2.3. Khung giờ vàng để quảng cáo
* **Hành vi khách hàng:** Lượng đơn đặt hàng tăng cao và đạt đỉnh vào hai khung giờ chính:
    1.  **Buổi trưa:** 11:00 - 12:00.
    2.  **Buổi tối:** 19:00.
* Điều này cho thấy khách hàng thường mua sắm vào giờ nghỉ trưa hoặc sau giờ làm việc.
![hhihi](https://github.com/CuongDAol/phantichkinhdoanhcuahangcongnghe/blob/304453324d07b2d798b18e080065cf8643448342/Q3.png)


### 2.4. Phân tích Sản phẩm
* **Sản phẩm bán chạy nhất (Số lượng):** Pin AA và Pin AAA. (Giá rẻ, nhu cầu thay thế cao).
* **Sản phẩm doanh thu cao nhất:** Macbook Pro Laptop. (Giá trị cao).
* **Mối tương quan:** Có sự nghịch đảo giữa giá thành và số lượng bán. Sản phẩm giá thấp bán được số lượng lớn, trong khi sản phẩm giá cao bán ít hơn nhưng là nguồn thu chính.



![Image of Chart comparing product price vs quantity sold](https://github.com/CuongDAol/phantichkinhdoanhcuahangcongnghe/blob/304453324d07b2d798b18e080065cf8643448342/Q4.png)
![hihi](https://github.com/CuongDAol/phantichkinhdoanhcuahangcongnghe/blob/304453324d07b2d798b18e080065cf8643448342/Q5.png)

---

## 3. Đề Xuất Hành Động Chiến Lược (Action Plan)

Dựa trên các insight thu được, dưới đây là kế hoạch hành động cụ thể:

### 3.1. Chiến lược Marketing & Quảng cáo
* **Tối ưu lịch chạy Ads:** Thiết lập hiển thị quảng cáo mạnh mẽ vào lúc **10:30 sáng** và **18:30 tối** (trước giờ cao điểm 30 phút) để đón đầu nhu cầu mua sắm.
* **Target theo địa lý:** Dồn ngân sách marketing lớn nhất cho khu vực **San Francisco**, tiếp theo là LA và NYC. Cân nhắc các nội dung quảng cáo mang tính địa phương hóa cho các khu vực này.
* **Chiến dịch "Pre-Holiday":** Khởi động các chương trình khuyến mãi cuối năm ngay từ tháng 10 để tối đa hóa đà tăng trưởng của Quý 4.

### 3.2. Chiến lược Bán hàng (Sales Strategy)
* **Bán chéo (Cross-selling):** Triển khai hệ thống gợi ý tự động. Ví dụ: Khách mua điện thoại/laptop -> Gợi ý mua thêm dây sạc, tai nghe hoặc pin dự phòng.
* **Combo sản phẩm (Bundling):** Tạo các gói combo để tăng giá trị trung bình đơn hàng (AOV).
    * *Ví dụ:* Combo "Làm việc tại nhà" (Laptop + Màn hình + Dây kết nối).
    * *Ví dụ:* Combo "Đồ chơi" (Máy chơi game + Pin dự trữ).

### 3.3. Tối ưu Vận hành & Kho vận
* **Quản lý tồn kho:**
    * Tăng mức tồn kho an toàn cho **Pin và Dây sạc** (nhu cầu cao, ổn định).
    * Nhập kho dự trữ các thiết bị giá trị cao (Laptop, Điện thoại) vào đầu tháng 10.
* **Nhân sự:** Bố trí nhân viên trực page/hotline đông nhất vào khung giờ **11h-13h** và **18h-20h** để đảm bảo tỷ lệ chốt đơn và phản hồi khách hàng nhanh nhất.

### 3.4. Hướng phát triển tiếp theo
* **Nghiên cứu sâu:** Tìm hiểu nguyên nhân San Francisco có doanh số vượt trội (Do thu nhập dân cư? Do trụ sở công nghệ? Hay do tốc độ giao hàng tại đây nhanh hơn?).
* **Khách hàng thân thiết:** Xây dựng cơ sở dữ liệu để theo dõi tỷ lệ khách hàng quay lại (đặc biệt là nhóm khách hay mua vật tư tiêu hao như Pin).

---
*Báo cáo được tổng hợp dựa trên dữ liệu Sales_Data năm 2019.*

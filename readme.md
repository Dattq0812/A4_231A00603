# Lab A4 - Ứng dụng Máy tính & Kiểm tra BMI

Đây là ứng dụng Android được phát triển trong bài thực hành Lab A4, bao gồm hai tính năng chính: Máy tính 4 phép toán cơ bản (kèm chức năng nâng cao) và Công cụ tính chỉ số khối cơ thể (BMI).

## 🚀 Luồng hoạt động chính của ứng dụng

Ứng dụng áp dụng **quy trình kiểm tra dữ liệu 4 lớp** nghiêm ngặt trước khi thực hiện bất kỳ tính toán nào để đảm bảo không xảy ra lỗi (crash) ứng dụng:
1. **Kiểm tra rỗng:** Sử dụng `isEmpty()`. Báo lỗi tại ô nhập bằng `setError()`.
2. **Kiểm tra định dạng:** Chuyển đổi chuỗi sang số (`Double.parseDouble`) bên trong khối `try-catch`. Bắt lỗi `NumberFormatException` và thông báo bằng `Toast`.
3. **Kiểm tra miền giá trị:** Bẫy lỗi chia cho 0 (đối với phép chia) hoặc số âm/bằng 0 (đối với BMI).
4. **Xử lý nghiệp vụ:** Thực hiện tính toán và định dạng kết quả với `String.format` kèm `Locale`.

### 1. Tính năng Máy tính
* **Nhập liệu:** Người dùng nhập số vào hai ô (Số A và Số B).
* **Thao tác:** Bấm vào các nút phép toán (`+`, `-`, `*`, `/`).
* **Hiển thị:** Kết quả được làm tròn 2 chữ số thập phân và hiển thị bên dưới. Nút **Xóa trắng** giúp reset lại toàn bộ ô nhập và kết quả.

### 2. Tính năng Tính BMI
* **Nhập liệu:** Nhập Cân nặng (kg) và Chiều cao (hỗ trợ cả mét hoặc centimet. VD: `1.70` hoặc `170`). Ứng dụng tự động quy đổi về mét nếu phát hiện chiều cao > 3.
* **Thao tác:** Bấm nút **Tính BMI**.
* **Hiển thị:** In ra chỉ số BMI (1 chữ số thập phân) và đưa ra phân loại (Thiếu cân, Bình thường, Thừa cân, Béo phì) dựa trên **ngưỡng khuyến nghị của WHO dành riêng cho khu vực Châu Á**.

---

## 🌟 Chức năng Nâng cao (NC1 & NC2)

Ứng dụng đã được tích hợp thêm 2 tính năng nâng cao để tăng trải nghiệm người dùng.

### NC1: Bổ sung nút Phần trăm (%) và Đảo dấu (±)
Tính năng này được thiết kế để tương tác trực tiếp với ô nhập liệu mà người dùng đang thao tác (đang được focus).
* **Luồng hoạt động:** 
  * Ứng dụng kiểm tra xem ô nhập nào (`edtSoA` hay `edtSoB`) đang có trạng thái `hasFocus()`.
  * Đọc giá trị hiện tại của ô đó.
  * **Nút `%`**: Lấy giá trị chia cho 100 và ghi đè lại vào ô nhập.
  * **Nút `±`**: Nhân giá trị với `-1` để đổi dấu. Nếu là số nguyên, hệ thống tự động bỏ phần thập phân `.0` để giao diện hiển thị gọn gàng hơn.
  * Nếu người dùng chưa bấm vào ô nhập nào mà đã nhấn nút, ứng dụng sẽ hiện `Toast` nhắc nhở.

### NC2: Lưu và hiển thị lịch sử 5 phép tính gần nhất
Mọi phép toán sau khi thực hiện thành công sẽ được ghi lại để người dùng tiện theo dõi. Đặc biệt, **dữ liệu lịch sử không bị mất khi xoay màn hình điện thoại**.
* **Luồng hoạt động & Xử lý kỹ thuật:**
  * Khởi tạo một `ArrayList<String>` để lưu trữ các chuỗi kết quả.
  * Mỗi khi tính toán thành công, chuỗi kết quả mới sẽ được đẩy vào đầu danh sách `add(0, result)`. 
  * Nếu kích thước danh sách vượt quá 5, phần tử cũ nhất (ở cuối) sẽ bị xóa để duy trì số lượng tối đa là 5.
  * **Bảo toàn dữ liệu:** Ghi đè phương thức `onSaveInstanceState(Bundle outState)` để đóng gói danh sách lịch sử lại trước khi Activity bị hệ thống hủy do xoay màn hình. Trong hàm `onCreate()`, danh sách này được trích xuất lại từ `savedInstanceState` để render lại lên giao diện.

---

## 🛠 Môi trường phát triển
* **Ngôn ngữ:** Java
* **UI Toolkit:** XML
* **Minimum SDK:** 24
* **Công cụ:** Android Studio
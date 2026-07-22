# Đếm số lượng hạt gạo (Rice Counting)

Đây là bài tập thực hành môn Computer Vision (Thị giác máy tính). Mục tiêu của dự án là viết một đoạn code duy nhất có thể tự động đếm chính xác số lượng hạt gạo trong nhiều bức ảnh khác nhau.

## Yêu cầu bài toán

Chương trình phải xử lý được 4 loại ảnh đầu vào:
1. **Ảnh chuẩn:** Hạt gạo phân bố đều, ánh sáng rõ ràng.
2. **Ảnh nhiễu chấm (Salt & Pepper):** Ảnh có nhiều chấm trắng và đen nhỏ nằm rải rác.
3. **Ảnh sáng không đều (Sinus):** Có các dải sáng và dải tối xen kẽ nhau trên ảnh.
4. **Ảnh bị tối dần:** Phần trên của ảnh rất tối, làm hạt gạo gần như lẫn vào nền đen (như trong ảnh thứ 4).

## Cách giải quyết bài toán

Để xử lý được cả 4 loại ảnh trên bằng một quy trình chung, chúng ta thực hiện 5 bước sau:

### Bước 1: Tiền xử lý (Lọc nhiễu)
- Chuyển ảnh màu sang ảnh xám.
- Dùng bộ lọc trung vị (Median Blur) để xóa các chấm nhiễu trắng đen trong ảnh thứ 2 mà không làm mờ viền hạt gạo.

### Bước 2: Tách hạt gạo ra khỏi nền (Phân ngưỡng)
- Không dùng một mức sáng cố định để tách nền, vì ảnh số 3 và số 4 có độ sáng tối không đều.
- Dùng phương pháp phân ngưỡng thích ứng (Adaptive Thresholding). Thuật toán này sẽ tính toán mức sáng riêng cho từng vùng nhỏ của ảnh, giúp nhận diện được hạt gạo ngay cả ở những chỗ rất tối.

### Bước 3: Làm gọn hình dáng hạt gạo (Hình thái học)
- Dùng phép toán Mở (Opening) để xóa các đốm trắng rác nhỏ còn sót lại.
- Dùng phép toán Đóng (Closing) để lấp đầy các lỗ rỗng bên trong hạt gạo.

### Bước 4: Tìm viền và đếm số lượng
- Dùng hàm tìm đường viền (`cv2.findContours`) để xác định hình dáng các hạt gạo.
- Tính diện tích của các đường viền. Bỏ qua các đường viền quá nhỏ (đó là nhiễu, không phải hạt gạo).
- Số lượng đường viền giữ lại chính là tổng số hạt gạo.

### Bước 5: Hiển thị kết quả
- Vẽ đường viền quanh các hạt gạo tìm được lên ảnh gốc.
- In tổng số hạt gạo lên màn hình.

---
**Công cụ sử dụng:** Python, OpenCV, NumPy, Matplotlib.

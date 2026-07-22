# 🍚 Hạt Gạo Đếm Ngược (Rice Counting - Computer Vision)

Dự án này là một bài tập thực hành cốt lõi trong môn học **Computer Vision (Thị giác máy tính)**, nhằm mục đích sử dụng các kỹ thuật xử lý ảnh truyền thống (OpenCV) để tự động hóa việc đếm số lượng vật thể.

## 🎯 Yêu cầu bài toán

Mục tiêu chính là xây dựng **một chuỗi xử lý duy nhất (a single pipeline)** có khả năng đếm chính xác số hạt gạo trong tất cả các bức ảnh đầu vào.
Tuy nhiên, dữ liệu đầu vào chứa nhiều thử thách thực tế, bao gồm:
1. **Ảnh chuẩn:** Hạt gạo phân bố bình thường, ánh sáng tốt.
2. **Nhiễu hạt (Salt & Pepper Noise):** Ảnh bị chèn các đốm trắng đen li ti giống như rắc muối tiêu.
3. **Nhiễu độ sáng không đều (Uneven Illumination / Sinus Noise):** Vùng sáng vùng tối xen kẽ khiến cho các phương pháp nhị phân hóa thông thường bị thất bại.
4. **Hạt gạo dính nhau:** Một số hạt gạo nằm sát và dính liền vào nhau.

## 🧠 Phương pháp giải quyết (Approach)

Để vượt qua các thử thách trên mà chỉ dùng 1 pipeline duy nhất, dự án áp dụng quy trình 5 bước kinh điển trong Xử lý ảnh:

### Bước 1: Tiền xử lý (Preprocessing)
- **Grayscale:** Chuyển ảnh màu (RGB) sang ảnh xám (1 kênh màu) để giảm khối lượng tính toán.
- **Khử nhiễu (Noise Removal):** Sử dụng bộ lọc **Median Blur**. Bộ lọc trung vị (Median) là "khắc tinh" của nhiễu muối tiêu, giúp làm sạch hoàn toàn các đốm nhiễu mà vẫn giữ nguyên được độ sắc nét viền của hạt gạo.

### Bước 2: Nhị phân hóa (Thresholding)
- Để khắc phục vấn đề độ sáng không đồng đều (Sinus Noise), ta không sử dụng ngưỡng cố định (Global Threshold). Thay vào đó, áp dụng **Adaptive Thresholding** (Phân ngưỡng thích ứng).
- Thuật toán sẽ tính toán ngưỡng sáng cục bộ cho từng vùng ảnh nhỏ, giúp tách chính xác hạt gạo màu trắng ra khỏi nền đen dù ở trong vùng tối hay vùng sáng.

### Bước 3: Toán học Hình thái học (Morphology)
Sau khi nhị phân, các hạt gạo có thể bị lởm chởm hoặc có lỗ rỗng bên trong.
- **Phép Opening** (Xói mòn kết hợp Phình to) giúp xóa bỏ các hạt bụi trắng li ti còn sót lại.
- **Phép Closing** (Phình to kết hợp Xói mòn) giúp lấp đầy các lỗ đen bên trong thân hạt gạo.

### Bước 4: Tìm viền và Đếm (Contour Detection)
- Sử dụng thuật toán tìm đường viền (`cv2.findContours`) để xác định ranh giới của các đốm trắng (hạt gạo).
- **Lọc nhiễu bằng Diện tích (Area Filtering):** Loại bỏ các đường viền có diện tích quá nhỏ (rác) hoặc quá lớn (bất thường). Tổng số lượng đường viền hợp lệ còn lại chính là tổng số hạt gạo.

### Bước 5: Trực quan hóa (Visualization)
- Vẽ khung bao (Bounding Box) hoặc viền đỏ quanh từng hạt gạo trên ảnh màu gốc và hiển thị số lượng đếm được lên góc ảnh.

---
**🛠 Công cụ sử dụng:** Python, OpenCV (`cv2`), NumPy, Matplotlib.

# Trợ lý học tập điều khiển bằng giọng nói
Tên sinh viên: Nguyễn Thị Bảo Tuyến
MSSV: N23DCCI077
Lớp: D23CQCI01-N

# 🎙️ Hệ Thống Nhận Dạng Từ Khóa Giọng Nói (Keyword Spotting - KWS)
Dự án nhận dạng từ khóa giọng nói phục vụ điều khiển trợ lý học tập bằng giọng nói, được thiết kế và huấn luyện trên nền tảng **Edge Impulse** sử dụng mạng nơ-ron tích chập (CNN) và triển khai thực tế thông qua **WebAssembly (WASM)**.

## 📂 Cấu trúc thư mục dự án

TRO_LY_HOC_TAP-WASM-V1-IMPULSE-#1
├── 📁 browser/                  # Phiên bản chạy trực tiếp trên Trình duyệt Web
│   ├── edge-impulse-standalone.js    # Thư viện wrapper JavaScript
│   ├── edge-impulse-standalone.wasm  # Mô hình CNN đã biên dịch sang mã máy WASM
│   ├── index.html                    # Giao diện hiển thị và nhận tín hiệu Micro
│   └── run-impulse.js                # Script xử lý logic nhận dạng trên trình duyệt
├── server.py                    # Script Python hỗ trợ khởi chạy Local Web Server
└── 📁 node/                     # Phiên bản chạy trên môi trường Node.js (Backend)
    ├── edge-impulse-standalone.js
    ├── edge-impulse-standalone.wasm
    └── run-impulse.js                # Script chạy mô hình bằng dòng lệnh Node.js


## 🚀 Hướng dẫn cài đặt và chạy dự án

### Bước 1: Tạo Project
Đăng nhập vào Edge Impulse và tiến hành tạo một dự án mới (`Create new project`).

### Bước 2: Thu thập và gán nhãn dữ liệu
Tạo 5 nhãn dữ liệu tương ứng với các lớp: `bao_tuyen`, `bat_dau`, `tam_dung`, `ket_thuc`, `noise`. 
* **Sampling Rate (Tần số lấy mẫu):** 16000 Hz
* **Sample Length (Độ dài mẫu):** 1000 ms (1 giây)

### Bước 3: Thiết kế cấu trúc Impulse
Tại mục **Impulse Design**, cấu hình luồng xử lý như sau:
* **Input Block:** Audio (1 second)
* **Processing Block:** MFCC
* **Learning Block:** Classification (CNN)

### Bước 4: Trích xuất đặc trưng (Feature Generation)
Di chuyển vào mục **MFCC**, giữ nguyên các cấu hình mặc định và chọn **Generate Features** để chuyển đổi tín hiệu âm thanh miền thời gian sang tập đặc trưng phổ tần số Mel.

### Bước 5: Huấn luyện mô hình
Vào mục **Classifier**, thiết lập các tham số mạng CNN và chọn **Start Training**. Mô hình sẽ tự động tối ưu hóa dựa trên tập đặc trưng MFCC đã sinh ra.

### Bước 6: Đánh giá mô hình
Sau khi huấn luyện xong, kiểm tra độ chính xác tổng quan và độ thu gọn của các cụm đặc trưng tại các mục:
* *Model Testing / Validation set* (`Accuracy`, `Precision`, `Recall`, `F1-score`)
* *Confusion Matrix* (Ma trận nhầm lẫn)
* *Feature Explorer* (Bản đồ phân cụm không gian đặc trưng)

### Bước 7: Chạy thử nghiệm thời gian thực (Live Classification)
1. Mở mục **Live Classification** trên Edge Impulse.
2. Nhấn **Start Sampling** và đọc một trong các từ khóa lệnh.
3. Hệ thống sẽ trả về xác suất dự đoán (%) của từng lớp theo thời gian thực.


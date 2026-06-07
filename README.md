# Trợ lý học tập điều khiển bằng giọng nói
Tên sinh viên: Nguyễn Thị Bảo Tuyến
MSSV: N23DCCI077
Lớp: D23CQCI01-N

# 🎙️ Hệ Thống Nhận Dạng Từ Khóa Giọng Nói (Keyword Spotting - KWS)

Dự án nhận dạng từ khóa giọng nói phục vụ điều khiển trợ lý học tập bằng giọng nói, được thiết kế và huấn luyện trên nền tảng **Edge Impulse** sử dụng mạng nơ-ron tích chập (CNN) và triển khai thực tế thông qua **WebAssembly (WASM)**.

---

## 📝 Giới thiệu
Hệ thống có khả năng nhận dạng các từ khóa tiếng Việt thời gian thực với độ trễ thấp và độ chính xác cao trực tiếp trên môi trường máy tính hoặc trình duyệt web.

**Các từ khóa được hỗ trợ:**
* `BAO_TUYEN` (Từ khóa kích hoạt / Wake-word)
* `BAT_DAU`
* `TAM_DUNG`
* `KET_THUC`
* `NOISE` (Tập nhiễu môi trường)

---

## 🛠️ Công nghệ sử dụng
* **Nền tảng huấn luyện:** Edge Impulse
* **Trích xuất đặc trưng:** MFCC (Mel-Frequency Cepstral Coefficients)
* **Mô hình học máy:** CNN (Convolutional Neural Network)
* **Định dạng triển khai:** WebAssembly (WASM) & JavaScript độc lập (`standalone`)

---

## 📂 Cấu trúc thư mục dự án

```text
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
## 📝 Giới thiệu
Hệ thống có khả năng nhận dạng các từ khóa tiếng Việt thời gian thực với độ trễ thấp và độ chính xác cao.

**Các từ khóa được hỗ trợ:**
* `BAO_TUYEN` (Từ khóa kích hoạt / Wake-word)
* `BAT_DAU`
* `TAM_DUNG`
* `KET_THUC`
* `NOISE` (Tập nhiễu môi trường)

---

## 🛠️ Công nghệ sử dụng
* **Nền tảng:** Edge Impulse
* **Trích xuất đặc trưng:** MFCC (Mel-Frequency Cepstral Coefficients)
* **Mô hình học máy:** CNN (Convolutional Neural Network)
* **Bài toán:** Audio Classification (Phân loại âm thanh)

---

## 💻 Yêu cầu hệ thống
* Trình duyệt Web hiện đại (Chrome, Edge, Firefox,...)
* Tài khoản trên nền tảng [Edge Impulse](https://edgeimpulse.com/)
* Microphone phần cứng hoạt động bình thường.

---

## 🚀 Hướng dẫn cài đặt và chạy dự án

### Bước 1: Tạo Project
Đăng nhập vào Edge Impulse và tiến hành tạo một dự án mới (`Create new project`).

### Bước 2: Thu thập và gán nhãn dữ liệu
Tạo 5 nhãn dữ liệu tương ứng với các lớp: `BAO_TUYEN`, `BAT_DAU`, `TAM_DUNG`, `KET_THUC`, `NOISE`. 
*Tải lên hoặc thu âm trực tiếp các tệp âm thanh với thông số chuẩn:*
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

---

## 📊 Quy trình hoạt động (Pipeline)

```text
Audio Input (Microphone) 
   └──> MFCC Feature Extraction (Trích xuất phổ)
           └──> CNN Classification (Phân loại mạng tích chập)
                   └──> Keyword Recognition (Đưa ra từ khóa điều khiển)

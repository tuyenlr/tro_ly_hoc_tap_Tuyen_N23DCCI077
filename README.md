# Trợ Lý Học Tập - Edge Impulse WebAssembly Demo

Dự án này chứa mô hình học máy được biên dịch sang WebAssembly (WASM) từ Edge Impulse để chạy suy luận (inference) trực tiếp trên trình duyệt hoặc môi trường Node.js, cùng với một script Python để thử nghiệm thu âm tín hiệu giọng nói từ Microphone.

---

## 📂 Cấu trúc thư mục (Directory Structure)

```text
tro_ly_hoc_tap-wasm-v1-impulse/
├── edge-impulse-standalone.wasm  # File WebAssembly gốc chứa mô hình AI
├── browser/                       # Chạy suy luận trực tiếp trên Trình duyệt (HTML/JS)
│   ├── README.md                 # Hướng dẫn chạy phía trình duyệt
│   ├── index.html                # Giao diện web chạy thử nghiệm suy luận
│   ├── edge-impulse-standalone.js# Thư viện wrapper JavaScript của Edge Impulse
│   ├── edge-impulse-standalone.wasm # File WASM copy sử dụng cho trình duyệt
│   ├── run-impulse.js            # Module điều khiển tải và khởi chạy mô hình WASM
│   ├── server.py                 # Máy chủ HTTP đơn giản phục vụ HTML & WASM (Cổng 8082)
│   └── run_voice.py              # Script Python thu âm và kiểm tra luồng tín hiệu micro
└── node/                          # Chạy suy luận trên môi trường Node.js (Backend)
    ├── README.md                 # Hướng dẫn chạy phía Node.js
    ├── edge-impulse-standalone.js# Thư viện wrapper Node.js
    ├── edge-impulse-standalone.wasm # File WASM copy sử dụng cho Node.js
    └── run-impulse.js            # Script Node.js nhận dữ liệu đầu vào và chạy suy luận
```

---

## 🚀 Hướng dẫn khởi chạy chi tiết

### 1. Cách chạy Script Nhận diện/Thu âm Micro (`run_voice.py`)

File này nằm trong thư mục `browser/run_voice.py`. Nó dùng để kiểm tra tín hiệu Micro của bạn và chuẩn bị cho việc kết nối điều khiển giao diện học tập.

#### Bước 1: Cài đặt Python
* Đảm bảo máy tính của bạn đã cài đặt Python 3 (tải tại [python.org](https://www.python.org/)).
* Trong quá trình cài đặt trên Windows, nhớ tích chọn **"Add Python to PATH"**.

#### Bước 2: Cài đặt các thư viện cần thiết
Mở Terminal / Command Prompt (CMD) hoặc terminal trong VS Code và cài đặt các thư viện sau:
```bash
pip install numpy sounddevice requests
```
*(Nếu gặp lỗi trên Windows liên quan đến `sounddevice`, hãy đảm bảo thiết bị thu âm Microphone của bạn hoạt động bình thường).*

#### Bước 3: Chạy chương trình
Di chuyển vào thư mục `browser` và chạy file Python:
```bash
cd browser
python run_voice.py
```
Hoặc chạy trực tiếp từ thư mục gốc:
```bash
python browser/run_voice.py
```

* **Kết quả dự kiến:** Màn hình sẽ hiện `"🤖 HỆ THỐNG TRỢ LÝ GIỌNG NÓI AI KHỞI ĐỘNG THÀNH CÔNG!"`. Khi bạn nói hoặc có tiếng động lớn vào Micro (độ lớn > 0.03), hệ thống sẽ in ra dòng: `🎤 Đang thu âm tín hiệu...`.
* Để dừng chương trình, nhấn tổ hợp phím `Ctrl + C`.

---

### 2. Cách chạy trên Trình duyệt Web (Browser)

Thư mục `browser` cho phép chạy thử nghiệm mô hình trực quan qua giao diện web.

#### Bước 1: Khởi động Web Server
Do cơ chế bảo mật (CORS) của trình duyệt đối với các file WebAssembly, bạn cần khởi động một Web Server nội bộ thay vì click đúp vào file `.html`.
Chạy máy chủ Python có sẵn:
```bash
cd browser
python server.py
```

#### Bước 2: Truy cập giao diện
Mở trình duyệt web của bạn và truy cập địa chỉ:
👉 **[http://localhost:8082](http://localhost:8082)**

#### Bước 3: Chạy suy luận
* Dán danh sách các đặc trưng (features) thô được trích xuất từ dữ liệu (ví dụ: các số thực cách nhau bằng dấu phẩy từ Edge Impulse Studio).
* Nhấp vào **"Run inference"** để nhận kết quả phân loại nhãn và độ tin cậy tương ứng.

---

### 3. Cách chạy trên môi trường Node.js

Thư mục `node` cho phép bạn chạy suy luận mô hình trực tiếp qua dòng lệnh bằng Node.js.

#### Bước 1: Chuẩn bị Node.js
Đảm bảo bạn đã cài đặt Node.js trên máy (tải tại [nodejs.org](https://nodejs.org/)).

#### Bước 2: Chạy câu lệnh suy luận
Truy cập thư mục `node` và thực hiện lệnh phân tích dữ liệu đặc trưng:
```bash
cd node
node run-impulse.js "dán_các_chỉ_số_features_vào_đây"
```
*(Ví dụ các đặc trưng: `-19.8800, -0.6900, 8.2300, -17.6600, ...`)*

---

## 📝 Lưu ý & Khắc phục lỗi

1. **Lỗi thiết bị Micro (`❌ Lỗi thiết bị Micro: ...`):**
   * Đảm bảo Microphone của máy tính đã được bật và được cấp quyền truy cập ứng dụng (trong Windows Settings > Privacy & security > Microphone).
   * Đảm bảo không có ứng dụng nào khác đang chiếm quyền sử dụng độc quyền (exclusive control) micro.
2. **Khởi chạy file WASM:**
   * File `.js` và `.wasm` trong cùng thư mục phải luôn đi kèm với nhau để thư viện Wrapper tải đúng tài nguyên.

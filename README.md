# Công Cụ Kiểm Thử Tự Động Lỗ Hổng Reflected XSS (XSS Scanner)

Công cụ tự động hóa quá trình phát hiện, phân tích ngữ cảnh và xác minh khả năng khai thác lỗ hổng **Reflected Cross-Site Scripting (Reflected XSS)** trên các ứng dụng web theo phương pháp kiểm thử hộp đen (Black-box Testing).

---

## Tính Năng Chính

* **Thu thập Endpoint (Crawler):** Tự động duyệt ứng dụng web theo thuật toán BFS để trích xuất URL, tham số (GET/POST) và biểu mẫu (Form).
* **Phân tích Ngữ cảnh Phản chiếu (Context Analysis):** Tự động xác định vị trí dữ liệu đầu vào xuất hiện trong phản hồi HTML (HTML Text Node, HTML Attribute, JavaScript String/Code).
* **Thăm dò Bộ lọc (Filter Prober):** Đánh giá hành vi của WAF hoặc bộ lọc phía máy chủ bằng cách gửi các ký tự đặc biệt, thẻ HTML và thuộc tính sự kiện (Event Handlers) để xây dựng `FilterMap`.
* **Sinh Payload Linh hoạt (Smart Payload Generator):** Tự động tạo payload tối ưu dựa trên ngữ cảnh và thông tin từ bộ lọc, hạn chế thử sai ngẫu nhiên.
* **Xác minh Thực thi Thực tế (Browser Verifier):** Sử dụng trình duyệt không giao diện (**Playwright / Chromium**) để giả lập người dùng, thực thi JavaScript và bắt các sự kiện Dialog (`alert`, `confirm`, `prompt`), giúp loại bỏ hoàn toàn các báo cáo sai (False Positives).
* **Báo cáo Đa dạng (Reporter):** Xuất kết quả chi tiết dưới dạng dữ liệu cấu trúc `JSON` và giao diện `HTML` trực quan (đi kèm bằng chứng PoC).

---

## Kiến Trúc Hệ Thống

Chương trình hoạt động theo mô hình chuỗi xử lý (Pipeline):

```text
[Crawler] ──> [Scanner] ──> [Filter Prober] ──> [Verifier & Generator] ──> [Reporter]
```

**1. Crawler:** Thu thập toàn bộ URL và Endpoint tiềm năng.

**2. Scanner:** Gửi Marker độc nhất để phát hiện điểm phản chiếu và gán ngữ cảnh.

**3. Filter Prober:** Thăm dò các ký tự `<, >, ", ', \, &#96` và các cặp `(Tag, Event)` cho phép.

**4. Verifier:** Sinh payload theo ngữ cảnh và dùng Playwright xác minh thực thi trên trình duyệt thật.

**5. Reporter:** Tổng hợp kết quả và xuất báo cáo.

---

## Hướng Dẫn Cài Đặt

**1. Yêu cầu Môi trường**

* Python: Phiên bản 3.9 trở lên.
* Node.js & npm: Để chạy môi trường thử nghiệm (XSS Lab).

**2. Cài đặt Công cụ Kiểm thử (Scan_Reflected_XSS)**

**Bước 1: Clone repository về máy local**

```bash
git clone [https://github.com/ngocch2932005-lang/ScanReflectedXSS.git](https://github.com/ngocch2932005-lang/ScanReflectedXSS.git)
cd ScanReflectedXSS
```

**Bước 2: Cài đặt các thư viện Python phụ thuộc:**

```bash
pip install -r requirements.txt
```

**Bước 3: Cài đặt trình duyệt Chromium cho Playwright:**

```bash
playwright install chromium
```

**3. Cài đặt Môi trường Thử nghiệm (XSS Lab Node.js)**

Để thử nghiệm công cụ, bạn có thể cài đặt ứng dụng web chứa các kịch bản lỗ hổng mẫu:

```bash
# Clone source code bài lab
git clone [https://github.com/TMT-CS/xss_lab_nodejs](https://github.com/TMT-CS/xss_lab_nodejs)
cd xss_lab_nodejs

# Cài đặt các gói phụ thuộc
npm install

# Khởi chạy server lab (Mặc định chạy tại http://localhost:3000)
node app.js
```

---

## Hướng Dẫn Sử Dụng

Kiểm tra hướng dẫn các tham số dòng lệnh:

```bash
python main.py --help
```

**1. Kiểm thử trực tiếp một Endpoint cụ thể**

```bash
python main.py --url "http://localhost:3000/reflected/level1?q=test"
```

**2. Quét tự động toàn bộ trang web (Crawler + Scan)**

```bash
python main.py http://localhost:3000/ --depth 2 --max-urls 20 --report-dir ./reports
```

**Giải thích tham số:**
* `seed_url / --url`: URL mục tiêu để bắt đầu quét hoặc kiểm thử trực tiếp.
* `--depth`: Độ sâu tối đa khi thu thập liên kết (Mặc định: 2).
* `--max-urls`: Giới hạn số lượng URL tối đa sẽ thu thập (Mặc định: 20).
* `--report-dir`: Thư mục lưu trữ báo cáo kết quả JSON và HTML (Mặc định: ./reports).

---

## Tác Giả

* Nguyễn Ngọc Chinh
* Trần Minh Thanh
* Đỗ Mai Thu Trang
* Bùi Thảo Linh
* Phùng Văn Hưng

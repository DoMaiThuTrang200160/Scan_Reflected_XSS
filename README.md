# Công Cụ Kiểm Thử Tự Động Lỗ Hổng Reflected XSS (XSS Scanner)

Công cụ tự động hóa quá trình phát hiện, phân tích ngữ cảnh và xác minh khả năng khai thác lỗ hổng **Reflected Cross-Site Scripting (Reflected XSS)** trên các ứng dụng web theo phương pháp kiểm thử hộp đen (Black-box Testing).

---

## 📌 Tính Năng Chính

* **Thu thập Endpoint (Crawler):** Tự động duyệt ứng dụng web theo thuật toán BFS để trích xuất URL, tham số (GET/POST) và biểu mẫu (Form).
* **Phân tích Ngữ cảnh Phản chiếu (Context Analysis):** Tự động xác định vị trí dữ liệu đầu vào xuất hiện trong phản hồi HTML (HTML Text Node, HTML Attribute, JavaScript String/Code).
* **Thăm dò Bộ lọc (Filter Prober):** Đánh giá hành vi của WAF hoặc bộ lọc phía máy chủ bằng cách gửi các ký tự đặc biệt, thẻ HTML và thuộc tính sự kiện (Event Handlers) để xây dựng `FilterMap`.
* **Sinh Payload Linh hoạt (Smart Payload Generator):** Tự động tạo payload tối ưu dựa trên ngữ cảnh và thông tin từ bộ lọc, hạn chế thử sai ngẫu nhiên.
* **Xác minh Thực thi Thực tế (Browser Verifier):** Sử dụng trình duyệt không giao diện (**Playwright / Chromium**) để giả lập người dùng, thực thi JavaScript và bắt các sự kiện Dialog (`alert`, `confirm`, `prompt`), giúp loại bỏ hoàn toàn các báo cáo sai (False Positives).
* **Báo cáo Đa dạng (Reporter):** Xuất kết quả chi tiết dưới dạng dữ liệu cấu trúc `JSON` và giao diện `HTML` trực quan (đi kèm bằng chứng PoC).

---

## 🏗 Kiến Trúc Hệ Thống

Chương trình hoạt động theo mô hình chuỗi xử lý (Pipeline):

```text
[Crawler] ──> [Scanner] ──> [Filter Prober] ──> [Verifier & Generator] ──> [Reporter]
```

1. Crawler: Thu thập toàn bộ URL và Endpoint tiềm năng.
2. Scanner: Gửi Marker độc nhất để phát hiện điểm phản chiếu và gán ngữ cảnh.
3. Filter Prober: Thăm dò các ký tự <, >, ", ', \, \`` và các cặp (Tag, Event)` cho phép.
4. Verifier: Sinh payload theo ngữ cảnh và dùng Playwright xác minh thực thi trên trình duyệt thật.
5. Reporter: Tổng hợp kết quả và xuất báo cáo.

# Simple Quiz Platform (Trắc Nghiệm Đơn Giản)

Đây là một nền tảng trắc nghiệm gọn nhẹ, chạy hoàn toàn trên trình duyệt (client-side), không cần cơ sở dữ liệu hay backend phức tạp. Nó được thiết kế để đọc các đề thi từ file `.txt` (định dạng JSON) và chạy ngay lập tức.

## 🚀 Tính năng chính

* **Không Backend:** Chạy 100% trên trình duyệt.
* **Quản lý Đề thi:** Tải nhiều đề thi động từ một file `manifest.json`.
* **Đa dạng loại câu hỏi:**
    * Trắc nghiệm chọn 1 (`mcq`).
    * Trắc nghiệm chọn nhiều (`msq`).
    * Điền vào chỗ trống (`fitb`).
* **Hỗ trợ LaTeX:** Tự động render công thức toán học (sử dụng MathJax).
* **Giới hạn thời gian:** Mỗi câu hỏi có 30 giây đếm ngược, tự động chuyển câu khi hết giờ.
* **Chống gian lận (cơ bản):**
    * Tự động đảo thứ tự câu hỏi (`shuffle`) mỗi khi làm bài.
    * Không thể quay lại câu trước.
* **Xem lại Đáp án:** Hiển thị giải thích chi tiết và so sánh đáp án sau khi nộp bài.

## 🏃 Hướng dẫn chạy (Quan Trọng!)

Bạn **KHÔNG THỂ** chạy file `index.html` bằng cách nhấp đúp (qua `file:///`). Trình duyệt sẽ chặn yêu cầu tải file `quiz.txt` vì lý do bảo mật (CORS policy).

**Cách chạy đúng:** Bạn phải chạy dự án qua một máy chủ web cục bộ.

### Cách 1: Dùng VS Code + Live Server (Khuyên dùng)

1.  Mở thư mục dự án này bằng **Visual Studio Code**.
2.  Cài đặt tiện ích mở rộng (Extension) tên là **Live Server** của Ritwick Dey.
3.  Nhấn chuột phải vào file `index.html` và chọn **"Open with Live Server"**.
4.  Trình duyệt sẽ tự động mở (với địa chỉ như `http://127.0.0.1:5500`).



### Cách 2: Dùng Python (Nếu bạn không có VS Code)

1.  Mở Terminal (hoặc Command Prompt) tại thư mục gốc của dự án.
2.  Gõ lệnh:
    * Nếu bạn dùng Python 3: `python -m http.server 8000`
    * Nếu bạn dùng Python 2: `python -m SimpleHTTPServer 8000`
3.  Mở trình duyệt và truy cập `http://localhost:8000`.

### ⚠️ Sửa lỗi "Đang tải danh sách đề..."

Nếu trang web bị kẹt, hãy:
1.  Nhấn **F12** để mở Công cụ Lập trình viên.
2.  Vào tab **Console**.
3.  Bạn sẽ thấy lỗi 404 (Not Found). Điều này có nghĩa là cấu trúc thư mục của bạn bị sai. Hãy đảm bảo file `manifest.json` nằm chính xác trong thư mục `quizzes/`.

## 📝 Cách thêm/sửa đề thi

Đây là quy trình "của engineer" để thêm một đề thi mới.

### Bước 1: Tạo file `.txt` cho đề mới

* Tạo một file mới, ví dụ `quizzes/vat-ly.txt`.
* Nội dung file phải là một mảng (Array) các đối tượng (Object) JSON.

### Bước 2: Định dạng câu hỏi (JSON)

Bạn có 3 `type` (loại) câu hỏi:

**1. Trắc nghiệm 1 đáp án (mcq)**
* `answer` là `index` (số thứ tự) của đáp án đúng (bắt đầu từ 0).

```json
{
  "type": "mcq",
  "question": "Mặt trời có màu gì?",
  "options": ["Vàng", "Trắng", "Đỏ", "Cam"],
  "answer": 1,
  "explanation": "Mặt trời thực chất có màu trắng khi nhìn từ không gian. Chúng ta thấy nó màu vàng/đỏ do sự tán xạ của khí quyển."
}
2. Trắc nghiệm nhiều đáp án (msq)

answer là một mảng (Array) chứa các index đúng.

JSON

{
  "type": "msq",
  "question": "Những thứ nào sau đây là ngôn ngữ lập trình?",
  "options": ["HTML", "JavaScript", "CSS", "Python"],
  "answer": [1, 3],
  "explanation": "HTML và CSS là ngôn ngữ đánh dấu và tạo kiểu, không phải ngôn ngữ lập trình (theo nghĩa đầy đủ)."
}
3. Điền vào chỗ trống (fitb)

answer là một chuỗi (String). Đáp án không phân biệt hoa/thường.

JSON

{
  "type": "fitb",
  "question": "Ngọn núi cao nhất thế giới là ......",
  "answer": "Everest",
  "explanation": "Đỉnh Everest (Chomolungma) cao 8.848,86 mét."
}
LƯU Ý KHI DÙNG LaTeX: Để viết LaTeX, bạn phải gõ 2 dấu \ cho mỗi lệnh, vì 1 dấu \ bị JSON coi là ký tự escape.

Sai: "$x = \frac{-b}{2a}$"

Đúng: "$x = \\frac{-b}{2a}$"

JSON

{
  "type": "mcq",
  "question": "Công thức tính $x$ trong $ax^2 + bx + c = 0$ là gì?",
  "options": [
    "$x = b^2 - 4ac$",
    "$x = \\frac{-b \\pm \\sqrt{b^2-4ac}}{2a}$"
  ],
  "answer": 1,
  "explanation": "Đây là công thức nghiệm của phương trình bậc hai."
}
Bước 3: Cập nhật file manifest.json
Thêm một mục mới vào file quizzes/manifest.json để ứng dụng nhận diện đề thi của bạn.

JSON

[
  {
    "id": "q1",
    "name": "Đề 1: Tri Thức Tổng Hợp",
    "file": "quiz1.txt"
  },
  {
    "id": "q2",
    "name": "Đề 2: Lịch Sử",
    "file": "quiz2.txt"
  },
  {
    "id": "q-math",
    "name": "Đề 3: Toán Học (LaTeX)",
    "file": "math-quiz.txt"
  },
  {
    "id": "q-physics",
    "name": "Đề 4: Vật Lý Đại Cương",
    "file": "vat-ly.txt"
  }
]
Lưu file lại. Lần tới khi tải index.html, đề thi mới sẽ tự động xuất hiện trong danh sách chọn.

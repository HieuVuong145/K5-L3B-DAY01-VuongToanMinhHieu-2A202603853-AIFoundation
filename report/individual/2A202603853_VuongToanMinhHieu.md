# Báo cáo cá nhân — Day 01: Lọc tin nhắn rác bằng ML, DL, Embedding và LLM

> Đổi tên file này thành `MSSV_HoTen.md` của bạn, ví dụ `2A20260000_NguyenVanAn.md`, rồi điền từng mục. Xoá các dòng hướng dẫn in nghiêng khi xong.

- **MSSV: 2A202603853**
- **Họ và tên: Vương Toàn Minh Hiếu**
- **SEED** (in ở ô đầu notebook): 3658

## 1. Bằng chứng

| Tệp | Có? |
| --- | --- |
| [`day01_lab.ipynb`](day01_lab.ipynb) — notebook đã chạy, còn output | ☐ |
| [`lab1_report.json`](lab1_report.json) — do ô "Báo cáo cuối" sinh ra | ☐ |
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | ☐ |

## 2. Kết quả chính

*Chép bảng so sánh cuối Phần 5 (F1 test, F1 SUB 40, p95 ms/tin, USD/1k tin) của chính bạn.*
                                     f1  f1_SUB   p95 ms/tin
1. BoW + LogReg (tự code)        0.867   0.889    0.09ms   
2. BoW + MLP (deep learning)     0.921   0.947    0.38ms
3a. Encoder (MiniLM) + kNN       0.936   0.950    139.12ms
3b. Encoder (MiniLM) + LogReg    0.948   0.947    39.03ms
3c. Encoder (MiniLM) + MLP head  0.939   0.947    41.28ms
4a. LLM nhỏ zero-shot            0.878   0.878    2496.71ms
4b. LLM lớn zero-shot            0.857   0.857    2496.98ms
4c. LLM nhỏ few-shot             0.900   0.900    4113.18ms

## 3. Dự đoán và thực tế

*Chọn 3 dự đoán `predict(...)` lệch nhiều nhất so với kết quả. Mỗi cái: bạn đoán gì, ra gì, vì sao lệch.*
Dự đoán BoW tôi đoán 60-70%. Thực tế chỉ đạt 52% do cơ chế sửa các ký tự của từ khoá và bẫy ngữ cảnh
Dự đoán LLM lớn hơn LLM nhỏ ở điểm F1 khoảng 0.1. Thực tế ngược lại LLM nhỏ lớn hơn do các tác vụ phân loại ngắn và cấu trúc cố định, đối với mô hình lớn dễ bị nghĩ quá sâu và bị đánh lừa bởi ngữ cảnh phức tạp.
Dự đoán Encoder+LogReg nằm bên phải vạch SLA 50ms nhưng thực tế nó nằm bên trái, do đánh giá cao độ trễ của việc forawrd qua encoder cục bộ.

## 4. Phân tích 50 case khó

*Model nào tụt nhiều nhất, ở nhóm case nào? Nêu 2 case cụ thể và cơ chế khiến model sai.*
BoW+LogReg tụt nhiều nhất từ 0.867 -> 0.333 gặp khó ở các case né từ khoá và ngữ cảnh giống spam nhưng không phải.
"C0ngr4tulat10ns!..." BoW chỉ tách từ rời rạc theo từ điển mà từ này được sửa và không nhận diện được trong từ điển nên model bỏ qua
"FREE pizza..." sự xuất hiện của từ khoá spam mạnh nên ngay lập tức đoán là spam

## 5. Thử thách cá nhân

*Đề theo SEED của bạn, cách làm, kết quả, giải thích.*

## 6. Phương án cho sản phẩm thật

*Với yêu cầu p95 < 50 ms/tin cho 10 triệu SMS/ngày: chọn phương án nào, dẫn số liệu nào, chấp nhận đánh đổi gì.*
Sử dụng Encoder+LogReg: độ trễ p95 đạt 39.03ms < 50ms. F1 = 0.948
Chấp nhận một lượng chi phí tính toán ban đầu cho phần cứng chạy mô hình Encoder cục bộ

## 7. AI log và điều đã học

*Mỗi lần dùng AI: hỏi gì, nhận gì, bạn sửa gì. Sau đó 2–3 câu: điều bạn hiểu ra hôm nay mà trước đó chưa hiểu.*
Cách vecto hoá BoW, gợi ý syntax khởi tạo ma trận. Tính Cosine Similarity. Hỏi AI các câu hỏi ở dưới phần code của các phần để hiểu rõ hơn về đặc tính của từng loại model. Gợi ý các biện pháp chống lại Prompt Injection khỏi LLM.
Hiểu biết hơn và cách xây dựng model BoW, Embedding và LLM. Sự đánh đổi về độ chính xác mô hình và độ trễ.


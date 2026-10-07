# Báo Cáo Lab Day 21 - CI/CD cho AI Systems

| | |
|---|---|
| Họ và tên | Tran Van Khanh |
| MSSV | 2A202602413 |
| Lớp / Khóa | K4 |
| Repo GitHub | ___ |
| Ngày nộp | 07/10/2026 |

---

## 1. Bộ Siêu Tham Số Đã Chọn và Lý Do

| Lần chạy | n_estimators | learning_rate | max_depth | f1_score | accuracy |
|---|---|---|---|---|---|
| 1 | 100 | 0.1 | 3 | 0.7109 | 0.8780 |
| 2 | 50 | 0.05 | 2 | 0.6051 | 0.8460 |
| 3 | 200 | 0.1 | 5 | 0.7149 | 0.8740 |

**Bộ siêu tham số đã chọn:** `n_estimators=200`, `learning_rate=0.1`, `max_depth=5`.

**Lý do:** Bộ tham số thứ 3 cho điểm f1_score cao nhất (0.7149), vượt ngưỡng chất lượng 0.65 của bài toán, đồng thời accuracy vẫn xấp xỉ lần 1 nhưng f1_score cải thiện do mô hình sâu hơn (max_depth=5) và nhiều cây hơn (n_estimators=200) giúp học được đặc trưng phức tạp của dữ liệu. Accuracy và f1_score không phải lúc nào cũng tương đồng với nhau trong bài toán dữ liệu mất cân bằng. Ngoài ra, việc giảm learning rate đòi hỏi phải tăng n_estimators để mô hình học đủ tốt (đánh đổi giữa 2 tham số).

---

## 2. Vì Sao Ngưỡng Chất Lượng Đặt Trên F1 Chứ Không Phải Accuracy

Trong bài toán này, tập dữ liệu có sự mất cân bằng lớp đáng kể, với chỉ 24.8% mẫu thuộc lớp thu nhập cao (>50K). Nếu sử dụng một mô hình "luôn trả lời thu nhập thấp", độ chính xác (accuracy) sẽ đạt tới mức 75.2%. Tuy nhiên, mô hình như vậy hoàn toàn vô dụng vì không thể dự đoán được trường hợp nào thuộc lớp thu nhập cao. F1 score của lớp dương khắc phục điều này bằng cách kết hợp cả Precision và Recall, phản ánh đúng khả năng phát hiện các trường hợp thu nhập cao. Đồng thời, không dùng average="weighted" hoặc average="macro" vì các giá trị này sẽ bị kéo lên bởi lớp đa số (thu nhập thấp), làm mất đi độ nhạy cảm cần thiết trong việc đánh giá hiệu suất thực sự đối với lớp thiểu số.

---

## 3. Khó Khăn Gặp Phải và Cách Giải Quyết

| Khó khăn | Nguyên nhân | Cách giải quyết |
|---|---|---|
| Xung đột phiên bản thư viện mlflow và sqlalchemy | mlflow 2.13.0 không tương thích với sqlalchemy phiên bản 2.1.3 mới nhất | Cài đặt `sqlalchemy<2.1` để tương thích |
| Lỗi import khi chạy script prepare_data | Môi trường ảo mặc định (Python 3.13) không hỗ trợ build scikit-learn cũ | Sử dụng Python 3.11 tạo virtual environment |
| ___ | ___ | ___ |

---

## 4. So Sánh Bước 2 và Bước 3 (bắt buộc, 2 - 3 câu)

| | f1_score | accuracy |
|---|---|---|
| Bước 2 (chỉ `train_batch1`) | 0.7149 | 0.8740 |
| Bước 3 (thêm `train_batch2`) | 0.7354 | 0.8820 |

**Nhận xét:** Việc bổ sung thêm dữ liệu mới giúp cải thiện một chút f1_score và accuracy. Mặc dù dữ liệu mới có cùng phân phối, nhưng với kích thước mẫu tăng gấp đôi (22.361 -> 44.722), GradientBoostingClassifier học được các ranh giới quyết định (decision boundary) tốt hơn và giảm thiểu overfitting, dẫn đến hiệu suất trên tập holdout tăng lên nhẹ. Quan trọng hơn là hệ thống đã có thể tự động chạy lại toàn bộ pipeline để cập nhật mô hình mới khi có dữ liệu vào.

---

## 5. Phần Bonus Đã Thực Hiện (nếu có)

- [ ] Bonus 1 - Tracking MLflow từ xa với DagsHub: ___
- [ ] Bonus 2 - Điều chỉnh ngưỡng quyết định: ___
- [ ] Bonus 3 - Báo cáo precision / recall tự động: ___
- [ ] Bonus 4 - Hoàn trả về phiên bản trước: ___
- [ ] Bonus 5 - Cảnh báo lệch lạc dữ liệu: ___

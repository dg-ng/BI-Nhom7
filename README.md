# 📊 Phân tích Hành vi và Dự báo Giá trị Khách hàng ứng dụng Machine Learning

> **Đồ án môn học** | Hệ hỗ trợ quản trị thông minh (26D1INF50908501)  
> **Giảng viên hướng dẫn:** ThS. Hồ Thị Thanh Tuyến  
> **Trường:** Đại học Kinh tế TP. Hồ Chí Minh – Khoa Công nghệ Thông tin Kinh doanh

---

## 👥 Nhóm thực hiện

| Họ và tên | MSSV |
|---|---|
| Huỳnh Thụy Mai Nguyên | 31231020894 |
| Nguyễn Đỗ Nhật Minh | 31231020749 |
| Dương Phương Anh | 31231021067 |
| Tăng Gia Hoàng | 31231021890 |
| Nguyên Lợi Thanh Dũng | 31231021226 |

---

## 📌 Giới thiệu dự án

Dự án xây dựng một hệ thống **Business Intelligence** toàn diện cho lĩnh vực ngân hàng, phân tích hành vi giao dịch thẻ tín dụng của khách hàng nhằm:

- 🔍 **Phân khúc khách hàng** theo mô hình RFM (Recency – Frequency – Monetary)
- 📈 **Dự báo giá trị vòng đời khách hàng** (Customer Lifetime Value – CLV) 6 tháng bằng XGBoost
- ⚠️ **Phát hiện sớm rủi ro suy giảm hành vi** (Behavioral Decline) bằng Logistic Regression
- 🗺️ **Trực quan hóa** toàn bộ kết quả qua dashboard Plotly Dash 5 tab tương tác

---

## 🗂️ Cấu trúc thư mục

```
├── preprocess+model/
│   ├── RFM.ipynb                           # Mô hình phân khúc RFM
│   ├── BI-CLV.ipynb                        # Mô hình dự báo CLV (XGBoost + K-Means + PCA)
│   └── Churn Analysis & Prediction.ipynb  # Mô hình dự đoán Behavioral Decline (Logistic Regression)
│   └── preprocess BI.ipynb                 # ETL & tiền xử lý dữ liệu
├── my-dashboard                           # Source code Plotly Dash dashboard
├── Financial Transactions.rar              # Dữ liệu gốc (giải nén vào data/)
└── README.md
```

---

## 📂 Dữ liệu

Bộ dữ liệu gồm **~1.04 triệu giao dịch thẻ tín dụng** của 136 khách hàng trong giai đoạn 2010–2019, tổ chức thành 4 file:

| File | Mô tả |
|---|---|
| `transactions_data.csv` | Lịch sử giao dịch (số tiền, thời gian, MCC, lỗi...) |
| `users_data.csv` | Thông tin khách hàng (tuổi, giới tính, thu nhập, điểm tín dụng...) |
| `cards_data.csv` | Thông tin thẻ (loại thẻ, hạn mức, thương hiệu...) |
| `mcc_codes.json` | Bảng tra cứu mã ngành hàng quốc tế (MCC) |

---

## 🚀 Hướng dẫn cài đặt & Chạy ứng dụng

### Yêu cầu hệ thống

- Python **3.8+**
- RAM tối thiểu **8GB** (khuyến nghị 16GB)

---

### Bước 1 – Clone repository

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
```

---

### Bước 2 – Cài đặt thư viện

```bash
pip install pandas numpy scikit-learn xgboost imbalanced-learn plotly dash dash-bootstrap-components scipy matplotlib seaborn joblib jupyter
```

---

### Bước 3 – Chuẩn bị dữ liệu

Giải nén `Financial Transactions.rar` vào thư mục `data/`:

```
data/
├── transactions_data.csv
├── cards_data.csv
├── users_data.csv
└── mcc_codes.json
```

---

### Bước 4 – Tiền xử lý dữ liệu

Chạy notebook `preprocess BI.ipynb` để sinh ra file `preprocess_data.csv`:

```bash
jupyter notebook "preprocess BI.ipynb"
```


---

### Bước 5 – Huấn luyện mô hình

Chạy lần lượt 3 notebook trong thư mục `model/`:

```bash
jupyter notebook model/RFM.ipynb
jupyter notebook model/BI-CLV.ipynb
jupyter notebook "model/Churn Analysis & Prediction.ipynb"
```

Các file kết quả (CSV, pickle) sẽ được export tự động vào thư mục `data/`.

---

### Bước 6 – Chạy Dashboard

```bash
cd my-dashboard
python app.py
```

Mở trình duyệt và truy cập: **[http://127.0.0.1:8050](http://127.0.0.1:8050)**

> 💡 Lần khởi động đầu tiên có thể mất **1–3 phút** do pre-aggregate ~1 triệu dòng dữ liệu vào bộ nhớ.

---

## 📊 Tính năng Dashboard

Dashboard Plotly Dash gồm **5 tab** phục vụ các nhóm người dùng khác nhau:

| Tab | Dành cho | Nội dung |
|---|---|---|
| **Overview** | Ban quản trị | KPI tổng quan, doanh thu & khối lượng giao dịch theo thời gian, top ngành hàng |
| **Behavior** | Product Manager | Phân khúc RFM, xu hướng theo thời gian, bản đồ chi tiêu theo bang, heatmap giờ giao dịch |
| **Risk & Customer** | Risk Manager | Phân tích 7 loại lỗi giao dịch, hồ sơ khách hàng theo credit score, độ tuổi, giới tính |
| **CLV Intelligence** | Marketing Strategy | Dự báo CLV 6 tháng, phân cụm khách hàng, top 10 khách hàng ưu tiên chiến lược |
| **Decline Analysis** | Retention Team | Xu hướng decline, phân tích pattern suy giảm, category mix, heatmap theo tháng |

---

## 🤖 Mô hình Machine Learning

| Mô hình | Mục tiêu | Kết quả |
|---|---|---|
| **RFM + K-Means** | Phân khúc khách hàng (Champion, Loyal, Potential, Need Attention, At Risk) | 5 phân khúc, theo dõi theo chu kỳ 6 tháng |
| **XGBoost Regression** | Dự báo CLV 6 tháng | R² = 0.8998, MAE ≈ $2,067 |
| **PCA + K-Means** | Phân cụm khách hàng theo hành vi + CLV | 3 cụm tối ưu (Silhouette + Elbow) |
| **Logistic Regression** | Dự đoán nguy cơ suy giảm hành vi | ROC-AUC = 0.80, Recall = 69% |

---

## 🏗️ Kiến trúc hệ thống

```
Phase 1 – Raw Data
    transactions.csv + cards.csv + users.csv + mcc_codes.json
            ↓
Phase 2 – Preprocessing (clean + merge + transform)
    Clean transactions → Join MCC codes → Merge customer data → master dataset
            ↓
Phase 3 – Feature Engineering
    RFM metrics | Spending patterns (% per MCC) | Behavioral features
            ↓
Phase 4 – Model Training
    K-Means (segmentation) | XGBoost (CLV) | Logistic Regression (Churn/Decline)
            ↓
Phase 5 – Visualization
    Plotly Dash Dashboard (Interactive charts, KPIs, Filters)
```

---

## 📋 Insight kinh doanh nổi bật

- **Decline không đột ngột** – khách hàng trải qua giai đoạn mất ổn định chi tiêu (`spending_std` tăng) trước khi rời bỏ.
- **3 pattern decline phổ biến nhất:** High Volatility (~30%), Fewer but Bigger Transactions (~16%), Essential Shift (~13%).
- **Need Attention chiếm 29%** tổng khách hàng với tỷ lệ phục hồi rất thấp (chỉ 21% thăng lên Potential) – cần can thiệp chủ động.
- **Insufficient Balance chiếm 60%** tổng lỗi giao dịch, tập trung ở nhóm tín dụng thấp (Very Poor).
- **Cụm 1 (CLV)** có thu nhập cao nhất nhưng chưa khai thác hết – tiềm năng tăng trưởng lớn nhất trong 6 tháng tới.

---


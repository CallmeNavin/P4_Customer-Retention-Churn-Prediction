**Telco Customer Churn Prediction**

**1. Mục tiêu**
Dự đoán khả năng khách hàng rời bỏ (**Churn**) dựa trên dữ liệu giao dịch và thông tin hợp đồng, từ đó hỗ trợ chiến lược giữ chân khách hàng hiệu quả.

**2. Dataset**
- Nguồn: Kaggle – WA_Fn-UseC_-Telco-Customer-Churn.csv  
- Số mẫu: 7,043 khách hàng  
- Biến đầu vào: 30 (sau xử lý)  
- Tỉ lệ churn: 26.6% khách hàng rời, 73.4% ở lại → dữ liệu mất cân bằng

**3. Xử lý dữ liệu**
- Chuyển `TotalCharges` từ object → float, xử lý 11 giá trị thiếu  
- One-hot encoding cho các biến categorical (`drop_first=True`)  
- Loại bỏ `customerID` vì không mang ý nghĩa dự đoán  
- Chuẩn hóa dữ liệu numeric trước khi huấn luyện mô hình

**4. Phân tích tương quan**
Numeric (Point Biserial Correlation)
| Feature        | r_pb    | Giải thích |
|----------------|---------|------------|
| tenure         | -0.354  | Khách hàng ở lâu → ít rời |
| TotalCharges   | -0.199  | Giá trị thanh toán tích lũy thấp → dễ rời |
| MonthlyCharges | +0.193  | Phí hàng tháng cao → dễ rời |

Categorical (Cramér’s V)
| Feature                          | V       | Giải thích |
|-----------------------------------|---------|------------|
| InternetService_Fiber optic      | 0.307   | Liên quan mạnh tới churn |
| Contract_Two year                 | 0.301   | Hợp đồng dài hạn → giảm churn |
| PaymentMethod_Electronic check    | 0.301   | Liên quan mạnh tới churn |

**6. Chiến lược lựa chọn model**
- Muốn bắt hết churn (ưu tiên Recall) → Logistic Regression tuned (Recall ~ 79.7%)  
- Muốn cân bằng Precision & Recall → Random Forest tuned (F1 = 0.6323, ROC-AUC cao nhất)

**7. Insight chính cho doanh nghiệp**
- Phí hàng tháng cao và dùng Fiber optic → rủi ro rời cao  
- Hợp đồng 2 năm và thời gian gắn bó lâu → giữ chân khách tốt  
- Thanh toán Electronic check → cần xem lại trải nghiệm / quy trình thanh toán

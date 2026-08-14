# Báo cáo Thực hành Lab 16: Cloud AI Environment Setup

- **Học viên:** Nguyễn Tiến
- **Mã số / Account:** Tienes2810 / 701935371831
- **Provider:** Amazon Web Services (AWS) - Region `us-east-1`
- **Kiến trúc hạ tầng:** Triển khai tự động bằng Terraform gồm VPC, Public/Private Subnets, NAT Gateway, Bastion Host, Load Balancer, và Compute Node (`t3.micro`).

---

## 1. Kết quả Huấn luyện & Benchmark mô hình (LightGBM)

| Chỉ số / Metric | Kết quả đạt được | Nhận xét |
| :--- | :---: | :--- |
| **Thời gian load dataset** | `2.3678 s` | Tập dữ liệu Credit Card Fraud gồm 284,807 bản ghi. |
| **Thời gian training** | `3.8974 s` | Huấn luyện 100 cây quyết định LightGBM trên CPU 2 vCPU. |
| **Best iteration** | `100` | Mô hình hội tụ tốt. |
| **AUC-ROC** | `0.8618` | Phân biệt tốt giữa giao dịch gian lận và hợp lệ. |
| **Accuracy** | `0.9981` (99.81%) | Độ chính xác tổng thể cao. |
| **F1-Score** | `0.5668` | Cân bằng giữa Precision và Recall trên tập mất cân bằng. |
| **Precision** | `0.4698` | Tỷ lệ phát hiện đúng trong các ca cảnh báo gian lận. |
| **Recall** | `0.7143` | Bắt được 71.43% tổng số ca gian lận thực tế. |
| **Inference Latency (1 row)** | `1.225 ms` | Độ trễ cực thấp, đáp ứng thời gian thực (Real-time). |
| **Inference Throughput** | `231,764 rows/s` | Xử lý lượng lớn giao dịch đồng thời nhanh chóng. |

---

## 2. Nhận xét đánh giá
- **Hiệu năng:** Mặc dù sử dụng instance CPU nhỏ (`t3.micro` với 1GB RAM + 2GB Swap), LightGBM cho tốc độ nạp dữ liệu và huấn luyện cực kỳ ấn tượng (chỉ mất ~3.9 giây cho gần 300,000 dòng dữ liệu).
- **Độ trễ dự đoán:** Tốc độ inference 1.225 ms/giao dịch cho thấy mô hình hoàn toàn đủ khả năng triển khai thực tế vào các hệ thống phát hiện gian lận thanh toán trực tuyến.
- **Quản lý hạ tầng & Chi phí:** Toàn bộ quá trình khởi tạo và thu hồi tài nguyên (27 resources) được tự động hóa 100% bằng Terraform. Hạ tầng được dọn dẹp sạch sẽ ngay sau khi hoàn thành đo đạc, bảo toàn 100% credit AWS ($140.00 khả dụng).

---

## 3. Danh sách Bằng chứng (Evidence)
- `evidence.png`: Terminal output quá trình chạy `benchmark.py` và kết quả đánh giá mô hình.
- `evidence2.png` & `evidence3.png`: File kết quả `benchmark_result.json`, thông số RAM (`free -h`) và Network (`ip -s link`).
- `credit.png` & `bills.png`: AWS Billing & Credit Dashboard ($140.00 remaining, $0.00 used).

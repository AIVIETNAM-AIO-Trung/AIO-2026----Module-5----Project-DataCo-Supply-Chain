# DataCo Supply Chain - Dự đoán giao hàng trễ

Project luyện tập AIO 2026 - Module 5. Bài toán **binary classification**: dự đoán đơn hàng có bị giao trễ hay không (`Late_delivery_risk`).

## Submission 1
| File | Nội dung |
|---|---|
| `BaoCao_Submission1.pdf` | Báo cáo (biên dịch từ LaTeX) |
| `report/BaoCao_Submission1.tex` | Mã nguồn LaTeX của báo cáo |
| `DataCo_Submission1.ipynb` | Notebook (EDA, tiền xử lý) đã chạy kèm output |
| `DataCo_Submission1.html` | Bản HTML của notebook |
| `figures/` | Các biểu đồ dùng trong báo cáo |
| `results/` | Danh sách các cột bị loại kèm lý do |

## Submission 2
| File | Nội dung |
|---|---|
| `BaoCao_Submission2.pdf` | Báo cáo tiến độ (biên dịch từ LaTeX) |
| `report/BaoCao_Submission2.tex` | Mã nguồn LaTeX của báo cáo |
| `DataCo_Submission2.ipynb` | Notebook (làm sạch, EDA, EDA thời gian, baseline, mô hình) đã chạy kèm output |
| `DataCo_Submission2.html` | Bản HTML của notebook |
| `figures_s2/` | Các biểu đồ của Submission 2 |
| `results_s2/` | Danh sách cột bị loại, kết quả baseline, mô hình và 5 cách chia |

**Kết quả chính** (mức đơn, tập test 20%, `random_state=42`): chỉ áp dụng cho đơn không bị hủy.

| Cách làm | Accuracy |
|---|---|
| Luôn đoán trễ | 0,5731 |
| Baseline 1: Shipping Mode | 0,6984 |
| Baseline 2: Shipping Mode + quy tắc 12h của Same Day | 0,7242 |
| Logistic Regression + feature tương tác | 0,7242 |
| Decision Tree (`max_depth=6`) | 0,7238 |

Với các feature hiện có, mô hình chỉ học lại quy tắc đơn giản, chưa vượt được baseline 2.

## Nhật ký
| File | Nội dung |
|---|---|
| `nhat_ky/NhatKy_2026-10-29.pdf` | Nhật ký ngày 29/10/2026: khái niệm đã học và các findings đầu tiên (nguồn LaTeX cùng thư mục) |
| `nhat_ky/NhatKy_2026-11-05.pdf` | Nhật ký cập nhật đến 05/11/2026: các việc cần làm, số quan trọng và điều học được (nguồn LaTeX cùng thư mục) |

## Quy trình làm bài

```
Đọc mô tả dữ liệu → Xác định bài toán → Tổng quan & chất lượng dữ liệu → Kiểm tra leakage
→ EDA → Tiền xử lý → Báo cáo
```

1. **Đọc file mô tả & dữ liệu** – đọc `DescriptionDataCoSupplyChain.csv`, load dataset (encoding `latin-1`), hiểu mỗi dòng là 1 *order item*.
2. **Xác định bài toán** – so sánh 3 cột có thể làm target (`Late_delivery_risk`, `Delivery Status`, `Order Status`) → chọn **binary classification** với `Late_delivery_risk` (54,8% / 45,2%, khá cân bằng).
3. **Tổng quan & chất lượng dữ liệu** – kiểu dữ liệu, missing, dòng trùng lặp, cột chỉ có 1 giá trị, các cặp cột giống hệt nhau.
4. **Kiểm tra data leakage** – tìm các cột chỉ biết sau khi giao hàng (`Delivery Status`, `Days for shipping (real)`, `shipping date`, `Order Status`), chứng minh bằng crosstab và luật so sánh số ngày giao thực tế với số ngày dự kiến.
5. **EDA** – tỉ lệ trễ theo từng nhóm, so với mức trung bình 54,83%: vận chuyển → địa lý → sản phẩm & khách hàng → biến số & tương quan.
6. **Tiền xử lý** – bỏ các cột leakage, định danh, missing nhiều, trùng lặp, quá nhiều giá trị; one-hot encoding cho các cột chữ.
7. **Báo cáo** – tổng hợp kết quả vào báo cáo LaTeX; mọi hình/bảng đều lưu thành file trong `figures/` và `results/` làm minh chứng.

Submission 2 tiếp tục sau bước kiểm tra leakage và bổ sung:

8. **Làm sạch** – bỏ 7.754 dòng đơn hủy (2.855 đơn), còn 172.765 dòng và 62.897 đơn; chạy lại EDA.
9. **EDA thời gian** – phân tích ở mức đơn theo giờ, tháng, thứ và thay đổi từ 10/2017.
10. **Chia dữ liệu và baseline** – đánh giá ở mức đơn, chia 80/20, đặt 3 mốc so sánh.
11. **Mô hình** – Logistic Regression và Decision Tree, so với baseline trên 5 cách chia.

**Trạng thái:**
- [x] Submission 1: bước 1 → 7
- [x] Submission 2: bước 8 → 11
- [ ] Chia train/test theo thời gian
- [ ] Tìm feature mới cho Second Class và Standard Class
- [ ] Viết blog và làm video

## Dữ liệu
File `DataCoSupplyChainDataset.csv` (~91.5MB) không được đưa lên repo. Tải từ Kaggle: *DataCo SMART SUPPLY CHAIN FOR BIG DATA ANALYSIS* và đặt cùng thư mục với notebook. File mô tả cột: `DescriptionDataCoSupplyChain.csv`.

## Chạy lại
```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
jupyter notebook DataCo_Submission2.ipynb
```

Biên dịch lại báo cáo (cần XeLaTeX hoặc tectonic):
```bash
cd report && xelatex BaoCao_Submission2.tex
```

Nhật ký biên dịch tương tự trong thư mục `nhat_ky/`. Báo cáo và nhật ký dùng font Times New Roman (cần có sẵn trên máy).

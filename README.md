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

> Phần EDA theo thời gian (ngày/giờ đặt hàng) tạm thời chưa đưa vào, sẽ nghiên cứu và bổ sung sau.

| File | Nội dung |
|---|---|
| `nhat_ky/NhatKy_2026-10-29.pdf` | Nhật ký ngày 29/10/2026: khái niệm đã học và các findings đầu tiên (nguồn LaTeX cùng thư mục) |

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

**Trạng thái:**
- [x] Submission 1: bước 1 → 7
- [ ] EDA theo thời gian (ngày/giờ đặt hàng)
- [ ] Xây dựng mô hình (đang thử nghiệm, chưa chốt)

## Dữ liệu
File `DataCoSupplyChainDataset.csv` (~91.5MB) không được đưa lên repo. Tải từ Kaggle: *DataCo SMART SUPPLY CHAIN FOR BIG DATA ANALYSIS* và đặt cùng thư mục với notebook. File mô tả cột: `DescriptionDataCoSupplyChain.csv`.

## Chạy lại
```bash
pip install pandas numpy matplotlib seaborn jupyter
jupyter notebook DataCo_Submission1.ipynb
```

Biên dịch lại báo cáo (cần XeLaTeX hoặc tectonic):
```bash
cd report && xelatex BaoCao_Submission1.tex
```

Nhật ký biên dịch tương tự trong thư mục `nhat_ky/`. Báo cáo và nhật ký dùng font Times New Roman (cần có sẵn trên máy).

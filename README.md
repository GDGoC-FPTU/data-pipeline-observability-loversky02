[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23573973&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** 26ai.vuongtdm@vinuni.edu.vn
**Name:** Trần Đình Minh Vương

---

## Mô tả

Bài lab này xây dựng một ETL (Extract-Transform-Load) Pipeline tự động để xử lý dữ liệu sản phẩm và kiểm tra tác động của data quality lên AI Agent.

**Những gì đã làm:**
- Xây dựng ETL pipeline với 4 bước: Extract → Validate → Transform → Load
- Validate dữ liệu: loại bỏ records có giá ≤ 0 hoặc category rỗng
- Transform: tính giá giảm 10%, chuẩn hóa category sang Title Case, thêm timestamp
- Chạy thí nghiệm so sánh Agent response với clean data vs garbage data
- Phân tích tác động của data quality issues (outliers, duplicates, wrong types, nulls) lên AI decision-making

---

## Cách chạy (How to Run)

### Prerequisites
```bash
pip install pandas
```

### Chạy ETL Pipeline
```bash
python solution.py
```

### Chạy Agent Simulation (Stress Test)
```bash
# Bước 1: Tạo garbage data
python generate_garbage.py

# Bước 2: Chạy thí nghiệm với cả clean và garbage data
python agent_simulation.py
```

Kết quả thí nghiệm được ghi trong `experiment_report.md`

---

## Cấu trúc thư mục

```
├── solution.py              # ETL Pipeline script
├── raw_data.json            # Dữ liệu nguồn (5 records)
├── processed_data.csv       # Output của pipeline (clean data)
├── generate_garbage.py      # Script tạo garbage data
├── garbage_data.csv         # Dữ liệu bị "poison" cho thí nghiệm
├── agent_simulation.py      # Script test Agent với clean vs garbage data
├── experiment_report.md     # Báo cáo thí nghiệm chi tiết
└── README.md                # File này
```

---

## Kết quả

**ETL Pipeline:**
- Tổng records đầu vào: 5 records (từ `raw_data.json`)
- Records hợp lệ: 3 records (Laptop, Chair, Monitor)
- Records bị loại: 2 records
  - Record #3: Mystery Box (price = -10, giá âm không hợp lệ)
  - Record #4: Phone (category rỗng)
- Output: `processed_data.csv` với 3 records đã được transform

**Agent Simulation:**
- Clean Data: Agent trả lời chính xác (Laptop $1200) - Accuracy: 10/10
- Garbage Data: Agent trả lời sai (Nuclear Reactor $999,999) - Accuracy: 2/10
- Kết luận: Quality Data > Quality Prompt - dữ liệu tốt quan trọng hơn prompt tốt

Chi tiết phân tích xem trong `experiment_report.md`

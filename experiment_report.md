# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** 2A202600495
**Name:** Trần Đình Minh Vương
**Date:** 15/04/2026

---

## 1. Kết quả thí nghiệm

Chạy `agent_simulation.py` với 2 bộ dữ liệu và ghi lại kết quả:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200. | 10 | Correct response - Laptop is the highest priced electronics item in clean data |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 2 | Incorrect - Nuclear Reactor is an extreme outlier/poisoned data, not a realistic product |

---

## 2. Phân tích & nhận xét (Phan tich & nhan xet)

### Tại sao Agent trả lời sai khi dùng Garbage Data?

Agent trả lời sai vì dữ liệu garbage chứa nhiều vấn đề chất lượng nghiêm trọng:

**1. Extreme Outliers (Giá trị ngoại lệ):** "Nuclear Reactor" với giá $999,999 là một outlier không thực tế. Agent chỉ dùng logic đơn giản (chọn sản phẩm có giá cao nhất) nên chọn sản phẩm này, mặc dù nó không phải là một electronics product hợp lý trong thực tế.

**2. Duplicate IDs:** Có 2 records với id=1 (Laptop và Banana). Điều này gây nhiễu loạn trong dữ liệu vì mỗi ID nên là duy nhất. Khi xử lý, pandas có thể ghi đè hoặc giữ cả hai records, dẫn đến kết quả không nhất quán.

**3. Wrong Data Types:** Giá "ten dollars" (string) thay vì số (integer/float) cho "Broken Chair" gây lỗi khi thực hiện các phép tính toán hoặc so sánh giá. Agent có thể bị crash hoặc bỏ qua record này.

**4. Null Values:** Record với id=None và category=None làm cho dữ liệu không đầy đủ. Nếu Agent không xử lý null values đúng cách, có thể gây lỗi hoặc trả về kết quả sai.

**Kết quả:** Agent tin tưởng vào dữ liệu mà không có cơ chế validation, nên trả lời "Nuclear Reactor" - một gợi ý hoàn toàn vô lý và nguy hiểm cho người dùng. Đây là ví dụ rõ ràng cho thấy dữ liệu xấu dẫn đến AI decision-making tồi tệ.

---

## 3. Kết luận

**Quality Data > Quality Prompt?** (Đồng ý hay không? Giải thích ngắn gọn.)

**Đồng ý.** Thí nghiệm này chứng minh rằng dữ liệu chất lượng cao quan trọng hơn prompt tốt. Ngay cả khi prompt rõ ràng ("What is the best electronic product?"), nếu dữ liệu bị "poison" với outliers và lỗi, Agent vẫn trả lời sai hoàn toàn (Nuclear Reactor $999,999). 

Một prompt xuất sắc không thể sửa chữa được dữ liệu tệ. Ngược lại, với clean data, ngay cả logic Agent đơn giản cũng cho kết quả chính xác (Laptop $1200). 

**Bài học:** Trong AI/ML systems, "Garbage In = Garbage Out". Data quality phải là ưu tiên hàng đầu trước khi tối ưu hóa prompts hay models. Data Observability và validation là bước không thể thiếu trong mọi pipeline.

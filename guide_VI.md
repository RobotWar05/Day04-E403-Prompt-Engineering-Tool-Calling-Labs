# Hướng Dẫn

## 1. Bắt Đầu Với Baseline

Chạy weak baseline trước:

```bash
python grade/scoring.py --module simple_solution.agent.graph --provider google
```

Việc này cho bạn điểm khởi đầu. Nhiệm vụ của bạn là cải thiện `src/` và vượt qua nó.

## 2. Hiểu Task

Agent nên xử lý bốn hành vi:

- tạo đơn hàng hợp lệ
- clarification khi thiếu thông tin khách hàng
- refusal khi request vi phạm policy
- xác nhận có căn cứ sau khi lưu thành công

Với một đơn hàng hợp lệ, tool flow dự kiến là:

1. `list_products`
2. `get_product_details`
3. `get_discount`
4. `calculate_order_totals`
5. `save_order`

## 3. Làm Việc Ở Đâu

Tập trung vào:

- `src/agent/graph.py`
- `src/utils/data_store.py`

Tham khảo hữu ích:

- `data/graded_cases.json`
- `data/expected_orders/`
- `simple_solution/`

## 4. Cần Cải Thiện Gì

### Prompt

System prompt của bạn nên nêu rõ các quy tắc này:

- trả lời bằng tiếng Việt
- không bịa product facts, discounts, totals hoặc file paths
- hỏi thông tin khách hàng còn thiếu trước bất kỳ tool call nào
- từ chối unsafe requests mà không gọi tools
- đi theo expected tool order
- chỉ save sau khi validation thành công

### Tool Schema

Tool schema tốt giúp giảm lỗi của agent. Ưu tiên:

- tool names rõ ràng
- docstrings rõ ràng
- required arguments rõ ràng
- structured inputs khớp với workflow

### Guardrails

Agent nên từ chối các request yêu cầu:

- bypass stock
- ép fake discounts
- tạo fake invoices
- bỏ qua catalog hoặc policy

### Clarification

Trước khi dùng tools, agent nên có:

- tên khách hàng
- số điện thoại
- email
- địa chỉ giao hàng
- ít nhất một item và quantity

Nếu thiếu bất kỳ thông tin nào, nó nên hỏi lại và dừng.

## 5. Debug Như Thế Nào

Khi một case fail, kiểm tra:

- tool trace: model có gọi tools quá sớm hoặc sai thứ tự không?
- saved JSON: nó có save sai payload hoặc save khi không nên save không?
- final answer: clarification, refusal hoặc confirmation có grounded và concise không?

## 6. Vòng Lặp Cải Thiện

Dùng vòng lặp này:

1. chạy `simple_solution`
2. chạy `src`
3. xem các failing cases
4. siết prompt
5. siết tool schema
6. chạy lại grader

Chạy phần triển khai của bạn bằng:

```bash
python grade/scoring.py --module src.agent.graph --provider google
```

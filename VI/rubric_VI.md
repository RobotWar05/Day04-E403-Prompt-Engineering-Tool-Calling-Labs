# Rubric

## Grader Kiểm Tra Gì

Grader kết hợp deterministic behavior checks với một LLM judge.

Nó xem xét:

- độ đúng của saved JSON
- độ đúng của tool usage
- chất lượng final answer

## Save Cases

Với các case tạo đơn hàng bình thường, grader kiểm tra:

- `saved_order` được trả về
- file đã lưu trong `artifacts/orders/`
- nội dung JSON so với `data/expected_orders/`
- required tool sequence
- final answer theo một rubric

Trọng số điển hình:

- `json_output`: 70
- `tools`: 20
- `llm_judge`: 10

`created_at` được bỏ qua trong quá trình so sánh JSON.

## Non-Save Cases

Với các case clarification, refusal và stock-failure, grader kiểm tra:

- không có đơn hàng nào được lưu
- tool trace khớp với expected behavior
- final answer phù hợp với case rubric

Trọng số điển hình:

- `json_output`: 55
- `tools`: 25
- `llm_judge`: 20

## Tool Expectations

Với valid orders, expected workflow là:

1. `list_products`
2. `get_product_details`
3. `get_discount`
4. `calculate_order_totals`
5. `save_order`

Với clarification và refusal cases, expected tool usage thường là không dùng tools.

## Sinh Viên Mất Điểm Như Thế Nào

- prompt quá mơ hồ, nên model hành động quá sớm
- tool schema quá lỏng, nên arguments bị thiếu hoặc sai
- guardrails yếu, nên model chấp nhận invalid requests
- grounding yếu, nên saved JSON bị sai
- câu trả lời clarification/refusal chất lượng thấp, nên LLM judge trừ điểm

## Diễn Giải Điểm Số

- `90-100`: kiểm soát hành vi mạnh
- `80-89`: hầu hết đúng, còn vài lỗi quality hoặc workflow
- `65-79`: kiểm soát một phần, vẫn còn quá lỏng
- `0-64`: prompt/schema/guardrail design yếu

## Lưu Ý Quan Trọng

Lab này không chỉ nói về business logic. Điểm thấp thường đến từ prompt engineering yếu:

- instructions không rõ
- tools chưa được đặc tả đủ
- validation order kém
- refusal rules yếu

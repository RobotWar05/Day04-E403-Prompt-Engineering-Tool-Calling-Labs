# Lab Prompt Engineering OrderDesk

Xây dựng một agent đặt hàng dùng LLM cho một nhà bán lẻ thiết bị điện tử và cải thiện điểm số của nó thông qua prompt engineering.

Trong lab này, agent phải:

- hiểu các yêu cầu đặt hàng tiếng Việt và mixed-language
- dùng tool theo đúng thứ tự
- hỏi lại thông tin còn thiếu trước khi hành động
- từ chối các yêu cầu không an toàn hoặc vi phạm policy
- lưu đơn hàng cuối cùng thành JSON có căn cứ

Mục tiêu chính không chỉ là làm cho code chạy được. Mục tiêu là cải thiện hành vi của agent bằng cách siết chặt prompt, tool schema và guardrails.

## Bạn Sẽ Thực Hành Gì

- viết system prompt mạnh hơn
- thiết kế tool schema rõ ràng hơn
- bắt buộc clarification trước khi dùng tool
- thêm guardrails cho các yêu cầu không an toàn
- grounding final answer theo kết quả tool
- debug lỗi từ tool traces và saved artifacts

## Bản Đồ Repository

- `src/`: phần triển khai của bạn
- `simple_solution/`: baseline yếu
- `data/products.json`: catalog sản phẩm
- `data/graded_cases.json`: các scenario được chấm
- `data/expected_orders/`: JSON đã lưu kỳ vọng cho các save case
- `grade/scoring.py`: grader
- `guide.md`: workflow từng bước
- `rubric.md`: quy tắc chấm điểm

## Workflow Khuyến Nghị

1. Chạy weak baseline trước.
2. Ghi lại điểm của nó.
3. Cải thiện `src/`.
4. Chạy grader trên `src/`.
5. Lặp lại cho đến khi điểm của bạn vượt baseline rõ ràng.

## Setup

Tạo một file `.env`:

```bash
GOOGLE_API_KEY=...
LLM_MODEL=gemini-2.5-flash
```

Optional local model:

```bash
OLLAMA_MODEL=qwen3.5:3b
OLLAMA_BASE_URL=http://localhost:11434
```

## Commands

Chạy weak baseline:

```bash
python grade/scoring.py --module simple_solution.agent.graph --provider google
```

Chạy phần triển khai của bạn:

```bash
python grade/scoring.py --module src.agent.graph --provider google
```

Chạy tests:

```bash
pytest -q
```

## Một Bài Nộp Mạnh Sẽ Làm Gì

- clarification trước khi dùng tool khi các required fields bị thiếu
- từ chối invalid requests mà không gọi tools
- đi theo expected tool sequence trên valid orders
- lưu đúng JSON artifact
- đưa ra câu trả lời tiếng Việt ngắn gọn và có căn cứ

Đọc [guide.md](/Users/duongnh59.al1/Documents/Project/Vin20K/Cohort2/Day-4-Lab/labs_update/guide.md) trước khi chỉnh sửa `src/`.

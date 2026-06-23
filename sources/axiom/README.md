# AXIOM v9 cho Claude Code

Bản dựng lại của **AXIOM v9 — Living Factory** thành một hệ thống agent chạy được
trong [Claude Code](https://docs.claude.com/en/docs/claude-code/overview). File HTML
gốc là một bản *minh hoạ* quy trình; bản này biến quy trình đó thành một dây chuyền
agent thật, dùng cho việc **nghiên cứu và viết văn bản — từ học thuật đến văn học**.

Nguyên tắc vàng giữ nguyên: **Wow = ấn tượng VÀ đúng sự thật**, kèm bằng chứng kiểm
được.

## Cài đặt

Chép cây thư mục này vào gốc dự án của bạn:

```
your-project/
├── CLAUDE.md                  ← hiến pháp/kỷ luật (9 quy tắc, lỗi cần tránh, chấm độ tin cậy, cổng)
└── .claude/
    ├── agents/                ← 6 subagent = "các robot trên sàn"
    │   ├── axiom-intake.md        (1 · Tiếp nhận)
    │   ├── axiom-research.md      (2 · Nghiên cứu)
    │   ├── axiom-design.md        (3 · Thiết kế)
    │   ├── axiom-assembly.md      (4 · Lắp ráp)
    │   ├── axiom-coach.md         (5 · Huấn luyện viên)
    │   └── axiom-inspector.md     (6 · Kiểm định độc lập)
    └── commands/
        └── axiom.md           ← lệnh /axiom điều phối cả dây chuyền + cổng + giao hàng
```

Có thể đặt ở phạm vi dự án (`.claude/`) hoặc phạm vi người dùng (`~/.claude/`).
Kiểm tra bằng `/agents` trong Claude Code để thấy 6 agent đã nạp.

## Dùng

Cách đơn giản nhất — chạy cả dây chuyền:

```
/axiom Viết một tiểu luận 1500 từ về ảnh hưởng của chủ nghĩa hiện sinh lên Truyện Kiều
```

Orchestrator sẽ lần lượt: phân loại → nghiên cứu (kèm nguồn) → thiết kế kế hoạch →
lắp ráp bản thảo → huấn luyện viên hỏi "đã tốt nhất chưa?" → **kiểm định độc lập** →
qua **cổng an toàn** → giao kèm **gói bằng chứng**.

Gọi lẻ một trạm khi cần, ví dụ:

```
Dùng subagent axiom-inspector soát lại bản thảo trong .axiom/runs/<id>/
```

Mọi biên nhận nằm trong `.axiom/runs/<timestamp>-<slug>/` — đây là bằng chứng từng
bước đã thật sự chạy. Người kiểm định đọc **bản thảo thật**, không đọc tóm tắt — đó
là điểm khiến cuộc kiểm tra thật sự độc lập.

## Đối chiếu với file HTML gốc

| Trong file gốc | Trong bản Claude Code này |
|---|---|
| 7 trạm nhà máy | 6 subagent (trạm 1–6) + trạm 7 (Giao hàng) do orchestrator làm |
| Các "robot" (worker, carrier, research, verifier, coach) | Mapping vào subagent + cơ chế delegate của Claude Code |
| Cổng An toàn | Bước kiểm tra trong `/axiom` (không mở nếu chưa có kết quả kiểm định thật) |
| 9 quy tắc (AXIOMS) | Mục "The 9 Axioms" trong `CLAUDE.md` |
| 6 lỗi cần tránh (FMS R1–R6) | Mục "The 6 failure modes" trong `CLAUDE.md` |
| 4 mức độ chắc chắn A–D | Bảng chấm độ tin cậy trong `CLAUDE.md`, áp dụng ở mọi trạm |
| Vòng trả-về sửa | Vòng FIX-IT trong `/axiom` (kiểm lại từ đầu, không tin báo cáo cũ) |
| Gói hồ sơ khi giao | `manifest.md` (danh sách độ tin cậy + giả định + biên nhận) |

## Vì sao mô hình này hợp với Claude Code

- **Subagent có context riêng** → người kiểm định *thật sự* độc lập, không bị ngữ
  cảnh của người làm chi phối.
- **Artifact dạng file** → quy tắc "đọc thứ thật, không đọc tóm tắt" được *cưỡng chế*
  chứ không chỉ là lời khuyên.
- **Delegate tuần tự/song song** → đúng tinh thần "đội nhỏ nhất mà vẫn xong việc".

## Tuỳ biến

- Đổi `model:` trong frontmatter mỗi agent để cân chi phí/chất lượng (haiku/sonnet/opus).
- Bản này viết prompt bằng tiếng Anh cho gọn và dễ chia sẻ; output sẽ theo ngôn ngữ
  bạn giao việc. Có thể dịch system prompt sang tiếng Việt nếu muốn.

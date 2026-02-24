# Showcase Script — Kịch Bản Demo Trước BGK

> Thời lượng ước tính: 10–15 phút
> Công cụ: Claude Pro (Projects) — đã setup theo `SETUP_GUIDE.md`

---

## Mở đầu (1 phút)

**Nói:**
> "Xin chào BGK, em là Sella. Hôm nay em sẽ demo AI Skill 'Smart Test Case Generator' — skill giúp tự động chuyển đổi Product Spec thành bộ Test Case hoàn chỉnh, bao gồm cả báo cáo và phân tích rủi ro."

**Show:** Mở README.md trên GitHub → chỉ vào pipeline diagram.

> "Skill hoạt động theo 6 bước pipeline, xử lý từ validate input đến generate test cases, report template, và review loop. Hỗ trợ lên đến 7 loại test case."

---

## Demo Live — Phần 1: Happy Path (5–6 phút)

### Action 1 — Paste Spec

**Nói:**
> "Em sẽ paste một spec thật vào — đây là feature User Registration."

**Làm:** Paste nội dung `demo/sample_input.md` vào Claude Project chat.

### Action 2 — Pre-Analysis Report

**Chờ AI output Pre-Analysis Report.**

**Nói (khi AI hiển thị):**
> "Trước khi generate test cases, AI phân tích spec trước: đếm ACs, phát hiện Logic Gaps, đánh giá Security Risks, và ước tính số lượng test cases. Đây là bước giúp QC nắm được scope trước khi commit."

**Highlight cho BGK:**
- Số ACs detected
- Logic Gaps found (AI tìm ra những gì spec chưa định nghĩa)
- Security Risks (auto-detected từ keywords trong spec)
- Estimated test case count by type

### Action 3 — Generate

**Làm:** Reply "Yes" để AI generate.

**Nói (trong khi chờ):**
> "AI sẽ generate test cases theo từng loại: Happy Path trước, rồi Edge Cases, Negative, Security, UI/UX, và Mobile. Mỗi test case đều có Design Logic giải thích kỹ thuật đã dùng và Execution Pro-tip hướng dẫn cách chạy."

**Khi AI hiển thị test cases, chỉ vào:**
1. **Một Happy Path case** → "Đây là flow chính — đăng ký thành công."
2. **Một Edge Case** → "AI tự áp dụng BVA — test ở boundary min/max." 
3. **Một Security Case** → "AI phát hiện keyword 'form' và 'password' trong spec nên tự trigger SQL injection và XSS cases."
4. **Mentor Fields** → "Design Logic giải thích tại sao test case này tồn tại — hữu ích cho Junior Tester."

---

## Demo Live — Phần 2: Tính Năng Nổi Bật (3–4 phút)

### Action 4 — Hallucination Check

**Nói:**
> "Một tính năng quan trọng: Anti-Hallucination. Khi AI không tìm thấy giá trị trong spec, nó đánh dấu [TBD] thay vì bịa ra."

**Chỉ vào:** Một test case có `[TBD]` + `# REVIEW: value not in spec`.

### Action 5 — AC Coverage Matrix

**Nói:**
> "Cuối output có AC Coverage Matrix — mapping 100% từ ACs sang Test Case IDs. BGK có thể verify ngay: mọi AC đều được cover."

**Chỉ vào:** AC Coverage Matrix → tất cả ACs đều ✅ Covered, Logic Gaps có ⚠️ Flagged.

### Action 6 — Review Loop

**Nói:**
> "AI không tự export — luôn hỏi user review trước. User có thể edit, reject, hoặc thêm test cases."

**Làm:** Demo edit 1 test case:
```
Edit TC-REG-012: change expected result to "Error: Full Name must be at least 2 characters"
```

**Chỉ vào:** AI cập nhật test case và hỏi lại confirm.

### Action 7 — Senior Mode (Optional)

**Nếu còn thời gian:**
> "Cho Senior QC không cần Mentor Fields, chỉ cần nói 'Senior mode'."

**Làm:** Type "Regenerate TC-REG-001 in Senior mode"

**Chỉ vào:** Output gọn hơn, không có Design Logic và Pro-tip.

---

## Demo Live — Phần 3: Export (1 phút)

**Nói:**
> "Khi đã approve, export được 3 formats."

**Làm:** Type "(1) Approve all and export"

**Chỉ vào:** Full Markdown output với Report Template.

**Nói:**
> "Ngoài Markdown, còn hỗ trợ JSON cho automation và CSV cho TestRail/Jira Xray import."

---

## Kết (1 phút)

**Nói:**
> "Tổng kết: Từ 1 spec khoảng 350 từ, skill generate 32 test cases thuộc 6 loại khác nhau, bao gồm Pre-Analysis Report, Report Template, AC Coverage Matrix, và Risk Notes. Toàn bộ trong khoảng 5 phút, so với 4-8 giờ nếu làm thủ công."

**Show slide/bảng (nếu có):**

| | Thủ công | Với Skill |
|--|---------|-----------|
| Thời gian | 4–8 giờ | 5–10 phút |
| Coverage | Tùy kinh nghiệm | 7 loại test có hệ thống |
| Security | Thường bỏ sót | OWASP auto-trigger |
| Format | Mỗi người mỗi kiểu | Chuẩn hóa 100% |

> "Cảm ơn BGK đã lắng nghe. Em sẵn sàng trả lời câu hỏi."

---

## ❓ Câu Hỏi BGK Thường Gặp — Chuẩn Bị Trả Lời

| Câu hỏi | Gợi ý trả lời |
|---------|---------------|
| "Nếu spec quá dài thì sao?" | "Skill có input limit: > 10,000 từ sẽ yêu cầu split. > 20 ACs sẽ batch tự động." |
| "AI có bịa test data không?" | "Có cơ chế Hallucination Self-check — giá trị không có trong spec sẽ bị đánh dấu [TBD]. Đây là safety net." |
| "Junior Tester dùng được không?" | "Mặc định có Mentor Fields giải thích technique + pro-tip. Steps viết đủ chi tiết để Junior chạy mà không cần hỏi Senior." |
| "Có integrate được với Jira/TestRail không?" | "Hiện hỗ trợ CSV export compatible với Xray/TestRail import. Direct API sync nằm trong roadmap." |
| "Tại sao chọn Claude?" | "Context window lớn, tone chuyên nghiệp, Artifacts giúp render bảng đẹp, và Projects giúp lưu System Prompt + Knowledge cố định." |
| "So với manual, có gì AI không làm được?" | "Skill không thay thế QC — nó tăng tốc phần viết test cases. QC vẫn cần review, bổ sung cases đặc thù, và quyết định cuối cùng thông qua review loop." |
| "Security cases có đủ tin cậy không?" | "Skill cover OWASP Top 10 basics — đủ cho QA general. Không thay thế pentest chuyên sâu, và em đã ghi rõ trong Limitations." |

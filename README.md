# BÀI TẬP THỰC HÀNH

## 1. Tổng quan quy trình

```
[Prompt 1] Nghiên cứu khách hàng + Pain points
      │  Output: DỮ LIỆU_1 (JSON)
      ▼
[Prompt 2] Lập dàn ý (bắt buộc dùng DỮ LIỆU_1)
      │  Output: DỮ LIỆU_2 (JSON)
      ▼
[Prompt 3] Viết bài + tối ưu SEO (bắt buộc dùng DỮ LIỆU_2)
      │  Output: Bài PR hoàn chỉnh (Markdown)
      ▼
[Prompt 3-C] Lệnh tiếp tục nếu bị ngắt do max token
```

**Thông tin đầu vào chung (điền trước khi chạy):**

| Biến | Giá trị ví dụ |
|---|---|
| `{{SẢN_PHẨM}}` | NovaBot – AI Chatbot cho doanh nghiệp |
| `{{TÍNH_NĂNG}}` | Trả lời 24/7, tích hợp CRM, hiểu tiếng Việt, chuyển tiếp nhân viên |
| `{{TONE}}` | Chuyên nghiệp, tin cậy, rõ ràng, không phóng đại |
| `{{TỪ_KHÓA_CHÍNH}}` | AI chatbot cho doanh nghiệp |

---

## 2. Prompt 1: Nghiên cứu khách hàng và pain points

```
Bạn là chuyên gia nghiên cứu thị trường B2B.

NHIỆM VỤ: Phân tích đối tượng khách hàng mục tiêu và liệt kê pain points
mà sản phẩm {{SẢN_PHẨM}} giải quyết.
Tính năng chính: {{TÍNH_NĂNG}}.

YÊU CẦU:
- Xác định 3 nhóm khách hàng mục tiêu (ngành, quy mô, vai trò người ra quyết định).
- Mỗi nhóm có 2-3 pain points cụ thể, gắn với 1 tính năng giải quyết nó.
- Không bịa số liệu thống kê; nếu cần số liệu, ghi "[cần xác minh]".

ĐỊNH DẠNG ĐẦU RA (chỉ trả về JSON, không giải thích thêm):
{
  "san_pham": "...",
  "tone": "{{TONE}}",
  "nhom_khach_hang": [
    {
      "id": "KH1",
      "ten": "...",
      "mo_ta": "...",
      "pain_points": [
        {"id": "PP1", "van_de": "...", "tinh_nang_giai_quyet": "..."}
      ]
    }
  ]
}
```

---

## 3. Prompt 2: Lập dàn ý chi tiết (đầu vào = output Prompt 1)

```
Bạn là biên tập viên nội dung SEO.

ĐẦU VÀO BẮT BUỘC (DỮ LIỆU_1 từ bước trước):
<DU_LIEU_1>
{{dán nguyên JSON output của Prompt 1}}
</DU_LIEU_1>

NHIỆM VỤ: Lập dàn ý chi tiết cho bài PR 1000-1500 từ về {{SẢN_PHẨM}}.

QUY TẮC:
- Chỉ dùng thông tin trong DU_LIEU_1. Mỗi pain point (PP) phải xuất hiện
  ít nhất 1 lần trong dàn ý, ghi rõ id.
- Cấu trúc bắt buộc: Headline → Lead → các H2 (có H3 nếu cần) → CTA.
- Phân bổ số từ cho từng phần, tổng 1000-1500 từ.
- Từ khóa chính: "{{TỪ_KHÓA_CHÍNH}}"; đề xuất 3-5 từ khóa phụ và vị trí đặt.
- Giữ tone: {{TONE}}.

ĐỊNH DẠNG ĐẦU RA (chỉ trả về JSON):
{
  "headline": "...",
  "tu_khoa_chinh": "...",
  "tu_khoa_phu": ["...", "..."],
  "tone": "...",
  "lead": {"y_chinh": "...", "pain_points": ["PP1", "PP2"], "so_tu": 120},
  "cac_phan": [
    {
      "id": "P1",
      "h2": "...",
      "h3": ["...", "..."],
      "y_chinh": "...",
      "pain_points": ["PP1"],
      "tu_khoa": ["..."],
      "so_tu": 250
    }
  ],
  "cta": {"noi_dung": "...", "hanh_dong": "..."}
}
```

---

## 4. Prompt 3: Viết bài chi tiết + tối ưu SEO (đầu vào = output Prompt 2)

```
Bạn là copywriter PR B2B chuyên nghiệp.

ĐẦU VÀO BẮT BUỘC:
<DU_LIEU_2>
{{dán nguyên JSON output của Prompt 2}}
</DU_LIEU_2>

NHIỆM VỤ: Viết hoàn chỉnh bài PR theo đúng DU_LIEU_2, theo thứ tự:
headline → lead → từng phần P1, P2, ... → CTA.

QUY TẮC BẮT BUỘC:
1. Định dạng Markdown: # Headline, ## H2, ### H3.
2. Lead hấp dẫn, nêu thẳng các pain points trong dàn ý.
3. Từ khóa chính xuất hiện ở headline, lead, ít nhất 1 H2 và phần kết;
   mật độ 1-1.5%, từ khóa phụ đặt tự nhiên, không nhồi nhét.
4. Giữ NGUYÊN tone "{{TONE}}" từ đầu đến cuối; không đổi ngôi xưng,
   không dùng ngôn từ quảng cáo thái quá.
5. Không lặp ý giữa các phần; không thêm số liệu ngoài dữ liệu cung cấp.
6. CTA rõ ràng ở cuối: nêu hành động cụ thể (đăng ký demo, liên hệ...).
7. Cuối bài, thêm Meta title (≤60 ký tự) và Meta description (≤160 ký tự).
8. Khi viết xong toàn bộ, kết thúc bằng dòng: <<HẾT_BÀI>>
   (Nếu chưa có dòng này, bài chưa hoàn thành.)
```

---

## 5. Quy tắc xử lý khi bài bị ngắt giữa chừng (max token)

**Dấu hiệu:** bài dừng đột ngột, câu bị cụt và chưa có `<<HẾT_BÀI>>`.

**Prompt 3-C (gửi ngay trong cùng cuộc hội thoại):**

```
Bài viết bị ngắt giữa chừng. Hãy viết tiếp CHÍNH XÁC từ điểm dừng.

Đoạn cuối cùng đã viết:
"""
{{dán 2-3 câu cuối cùng của output bị cắt}}
"""

QUY TẮC:
- Bắt đầu NGAY từ ký tự/từ tiếp theo sau đoạn trên. Nếu câu bị cụt, hoàn thành
  câu đó trước (không lặp lại phần đã viết, không mở đầu bằng lời dẫn
  như "Tiếp tục...", "Dưới đây là...").
- Giữ nguyên tone {{TONE}}, ngôi xưng, định dạng heading, từ khóa như trước.
- Tiếp tục theo đúng DU_LIEU_2, bắt đầu từ phần còn dang dở: {{id phần, vd P3}}.
- Chỉ viết các phần CÒN LẠI, đến hết CTA, Meta rồi kết thúc bằng <<HẾT_BÀI>>.
- Nếu lại sắp hết dung lượng, dừng ở cuối một đoạn hoàn chỉnh để tôi gọi tiếp.
```

**Kiểm tra sau khi nối:**

| Tiêu chí | Cách kiểm tra |
|---|---|
| Nối khớp | Đọc câu cuối bản cũ + câu đầu bản mới liền mạch, không lặp |
| Nhất quán tone | Cùng ngôi xưng, không đổi giọng |
| Đủ cấu trúc | Có Headline, Lead, H2/H3, CTA, `<<HẾT_BÀI>>` |
| Đủ độ dài | Tổng 1000-1500 từ |

---

## 6. Cấu trúc bài PR đầu ra (chuẩn SEO)

```
# Headline (có từ khóa chính)
Lead: 100-150 từ, nêu pain points
## H2 – Phần 1 (PP...)
### H3 ...
## H2 – Phần 2
### H3 ...
## H2 – Phần 3 ...
## CTA: Hành động cụ thể
Meta title / Meta description
```

---

## 7. Đối chiếu ràng buộc kỹ thuật

| Ràng buộc | Cách đáp ứng |
|---|---|
| Không gộp 1 prompt | Tách 3 prompt riêng, mỗi bước có vai trò và đầu ra riêng |
| Output trước là input bắt buộc sau | Prompt 2 và 3 có thẻ `<DU_LIEU_x>` bắt buộc dán dữ liệu bước trước |
| Dữ liệu truyền nhất quán | Dùng JSON với `id` (KH, PP, P) để truy vết |
| Giữ nguyên tone | Trường `tone` truyền qua cả 3 bước + quy tắc số 4 ở Prompt 3 |
| Nối khớp khi đứt gãy | Prompt 3-C: dán đoạn cuối, cấm lặp và cấm lời dẫn |
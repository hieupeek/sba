# HMS - Đánh giá thiếu hụt yêu cầu nghiệp vụ thực tế
> Phạm vi yêu cầu (Requirement scope): Khách sạn độc lập 3-4 sao, 60-120 phòng (rooms), 25-50 nhân viên (staff)
> Tài liệu gốc: HMS-SRS v1.2
> Ngày đánh giá: 2026-10-04
> Cách đọc: Thuật ngữ yêu cầu tiếng Anh kèm nghĩa tiếng Việt trong ngoặc. Chỉ dùng ngôn từ yêu cầu (requirement), không dùng ngôn từ thiết kế (design).

## 1. Kết luận nhanh (về yêu cầu)

User requirements (yêu cầu người dùng) hiện tại bao phủ tốt quy trình nghiệp vụ (business process) lõi:
`Search (Tìm phòng) -> Reservation (Đặt phòng) -> Check-In (Nhận phòng) -> Stay (Lưu trú) -> Folio (Sổ chi phí) -> Invoice (Hóa đơn) -> Payment (Thanh toán) -> Check-Out (Trả phòng) -> Review (Đánh giá)` cho khách lẻ online.

Mức bao phủ yêu cầu ước tính ~70% ngày thường, ~45% giờ cao điểm cuối tuần / đoàn / công nợ.

Thiếu nặng nhất theo nhóm yêu cầu:
1. Finance business rules (Quy tắc tài chính): Deposit (Cọc), VAT E-Invoice (Hóa đơn điện tử), City Ledger (Công nợ)
2. Front Office user requirements (Yêu cầu lễ tân): Walk-In (Khách vãng lai), Group/Corporate/OTA (Đoàn/Công ty/Bán qua mạng), No-Show (Không đến), Night Audit (Chốt đêm)
3. Housekeeping user requirements (Yêu cầu buồng phòng): 2-actor inspection business rule (Quy tắc kiểm tra 2 vai), work assignment business rule (quy tắc chia việc), Lost & Found concept (Khái niệm đồ thất lạc)
4. Service user requirements (Yêu cầu dịch vụ): order status business rules (quy tắc trạng thái đơn), laundry business process (quy trình giặt là), damage business rule (quy tắc đền bù), banquet requirement (yêu cầu hội trường)

## 2. Quy mô tổ chức đối chiếu yêu cầu

### 2.1 Yêu cầu quy mô chuẩn 3-4 sao 60-120 phòng
- Business constraint (Ràng buộc nghiệp vụ) giờ cao điểm 12:00 Check-Out + 14:00 Check-In cùng lúc 30-50 khách.
- 3 shifts (ca) xoay, cash handover business rule (quy tắc bàn giao két) mỗi ca.
- Business scope (phạm vi nghiệp vụ) doanh thu điển hình: 50-60% phòng lẻ, 20-30% đoàn/công ty/OTA, 10-20% F&B/hội trường/dịch vụ.

### 2.2 Yêu cầu tổ chức trong HMS đang giả định gì?
- Chỉ có 4 khối chữ, không có actor hierarchy requirement (yêu cầu phân cấp tác nhân) về số lượng, không rõ reporting business rule (quy tắc báo cáo) ai kiểm tra ai.
- Gộp Maintenance actor (Tác nhân kỹ thuật) vào Housekeeping Staff, gộp General Manager (Tổng giám đốc) + Accountant (Kế toán) + IT Admin (Quản trị hệ thống) thành 1 Administrator actor requirement.
- Hệ quả yêu cầu: BR-06 thiếu Service Staff (Nhân viên dịch vụ), UC-20 không phân biệt actor báo hỏng vs actor sửa, authorization requirement (yêu cầu phân quyền) sai.

Khuyến nghị yêu cầu: bổ sung actor hierarchy requirement ở Section 1.1, tổng ~35-40 người.

## 3. Front Office (Lễ tân) - Thiếu yêu cầu chi tiết

### 3.1 Walk-In business requirement (Yêu cầu khách vãng lai)
- Business need (Nhu cầu nghiệp vụ): Tối thứ 7 khách tới trực tiếp, cần Rack Rate business rule (Quy tắc giá niêm yết tại quầy) khác giá online.
- Missing requirement: Walk-In use case requirement check-in nhanh, không cần booking trước.
- Impact (Ảnh hưởng yêu cầu): lễ tân phải tạo booking giả, sai business source requirement, sai report requirement (yêu cầu báo cáo).
- Đề xuất yêu cầu: thêm UC-27b Walk-In Check-In requirement, BR: `Walk-In requires ID + full-night deposit business rule`.

### 3.2 Group / Corporate / Travel Agent - TA (Đoàn / Công ty / Đại lý)
- Business need: Đoàn 20-30 phòng, 1 master requirement (yêu cầu chung) + nhiều sub-requirements (yêu cầu lẻ), 1 payer (người trả), Contract Rate business rule (Quy tắc giá hợp đồng).
- Missing requirement: master/sub reservation business concepts, room block business rule, credit limit constraint (ràng buộc hạn mức).
- Đề xuất yêu cầu: business concepts Corporate/TA + Group Booking requirement, BR-47 City Ledger đã thêm, cần payment use case requirement cho công nợ.

### 3.3 OTA + Channel business requirements (Yêu cầu bán qua mạng + đồng bộ kênh)
- Business need: 30-50% từ Booking.com/Agoda/Expedia, trừ Commission business rule (Quy tắc hoa hồng) 15-20%.
- Missing requirement: business source requirement `source = Direct/OTA/Corporate`, OTA reference business requirement, over-allotment business rule.
- Impact: sai revenue requirement (yêu cầu doanh thu), trùng phòng.
- Đề xuất yêu cầu P1: thêm source requirement + net revenue report requirement sau commission.

### 3.4 No-Show / Overbooking / Waitlist (Không đến / Đặt lố / Chờ)
- Business need: Cho đặt 105/100 phòng để bù No-Show 5%. Khách không đến mất 1 đêm cọc, phòng nhả sau 06:00 hôm sau theo business rule.
- Requirement hiện tại: BR-44 đã thêm, UC-25 Cancel requirement (Yêu cầu hủy) chưa có nhánh No-Show, không có Waitlist requirement.
- Đề xuất yêu cầu: bổ sung status requirement `No-Show`, forfeit business rule `1 night`, auto-release business rule.

### 3.5 Pháp lý + Key-Card (Tạm trú + Thẻ khóa)
- Missing requirement: ID/Passport business attributes (thuộc tính nghiệp vụ: ảnh, quốc tịch, hết hạn), temporary residence declaration requirement (yêu cầu khai báo tạm trú), Key-Card business requirement.
- Requirement risk (Rủi ro yêu cầu): acceptance criteria (tiêu chí nghiệm thu) pháp lý không đạt.

### 3.6 Early / Late business rules (Nhận sớm / Trả muộn)
- BR-35 Early + BR-46 Late đã có (Late: <6h=50%, qua đêm=1 đêm sau 12:00) nhưng UC-27/UC-30 user requirements chưa có time + surcharge business requirements (yêu cầu giờ + phụ thu).

## 4. Housekeeping (Buồng phòng) - Thiếu yêu cầu

### 4.1 Status transition business rule 2 vai bắt buộc
- Required business rule: Dirty (Bẩn) -> Clean (Nhân viên dọn xong) -> Inspected (Giám sát kiểm tra đạt) -> mới assignable (được gán).
- Lỗi cũ BR-13 `any->any` đã sửa yêu cầu. Cần sửa UC-19 user requirement flow thành 2 actors 2 bước, actor constraint: nhân viên không tự Inspected phòng mình dọn.

### 4.2 Work assignment + turnaround constraint
- Missing requirement: assignment business rule theo tầng, turnaround time constraint 25-30p, escalation requirement (yêu cầu báo vượt giờ) cho lễ tân.
- Với 8-10 actors dọn 100 phòng, thiếu assignment requirement là sót phòng giờ cao điểm.

### 4.3 DND / MUR + long stay business rules
- DND - Do Not Disturb (Không làm phiền): access constraint (ràng buộc không vào).
- MUR - Make Up Room (Yêu cầu dọn): priority business rule (quy tắc ưu tiên).
- Khách ở 5 đêm: daily cleaning requirement, Linen replacement business rule (quy tắc thay ga gối) chưa có.

### 4.4 Minibar + Lost & Found + Linen business requirements
- Minibar (Tủ lạnh mini): verification business rule sau trả phòng để post business charge, hiện không có UC requirement.
- Lost & Found (Đồ thất lạc): storage business rule, return requirement, liquidation business rule sau 30-90 ngày.
- Linen business concept: availability constraint, thiếu thì phòng Clean cũng không assignable.

### 4.5 Out-of-Order vs Out-of-Service business rules
- Out-of-Order (Hỏng nặng): non-sellable business rule, trừ availability requirement.
- Out-of-Service (Tạm ngưng): temporary hold business rule, sắp mở lại.
- BR-48 đã thêm, UC-18 availability requirement và UC-20 chưa phân biệt, chưa có reopen business requirement sau sửa.

## 5. F&B & Ancillary Services (Ăn uống & Dịch vụ phụ) - Thiếu yêu cầu

### 5.1 Order status business rules
- Hiện tại: Order (Gọi) -> Void (Hủy). Thiếu: Preparing (Đang làm) -> Delivered (Đã giao) status requirements.
- Không có status requirement thì không có acceptance criteria cho thời điểm tính tiền.

### 5.2 Laundry business process (Quy trình giặt là)
- Missing business process requirement: thu sáng -> giặt -> trả chiều, pricing business rule theo món/kg, compensation business rule khi mất/hỏng.

### 5.3 Damage compensation business rule (Quy tắc đền bù)
- Missing requirement: ai kiểm tra (actor requirement), price list business rule theo bảng nào, posting time business rule vào Folio lúc nào.

### 5.4 Banquet / Hall business requirements (Yêu cầu tiệc / hội trường)
- 3-4 sao thường có 1-2 halls 100-200 khách, business scope cưới/hội nghị bằng 20 phòng.
- Chưa có hall booking requirement, banquet menu requirement, deposit business rule cho tiệc. Nếu requirement scope xác định không làm thì phải ghi explicit out-of-scope requirement, nếu không acceptance sẽ hỏi.

## 6. Finance (Tài chính) - Rủi ro yêu cầu cao nhất

### 6.1 Deposit / Refund / Partial business rules (Cọc / Hoàn / Trả một phần)
- Đã sửa BR-19 (giữ 24h auto-cancel business rule) + BR-33 (cho partial business rule) + BR-44 (cọc 1 đêm, hoàn khi hủy đúng hạn, mất khi No-Show).
- Còn thiếu ở UC requirements: UC-23 chưa ghi deposit percent business rule, hold method requirement, UC-39 chưa có refund use case requirement.

### 6.2 VAT E-Invoice business rules (Quy tắc hóa đơn điện tử)
- Business constraint ở VN: tách tiền chưa VAT + %VAT + tiền VAT, Tax Code business requirement (Yêu cầu mã số thuế), company attributes, signing + submission requirements.
- BR-45 đã thêm, UC-37 Compile Invoice requirement (Yêu cầu lập hóa đơn) và UC-40 Print requirement (Yêu cầu in) chưa có tax business requirements, chưa có tax configuration requirement.

### 6.3 City Ledger business rules (Quy tắc công nợ)
- BR-47 đã thêm: Corporate/TA được nợ trong Credit Limit constraint (Ràng buộc hạn mức), không chặn Check-Out.
- Thiếu: debt aging report requirement (yêu cầu báo cáo tuổi nợ), reminder business rule, sales block business rule khi quá hạn.

### 6.4 Cash handover + reconciliation business rules
- Thiếu: shift open/close cash business requirements, variance business rule, Exchange Rate business rule cho khách ngoại, gateway reconciliation business requirement.

### 6.5 Management report requirements
- OR-03 đã bổ sung ADR - Average Daily Rate (Giá bình quân), RevPAR - Revenue Per Available Room (Doanh thu/phòng sẵn bán) report requirements.
- UC-45 Occupancy requirement, UC-46 Revenue requirement, UC-50 Export requirement chưa có calculation business rules cho ADR/RevPAR, pickup/cancel rate requirements.

## 7. Admin / HR / Shift operation requirements

- 25-50 người 3 ca cần Shift business requirements (Yêu cầu ca), Handover record requirement (Yêu cầu bàn giao), authorization constraint theo ca (đêm không được hoàn tiền).
- Night Audit business rule BR-43 + OR-04 đã thêm (00:00-05:00 lock business rule cho check-in/out/invoice, rollback business rule khi lỗi) nhưng chưa có auditor use case requirement, chưa có lock authorization requirement.
- Business continuity requirement OR-05 đã thêm (nightly requirement, 30 ngày, RPO business constraint <24h), thiếu recovery time business requirement.

## 8. Requirement priority matrix (Ma trận ưu tiên yêu cầu)

| Nhóm yêu cầu thiếu | Ảnh hưởng nghiệp vụ | Ảnh hưởng nghiệm thu | Ưu tiên yêu cầu |
|---|---|---|---|
| VAT, Deposit/Refund, Night Audit business rules | Không vận hành thật được, sai legal constraint | Hỏi chắc chắn khi nghiệm thu | P0 |
| Walk-In, No-Show, 2-actor clean, Late fee, Damage business rules | Kẹt lễ tân/buồng phòng giờ cao điểm | Trừ nặng acceptance criteria | P0 |
| Group/OTA/City Ledger, Lost&Found, Laundry requirements | Mất 30% business scope đoàn/mạng | Trừ vừa | P1 |
| Banquet, Loyalty, Channel requirements | Mất business scope phụ | Điểm cộng | P2 |

## 9. Đề xuất chốt requirement scope cho nhóm

Giữ requirement scope hiện tại + bắt buộc bổ sung P0 (đã có BR-43~BR-48 làm business rule nền, chỉ cần sửa user requirements UC-19, UC-23, UC-25, UC-27, UC-30, UC-37, UC-39, UC-40 cho khớp traceability).
Ghi rõ trong SRS Section 1 Scope requirement: `Out-of-scope requirements: Channel auto-sync requirement, Banquet management requirement` nếu không làm để tránh acceptance mở rộng.

---
*File này đi kèm `HMS_Requirement_Issues.md` (requirement issues log) và `HMS_SoftwareRequirementSpecification.docx` Requirement Appendix Tables 5.0-5.4 đã cập nhật. No design content.*

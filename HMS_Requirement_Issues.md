# HMS - Requirement Issues Log
> Project: HMS (Hotel Management System - Hệ thống quản lý khách sạn)
> Document: HMS-SRS v1.2 (`HMS_SoftwareRequirementSpecification.docx`)
> Scope (Phạm vi): 3-4 star hotel, 60-120 rooms (phòng), 25-50 staff (nhân viên)
> Updated: 2026-10-04
> Status convention: OPEN (đang mở) / FIXED (đã sửa) / TODO (cần làm)
> Language rule: requirement terms only (chỉ dùng thuật ngữ yêu cầu). No design terms.

## 0. Summary (Tóm tắt)

Core business process (quy trình nghiệp vụ chính) Booking (Đặt phòng) -> Check-In (Nhận phòng) -> Stay (Lưu trú) -> Check-Out (Trả phòng) is covered ~70% as user requirements (yêu cầu người dùng).
Fixed in this round: BR (Business Rule - Quy tắc nghiệp vụ) 42 -> 48, CR (Common Requirement - Yêu cầu chung) 10 -> 12, MSG (Message Requirement - Yêu cầu thông báo) 12 -> 15, OR (Other Requirement - Yêu cầu khác) 3 -> 6, added Requirement Appendix Table 5.0 Classification (Phân loại).
Remaining OPEN items are mostly Front-Desk user requirements (yêu cầu lễ tân), Finance business rules (quy tắc tài chính), and Night Audit business rule (quy tắc chốt đêm).

---

## 1. Critical Inconsistencies (Mâu thuẫn nghiêm trọng giữa các yêu cầu)

### ISS-01: Room readiness business rules (Quy tắc phòng sẵn sàng) — FIXED
- Location (Vị trí yêu cầu): BR-10, BR-13, BR-14, UC-19 Update Housekeeping Status (Ca sử dụng cập nhật trạng thái dọn phòng), UC-27 Check-In Guest (Ca sử dụng nhận phòng)
- Business requirement conflict (Xung đột yêu cầu): BR-10 `Vacant (Trống) + Clean (Đã dọn)` but BR-14 requires `Vacant + Inspected (Đã kiểm tra)` for assignment. New rooms never assignable. BR-13 allowed `any -> any`.
- Requirement fix (Sửa yêu cầu): BR-10 = `Vacant + Inspected`. BR-13 = `Dirty (Bẩn) -> Clean -> Inspected, Inspected -> Dirty needs recorded business reason (lý do nghiệp vụ)`.
- TODO: Update UC-19 user requirement flow to enforce 2-actor business rule `Clean by staff (nhân viên dọn) -> Inspected by supervisor (giám sát kiểm tra)`.

### ISS-02: Actor definition mismatch (Lệch định nghĩa tác nhân) — FIXED
- Location: BR-06, UC-09 Assign Roles (Ca sử dụng gán vai trò), Actors (Tác nhân)
- Business requirement conflict: BR-06 listed only `Front-Desk Staff (Lễ tân), Housekeeping Staff (Buồng phòng), Administrator (Quản trị)` but UC-09 and Actor requirements include `Service Staff (Nhân viên dịch vụ)`.
- Requirement fix: BR-06 = 4 actor roles including Service Staff.
- TODO: Add Service Staff to actor requirements and business concept requirements. Currently business rules cover only Customer, Front-Desk, Housekeeping concepts.

### ISS-03: Pending hold vs payment business rules (Quy tắc giữ chỗ vs thanh toán) — FIXED (partial)
- Location: BR-19, UC-23 Create Reservation (Ca sử dụng tạo đặt phòng), UC-39 Process Payment (Ca sử dụng thu tiền)
- Business requirement conflict: BR-19 `Pending (Chờ) hold 24h` vs UC-23 `mandatory full payment/deposit (bắt buộc trả hết/cọc ngay)` vs UC-39 `must match balance (phải trả khớp)` vs `partial payment (trả một phần)` alternative requirement.
- Requirement fix: BR-19 clarified to `hold business quota 24h, auto-cancel if not confirmed/deposited`. BR-33 clarified partial allowed.
- TODO: Define deposit business rule in UC-23 pre-condition (điều kiện tiên quyết): % or 1 night? BR-44 Deposit/Refund (Cọc/Hoàn cọc) added, need UC requirement update.

### ISS-04: Duplicate use case identifier (Trùng mã ca sử dụng) — OPEN
- Location: User requirements Section 3.3.1 and 3.3.2 both labeled UC-26 Confirm Reservation (Xác nhận đặt phòng)
- Business requirement conflict: Two different user goals `Pending-booking requirement` and `Booking Details requirement` share same requirement ID. Breaks traceability requirement (yêu cầu truy vết).
- Action (Hành động yêu cầu): Rename requirement IDs to UC-26a / UC-26b, or UC-26 Confirm + UC-26x View. Update use case requirement overview Fig 2-3.

### ISS-05: Requirement illustration numbering (Đánh số minh họa yêu cầu sai) — OPEN
- Location: User requirements Section 3, illustration 3-1 reused 10+ times for different user requirements.
- Action: Renumber requirement illustrations sequentially 3-1..3-25. Update functional requirement references in Section 3.

---

## 2. Missing Business Coverage (Thiếu yêu cầu nghiệp vụ thực tế)

Requirement gaps are expressed as missing actors, missing use cases, missing business rules. No design solution prescribed.

### Front Office user requirements (Yêu cầu lễ tân)
| ID | Missing requirement | Business need for 3-4 star |
|----|---------|--------------------------|
| M-FO-01 | Walk-In user requirement (Yêu cầu khách vãng lai không đặt trước) | 10-20% business volume, needs new UC + BR |
| M-FO-02 | Group / Corporate / TA business concepts (Đoàn / Công ty / Đại lý) | Needs City Ledger business rule (quy tắc công nợ), contract rate business rule |
| M-FO-03 | OTA + Channel business requirements (Yêu cầu bán qua Booking.com/Agoda + đồng bộ kênh) | 30-50% bookings, commission business rule |
| M-FO-04 | No-Show (Không đến), Overbooking (Đặt lố), Waitlist (Chờ) business rules | BR-44 added, UC-25/UC-27 user requirements not updated |
| M-FO-05 | Room Block (Giữ phòng), Day-Use (Bán theo giờ), Extra Bed (Giường phụ) business rules | Common in 60-120 room business scope |
| M-FO-06 | ID + temporary residence declaration business rule (Quy tắc khai báo tạm trú) | Legal constraint (ràng buộc pháp lý) in VN |
| M-FO-07 | Key-Card business requirement (Yêu cầu thẻ khóa) | Operational constraint |
| M-FO-08 | Early Check-In (Nhận sớm) BR-35, Late Check-Out (Trả muộn) BR-46 but no use case requirements | Add surcharge business rules to UC-27/UC-30 |

### Housekeeping user requirements (Yêu cầu buồng phòng)
| ID | Missing requirement |
|----|---------|
| M-HK-01 | Work assignment business rule per floor/staff (Quy tắc chia việc), turnaround time constraint (ràng buộc thời gian dọn) |
| M-HK-02 | DND - Do Not Disturb (Không làm phiền) / MUR - Make Up Room (Yêu cầu dọn) business rules |
| M-HK-03 | Minibar verification business rule after check-out (Quy tắc kiểm tủ lạnh sau trả phòng) |
| M-HK-04 | Lost & Found (Đồ thất lạc), Linen (Đồ vải) business concepts |
| M-HK-05 | Out-of-Order (Hỏng nặng) vs Out-of-Service (Tạm ngưng) business rules — BR-48 added, UC-18/UC-20 user requirements not updated |

### F&B / Services user requirements (Yêu cầu ăn uống / dịch vụ)
| ID | Missing requirement |
|----|---------|
| M-SV-01 | Order status business rules: ordered -> preparing -> delivered (Đã gọi -> đang làm -> đã giao). Currently only Order -> Void (Gọi -> Hủy) |
| M-SV-02 | Laundry business process collect/wash/return (Thu -> giặt -> trả) |
| M-SV-03 | Damage compensation business rule (Quy tắc đền bù hư hại) |
| M-SV-04 | Banquet/Hall business requirements (Yêu cầu hội trường/tiệc) — key business scope for 3-4 star |

### Finance business rules (Quy tắc tài chính)
| ID | Status as requirement |
|----|--------|
| M-FN-01 | Deposit/Refund (Cọc/Hoàn cọc) business rules — BR-44 added, UC-23/UC-39 user requirement flows TODO |
| M-FN-02 | VAT E-Invoice (Hóa đơn điện tử) business rules — BR-45 added, need tax rate business rule + Tax Code business requirement in UC-37/UC-40 TODO |
| M-FN-03 | City Ledger (Sổ công nợ) business rules — BR-47 added, need credit limit constraint + aging report requirement TODO |
| M-FN-04 | Cash handover per shift business rule (Quy tắc bàn giao két theo ca), exchange rate business rule | OPEN |
| M-FN-05 | Management report requirements ADR (Giá bình quân), RevPAR (Doanh thu/phòng sẵn bán), cancel/no-show rate — OR-03 updated, UC-45/46/50 TODO | OPEN |

### Admin / HR user requirements
| ID | Missing requirement |
|----|---------|
| M-AD-01 | Shift business requirements (Yêu cầu ca), handover record requirement (yêu cầu nhật ký bàn giao) for 25-50 staff | OPEN |
| M-AD-02 | Authorization constraint by shift, Night Audit lock business rule (Quy tắc khóa khi chốt đêm) — BR-43 added, authorization requirement TODO |

---

## 3. Business Concept Gaps (Thiếu khái niệm nghiệp vụ)

Requirement-level only. No data structure, no storage solution.

- ENT-01 OPEN: Missing business relations (quan hệ nghiệp vụ) not explicit as requirements. Reservation -> Customer, Stay -> Reservation + Room, Folio -> Stay, Payment -> Invoice, Service Order -> Stay + Service must be stated as business rules, not assumed.
- ENT-02 OPEN: Missing business concepts as requirement scope: Service Staff actor, Deposit/Transaction concept, Tax concept, Rate Plan concept (Gói giá theo mùa), Corporate/TA concept, Lost&Found concept.
- ENT-03 OPEN: Status lifecycle business rules inconsistent: Folio `open/closed` vs Invoice `Unpaid/Paid` vs Reservation `Pending/Confirmed/Checked-In/Completed/Cancelled` vs Stay `active/completed` — need unified status transition business rules.
- ENT-04 TODO: Voucher `Discount Value` business rule — fixed amount or %? Need calculation business rule.

---

## 4. Non-Functional Requirement Gaps (Thiếu yêu cầu phi chức năng)

- NFR-01 OPEN: Performance requirement `3s` stated but without business load requirement: peak check-in 60-120 rooms, concurrent business users (25-50 staff + guests).
- NFR-02 FIXED partial: Business continuity requirement added OR-05 `nightly, 30 days, RPO <24h`. Missing recovery time business constraint.
- NFR-03 OPEN: Security requirements incomplete: password complexity business rule, lockout business rule after N fails, session timeout requirement, personal data protection constraint for ID/Passport, second-factor requirement for admin — not specified as requirements.
- NFR-04 FIXED partial: Usability language requirement OR-06 added. Missing supported business environment requirements.
- NFR-05 OPEN: Audit retention business rule now 12 months (OR-01), need retention enforcement requirement.

---

## 5. What Was Fixed on 2026-10-04 (Đã sửa ở mức yêu cầu)

1. Added Requirement Appendix Table 5.0 Classification G1-G6.
2. BR 42 -> 48: revised BR-06, BR-10, BR-13, BR-19, BR-33, BR-42; added BR-43 Night Audit (Chốt đêm), BR-44 Deposit/No-Show (Cọc/Không đến), BR-45 VAT, BR-46 Late Check-Out (Trả muộn), BR-47 City Ledger (Công nợ), BR-48 Out-of-Order vs Out-of-Service.
3. CR 10 -> 12: CR-11 audit money operations requirement, CR-12 VND rounding requirement.
4. MSG 12 -> 15: MSG13 deposit requirement, MSG14 audit requirement, MSG15 tax code requirement.
5. OR 3 -> 6: OR-01 12 months, OR-02 A4/A5, OR-03 ADR/RevPAR, OR-04 audit rollback requirement, OR-05 business continuity requirement, OR-06 language requirement.

File: `HMS_SoftwareRequirementSpecification.docx`, backup: `/tmp/HMS_backup.docx`.

---

## 6. Next Actions (Việc tiếp theo, ưu tiên yêu cầu)

**P0 - Must before acceptance (Bắt buộc trước nghiệm thu yêu cầu):**
- [ ] Fix ISS-04 duplicate UC requirement ID, ISS-05 illustration numbers
- [ ] Update UC-19 user requirement flow to Dirty->Clean->Inspected 2-actor business rule
- [ ] Update UC-23/UC-39 user requirements to reference BR-44 deposit/refund/partial business rules
- [ ] Update UC-37/UC-40 user requirements to reference BR-45 VAT business rules
- [ ] Add business relation requirements to Section 3.1.6 Business Concepts

**P1 - Should have (Nên có):**
- [ ] Add use case requirements for Walk-In, No-Show handling, City Ledger payment
- [ ] Add Lost&Found, Damage business rules to service requirements
- [ ] Add ADR/RevPAR to UC-45/46/50 report requirements

**P2 - Nice to have (Điểm cộng):**
- [ ] Channel / OTA business requirement flag, Loyalty (Khách thân thiết) requirement, Banquet requirement
- [ ] Password + session timeout security requirements

---

## 7. Traceability Hint (Gợi ý truy vết yêu cầu)

Each new BR (quy tắc nghiệp vụ) must be referenced in UC Description (mô tả ca sử dụng):
`BR-43 -> UC-30, UC-45, UC-46 | BR-44 -> UC-23, UC-25 | BR-45 -> UC-37, UC-40 | BR-46 -> UC-30 | BR-47 -> UC-26, UC-30, UC-39 | BR-48 -> UC-18, UC-20`
Acceptance rule (quy tắc nghiệm thu): UC without BR reference = incomplete requirement.

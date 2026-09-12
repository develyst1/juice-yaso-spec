# Juice Yaso — Spec (v1)

สัญญาผลิตภัณฑ์และเทคนิคสำหรับร้านน้ำเกร็ดหิมะ **Juice Yaso**  
รอบนี้: **เอกสารอย่างเดียว** — ห้าม scaffold / ลงโค้ด `juice-yaso-front` หรือ `juice-yaso-back`

## สถานะ
- v1 contract: **LOCKED** (ASK ME ปิดแล้ว)
- Testcases: ส่วนเดิม accepted-for-LOCKED ใน inbox; รอรอบ Tanya เติมสถานะ/สลิปซ้ำ/ยกเลิกหลัง push นี้
- **Jason / Fero ยังไม่ปล่อย** จนกว่า Sober จะปล่อยชัดเจน

## โครงสร้าง
- `docs/scope.md` — ขอบเขต in/out
- `docs/domain.md` — ราคา ลัง มัดจำ รส ชำระ รับที่ร้าน
- `docs/statuses.md` — enum สถานะ + transition + กติกายกเลิก
- `docs/usecases.md` — UC v1
- `docs/flows/customer-order.md` — โฟลว์ลูกค้า
- `docs/contracts/overview.md` — เอนทิตี / API intent
- `docs/open-questions.md` — ปิดแล้ว (เก็บประวัติ)
- `testcase/v1/` — เคสรอบถัดไปจาก Tanya

## Release gates
| บทบาท | สิทธิ์ตอนนี้ |
|--------|-------------|
| Tanya | เติม/อัปเดต testcase จากสัญญาล็อก |
| Jason / Fero | **ยังไม่ปล่อย** scaffold |
| โค้ด front/back | **ห้าม** ในรอบ spec นี้ |

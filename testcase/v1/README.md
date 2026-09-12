# Testcases — Juice Yaso v1

สถานะ: **filled after ASK ME closed** · Tanya · source `docs/statuses.md` + `docs/usecases.md` + `docs/domain.md`  
Acceptance ระดับโดเมน — ยังไม่ผูก HTTP path/DTO

| ไฟล์ | ครอบคลุม |
|------|----------|
| [customer.md](./customer.md) | UC-CUS-01..07 สั่ง/ราคา/มัดจำ/บัตรคิว/ชำระ/สลิปซ้ำ/track/ยกเลิก |
| [admin.md](./admin.md) | UC-ADM-01..04 ไลน์ approve/reject · PricingConfig · PaymentChannel · เลื่อนสถานะ |
| [statuses.md](./statuses.md) | transition 8 สถานะ · ยกเลิกก่อน/หลัง packing · slip reject loop |
| [deposit-scope.md](./deposit-scope.md) | UC-DEP-01 · e2e · นอกขอบเขต |

## DoD รอบนี้
- [x] เคสเดิมจาก inbox อัปเดตหลังปิด ASK ME (ชื่อ+เบอร์, ปนรส, ไม่ส่ง, ไม่ดูออเดอร์เก่า, QR/บัญชี)
- [x] transition 8 สถานะ + ยกเลิกก่อน/หลังกำลังแพ็ค
- [x] อัปสลิปซ้ำหลังปฏิเสธ
- [x] PaymentChannelConfig แอดมินแก้ได้
- [ ] (นอกงาน Tanya) Jason/Fero ยังไม่ปล่อย · ห้ามโค้ด front/back

# Contracts overview — Juice Yaso v1

- Domain entities: ด้านล่าง + `docs/domain.md` + `docs/statuses.md`
- **HTTP API:** [`api.md`](./api.md) — LOCKED สำหรับ implement
- **DB:** [`db.md`](./db.md) — LOCKED สำหรับ implement

## Entities
- **Order**: ชื่อ, เบอร์, บรรทัดลัง, ยอดสินค้า, ยอดมัดจำ, สถานะ, queue_code
- **OrderLine**: ขนาดลัง, จำนวนลัง, รส (หลายบรรทัด/หลายรสได้)
- **QueueCard**: = `queue_code` สาธารณะ
- **PaymentSlip**: ไฟล์สลิป + reject reason
- **PricingConfig / PaymentChannelConfig / DepositReturn / FlavorCatalog**

## Forbidden invent
- อย่าเพิ่มที่อยู่ส่ง / โซนส่ง ใน v1
- อย่าทำดูออเดอร์เก่าโดยไม่มี queue_code
- อย่า auto-refund ใน v1
- อย่าให้ front ต่อ Postgres

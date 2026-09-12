# API contract — Juice Yaso v1.1 (LOCKED for implement)

Base: REST JSON · back = Bun + Hono · ไม่มี login ลูกค้า  
Admin endpoints ใช้ header `X-Admin-Token` (ค่าจาก env `ADMIN_TOKEN` — ไม่ใส่ใน front repo)  

Status codes ใน body ใช้รหัสอังกฤษตาม `docs/statuses.md`

**v1.1 change:** `POST /orders` ใช้ `crates[]` แทน `lines[]` เพื่อรองรับลังคละรส · ห้ามรับ payload `lines` แบบเก่า (ตอบ 400 + ข้อความชัด)

## Public (ลูกค้า / บัตรคิว)

### `GET /api/v1/catalog`
ตอบ: flavors[], crateSizes[], depositBySize{}, pricing{basePricePerCup, bulkThresholdCups, bulkPricePerCup}, paymentChannel{qrImageUrl, bankAccountNumber, bankName}

### `POST /api/v1/orders`
body:
```json
{
  "customerName": "string non-empty",
  "customerPhone": "0XXXXXXXXX",
  "crates": [
    {
      "crateSize": 30,
      "fills": [
        { "flavor": "orange", "cups": 20 },
        { "flavor": "lychee", "cups": 10 }
      ]
    }
  ]
}
```
กฎ validate:
- `customerName` ไม่ว่างหลัง trim
- `customerPhone` ตรง `^0\d{9}$` (digits only)
- `crates.length >= 1`
- `crateSize` ∈ {30,50,60,100}
- แต่ละ `fills`: `cups` เป็นจำนวนเต็ม > 0 · `flavor` ในแคตตาล็อก · ห้ามซ้ำรสในลังเดียวกัน
- **Σ cups ของ fills ในลัง = crateSize** ทุกลัง
- server คำนวณ:  
  - cupsTotal = Σ crateSize  
  - depositTotal = Σ deposit(crateSize)  
  - productTotal = cupsTotal × unitPrice (5 หรือ 4.5 เมื่อ cupsTotal > threshold)

ตอบ 201: `{ orderId, queueCode, status: "awaiting_payment", cupsTotal, productTotal, depositTotal, paymentChannel }`

### `GET /api/v1/queue/:queueCode`
ตอบออเดอร์สาธารณะรวม:
- สถานะ, ยอด, `crates: [{ crateSize, deposit, fills: [{ flavor, cups }] }]`
- paymentChannel (ถ้ายังรอชำระ/ปฏิเสธ), slipRejectReason ล่าสุด, timestamps  
404 ถ้ารหัสผิด

### `POST /api/v1/queue/:queueCode/slips`
multipart: `file` (image)  
อนุญาตเมื่อ status ∈ awaiting_payment | slip_rejected  
→ status = awaiting_slip_review  
ตอบ: `{ status, slipId }`

### `POST /api/v1/queue/:queueCode/cancel`
อนุญาตเมื่อ status ∈ awaiting_payment | awaiting_slip_review | slip_rejected | in_queue  
→ cancelled · 403 ถ้ายกเลิกไม่ได้

## Admin
(ไม่เปลี่ยนจาก v1 ยกเว้นลิสต์/รายละเอียดออเดอร์แสดง `crates` แทน `lines`)

### `POST /api/v1/admin/slips/:slipId/approve` → in_queue
### `POST /api/v1/admin/slips/:slipId/reject` body `{ reason }` → slip_rejected
### `PATCH /api/v1/admin/orders/:orderId/status` body `{ status: packing|ready_for_pickup|picked_up }`
### `GET|PUT /api/v1/admin/config/pricing`
### `GET|PUT /api/v1/admin/config/payment-channel`
### `POST /api/v1/admin/orders/:orderId/deposit-returns` body `{ cratesReturned: true }`
### `GET /api/v1/admin/orders?status=` รวม `pendingSlipId` + สรุป crates

## Non-goals
- auth ลูกค้า / list ออเดอร์โดยไม่มี queueCode / shipping / auto-refund / deploy

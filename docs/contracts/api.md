# API contract — Juice Yaso v1 (LOCKED for implement)

Base: REST JSON · back = Bun + Hono · ไม่มี login ลูกค้า  
Admin endpoints ใช้ header `X-Admin-Token` (ค่าจาก env `ADMIN_TOKEN` — ไม่ใส่ใน front repo)  
Line webhook/notify = อินทิเกรตภายหลังได้ แต่ approve/reject ต้องเรียก API นี้ได้

Status codes ใน body ใช้รหัสอังกฤษตาม `docs/statuses.md`

## Public (ลูกค้า / บัตรคิว)

### `GET /api/v1/catalog`
ตอบ: flavors[], crateSizes[], depositBySize{}, pricing{basePricePerCup, bulkThresholdCups, bulkPricePerCup}, paymentChannel{qrImageUrl, bankAccountNumber, bankName}

### `POST /api/v1/orders`
body: `{ customerName, customerPhone, lines: [{ crateSize: 30|50|60|100, quantity: number>0, flavor: Flavor }] }`  
Flavor = `orange|grape|cocoa|lychee|blueberry` (หรือไทยเทียบเท่าที่ map ในโค้ด แต่เก็บรหัสอังกฤษใน DB)  
validate: ชื่อ+เบอร์ไม่ว่าง; lines ≥1; รสในแคตตาล็อก  
server คำนวณ cupsTotal, productTotal, depositTotal ตาม domain  
ตอบ 201: `{ orderId, queueCode, status: "awaiting_payment", cupsTotal, productTotal, depositTotal, paymentChannel }`

### `GET /api/v1/queue/:queueCode`
ตอบออเดอร์สาธารณะ: สถานะ, ยอด, lines, paymentChannel (ถ้ายังรอชำระ/ปฏิเสธ), slipRejectReason ล่าสุด (ถ้ามี), timestamps ที่จำเป็น  
404 ถ้ารหัสผิด

### `POST /api/v1/queue/:queueCode/slips`
multipart: `file` (image)  
อนุญาตเมื่อ status ∈ awaiting_payment | slip_rejected  
→ status = awaiting_slip_review  
ตอบ: `{ status, slipId }`

### `POST /api/v1/queue/:queueCode/cancel`
อนุญาตเมื่อ status ∈ awaiting_payment | awaiting_slip_review | slip_rejected | in_queue  
→ cancelled  
403 ถ้ายกเลิกไม่ได้

## Admin

### `POST /api/v1/admin/slips/:slipId/approve`
→ order status = in_queue

### `POST /api/v1/admin/slips/:slipId/reject`
body: `{ reason: string non-empty }`  
→ slip_rejected

### `PATCH /api/v1/admin/orders/:orderId/status`
body: `{ status: "packing"|"ready_for_pickup"|"picked_up" }`  
ต้องตาม transition ใน `docs/statuses.md` เท่านั้น

### `GET|PUT /api/v1/admin/config/pricing`
PUT body: `{ basePricePerCup, bulkThresholdCups, bulkPricePerCup }` — มีผลออเดอร์ใหม่

### `GET|PUT /api/v1/admin/config/payment-channel`
PUT: `{ bankAccountNumber, bankName }` + optional multipart qr image

### `POST /api/v1/admin/orders/:orderId/deposit-returns`
body: `{ cratesReturned: true }`  
คืนมัดจำได้เมื่อบันทึกคืนลังแล้วเท่านั้น

### `GET /api/v1/admin/orders?status=`
ลิสต์ออเดอร์ฝั่งร้าน (ไม่ใช่ลูกค้า) — สำหรับแพ็ค/อนุมัติ

## Non-goals API v1
- ไม่มี auth ลูกค้า / ไม่มี list ออเดอร์ลูกค้าโดยไม่มี queueCode
- ไม่มี shipping endpoints
- ไม่มี auto-refund endpoint

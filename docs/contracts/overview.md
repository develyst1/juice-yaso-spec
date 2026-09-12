# Contracts overview — intent only (LOCKED domain)

ยังไม่ล็อก HTTP path / DTO สุดท้าย — รอบนี้เป็น conceptual contract ให้ Tanya เขียน acceptance และห้าม Jason/Fero invent นอกนี้

## Entities
- **Order**: ชื่อ, เบอร์, บรรทัดลัง, ยอดสินค้า, ยอดมัดจำ, สถานะ, ลิงก์บัตรคิว
- **OrderLine**: ขนาดลัง, จำนวนลัง, รส (หลายบรรทัด/หลายรสได้)
- **QueueCard**: รหัสสาธารณะ — ชำระ / อัปสลิป / track / ยกเลิก โดยไม่ login
- **PaymentSlip**: ไฟล์สลิป + ประวัติ reject reason
- **AdminDecision**: approve | reject(+reason) จากไลน์
- **PricingConfig**: base / threshold / bulk price
- **DepositConfig**: map ขนาด → มัดจำ
- **PaymentChannelConfig**: รูป QR, เลขบัญชี, ชื่อธนาคาร (แอดมินแก้ได้เสมอ)
- **FlavorCatalog**: 5 รสที่ล็อก
- **DepositReturn**: บันทึกคืนลัง / คืนมัดจำ

## API surface (intent)
- สร้างออเดอร์ (anonymous) พร้อมชื่อ+เบอร์
- อ่านออเดอร์ด้วยรหัสบัตรคิว
- อัปสลิป / อัปสลิปใหม่หลัง reject
- ยกเลิกออเดอร์ (ถ้าสถานะอนุญาต)
- แอดมิน/ไลน์: approve/reject สลิป
- แอดมิน: อ่าน/อัปเดต PricingConfig, PaymentChannelConfig
- แอดมิน/ร้าน: เลื่อนสถานะ packing → ready → picked_up
- บันทึกคืนลัง/คืนมัดจำ

## Forbidden invent
- อย่าเพิ่มฟิลด์ที่อยู่ส่ง / โซนส่ง ใน v1
- อย่าทำระบบดูออเดอร์เก่าโดยไม่มีบัตรคิว
- อย่า auto-refund ใน v1 โดยไม่มีกติกาเพิ่มจากมนุษย์

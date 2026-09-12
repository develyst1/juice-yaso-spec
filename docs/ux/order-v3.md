# Order UX v3 — LOCKED (Sober)

เป้าหมาย: หน้าสั่งรู้สึกเป็นลูกค้า · มือถือไม่ดึงคีย์บอร์ดมั่ว · ลังคละรสได้  
API/DB: ดู `docs/contracts/api.md` + `db.md` (อัปเดตรอบนี้) · สถานะ/ราคา/มัดจำเดิม

## 1. Phone validation
- รับ **ตัวเลขเท่านั้น** (strip ช่องว่าง/ขีด ตอนกรอกได้ แต่ค่าที่ส่ง API = digits)
- รูปแบบล็อก: **10 หลัก ขึ้นต้นด้วย `0`** regex `^0\d{9}$`
- ตรวจทั้ง front และ back · ผิด → 400

## 2. Checkout step
1. หน้าแรกโฟกัส **จัดลัง/รส** + สรุปยอด (ยังไม่โชว์ฟอร์มชื่อ-เบอร์เต็มจอ)
2. กด CTA สั่ง → **step/modal** กรอกชื่อ + เบอร์ → ยืนยันสร้างออเดอร์
3. สำเร็จ → ไป `/q/[queueCode]`

## 3. Crate builder UI (มือถือก่อน)
- เลือกขนาดลังด้วย **ชิป/การ์ด** (30/50/60/100) — ไม่ใช้ textbox จำนวนลัง
- ในลัง: แสดง **แก้วสีต่อรส** (Tabler / สีแบรนด์) กด **+/−** เพิ่ม-ลด cups · **ห้าม** `<input type="number">` สำหรับจำนวนแก้ว/ลังบนมือถือ
- ผลรวม cups ในลังต้อง = ขนาดลัง ก่อนเพิ่มลังหรือก่อนไป step ชำระ
- เพิ่มหลายลังได้ · แต่ละลังคละรสในตัวเองได้
- animation: อนุญาต Mantine `Transition` / `Collapse` / CSS · ห้าม emoji

## 4. Mixed flavors in one crate (ธุรกิจ)
ตัวอย่าง: ลัง 50 = ส้ม 25 + ลิ้นจี่ 25  
มัดจำคิด **ต่อลังตามขนาด** (ไม่แยกตามรส)  
ราคาคิดจาก **แก้วรวมทั้งออเดอร์** ตามกติกาเดิม

## 5. Hydration
แก้ `data-mantine-color-scheme` mismatch ที่ `layout.tsx`  
แนะนำ: `suppressHydrationWarning` บน `<html>` + `defaultColorScheme="light"` บน `MantineProvider` + `ColorSchemeScript defaultColorScheme="light"`

## 6. นอกขอบเขต
Deploy · ส่งของ · login · เปลี่ยนกติคาสถานะ/ราคาฐาน

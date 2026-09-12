# Order UX / API v3 testcases (Tanya)

แหล่ง: `docs/ux/order-v3.md` + `docs/contracts/api.md` v1.1  
สถานะ: draft for Jason/Fero slice · หลังมี back/front ให้รันจริงแล้วอัปผล

## ตาราง

| TC | ชั้น | ประเภท | หัวข้อ |
|----|------|--------|--------|
| TC-ORD-V3-01 | API | happy | สร้างออเดอร์ด้วย `crates[].fills[]` คละรสในลัง |
| TC-ORD-V3-02 | API | happy | ลังเดียวรสเดียว (fills 1 รายการ = crateSize) |
| TC-ORD-V3-03 | API | happy | หลายลัง คละคนละแบบ · มัดจำต่อลังตามขนาด |
| TC-ORD-V3-04 | API | fail | `Σ cups ≠ crateSize` → 400 |
| TC-ORD-V3-05 | API | fail | รสซ้ำในลังเดียวกัน → 400 |
| TC-ORD-V3-06 | API | fail | cups ≤ 0 หรือไม่ใช่จำนวนเต็ม → 400 |
| TC-ORD-V3-07 | API | fail | ส่ง `lines` แบบเก่า → 400 ข้อความชัด |
| TC-ORD-V3-08 | API | fail | เบอร์ไม่ตรง `^0\d{9}$` → 400 |
| TC-ORD-V3-09 | API | fail | เบอร์มีตัวอักษร / ไม่ครบ 10 หลัก → 400 |
| TC-ORD-V3-10 | API | fail | ชื่อว่าง → 400 |
| TC-ORD-V3-11 | API | happy | ราคา/มัดจำยังคิดจากแก้วรวม + มัดจำต่อลัง (เช่น ลัง50 คละ = มัดจำ 90) |
| TC-ORD-V3-12 | API | happy | GET queue ตอบ `crates` ไม่ใช่ `lines` |
| TC-ORD-V3-13 | UI | happy | หน้าแรกโฟกัสจัดลัง/รส · ชื่อ-เบอร์เป็น step ตอนกดสั่ง |
| TC-ORD-V3-14 | UI | happy | เลือกขนาดลังด้วยชิป/การ์ด · แก้วรส +/− ไม่มี number input จำนวน |
| TC-ORD-V3-15 | UI | fail | ลังที่ fills ยังไม่ครบขนาด · บล็อกเพิ่มลัง/ไป step |
| TC-ORD-V3-16 | UI | fail | ช่องเบอร์รับแต่ตัวเลข · ส่ง API เป็น digits |
| TC-ORD-V3-17 | UI | happy | ตัวอย่างลัง 50 = ส้ม25 + ลิ้นจี่25 สร้างออเดอร์ได้ |
| TC-ORD-V3-18 | UI | happy | ไม่มี emoji ใน flow สั่ง |
| TC-ORD-V3-19 | UI | happy | hydration: ไม่มี mismatch `data-mantine-color-scheme` บน `/` (dev overlay) |

---

### TC-ORD-V3-01 คละรสในลัง
**ขั้นตอน:** `POST /orders` crates: `[{ crateSize:50, fills:[{flavor:orange,cups:25},{flavor:lychee,cups:25}] }]` + ชื่อ + เบอร์ถูกต้อง  
**คาดหวัง:** 201 · cupsTotal=50 · depositTotal=90 · status awaiting_payment

### TC-ORD-V3-02 รสเดียวเต็มลัง
**ข้อมูล:** crateSize 30 · fills orange 30  
**คาดหวัง:** 201 · cupsTotal=30 · deposit=50

### TC-ORD-V3-03 หลายลัง
**ข้อมูล:** ลัง30 ส้ม30 + ลัง50 ส้ม25/ลิ้นจี่25  
**คาดหวัง:** cupsTotal=80 · depositTotal=50+90=140 · product ตามราคา ≤100 → ×5

### TC-ORD-V3-04 ผลรวม cups ไม่เท่าขนาดลัง
**ข้อมูล:** crateSize 50 · fills รวม 40  
**คาดหวัง:** 400

### TC-ORD-V3-05 รสซ้ำในลัง
**ข้อมูล:** fills สองรายการ flavor เดียวกัน  
**คาดหวัง:** 400

### TC-ORD-V3-06 cups ไม่ถูกต้อง
**ข้อมูล:** cups 0 หรือติดลบหรือทศนิยม  
**คาดหวัง:** 400

### TC-ORD-V3-07 payload `lines` เก่า
**ขั้นตอน:** ส่ง body แบบ v1 `lines:[{crateSize,quantity,flavor}]`  
**คาดหวัง:** 400 + ข้อความบอกให้ใช้ `crates`

### TC-ORD-V3-08 / 09 เบอร์
| ค่า | คาดหวัง |
|-----|---------|
| `0812345678` | ผ่าน (คู่ happy) |
| `812345678` (ไม่มี 0) | 400 |
| `081234567` (9 หลัก) | 400 |
| `08123456789` (11) | 400 |
| `08abcdefg1` | 400 |
| `+66812345678` | 400 (ต้อง digits ขึ้นต้น 0) |

### TC-ORD-V3-10 ชื่อว่าง
**คาดหวัง:** 400

### TC-ORD-V3-11 มัดจำ/ราคา
ลัง 100 คละสองรส รวม 100 แก้ว · มัดจำ = 150 (ไม่แยกตามรส) · ถ้าออเดอร์นี้แก้วรวม =100 ใช้ราคา 5; ถ้ามีลังอื่นทำให้ >100 ใช้ 4.5 ทั้งออเดอร์

### TC-ORD-V3-12 GET queue
**คาดหวัง:** มี `crates[].fills[]` · ไม่พึ่ง `lines`

### TC-ORD-V3-13 UI step
**คาดหวัง:** ก่อนกด CTA ไม่บังคับฟอร์มชื่อ-เบอร์เต็มจอ · หลังกดมี step/modal กรอกแล้วค่อยสร้างออเดอร์

### TC-ORD-V3-14 UI แก้ว +/−
**คาดหวัง:** ไม่มี `<input type="number">` สำหรับจำนวนแก้ว/จำนวนลังบนหน้าสั่งมือถือ · ใช้ชิปขนาดลัง + ปุ่ม +/−

### TC-ORD-V3-15 UI บังคับเติมลังให้เต็ม
**คาดหวัง:** ถ้า cups ในลัง ≠ ขนาดลัง ไป step/เพิ่มลังไม่ได้ + ข้อความชัด

### TC-ORD-V3-16 UI เบอร์ตัวเลข
**คาดหวัง:** พิมพ์ตัวอักษรไม่เข้า (หรือถูก strip) · ค่าที่ส่งตรง `^0\d{9}$`

### TC-ORD-V3-17 UI ตัวอย่างคละ
**คาดหวัง:** สร้างลัง 50 ส้ม25+ลิ้นจี่25 ผ่าน UI แล้วได้บัตรคิว

### TC-ORD-V3-18 ไม่มี emoji
**คาดหวัง:** ไม่มีอักขระ emoji ใน UI สั่ง/step

### TC-ORD-V3-19 Hydration
**คาดหวัง:** เปิด `/` ใน dev ไม่ขึ้น overlay hydration เรื่อง `data-mantine-color-scheme`

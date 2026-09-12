# Flavor icons v3c — LOCKED

API: **ไม่เปลี่ยน** · UI only บน `juice-yaso-front` สาขา `Fero`

## Assets (source of truth ใน spec)
`docs/assets/flavors/`
| file | code | ไทย |
|------|------|------|
| flavor-orange.jpg | orange | ส้ม |
| flavor-grape.jpg | grape | องุ่น |
| flavor-cocoa.jpg | cocoa | โกโก้ |
| flavor-lychee.jpg | lychee | ลิ้นจี่ |
| flavor-blueberry.jpg | blueberry | บลูเบอร์รี่ |

คัดลอกไป front: `src/shared/assets/flavors/` หรือ `public/flavors/`

## UI rules
- แทนวงกลมสีทึบใน FlavorStepper ด้วยรูปด้านบน
- แสดงเป็น**วงกลม** (`border-radius: 999` / `object-fit: cover`)
- **ตัวเลขจำนวน**ต้องอ่านชัด — overlay ทับกลางรูป หรือ badge มุม
- คง +/− · +5/+10 · เต็มที่เหลือ ตาม v3b
- ห้าม emoji · ห้ามเปลี่ยน API

## DoD
- 5 รสเป็นรูปชุดเดียวกัน
- จำนวน+ปุ่มเร็วใช้ได้
- Tanya smoke หน้าสั่ง · merge ผ่าน develop · ไม่ deploy

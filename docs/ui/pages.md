# Pages & components — LOCKED

พฤติกรรมธุรกิจคงตาม usecase v1 · เปลี่ยนเฉพาะ UI

## `/` — สั่ง (ไม่ login)
Mantine: `Container`, `Title`, `Text`, `Card`, `Stack`, `Group`, `Select`/`NumberInput`, `TextInput`, `Button`, `Divider`, `Alert`
- เลือกลังหลายบรรทัด + รส (ปนได้)
- ชื่อ + เบอร์ บังคับ
- สรุปยอดสินค้า + มัดจำ (อ่านจาก catalog/pricing)
- CTA สั่ง → ไป `/q/[queueCode]`
Icons: Tabler เช่น `IconCup`, `IconPlus`, `IconTrash`, `IconShoppingCart` — **ไม่ใช้ emoji**

## `/q/[queueCode]` — บัตรคิว
Mantine: `Card`, `Badge`, `Image` (QR), `FileButton`/`Dropzone`, `Button`, `Alert`, `List`
- แสดงสถานะ (Badge ตาม theme.md)
- ช่องทางโอน + อัปสลิป เมื่อสถานะอนุญาต
- เหตุผลปฏิเสธเมื่อ `slip_rejected`
- ยกเลิกเมื่ออยู่ใน CANCELABLE
Icons: `IconQrcode`, `IconUpload`, `IconX`, `IconRefresh`

## `/admin` — แอดมินร้าน
Mantine: `AppShell` หรือ `Tabs` + `Table`/`Cards`, `PasswordInput` (token), `Modal` ยืนยัน
- ใส่ `X-Admin-Token` (sessionStorage) — ไม่ commit secret
- ลิสต์ออเดอร์ + filter สถานะ
- approve/reject ด้วย `pendingSlipId`
- เลื่อน packing → ready → picked_up
- แก้ PricingConfig / PaymentChannel (+ อัป QR)
- คืนลัง/มัดจำ
Icons: `IconShieldLock`, `IconCheck`, `IconBan`, `IconPackage`, …

## Shared UI
- `BrandHeader` — โลโก้/ชื่อร้าน + โทนส้ม (ใช้ภาพจาก brand-refs ได้)
- `StatusBadge` — map สถานะ
- `MoneyText` — จัดรูปแบบบาท
- `EmptyState` — ใช้ icon ไม่ใช้ emoji

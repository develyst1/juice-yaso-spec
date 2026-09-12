# Folder structure — LOCKED (scalable)

```
src/
  app/                          # Next.js App Router เท่านั้น
    layout.tsx                  # MantineProvider + theme
    page.tsx                    # หน้าสั่ง (หรือ re-export จาก features)
    q/[queueCode]/page.tsx
    admin/page.tsx
    globals.css                 # บาง ๆ — ส่วนใหญ่ใช้ Mantine theme
  features/
    order/                      # สั่งลัง/รส/ชื่อเบอร์
      ui/
      hooks/
      model/                    # types เฉพาะโดเมนถ้าต้อง
    queue/                      # บัตรคิว ชำระ สลิป ยกเลิก track
      ui/
      hooks/
    admin/                      # ลิสต์ออเดอร์ สลิป สถานะ config
      ui/
      hooks/
    payment/                    # แสดง QR/บัญชี (shared ระหว่าง queue/admin)
      ui/
  shared/
    api/                        # client เรียก back ตาม api.md เท่านั้น
    ui/                         # wrappers Mantine ใช้ซ้ำ (AppShell, PageHeader, …)
    lib/                        # money, status maps, formatters
    config/                     # env (NEXT_PUBLIC_API_BASE_URL)
    assets/                     # รูปแบรนด์ที่ commit ใน front (คัดจาก brand-refs)
  theme/
    index.ts                    # createTheme export
    colors.ts
```

กฎ:
- หน้าใน `app/` บาง · logic/UI อยู่ใน `features/*`
- **ห้าม** front ต่อ Postgres
- **ห้าม** invent endpoint — ถ้าขาดฟิลด์ ส่งกลับ Sober

# Mantine v7 theme — LOCKED

Stack: **Next.js App Router + TypeScript + Mantine v7** + `@tabler/icons-react`

## Color tokens
| Token | Value (approx) | ใช้กับ |
|-------|----------------|--------|
| `primary` | `#EA580C` (orange-600) | ปุ่มหลัก, ลิงก์, badge สำคัญ |
| `primaryHover` | `#C2410C` (orange-700) | hover |
| `cream` | `#FFF7ED` | พื้นหลังหน้า |
| `surface` | `#FFFFFF` | การ์ด |
| `ink` | `#431407` | ข้อความหลัก |
| `inkMuted` | `#9A3412` / orange-800 @ 70% | คำอธิบาย |
| `border` | `#FED7AA` (orange-200) | ขอบการ์ด/อินพุต |
| `danger` | `#DC2626` | ปฏิเสธ / ลบ |
| `success` | `#16A34A` | สำเร็จ / พร้อมรับ |

Mantine: สร้าง `createTheme` ด้วย `primaryColor: 'brand'` และ palette ส้มตามด้านบน · `defaultRadius: 'lg'` หรือ `'xl'`

## Shape / density
- radius: **lg–xl** ทั้งปุ่ม การ์ด อินพุต (โทนน่ารัก โค้งมน)
- spacing: สบายตา · การ์ดมี padding พอ
- shadows: นุ่ม (`sm`) ไม่เข้ม

## Typography
- ฟอนต์ไทยอ่านง่าย (เช่น Noto Sans Thai / IBM Plex Sans Thai) ผ่าน `next/font`
- หัวข้อ: bold สี primary/ink
- ห้ามใส่ emoji ในข้อความ UI

## Status colors (map จาก statuses.md)
ใช้ `Badge` + สีตามสถานะ — ไม่ใช้ emoji:
- awaiting_payment → orange
- awaiting_slip_review → yellow
- slip_rejected → red
- in_queue → blue/cyan
- packing → grape/violet
- ready_for_pickup → green
- picked_up → gray/teal
- cancelled → gray

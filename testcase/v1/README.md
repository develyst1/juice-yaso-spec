# Testcases — Juice Yaso v1 / v1.1

สถานะ: Tanya · source statuses + usecases + domain + **UX/API v3**  
Acceptance ระดับโดเมน — ยังไม่ผูก HTTP path นอกที่ระบุใน api.md

| ไฟล์ | ครอบคลุม |
|------|----------|
| [customer.md](./customer.md) | UC-CUS-01..07 (payload เดิม `lines` — **เลิกใช้หลัง v1.1**; คงไว้เป็นประวัติ) |
| [order-v3.md](./order-v3.md) | **ใช้ตรวจรอบนี้** — crates+fills คละรส · เบอร์ · step UI · hydration |
| [admin.md](./admin.md) | UC-ADM · รายละเอียดออเดอร์แสดง crates |
| [statuses.md](./statuses.md) | transition 8 · ยกเลิก · slip loop |
| [deposit-scope.md](./deposit-scope.md) | UC-DEP · e2e · นอกขอบเขต |

## หมายเหตุ v1.1
- `POST /orders` ต้องใช้ `crates[].fills[]` · ห้าม `lines`
- เบอร์ `^0\d{9}$`
- มัดจำยังคิดต่อลังตามขนาด · ราคาคิดจากแก้วรวมออเดอร์

## DoD รอบ UX v3
- [x] เคสคละรส / เบอร์ / step / UI แก้ว / hydration อยู่ใน `order-v3.md`
- [ ] Jason back slice ผ่านเคส API
- [ ] Fero front slice ผ่านเคส UI
- [ ] Tanya รันจริงหลังมี slice แล้วอัปผล

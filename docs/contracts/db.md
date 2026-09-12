# DB schema — Juice Yaso v1 (LOCKED for implement)

PostgreSQL · back เท่านั้นถือ connection string (env `DATABASE_URL`)  
**ถ้ายังไม่มี connection string จริง: scaffold schema/migration + รันด้วย local/docker ได้ แต่ห้าม commit secret; ต้อง ASK ME ก่อนผูก production**

## Tables

### flavors (seed)
- code PK text — orange, grape, cocoa, lychee, blueberry
- name_th text

### pricing_config (singleton row id=1)
- base_price_per_cup numeric
- bulk_threshold_cups int — default 100
- bulk_price_per_cup numeric — default 4.5
- updated_at

### payment_channel_config (singleton)
- bank_account_number text
- bank_name text
- qr_image_url text null
- updated_at

### orders
- id uuid PK
- queue_code text UNIQUE NOT NULL — สาธารณะ
- customer_name text NOT NULL
- customer_phone text NOT NULL
- status text NOT NULL — enum 8 ค่าอังกฤษ
- cups_total int NOT NULL
- product_total numeric NOT NULL
- deposit_total numeric NOT NULL
- unit_price_applied numeric NOT NULL — 5 หรือ 4.5 ตอนสั่ง
- slip_reject_reason text null
- created_at, updated_at
- cancelled_at null
- deposit_returned_at null
- crates_returned_at null

### order_lines
- id uuid PK
- order_id FK
- crate_size int — 30|50|60|100
- quantity int >0
- flavor_code text FK flavors
- line_cups int
- line_deposit numeric

### payment_slips
- id uuid PK
- order_id FK
- file_url text NOT NULL
- status text — pending|approved|rejected
- reject_reason text null
- created_at, reviewed_at null

### order_status_events (optional แต่แนะนำ)
- id, order_id, from_status, to_status, at, actor — admin|customer|system

### deposit_returns
- id, order_id UNIQUE, returned_at, amount, note null

## Indexes
- orders(queue_code) unique
- orders(status)
- payment_slips(order_id)

## Rules in DB/app
- Transition ตรวจใน service layer ตาม `docs/statuses.md` (อย่าพึ่ง DB trigger อย่างเดียวก็ได้ แต่ต้อง enforce)
- ไม่เก็บที่อยู่ส่ง


## v1.1 schema change — mixed flavors per crate

แทนที่โมเดล `order_lines` แบบหนึ่งบรรทัดหนึ่งรส ด้วย:

### order_crates
- id uuid PK
- order_id FK
- crate_size int — 30|50|60|100
- line_deposit numeric — มัดจำของลังนี้ตามขนาด

### order_crate_fills
- id uuid PK
- crate_id FK → order_crates
- flavor_code text FK flavors
- cups int > 0
- UNIQUE(crate_id, flavor_code)
- CHECK: ผลรวม cups ต่อ crate_id ต้องเท่า crate_size (enforce ใน service; DB trigger optional)

Migration: drop หรือเลิกใช้ `order_lines` สำหรับออเดอร์ใหม่ · ออเดอร์เก่ารอบ local ล้างได้

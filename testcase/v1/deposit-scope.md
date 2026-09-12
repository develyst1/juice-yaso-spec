# Deposit, E2E, out of scope

| TC | UC/scope | ประเภท | หัวข้อ |
|----|----------|--------|--------|
| TC-DEP-01-01 | UC-DEP-01 | happy | บันทึกคืนลังแล้ว → คืนมัดจำได้ สถานะมัดจำชัด |
| TC-DEP-01-02 | UC-DEP-01 | fail | คืนมัดจำโดยยังไม่บันทึกคืนลัง |
| TC-FLOW-01 | flow | e2e | สั่ง → ชำระ → approve → แพ็ค → พร้อมรับ → รับแล้ว |
| TC-FLOW-02 | flow | e2e | สั่ง → อัปสลิป → reject → อัปใหม่ → approve |
| TC-FLOW-03 | flow | e2e | สั่ง → ยกเลิกก่อน packing |
| TC-OUT-01 | scope | negative | ไม่คิดราคาตลาด ~9 ในแอป |
| TC-OUT-02 | scope | negative | ไม่มี login ลูกค้า |
| TC-OUT-03 | scope | negative | ไม่มีฟีเจอร์ส่งของใน v1 |
| TC-OUT-04 | scope | negative | ไม่เก็บที่อยู่ส่งใน v1 |
| TC-OUT-05 | scope | negative | ไม่มีดูออเดอร์เก่าโดยไม่มีบัตรคิว |

### TC-DEP-01-01
**เงื่อนไขก่อน:** ออเดอร์มีมัดจำ · บันทึกคืนลังแล้ว  
**คาดหวัง:** DepositReturn สำเร็จ; สถานะมัดจำ = คืนแล้ว; ยอดตรงมัดจำที่ผูกออเดอร์

### TC-DEP-01-02
**คาดหวัง:** คืนมัดจำไม่ได้จนกว่าจะบันทึกคืนลัง

### TC-FLOW-01 happy path รับที่ร้าน
1. สั่ง (ชื่อ+เบอร์, ปนรสได้) → รอชำระ  
2. เห็น QR/บัญชี → อัปสลิป → รอตรวจ  
3. approve → อยู่ในคิว  
4. packing → ready_for_pickup → picked_up  
**คาดหวัง:** track ด้วยบัตรคิวตลอด; ไม่มีขั้นตอนส่งของ

### TC-FLOW-02 slip loop
ตาม UC-CUS-05b จน approve

### TC-FLOW-03 cancel path
ยกเลิกสำเร็จก่อน packing

### TC-OUT-01..05
ยืนยันไม่มี UI/API ของฟีเจอร์นอกขอบเขต v1

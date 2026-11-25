# CACAO CORNER — Chocolate Store

ชุดเว็บไซต์ร้านช็อกโกแลตแบบ Static HTML/CSS/JS + Google Sheets/Apps Script backend พร้อมหน้าจัดการออเดอร์และเครื่องมือพิมพ์ Pick List, Packing List และ Shipping Label

## ไฟล์ในชุด
- `index.html` หน้าร้านและ Checkout
- `admin.html` หน้าดึงออเดอร์ พิมพ์รายการหยิบสินค้าและเอกสารแพ็ค/จ่าหน้า
- `style.css`, `script.js`
- `assets/images/` รูป SVG สินค้า/ตกแต่ง (แยก folder)
- `backend/apps-script.gs` API backend

## 1. สร้าง Google Sheets
สร้าง Google Spreadsheet ใหม่ แล้วสร้าง 2 sheet ตั้งชื่อ `Orders` และ `Order_Items` (ปล่อยว่างได้; สคริปต์จะเติม header ให้) คัดลอก Spreadsheet ID จาก URL: `https://docs.google.com/spreadsheets/d/ID/edit`

## 2. ตั้งค่า Apps Script
1. ใน Spreadsheet เปิด Extensions → Apps Script
2. คัดลอกเนื้อหา `backend/apps-script.gs` ลงไฟล์ Code.gs
3. แก้ `SHEET_ID` ให้เป็น Spreadsheet ID และเปลี่ยน `ADMIN_TOKEN` เป็น token ส่วนตัวที่ยาวและเดายาก
4. Save แล้วเลือกฟังก์ชัน `init` และ Run ครั้งแรกเพื่ออนุญาต
5. Deploy → New deployment → เลือก type **Web app**
6. Execute as: **Me**; Who has access: **Anyone** (จำเป็นสำหรับหน้า Checkout; admin listOrders จะตรวจ token)
7. Deploy และคัดลอก Web app URL (ลงท้าย `/exec`)

## 3. เชื่อมหน้าร้าน
เปิด `script.js` แก้บรรทัด `const API_URL=''` ให้เป็น URL Apps Script เช่น `const API_URL='https://script.google.com/macros/s/DEPLOYMENT_ID/exec';`

Checkout ใช้ POST แบบ `no-cors` เนื่องจากข้อจำกัด CORS ของ Apps Script จึงแสดงข้อความแจ้งหลังส่ง แต่ browser ไม่สามารถอ่านผลตอบกลับ/เลข order ID ได้ หากต้องการการยืนยันที่เชื่อถือได้ควรทำ proxy API บน server ที่มี CORS support และเพิ่มการป้องกัน spam/validation

## 4. ใช้หน้า Admin
- เปิด `admin.html`
- ใส่ Web App URL และ token ที่ตั้งไว้ใน Apps Script
- กด โหลดคำสั่งซื้อ
- พิมพ์ Pick List (ยอดรวมทุกออเดอร์), Packing List รายออเดอร์ และ Shipping Label

**คำเตือน:** Admin token ที่ฝังใน Apps Script และป้อนผ่าน browser เป็นแนวทาง demo สำหรับงานเรียน ไม่ใช่ระบบ production; token มีความเสี่ยงหากแชร์ และ API นี้ไม่ควรเปิดข้อมูลลูกค้าให้สาธารณะจริง

## 5. Upload GitHub & Deploy Vercel
1. สร้าง GitHub repository ใหม่
2. Upload files ทั้งหมด (รวม assets และ backend) โดยรักษา folder structure
3. ที่ Vercel เลือก Add New → Project → Import Git Repository
4. Framework Preset: Other; Build Command ว่าง; Output Directory `.`
5. Deploy แล้วใช้ URL ที่ Vercel สร้างให้

## SKU สาธิต
- CC-DARK70 — Dark Chocolate 70% — 159 THB
- CC-MILK — Creamy Milk Chocolate — 129 THB

แก้สินค้า/ราคา/รายละเอียด/รูปใน `script.js` (ตัวอย่าง 2 SKU) ก่อนใช้งานจริง

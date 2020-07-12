---
layout: post
title: ขั้นตอน/วิธีเปิดโหมด Kiosk ใน Chrome สำหรับ Windows
---
**Kiosk** คือเครื่องให้บริการแบบเป็นตู้ๆ วางไว้ตามที่ต่างๆ ที่ำกันสิทธิของ User ให้ทำได้แค่ไม่กี่อย่าง
1. สร้าง shortcut ของ Chrome
2. คลิกขวาเปิด **Properties**
3. ที่ช่อง **Target:** เพิ่ม `...chrome.exe" --kiosk --kiosk-print --overscroll-history-navigation=0 "https://gist.github.com/"` อาจจะแก้เป็นเว็บที่เราต้องการ
    - `--kiosk` - เปิดโหมด kiosk 
    - `--kiosk-print` - เพิ่มซ่อน dialog print จาก User
    - `--disable-pinch` - เพื่อให้จอ kiosk ของ Zoom ไม่ได้ Lock การ Zoom นั้นเอง
    - `--overscroll-history-navigation=0` - Lock ไม่ให้ปัดน่าจอเพื่อย้อนกลับ
4. คลิก **Apply**
5. เปิด shortcut chrome สร้างเอาไว้
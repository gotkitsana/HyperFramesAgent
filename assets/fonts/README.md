# Fonts

ฟอนต์ไทย bold sans-serif สำหรับสไตล์ "สาระไม่น่ารู้"

## ต้องโหลดมาวางที่นี่

ดาวน์โหลดเป็น **.woff2** (ขนาดเล็ก, lint ผ่าน) แล้ววางตรงนี้:

| ไฟล์ที่คาดหวัง | น้ำหนัก | ใช้กับ | แหล่งโหลด |
|---|---|---|---|
| `IBMPlexSansThai-Bold.woff2` | 700 | body / caption | https://fonts.google.com/specimen/IBM+Plex+Sans+Thai |
| `IBMPlexSansThai-Regular.woff2` | 400 | body small | (เดียวกัน) |
| `Anuphan-ExtraBold.woff2` | 800 | display / keyword punch | https://fonts.google.com/specimen/Anuphan |
| `Anuphan-Bold.woff2` | 700 | headline | (เดียวกัน) |
| `BaiJamjuree-SemiBold.woff2` | 600 | alt body | https://fonts.google.com/specimen/Bai+Jamjuree |

## แปลง .ttf → .woff2 (ถ้า Google โหลดมาเป็น .ttf)

```bash
# ใช้ woff2 CLI (brew install woff2)
woff2_compress IBMPlexSansThai-Bold.ttf
```

หรือเว็บแปลง: https://cloudconvert.com/ttf-to-woff2

## การใช้ใน CSS

ดู `compositions/sarra-tokens.css` — `@font-face` ถูก declare ไว้ให้แล้ว
แค่วางไฟล์ตามชื่อข้างบนก็ใช้งานได้ทันที

## ⚠️ หลีกเลี่ยง

- `Kanit`, `Prompt` — เกร่อมาก และอยู่ใน avoid list ของ HyperFrames design guide
- `Noto Sans Thai` — อยู่ใน avoid list
- ฟอนต์ที่ไม่มีน้ำหนัก ≥700 — DNA ต้องการตัวอักษรหนา

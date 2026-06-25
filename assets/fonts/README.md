# Fonts

ฟอนต์ของช่อง "สาระไม่น่ารู้"

## โครงสร้างปัจจุบัน

```
fonts/
├── Mali/              ← PRIMARY — ฟอนต์หลักของช่อง
├── Anuphan/           ← Fallback (variable, น้ำหนัก 100-900)
└── Bai_Jamjuree/      ← Fallback / alt body
```

## Mali (primary)

ใช้กับทุก scene เป็น default ผ่าน `var(--font-display)` และ `var(--font-body)`
ใน `compositions/sarra-tokens.css`

น้ำหนักที่ register ไว้ใน `@font-face`:
- 400 Regular
- 500 Medium
- 600 SemiBold
- 700 Bold (น้ำหนักสูงสุดที่ Mali มี — ใช้กับ headline/keyword คู่กับ outline)
- 700 italic Bold Italic

Mali ไม่มี ExtraBold/Black — ใช้ `--text-outline` (อยู่ใน tokens.css)
เพื่อให้ดูหนาแน่นบนภาพพื้นหลังแทน

## Fallback

- **Anuphan** — variable font, ใช้ได้ทุกน้ำหนัก 100-900 (สำหรับ scene ที่ต้องการ ExtraBold)
- **Bai Jamjuree** — Bold + SemiBold สำหรับ caption เล็ก

## ⚠️ Format note

ไฟล์ตอนนี้เป็น `.ttf` ทำงานได้ปกติใน HyperFrames
ถ้าอยากลดขนาด ติดตั้ง `brew install woff2` แล้วแปลง:
```bash
woff2_compress Mali-Bold.ttf
```

## License

ทุกฟอนต์อยู่ภายใต้ **SIL Open Font License (OFL)** — ดู `OFL.txt` ในแต่ละโฟลเดอร์
ใช้เชิงพาณิชย์ได้ฟรี

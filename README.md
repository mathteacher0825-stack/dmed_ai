# 🏥 Medical Bot — Render Deploy Yo'riqnomasi

## Fayllar
- `bot.py` — Asosiy Flask bot kodi
- `requirements.txt` — Python kutubxonalar
- `Procfile` — Render uchun start buyrug'i

---

## Render.com da Deploy Qilish

### 1. GitHub ga yuklang
```bash
git init
git add .
git commit -m "Medical bot"
git remote add origin https://github.com/SIZNING/REPO.git
git push -u origin main
```

### 2. Render.com da yangi Web Service yarating
- **Build Command:** `pip install -r requirements.txt`
- **Start Command:** `gunicorn bot:app --bind 0.0.0.0:$PORT --workers 1 --timeout 120`

### 3. Environment Variables (Render → Environment)
```
BOT_TOKEN=PASTE_YOUR_TELEGRAM_BOT_TOKEN
ADMIN_ID=SIZNING_TELEGRAM_ID  ← @userinfobot orqali toping
TEST_MODE=true                 ← test uchun (1 soat), keyin false qiling
PORT=10000

# ixtiyoriy:
DB_PATH=medical_bot.db
AUTO_SET_WEBHOOK=false
PUBLIC_URL=https://SIZNING-APP.onrender.com
```

### 4. Webhook o'rnatish
Deploy tugagach, brauzerda:
```
https://SIZNING-APP.onrender.com/set_webhook?url=https://SIZNING-APP.onrender.com
```

---

## Botdan foydalanish

### Admin (siz):
- `/start` — Admin panelini ochadi
- Shifokorlar ro'yxati, tasdiqlash, bloklash
- Barcha bemorlar va statistika ko'rish

### Shifokor:
1. `/start` bosadi → ro'yxatdan o'tadi
2. Admin tasdiqlaydi
3. Keyin: bemor qo'shish, ro'yxat ko'rish, status tekshirish

---

## Test Rejimi
`TEST_MODE=true` bo'lsa — 1 SOATDAN keyin xabar keladi  
`TEST_MODE=false` bo'lsa — 80 KUNDAN keyin xabar keladi

---

## Muhim eslatma (DB)
Bot SQLite (`medical_bot.db`) ishlatadi. Ba’zi hostinglarda fayl tizimi deploy/restart’da yangilanib ketishi mumkin.
Agar ma’lumotlar yo‘qolmasin desangiz, Render’da Persistent Disk ulash yoki keyinroq Postgres/MySQL’ga o‘tishni tavsiya qilaman.

---

---

## 🔒 Majburiy obuna (Force Subscribe)
Bot foydalanuvchilardan avval @aidmedbot_med kanaliga a'zo bo'lishni talab qiladi.

**MUHIM SOZLASH:**
1. Botni `@aidmedbot_med` kanaliga **administrator** qilib qo'shing (kamida "Add users via link" / a'zolarni ko'rish huquqi bilan). Bu shart — aks holda `getChatMember` ishlamaydi va hech kim botdan foydalana olmaydi.
2. Ixtiyoriy: boshqa kanal ishlatmoqchi bo'lsangiz, Render Environment'da `FORCE_SUB_CHANNEL` (masalan `@mychannel`) va `FORCE_SUB_CHANNEL_URL` (masalan `https://t.me/mychannel`) qiymatlarini o'zgartiring.

**Qanday ishlaydi:**
- `/start` yoki istalgan tugma/xabar yuborilganda bot avval foydalanuvchining kanalga a'zoligini tekshiradi.
- A'zo bo'lmasa — "📢 Kanalga qo'shilish" va "✅ Tekshirish" tugmalari bilan xabar chiqadi, boshqa hech narsa ishlamaydi.
- "✅ Tekshirish" bosilganda qayta tekshiriladi; a'zo bo'lsa — odatdagi menyu ochiladi, bo'lmasa — yana shu oyna chiqadi.

---

## Endpoint'lar
| URL | Vazifa |
|-----|--------|
| `GET /` | Bot holati |
| `POST /webhook` | Telegram webhook |
| `GET /set_webhook?url=...` | Webhook o'rnatish |
| `GET /health` | DB statistika |

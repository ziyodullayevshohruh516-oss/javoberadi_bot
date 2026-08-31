# Telegram AI Bot (Claude asosida)

## 1. O'rnatish

```bash
pip install -r requirements.txt
```

## 2. API kalitlarini sozlash

Ikkita kalit kerak:

- **Telegram bot tokeni** — @BotFather orqali oling
- **Anthropic API kaliti** — console.anthropic.com dan oling

Muhit o'zgaruvchilari sifatida o'rnating:

```bash
export TELEGRAM_BOT_TOKEN="123456:ABC-DEF..."
export ANTHROPIC_API_KEY="sk-ant-..."
```

(Windows PowerShell'da: `$env:TELEGRAM_BOT_TOKEN="..."`)

## 3. Botni ishga tushirish

```bash
python bot.py
```

## Qanday ishlaydi

- `bot.py` ichidagi `SYSTEM_PROMPT` — botning xarakteri va qoidalarini belgilaydi.
- Har bir foydalanuvchi uchun suhbat tarixi xotirada saqlanadi (oxirgi 20 xabar), shu orqali bot oldingi kontekstni eslab qoladi.
- `/start` — yangi suhbat boshlaydi va tarixni tozalaydi.
- `/reset` — joriy suhbat tarixini tozalaydi.

## Eslatma

Hozirgi versiyada suhbat tarixi **RAM'da** saqlanadi — bot qayta ishga tushganda barcha tarixlar o'chib ketadi. Agar tarixni doimiy saqlash kerak bo'lsa (masalan, SQLite yoki Redis orqali), buni alohida so'rang — men qo'shib beraman.

---

## Render + GitHub orqali doimiy ishlab turadigan qilib deploy qilish

Kod avtomatik ravishda: lokal kompyuterda **polling** rejimida, Render'da esa **webhook** rejimida ishlaydi (`bot.py` buni o'zi aniqlaydi).

### 1. Kodni GitHub'ga yuklash

```bash
cd telegram_bot
git init
git add .
git commit -m "Telegram AI bot"
git branch -M main
git remote add origin https://github.com/SIZNING_USERNAME/SIZNING_REPO.git
git push -u origin main
```

### 2. Render'da yangi servis yaratish

1. [render.com](https://render.com) ga kiring, GitHub akkauntingizni ulang.
2. **New +** → **Web Service** ni tanlang.
3. GitHub repo'ingizni tanlang (repo ichida `render.yaml` bo'lgani uchun Render sozlamalarni avtomatik o'qiydi — **Apply** tugmasini bosing).
4. Agar `render.yaml` avtomatik ishlamasa, qo'lda kiriting:
   - **Runtime:** Python 3
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `python bot.py`

### 3. Muhit o'zgaruvchilarini kiritish

Render dashboard'da **Environment** bo'limiga o'ting va qo'shing:

| Key | Value |
|---|---|
| `TELEGRAM_BOT_TOKEN` | @BotFather'dan olingan token |
| `ANTHROPIC_API_KEY` | console.anthropic.com'dan olingan kalit |

`PORT` va `RENDER_EXTERNAL_URL` — Render tomonidan **avtomatik** beriladi, ularni qo'lda kiritish shart emas.

### 4. Deploy va tekshirish

- **Create Web Service** tugmasini bosing — Render kodni build qilib, ishga tushiradi.
- Loglarda `Bot webhook rejimida ishga tushdi: ...` degan xabarni ko'rsangiz — bot ishlayapti.
- Telegram'da botga `/start` yozib tekshiring.

### Free plan haqida muhim eslatma

Render'ning **bepul** rejasida servis 15 daqiqa faoliyatsiz qolsa, "uxlab qoladi" va keyingi so'rovda 30-60 soniya sekin uyg'onadi (webhook rejimida bu odatda muammo tug'dirmaydi, chunki Telegram xabarni qayta yuboradi, lekin birinchi javob kechikishi mumkin). **Har doim, kechikishsiz** ishlab turishi uchun Render'ning pullik **Starter** rejasiga (~$7/oy) o'tish tavsiya etiladi.

### Kodni yangilaganda

```bash
git add .
git commit -m "Yangilanish tavsifi"
git push
```

Render GitHub'dagi o'zgarishni avtomatik aniqlab, qayta deploy qiladi.

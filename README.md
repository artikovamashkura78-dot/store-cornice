# CORNICE DECOR STORE

Bambuk panellar va interyer dekorlari uchun to‘liq ishlaydigan onlayn do‘kon: sayt, admin panel, mahsulotlar, kategoriyalar va buyurtmalarni boshqarish.

**Texnologiyalar:** React + Vite + Tailwind CSS · Node.js + Express · MongoDB Atlas · Cloudinary · JWT + bcrypt

---

## ▶️ Kompyuterda ochish (eng oson yo‘l)

1. [nodejs.org](https://nodejs.org) dan **LTS** versiyani o‘rnating (bir marta).
2. Arxivni oching.
3. Ikki marta bosing:
   - **Windows:** `ISHGA-TUSHIRISH-Windows.bat`
   - **Mac:** `ishga-tushirish-Mac.command` (birinchi marta: o‘ng tugma → **Open**)
4. Kuting — birinchi marta 3–5 daqiqa (paketlar va ma’lumotlar bazasi yuklanadi, internet kerak). Keyingi safar ~10 soniya.
5. Google Chrome o‘zi ochiladi: **http://localhost:5000**
   Admin panel: **http://localhost:5000/admin** → login `admin`, parol `455217`

Qora oynani yopmang — u yopilsa sayt ham to‘xtaydi. Ma’lumotlar (mahsulotlar, buyurtmalar) `server/local-data` papkasida saqlanadi va keyingi safar ham joyida bo‘ladi.

> Hech qanday MongoDB hisobi yoki `.env` fayl kerak emas. `server/.env` da haqiqiy `MONGODB_URI` yozilgan bo‘lsa, o‘sha baza ishlatiladi.
> 5000-port band bo‘lsa (masalan, Mac’da AirPlay), server avtomatik 5001, 5002… portni tanlaydi va to‘g‘ri manzilni o‘zi ochadi.

---

## 🌐 Netlify’ga joylash

Loyiha Netlify uchun tayyor: `netlify.toml` bor. Sayt Netlify CDN’dan, API esa Netlify Functions orqali ishlaydi, ikkalasi bitta havolada. Netlify’da sayt «uxlab qolmaydi».

**Kerak bo‘ladi (hammasi bepul):** MongoDB Atlas, Cloudinary, GitHub, Netlify.

1. **MongoDB Atlas** — pastdagi «1-qadam»ni bajaring va ulanish satrini oling.
2. **Cloudinary** — «2-qadam». Netlify’da rasmlar faqat Cloudinary’ga saqlanadi, shuning uchun bu majburiy.
3. **GitHub** — «3-qadam»: zip’ni **yangi bo‘sh papkaga** chiqarib, ichidagi fayllarni yangi repoga yuklang (`node_modules` papkasi bo‘lmasin).
4. **Netlify** — https://app.netlify.com → GitHub bilan kiring:
   - **Add new site → Import an existing project → GitHub** → repongizni tanlang.
   - Build sozlamalari `netlify.toml` dan o‘zi olinadi — hech narsani o‘zgartirmang.
   - **Add environment variables** (yoki keyin: Site configuration → Environment variables):

     | Key | Value |
     |---|---|
     | `MONGODB_URI` | Atlas ulanish satri |
     | `JWT_SECRET` | 40+ tasodifiy belgi |
     | `ADMIN_USERNAME` | `admin` |
     | `ADMIN_PASSWORD` | `455217` |
     | `CLOUDINARY_CLOUD_NAME` | Cloudinary’dan |
     | `CLOUDINARY_API_KEY` | Cloudinary’dan |
     | `CLOUDINARY_API_SECRET` | Cloudinary’dan |

   - **Deploy** → 2–4 daqiqa. Havola: `https://NOM.netlify.app` (Site configuration → Change site name orqali chiroyli nom qo‘yish mumkin, masalan `cornice-decor.netlify.app`).
5. Tekshirish: `https://NOM.netlify.app/api/health` ochilsa `{"ok":true,"db":true,"images":"cloudinary"}` ko‘rinishi kerak.
6. `/admin` ga kiring → Sozlamalar → kontaktlar, so‘ng parolni o‘zgartiring.

> Env o‘zgaruvchilarni deploy’dan keyin qo‘shsangiz yoki o‘zgartirsangiz: **Deploys → Trigger deploy → Deploy site**.

### Admin panelni alohida saytga ajratish (ixtiyoriy)

Bitta GitHub repodan **ikkita** Netlify sayti yaratiladi. Ikkalasi bitta MongoDB bazasidan foydalanadi: admin saytida qo‘shilgan mahsulot darhol do‘konda ko‘rinadi, do‘kondagi buyurtma esa admin saytiga tushadi.

**1) Do‘kon sayti** (masalan `cornice-decor.netlify.app`) — mijozlar uchun:

| Key | Value |
|---|---|
| `SITE_MODE` | `shop` |
| `MONGODB_URI` | Atlas ulanish satri |

Bu saytda admin panel kodi umuman bo‘lmaydi: `/admin` ochilmaydi, serverda ham login va admin so‘rovlari o‘chirilgan.

**2) Admin sayti** (masalan `cornice-admin.netlify.app`) — faqat siz uchun:
Netlify → **Add new site → Import an existing project** → **o‘sha repo**ni yana tanlang.

| Key | Value |
|---|---|
| `SITE_MODE` | `admin` |
| `SHOP_URL` | do‘kon havolasi, masalan `https://cornice-decor.netlify.app` |
| `MONGODB_URI` | **xuddi o‘sha** Atlas satri |
| `JWT_SECRET` | 40+ tasodifiy belgi |
| `ADMIN_PASSWORD` | `455217` |
| `CLOUDINARY_CLOUD_NAME` / `_API_KEY` / `_API_SECRET` | Cloudinary’dan |

Admin saytining istalgan sahifasi admin panelga olib boradi, Google’da ko‘rinmaydi (noindex). «Saytni ko‘rish» va «Saytda ko‘rish» tugmalari `SHOP_URL` dagi do‘konni ochadi.

> `SITE_MODE` qo‘yilmasa — bitta sayt, admin `/admin` manzilida (oldingi variant).

---

## 🇺🇿 O‘zbekcha qo‘llanma

### Nima bor

**Sayt (xaridor uchun)**
- Bosh sahifa: katta rasm, “Interyeringizga zamonaviy ko‘rinish bering” sarlavhasi, kategoriyalar, mashhur mahsulotlar, “Biz haqimizda”, ish vaqti, aloqa va xarita
- Katalog: nom va tavsif bo‘yicha qidiruv, kategoriya bo‘yicha filtr, narx bo‘yicha saralash (arzon→qimmat, qimmat→arzon)
- Mahsulot sahifasi: rasmlar galereyasi (telefonda surish bilan), batafsil tavsif, miqdor, savatga qo‘shish
- Savat: brauzerda saqlanadi (sahifa yangilansa ham yo‘qolmaydi), ochilganda narxlar serverdan qayta tekshiriladi
- Buyurtma formasi: ism-familiya, telefon (+998 format tekshiriladi), yosh (ixtiyoriy), manzil, viloyat, mahalla/ko‘cha/uy, izoh
- Buyurtma raqami (CD-001001, …), muvaffaqiyat sahifasi, savat avtomatik tozalanadi

**Admin panel — `/admin`**
- Login + parol bilan kirish (standart: `admin` / `455217`)
- Boshqaruv paneli: yangi/bugungi buyurtmalar, savdo summasi, so‘nggi buyurtmalar
- Mahsulotlar: qo‘shish, tahrirlash, o‘chirish, bir nechta rasm yuklash (tartiblash, asosiy rasm), saytda ko‘rsatish / mavjudlik / tanlangan tugmalari
- Kategoriyalar: qo‘shish, tahrirlash, o‘chirish, muqova rasmi, tartib
- Buyurtmalar: holat bo‘yicha filtr, qidiruv, holatni o‘zgartirish (Yangi → Tasdiqlandi → Yetkazilmoqda → Yakunlandi / Bekor qilindi), tarix, admin izohi, chop etish
- Sozlamalar: telefon, Telegram, Instagram, manzil, xarita, ish vaqti, “Biz haqimizda” matni, bosh sahifa rasmi, parolni o‘zgartirish

**Xavfsizlik**
- Parol frontend kodida yo‘q; serverda `ADMIN_PASSWORD` muhit o‘zgaruvchisidan olinadi va bazaga faqat **bcrypt xesh** sifatida yoziladi
- Barcha admin API’lar JWT tokenni tekshiradi; `/admin` sahifasini ochishning o‘zi hech qanday huquq bermaydi
- Parol o‘zgartirilsa, eski tokenlar avtomatik bekor bo‘ladi
- Kirish urinishlari cheklangan (15 daqiqada 10 ta xato), buyurtmalar ham cheklangan
- Buyurtma summasi **faqat serverda** ma’lumotlar bazasidagi narxlar bo‘yicha hisoblanadi — brauzerdan kelgan narx va summa e’tiborga olinmaydi
- Yuklangan fayllar haqiqiy rasm ekanligi tekshiriladi

---

### Saytni internetga chiqarish (taxminan 20 daqiqa, bepul)

Sizga 3 ta bepul hisob kerak bo‘ladi: **MongoDB Atlas**, **Cloudinary**, **Render** (va kodni joylash uchun **GitHub**).

#### 1-qadam. MongoDB Atlas (ma’lumotlar bazasi)
1. https://www.mongodb.com/cloud/atlas ga kiring → ro‘yxatdan o‘ting.
2. **Create** → **M0 (Free)** klasterni tanlang → yarating.
3. **Database Access** → **Add New Database User** → login va parol yozing (parolni saqlab qo‘ying).
4. **Network Access** → **Add IP Address** → **Allow access from anywhere** (`0.0.0.0/0`) → Confirm.
5. **Database** → **Connect** → **Drivers** → ulanish satrini nusxalang. U shunga o‘xshaydi:
   ```
   mongodb+srv://LOGIN:PAROL@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
   ```
   `LOGIN` va `PAROL` ni o‘zingiznikiga almashtiring va `.net/` dan keyin baza nomini qo‘shing:
   ```
   mongodb+srv://LOGIN:PAROL@cluster0.xxxxx.mongodb.net/cornice_decor?retryWrites=true&w=majority
   ```

#### 2-qadam. Cloudinary (rasmlar)
1. https://cloudinary.com → bepul ro‘yxatdan o‘ting.
2. **Dashboard** (yoki Settings → API Keys) dan uchta qiymatni oling: **Cloud name**, **API Key**, **API Secret**.

> Cloudinary’siz ham sayt ishlaydi, lekin Render bepul tarifida server qayta ishga tushganda yuklangan rasmlar o‘chib ketadi. Shuning uchun Cloudinary’ni albatta ulang.

#### 3-qadam. Kodni GitHub’ga joylash
1. https://github.com → **New repository** → nom: `cornice-decor-store` → **Private** → Create.
2. Arxivdagi `cornice-decor-store` papkasini shu repoga yuklang (sahifadagi **uploading an existing file** havolasi orqali fayllarni sudrab tashlash mumkin).

#### 4-qadam. Render (hosting)
1. https://render.com → GitHub orqali kiring.
2. **New +** → **Web Service** → repongizni tanlang.
3. Sozlamalar:
   - **Runtime:** Node
   - **Build Command:** `npm run build`
   - **Start Command:** `npm start`
   - **Instance type:** Free
4. **Environment Variables** bo‘limiga quyidagilarni kiriting:

   | Kalit | Qiymat |
   |---|---|
   | `NODE_ENV` | `production` |
   | `MONGODB_URI` | 1-qadamdagi ulanish satri |
   | `JWT_SECRET` | 40+ belgili tasodifiy matn (masalan, parol generatoridan) |
   | `ADMIN_USERNAME` | `admin` |
   | `ADMIN_PASSWORD` | `455217` |
   | `CLOUDINARY_CLOUD_NAME` | Cloudinary’dan |
   | `CLOUDINARY_API_KEY` | Cloudinary’dan |
   | `CLOUDINARY_API_SECRET` | Cloudinary’dan |

5. **Create Web Service** → 3–5 daqiqa kuting. Tayyor bo‘lgach, `https://cornice-decor-store.onrender.com` kabi havola paydo bo‘ladi.

> Muqobil: repoda `render.yaml` bor — Render’da **New + → Blueprint** ni tanlasangiz, sozlamalar avtomatik to‘ldiriladi, faqat maxfiy qiymatlarni kiritasiz.

#### 5-qadam. Birinchi sozlash
1. `https://SAYTINGIZ.onrender.com/admin` ni oching → `admin` / `455217` bilan kiring.
2. **Sozlamalar** → haqiqiy telefon, Telegram, Instagram, manzil, xarita, ish vaqtini kiriting → **Saqlash**.
3. **Sozlamalar** → **Parolni o‘zgartirish** — standart parolni o‘zingiznikiga almashtiring (tavsiya etiladi).
4. **Mahsulotlar** → **Yangi mahsulot** → rasmlar, narx, tavsif → saqlang.
5. Bosh sahifa rasmini o‘z interyer/bambuk rasmingizga almashtiring (Sozlamalar → Bosh sahifa rasmi).

#### 6-qadam. Telegram’da ulashish
- Havolani istalgan chat yoki kanalga yuboring — Telegram ichida to‘g‘ridan-to‘g‘ri ochiladi, sarlavha va rasm bilan ko‘rinadi.
- Bot menyusiga qo‘yish: **@BotFather** → botingizni tanlang → **Bot Settings** → **Menu Button** → sayt havolasini kiriting.

#### Ixtiyoriy: yangi buyurtma haqida Telegram xabar
1. **@BotFather** → `/newbot` → bot yarating, **token**ni oling.
2. Botga istalgan xabar yozing, so‘ng **@userinfobot** orqali o‘z **chat ID**ingizni bilib oling.
3. Render’da `TELEGRAM_BOT_TOKEN` va `TELEGRAM_CHAT_ID` ni qo‘shing. Endi har bir buyurtma Telegram’ga keladi.

#### Muhim eslatmalar
- **Render bepul tarifi** 15 daqiqa faoliyatsizlikdan so‘ng “uxlaydi”, keyingi birinchi ochilish ~30–50 soniya oladi. Doimiy tez ishlashi uchun pullik tarif ($7/oy) yoki uptime-monitoring xizmatidan foydalaning.
- **Parolni unutsangiz:** Render’da `ADMIN_RESET_PASSWORD=true` qo‘shing → servis qayta ishga tushadi va parol `ADMIN_PASSWORD` ga qaytadi → so‘ng bu o‘zgaruvchini o‘chiring.
- **O‘z domeningiz** (masalan, `cornicedecor.uz`): Render → Settings → Custom Domains.

---

### Kompyuterda ishga tushirish (dasturchilar uchun)

Node.js 18.17+ kerak.

```bash
# 1. Paketlarni o‘rnatish
npm run install:all

# 2. Sozlamalar
cp server/.env.example server/.env    # va qiymatlarni to‘ldiring

# 3. Ikki terminalda:
npm run dev:server     # API: http://localhost:5000
npm run dev:client     # Sayt: http://localhost:5173  (API ga avtomatik proksi)
```

Production rejimi: `npm run build && npm start` → http://localhost:5000

---

## 🇬🇧 English summary

A full-stack store for bamboo panels and interior decor. One Node service serves both the REST API and the built React app, so the whole store lives at a single URL that opens cleanly inside Telegram.

**Deploy:** MongoDB Atlas (free M0) → Cloudinary (free) → push to GitHub → Render Web Service with build `npm run build`, start `npm start`, and the env vars listed in `server/.env.example`. Then log in at `/admin` (default `admin` / `455217`), fill in contacts in Settings, and change the password.

**Security highlights:** admin password comes from `ADMIN_PASSWORD` on the server and is stored only as a bcrypt hash; every admin endpoint verifies a signed JWT (role + token version, so a password change revokes old sessions); login and order endpoints are rate-limited; order totals are always recomputed server-side from DB prices; uploads are magic-byte checked; Helmet CSP is enabled.

### Project layout

```
cornice-decor-store/
├── package.json            # root scripts: build / start / dev
├── render.yaml             # optional Render Blueprint
├── server/
│   ├── .env.example
│   └── src/
│       ├── index.js        # Express app, security headers, serves client/dist
│       ├── models/         # Admin, Category, Product, Order, Counter, Settings
│       ├── routes/         # auth, categories, products, orders, settings, upload, stats
│       ├── middleware/     # JWT admin guard, error handler
│       └── utils/          # Cloudinary/local storage, Telegram notify, bootstrap
└── client/
    └── src/
        ├── pages/          # Home, Products, ProductDetail, Cart, Checkout, OrderSuccess, About, Contacts
        ├── components/     # Header, Footer, ProductCard, ContactSection, UI kit
        ├── context/        # Cart (localStorage), Store settings, Toasts
        └── admin/          # Login, Dashboard, Products, ProductForm, Categories, Orders, Settings
```

### API

| Method | Path | Access |
|---|---|---|
| GET | `/api/products?q=&category=&sort=price_asc\|price_desc\|newest&page=&limit=&featured=1` | public |
| GET | `/api/products/:slug` | public |
| POST | `/api/products/lookup` | public (cart price refresh) |
| GET | `/api/categories` | public |
| GET | `/api/settings` | public |
| POST | `/api/orders` | public (rate-limited) |
| POST | `/api/auth/login` | public (rate-limited) |
| GET | `/api/auth/me` · PUT `/api/auth/password` | admin |
| GET `/api/products/admin/list`, `/api/products/admin/:id` · POST/PUT/PATCH/DELETE `/api/products` | admin |
| POST/PUT/DELETE `/api/categories` | admin |
| GET/PATCH/DELETE `/api/orders` | admin |
| PUT `/api/settings` · POST `/api/upload` · GET `/api/admin/stats` | admin |

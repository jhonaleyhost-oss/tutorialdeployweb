# 🚀 Panduan Deploy ke Vercel

## Cara 1: Deploy via GitHub (Recommended)

### 1. Push ke GitHub
Pastikan project sudah ada di repository GitHub.

### 2. Login ke Vercel
- Buka [vercel.com](https://vercel.com)
- Login dengan akun GitHub

### 3. Import Project
- Klik **"Add New Project"**
- Pilih repository **user-server-hub**
- Klik **Import**

### 4. Konfigurasi Build
Vercel biasanya auto-detect, tapi pastikan:
- **Framework Preset:** Vite
- **Build Command:** `npm run build`
- **Output Directory:** `dist`

### 5. Tambahkan Environment Variables
Klik **Environment Variables** dan tambahkan:

| Name | Value |
|------|-------|
| `VITE_SUPABASE_URL` | `https://qjkaoghqatkminsufqfn.supabase.co` |
| `VITE_SUPABASE_PUBLISHABLE_KEY` | *(anon key dari project)* |
| `VITE_SUPABASE_PROJECT_ID` | `qjkaoghqatkminsufqfn` |

### 6. Deploy!
Klik **Deploy** dan tunggu proses build selesai.

### 7. Selesai! 🎉
Website live di `https://nama-project.vercel.app`

Setiap push ke branch `main` akan auto-deploy.

---

## Cara 2: Deploy Manual (Tanpa GitHub)

### 1. Install Vercel CLI
```bash
npm install -g vercel
```

### 2. Login
```bash
vercel login
```

### 3. Build Project
```bash
npm run build
```

### 4. Deploy
```bash
cd dist
vercel --prod
```

Ikuti instruksi di terminal untuk setup project.

---

## ⚠️ Catatan

- File `vercel.json` sudah ada di project untuk handle SPA routing (redirect semua path ke `index.html`)
- Tidak perlu `.htaccess` — Vercel pakai `vercel.json`
- Vercel punya free tier yang cukup untuk project ini
- Custom domain bisa ditambahkan di **Settings → Domains**

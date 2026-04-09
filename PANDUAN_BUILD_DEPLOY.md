# Panduan Build & Deploy ke Vercel dari Termux

## Persiapan (Sekali Saja)

### 1. Install Node.js di Termux
```
pkg update && pkg upgrade
pkg install nodejs-lts
```

### 2. Install Vercel CLI
```
npm install -g vercel
```

### 3. Login Vercel
```
vercel login
```
Pilih metode login (email/GitHub), ikuti instruksinya.

---

## Build & Deploy

### 4. Download & Extract Source Code
Download file `source-code.zip` dari Lovable, lalu:
```
cd ~
pkg install unzip
unzip source-code.zip -d myproject
cd myproject
```

### 5. Hapus file .env lama (penting!)
```
rm -f .env
```

### 6. Install Dependencies
```
npm install
```
Tunggu sampai selesai (bisa 2-5 menit).

### 7. Build dengan Environment Variables Baru
Copy-paste SATU baris ini (jangan dipotong):
```
VITE_SUPABASE_URL="https://szppuwkptnwanfhdexjc.supabase.co" VITE_SUPABASE_PUBLISHABLE_KEY="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InN6cHB1d2twdG53YW5maGRleGpjIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzU3NDIyMjEsImV4cCI6MjA5MTMxODIyMX0.K5JuXyP8CXMJEXMvlNMgRrphXT-FOGJx5l28YSt0gWI" VITE_SUPABASE_PROJECT_ID="szppuwkptnwanfhdexjc" npm run build
```
Tunggu sampai muncul "✓ built in ...".

### 8. Deploy ke Vercel
```
cd dist
vercel --prod
```

Saat ditanya:
- **Set up and deploy?** → `Y`
- **Which scope?** → pilih akun kamu
- **Link to existing project?** → `N` (kalau baru pertama kali)
- **Project name?** → ketik nama bebas (misal: `mypanel`)
- **Directory?** → tekan Enter (pakai `.` / current directory)
- **Override settings?** → `N`

### 9. Selesai! 🎉
Vercel akan kasih URL website kamu, misalnya:
```
https://mypanel-xxxxx.vercel.app
```

---

## Kalau Mau Update/Deploy Ulang

Ulangi dari langkah 7 (build) dan 8 (deploy). Tidak perlu install ulang.

---

## Troubleshooting

| Masalah | Solusi |
|---------|--------|
| `vite: not found` | Jalankan `npm install` dulu di folder project |
| `ENOENT package.json` | Pastikan kamu di folder yang benar (`cd ~/myproject`) |
| `JSON parse error` | Jangan edit package.json manual, pakai yang dari ZIP |
| Website masih pakai database lama | Pastikan build pakai env variables di langkah 7 |

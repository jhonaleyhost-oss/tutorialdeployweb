# Panduan Deploy Website Panel Hosting

## Persiapan

Pastikan Anda sudah memiliki:
- **Source code** project (dari GitHub atau download)
- **Supabase project** dengan database yang sudah di-setup
- 3 environment variables:
  - `VITE_SUPABASE_URL` → URL Supabase (contoh: `https://xxx.supabase.co`)
  - `VITE_SUPABASE_PUBLISHABLE_KEY` → Anon key Supabase
  - `VITE_SUPABASE_PROJECT_ID` → Project ID Supabase

---

## Metode 1: Vercel CLI (dari Termux/Terminal)

### Langkah-langkah:

```bash
# 1. Install Vercel CLI (jika belum)
npm install -g vercel

# 2. Login ke Vercel
vercel login

# 3. Masuk ke folder project
cd nama-folder-project

# 4. Install dependencies
npm install

# 5. Set environment variables di Vercel
vercel env add VITE_SUPABASE_URL
# → paste: https://xxx.supabase.co

vercel env add VITE_SUPABASE_PUBLISHABLE_KEY
# → paste: eyJxxx...

vercel env add VITE_SUPABASE_PROJECT_ID
# → paste: xxx

# 6. Deploy ke production
vercel --prod
```

### Catatan:
- Pertama kali deploy, Vercel akan tanya konfigurasi:
  - **Set up and deploy?** → `Y`
  - **Which scope?** → pilih akun Anda
  - **Link to existing project?** → `N`
  - **Project name?** → terserah Anda
  - **In which directory is your code located?** → `./`
- Setelah deploy, Anda akan mendapat URL seperti `https://nama-project.vercel.app`

---

## Metode 2: Vercel Web Dashboard

### Langkah-langkah:

1. **Push code ke GitHub**
   ```bash
   git init
   git add .
   git commit -m "initial commit"
   git remote add origin https://github.com/USERNAME/REPO.git
   git push -u origin main
   ```

2. **Buka [vercel.com/new](https://vercel.com/new)**

3. **Import repository** → pilih repo GitHub Anda

4. **Konfigurasi project:**
   | Setting | Value |
   |---------|-------|
   | Framework Preset | `Vite` |
   | Build Command | `npm run build` |
   | Output Directory | `dist` |

5. **Tambahkan Environment Variables** (expand bagian Environment Variables):
   | Key | Value |
   |-----|-------|
   | `VITE_SUPABASE_URL` | `https://xxx.supabase.co` |
   | `VITE_SUPABASE_PUBLISHABLE_KEY` | `eyJxxx...` |
   | `VITE_SUPABASE_PROJECT_ID` | `xxx` |

6. **Klik Deploy** ✅

### Keuntungan:
- Setiap push ke GitHub akan **auto-deploy**
- Ada preview untuk setiap pull request
- Dashboard untuk monitoring

---

## Metode 3: cPanel (Manual Upload)

### Langkah 1: Build Project

```bash
# Masuk folder project
cd nama-folder-project

# Install dependencies
npm install

# Build dengan environment variables
VITE_SUPABASE_URL="https://xxx.supabase.co" \
VITE_SUPABASE_PUBLISHABLE_KEY="eyJxxx..." \
VITE_SUPABASE_PROJECT_ID="xxx" \
npm run build
```

> **Penting:** `VITE_*` variables di-embed ke dalam file JS saat build. Harus ada **sebelum/saat** `npm run build`.

### Langkah 2: Upload ke cPanel

1. Buka **cPanel** → **File Manager**
2. Masuk ke folder `public_html` (atau subdomain folder)
3. **Hapus** semua file lama (jika ada)
4. **Upload** semua isi folder `dist/` ke `public_html`

### Langkah 3: Setup .htaccess

Buat file `.htaccess` di `public_html` dengan isi:

```apache
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /

  # Jangan rewrite file yang memang ada
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d

  # Redirect semua route ke index.html (SPA routing)
  RewriteRule . /index.html [L]
</IfModule>

# Cache static assets
<IfModule mod_expires.c>
  ExpiresActive On
  ExpiresByType text/css "access plus 1 year"
  ExpiresByType application/javascript "access plus 1 year"
  ExpiresByType image/png "access plus 1 year"
  ExpiresByType image/jpg "access plus 1 year"
  ExpiresByType image/jpeg "access plus 1 year"
  ExpiresByType image/svg+xml "access plus 1 year"
  ExpiresByType font/woff2 "access plus 1 year"
</IfModule>

# Gzip compression
<IfModule mod_deflate.c>
  AddOutputFilterByType DEFLATE text/html text/css application/javascript application/json image/svg+xml
</IfModule>
```

### Langkah 4: Test

Buka domain Anda di browser → website should be live ✅

---

## Troubleshooting

### Halaman blank / error setelah deploy
- Pastikan environment variables sudah benar
- Cek browser console (F12) untuk error
- Pastikan Supabase project aktif

### 404 saat refresh halaman
- Pastikan `.htaccess` sudah ada (cPanel)
- Pastikan `vercel.json` ada di project (Vercel)

### Database error
- Pastikan sudah menjalankan SQL setup di Supabase SQL Editor
- Pastikan URL dan anon key benar

### Build gagal di Termux
- Coba `npm install --legacy-peer-deps`
- Pastikan Node.js versi 18+ (`node -v`)
- Jika RAM tidak cukup, download ZIP dist/ yang sudah di-build

---

## File Penting

| File | Fungsi |
|------|--------|
| `vercel.json` | Konfigurasi Vercel (SPA routing) |
| `.htaccess` | SPA routing untuk Apache/cPanel |
| `setup_database.sql` | SQL untuk setup database Supabase |
| `dist/` | Folder hasil build, siap upload |

---

## Butuh Bantuan?

Jika tidak bisa build sendiri, Anda bisa minta di-build-kan dengan env variables yang sudah di-embed, tinggal upload folder `dist/` ke hosting.

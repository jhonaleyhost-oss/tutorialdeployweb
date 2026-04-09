# 📦 Panduan Deploy ke cPanel (Manual)

## Langkah-Langkah

### 1. Download File
Download file `dist_cpanel.zip` yang sudah disediakan.

### 2. Login ke cPanel
- Buka `https://domainanda.com/cpanel` atau URL cPanel dari hosting
- Login dengan username & password hosting

### 3. Buka File Manager
- Di halaman cPanel, klik **File Manager**
- Masuk ke folder **public_html** (atau subfolder jika ingin deploy di subdirectory)

### 4. Upload File ZIP
- Klik tombol **Upload** di toolbar atas
- Pilih file `dist_cpanel.zip`
- Tunggu sampai upload selesai

### 5. Extract File ZIP
- Klik kanan file `dist_cpanel.zip` yang sudah diupload
- Pilih **Extract**
- File akan ter-extract ke folder `dist/`

### 6. Pindahkan Isi Folder dist/
**PENTING:** Isi folder `dist/` harus dipindahkan ke `public_html/` langsung (bukan di dalam subfolder `dist/`).

- Masuk ke folder `dist/`
- Pilih semua file (**Select All**)
- Klik **Move**
- Pindahkan ke `/public_html/` (atau folder tujuan Anda)

### 7. Selesai! 🎉
Website sudah live di `https://domainanda.com`

---

## ⚠️ Catatan Penting

- File `.htaccess` sudah termasuk di dalam ZIP — ini diperlukan supaya routing SPA (React Router) berjalan dengan baik
- Pastikan **mod_rewrite** aktif di hosting (biasanya sudah aktif secara default)
- Jika deploy di subfolder (misal `/app`), edit `.htaccess` dan ubah `RewriteBase /` menjadi `RewriteBase /app/`

---

## 🔧 Environment Variables

Website ini membutuhkan koneksi ke backend. Environment variables sudah ter-embed saat build:
- `VITE_SUPABASE_URL`
- `VITE_SUPABASE_PUBLISHABLE_KEY`
- `VITE_SUPABASE_PROJECT_ID`

Jika perlu mengubah nilai-nilai ini, Anda harus rebuild project-nya.

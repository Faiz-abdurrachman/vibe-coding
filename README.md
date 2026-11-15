# Spec-Driven Vibe Coding & Secure Engineering Slide Deck (SKPL_10)

Slide presentasi web interaktif modern berbasis **Sky Blue Glassmorphism**, dirancang khusus untuk sesi live demo 120 menit oleh Faiz (Data Sorcerers Yogyakarta) di Politeknik Negeri Cilacap.

## Fitur Utama & Kepatuhan Aturan
- **Aesthetic**: Sky Blue Glassmorphism murni (`backdrop-blur-xl`, border hairline transparan, ambient gradient radial).
- **Zero Emojis**: 100% bebas emoji, menggunakan badge teknis berstandar industri dan vector SVG icons.
- **Tipografi**: *Plus Jakarta Sans* untuk UI & *JetBrains Mono* untuk kode / SQL.
- **Presenter Tools**:
  - **Speaker Notes Drawer (`N`)**: Panduan kata-kata & poin bahasan Faiz untuk setiap slide.
  - **Quick Grid Selector (`G`)**: Modal bento untuk berpindah langsung ke slide manapun.
  - **Fullscreen Mode (`F`)**: Tampilan layar penuh untuk proyektor Zoom / HDMI.
  - **1-Click Copy Buttons**: Salin prompt PRD dan query SQL Supabase secara instan saat demo.
  - **Responsive & Touch Friendly**: Mendukung navigasi keyboard (`ArrowLeft`, `ArrowRight`, `Space`) dan gesture swipe di layar sentuh / mobile.

---

## Cara Menjalankan Secara Lokal

Pastikan Node.js terpasang, lalu jalankan:

```bash
# Opsi 1: Menjalankan dev server langsung
npx serve -l 3000 .
```

Buka peramban di `http://localhost:3000`.

---

## Cara Deploy ke Vercel (100% Siap Deploy)

Aplikasi ini bersifat statis tanpa build step yang rentan error, dilengkapi dengan `vercel.json` dan security headers.

### Opsi A: Menggunakan Vercel CLI
```bash
# 1. Login ke Vercel (jika belum)
npx vercel login

# 2. Deploy ke production
npx vercel --prod
```

### Opsi B: Menggunakan Repositori GitHub
1. Inisialisasi git dan commit:
   ```bash
   git init
   git add .
   git commit -m "feat: complete interactive sky-blue slide deck"
   ```
2. Hubungkan ke repositori GitHub Anda:
   ```bash
   git branch -M main
   git remote add origin https://github.com/<username>/<repo-name>.git
   git push -u origin main
   ```
3. Buka dashboard [vercel.com](https://vercel.com), klik **"Add New Project"**, lalu impor repositori Anda.
4. Klik **Deploy** (Pengaturan framework: *Other*, Root directory: `./`). Aplikasi akan online dalam waktu kurang dari 15 detik!

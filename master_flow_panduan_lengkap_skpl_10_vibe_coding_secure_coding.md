# MASTER GUIDE & FLOW MENGAJAR FAIZ (120 MENIT)

**Studi Kasus Utama:** `SKPL_10 — Sistem Manajemen Kas Kelas (Cashflow Tracker)`

**Waktu Eksekusi:** Minggu, 15 November 2026 | 09.15 – 11.15 WIB (Zoom Meeting)

## BAGIAN 1: BEDAH DETAIL & MODIFIKASI STRATEGIS `SKPL_10` (WEWENANG BAB 6)

Setelah membedah dokumen `Salinan SKPL_10_Sistem_Manajemen_Kas_Kelas.pdf` dari panitia, ada beberapa celah dari template asli yang **wajib kita upgrade** menggunakan hak veto **Bab 6 (Format Narasumber)**:

> **Kutipan Bab 6 SKPL_10:** *"Apabila terdapat format penyampaian laporan, struktur pengumpulan dokumen, atau spesifikasi khusus yang diberikan secara langsung oleh Narasumber (Narsum)... maka format dari narsum tersebut bersifat mutlak dan harus menjadi acuan utama, mengesampingkan template dasar yang ada pada dokumen ini."*

### Tabel Modifikasi Spesifikasi (Asli Panitia vs Upgrade Narsum Faiz)

| Komponen di PDF SKPL_10 | Tertulis di PDF Asli Panitia | Modifikasi Mutlak Narsum (Faiz) | Alasan Teknis (Kenapa Diubah?) | 
| ----- | ----- | ----- | ----- | 
| **Bab 4: Tech Stack** | HTML5/CSS3/JS + **PHP/Node.js + MySQL/MariaDB** | **HTML5 + Tailwind CSS + Vanilla JS + Supabase (Cloud PostgreSQL) + Vercel** | Kalau 88 mahasiswa semester 1–2 disuruh install XAMPP/MySQL lokal via Zoom, waktu 2 jam habis buat *troubleshooting* port 3306 bentrok & tidak bisa di-deploy gratis ke Vercel. | 
| **Bab 5 & Modul 1: Fitur (`REQ-01` & `REQ-02`)** | Di tabel Modul 1 hanya tertulis `REQ-01` (Catat) & `REQ-02` (Kalkulasi Saldo). Tapi di **Bab 2 & Bab 5** tertulis wajib ada **Lihat Riwayat & Hapus/Edit Transaksi (CRUD)**. | Kita lengkapi menjadi **4 Requirement Utuh (`REQ-01` s/d `REQ-04`)** di dalam `PRD.md` agar fitur Hapus/Edit & Filter Riwayat tidak terlewat oleh AI. | AI hanya akan membuat fitur yang tertulis eksplisit di `PRD.md`. Kalau mengikuti tabel Modul 1 mentah-mentah, tombol Edit & Hapus tidak akan dibuat oleh AI. | 
| **Modul 2: Skema Tabel `cash_transactions`** | `id INT (PK)`, `tanggal DATE`, `jenis_transaksi ENUM('Masuk','Keluar')`, `nominal DECIMAL(10,2)`, `keterangan TEXT`. | Tetap memakai nama tabel & kolom yang **100% sama persis**, tetapi tipe datanya disesuaikan ke **PostgreSQL Supabase** + ditambah constraint `CHECK (nominal > 0)`. | Agar 100% patuh pada PDF panitia, sekaligus mengunci aturan `TC-01` (anti angka negatif) langsung di level database cloud. | 
| **Modul 3: Testing (`TC-01` & `TC-02`) + Security** | `TC-01` (Tolak `-50000` dengan pesan *"Nominal harus lebih dari 0"*), `TC-02` (Masuk `20000` -> Saldo `+20.000`), serta **Basic Security Assessment** (Bab 4). | Kita tambahkan **`TC-03` (Uji Anti-XSS pada kolom `keterangan`)** & **Audit API Key** untuk memenuhi syarat *Basic Security Assessment* bagi mahasiswa RKS & TI. | Kolom `keterangan TEXT` di tabel kas kelas adalah sasaran empuk injeksi XSS (`<img src=x onerror=alert(1)>`). Sangat pas untuk simulasi *Live Hacking*! | 

## BAGIAN 2: ROADMAP & ALUR MENGAJAR 120 MENIT (MENIT DEMI MENIT)

Berikut adalah pembagian waktu dan alur perpindahan layar (*screen switching*) kamu selama 120 menit:

```
[09.15 - 09.25] (10m) PEMBUKAAN & MINDSET SHIFT (Slide 1 - 5)
   └── Bahas Bab 6 SKPL, bedanya Blind Prompting vs Spec-Driven Vibe Coding, & 10 Modul SKPL.
[09.25 - 09.40] (15m) LANGKAH 1: BEDAH SKPL & BUAT PRD.md (Slide 6 + Live AI Chat & VS Code)
   └── Ekstrak PDF SKPL_10 jadi file PRD.md + Prompt UI Google Stitch.
[09.40 - 10.00] (20m) LANGKAH 2: GENERATE UI DI GOOGLE STITCH (Slide 7 + Live Stitch & VS Code)
   └── Jalur A: Demo Stitch MCP oleh Faiz | Jalur B: Peserta Export HTML/Tailwind -> index.html.
[10.00 - 10.10] (10m) LANGKAH 3: PASANG PAGAR KEAMANAN RULES.md (Slide 8 + Live VS Code)
   └── Buat file RULES.md (Anti-XSS .textContent, Validasi TC-01, Anti-Leak .gitignore).
[10.10 - 10.40] (30m) LANGKAH 4: SUPABASE DB & FULL-STACK WIRING (Slide 9 + Live Supabase & VS Code)
   └── Jalankan SQL cash_transactions + RLS Policy -> Sambungkan CDN Supabase JS ke index.html.
[10.40 - 11.05] (25m) LANGKAH 5: DEVTOOLS TESTING & LIVE HACKING XSS (Slide 10-11 + Browser F12)
   └── Uji TC-01 (-50000) & TC-02 (+20000) -> Simulasi bobol XSS di versi rentan vs versi aman.
[11.05 - 11.15] (10m) LANGKAH 6: PUSH GITHUB & DEPLOY VERCEL (Slide 11-12 + GitHub & Vercel)
   └── Cek .gitignore -> Push repo -> Deploy ke Vercel -> Dapat URL live portofolio!

```

## BAGIAN 3: KERANGKA SLIDE PPT & POIN BAHASAN (SPEAKER NOTES FAIZ)

Bagian ini adalah panduan apa saja yang tampil di slide dan **apa yang perlu kamu ucapkan** di setiap tahap:

### Slide 1: Cover — Spec-Driven Vibe Coding & Secure Coding

* **Isi Visual:** Judul Utama, Subjudul *"Dari Dokumen SKPL ke Aplikasi Full-Stack Aman di Vercel dalam 120 Menit"*, Nama Faiz (Data Sorcerers Yogyakarta).

* **Yang Perlu Dibahas (Talking Points):**

  * Sapa peserta lintas 5 prodi (RKS, TI, ALKS, TRPL, TRM).

  * Tekankan bahwa hari ini adalah **puncak eksekusi (Day 4)**: menggabungkan SKPL & RAB dari Day 2 serta UI/UX Google Stitch dari Day 3 menjadi aplikasi web yang hidup dan online.

### Slide 2: Konteks Peserta & Hak Veto Bab 6 SKPL

* **Isi Visual:** Angka statistik (88 Peserta, 5 Prodi, 10 Modul SKPL, 120 Menit) + Highlight **Bab 6 SKPL (Format Narasumber)**.

* **Yang Perlu Dibahas:**

  * *"Teman-teman, coba buka PDF SKPL kalian masing-masing di halaman 1 bagian nomor 4 dan nomor 6. Di nomor 4 tertulis pakai PHP dan MySQL. Tapi sesuai wewenang Bab 6 Format Narasumber, hari ini kita upgrade pakai stack industri modern: **HTML5 + Tailwind CSS + Vanilla JS + Supabase Cloud PostgreSQL + Vercel**. Kenapa? Supaya kalian nggak pusing install XAMPP error-erroran, dan dalam 2 jam aplikasi kalian langsung punya link publik yang bisa diakses dari HP!"*

### Slide 3: Mindset Shift — Blind Prompting vs Spec-Driven Vibe Coding

* **Isi Visual:** Perbandingan 2 kolom: *Asal Prompting* (Merah) vs *Spec-Driven Vibe Coding* (Hijau).

* **Yang Perlu Dibahas:**

  * Kesalahan terbesar pemula saat *Vibe Coding* adalah langsung buka ChatGPT/Cursor lalu ketik: *"Tolong buatkan aplikasi kas kelas yang bagus"*. Hasilnya? Fitur ngawur, desain berantakan, dan penuh celah keamanan (XSS).

  * Engineer sejati memakai **Spec-Driven Vibe Coding**: kita kunci dulu AI-nya menggunakan 3 dokumen pengendali di VS Code: **`PRD.md` (Otak Bisnis)**, **`DESIGN.md` (Aturan Visual)**, dan **`RULES.md` (Pagar Keamanan)**.

### Slide 4: Master Roadmap 6 Langkah (120 Menit)

* **Isi Visual:** Peta jalan 6 kotak dari Langkah 1 (`PRD.md`) sampai Langkah 6 (`Deploy Vercel`).

* **Yang Perlu Dibahas:**

  * Tunjukkan gambaran besar 2 jam ke depan agar peserta tahu posisi mereka di setiap menit dan tidak tertinggal.

### Slide 5: Pemetaan 10 Modul SKPL & Studi Kasus Demo (`SKPL_10`)

* **Isi Visual:** Tabel ringkasan `SKPL_1` s/d `SKPL_10` beserta highlight di baris **`SKPL_10 — Sistem Manajemen Kas Kelas`**.

* **Yang Perlu Dibahas:**

  * *"Saya akan mendemokan secara live menggunakan **SKPL_10 (Kas Kelas)**. Bagi teman-teman yang memegang SKPL 1 sampai 9, caranya **100% sama persis**! Kalian cukup mengganti teks SKPL di dalam prompt dengan isi PDF SKPL kalian masing-masing."*

### Slide 6: Langkah 1 (Menit 00–15) — Bedah SKPL & Buat `PRD.md`

* **Isi Visual:** Cara mengubah PDF SKPL menjadi `PRD.md` + cuplikan isi `PRD.md` (`cash_transactions`, `REQ-01` s/d `REQ-04`, `TC-01` & `TC-02`).

* **Yang Perlu Dibahas:**

  * Tunjukkan bahwa di PDF `SKPL_10`, bagian Alur Sistem (Bab 5) meminta fitur Hapus/Edit dan Riwayat, sedangkan tabel Modul 1 hanya menulis `REQ-01` dan `REQ-02`. Di tahap `PRD.md` inilah kita menyempurnakan spesifikasi tersebut agar AI membangun aplikasi CRUD yang utuh.

### Slide 7: Langkah 2 (Menit 15–35) — Integrasi UI Google Stitch ke VS Code

* **Isi Visual:** Komparasi **Jalur A (Stitch MCP Server)** vs **Jalur B (Direct Export HTML/Tailwind)**.

* **Yang Perlu Dibahas:**

  * Jawab pertanyaan panitia (Adit) tentang **MCP (Model Context Protocol)**: demokan bagaimana AI Agent di IDE bisa langsung menarik desain dari Google Stitch lewat konfigurasi MCP JSON (`https://stitch.googleapis.com/mcp`).

  * Untuk 88 peserta agar cepat dan bebas kendala instalasi Node.js, arahkan memakai **Jalur B**: klik *Export HTML/Tailwind* di Google Stitch, tempel ke `index.html`, lalu nyalakan ekstensi **Live Server (`Go Live`)**.

### Slide 8: Langkah 3 (Menit 35–45) — Pasang Pagar Pengaman `RULES.md`

* **Isi Visual:** 4 Pilar `RULES.md`: Patuh `PRD.md`, Anti-XSS (`.textContent`), Validasi Input (`TC-01`), dan Proteksi Kredensial (`.gitignore`).

* **Yang Perlu Dibahas:**

  * Ini jembatan penting untuk mahasiswa **RKS (Keamanan Siber)** dan **TRPL**.

  * Jelaskan bahwa AI itu cenderung memilih cara paling instan saat merender tabel HTML, yaitu memakai `element.innerHTML = ...`. Padahal itu celah fatal **Cross-Site Scripting (XSS)**. Lewat `RULES.md`, kita melarang AI memakai `.innerHTML` untuk data dari user.

### Slide 9: Langkah 4 (Menit 45–75) — Database Cloud Supabase & Full-Stack CRUD Wiring

* **Isi Visual:** Script SQL PostgreSQL `cash_transactions` + 3 tahap koneksi CDN `@supabase/supabase-js@2`.

* **Yang Perlu Dibahas:**

  * Perhatikan baris `nominal NUMERIC(12,2) NOT NULL CHECK (nominal > 0)`. Jelaskan konsep **Defense in Depth**: kita tidak hanya memvalidasi angka minus di tampilan depan (JavaScript), tapi juga menguncinya di level database PostgreSQL! Jadi kalau ada hacker yang menembak API langsung tanpa lewat form web, database Supabase tetap akan menolak angka `-50000`.

  * Jelaskan bedanya **`anon` key** (aman ditaruh di kode front-end asalkan RLS aktif) vs **`service_role` key** (rahasia mutlak, haram masuk ke kode HTML/JS).

### Slide 10: Langkah 5A (Menit 75–90) — Inspeksi Fungsional via Browser DevTools (`F12`)

* **Isi Visual:** 4 Checklist DevTools: Mobile View (`Ctrl+Shift+M`), Tab Console (0 error), Tab Network (`201 Created`), dan Eksekusi Tabel Black Box `TC-01` & `TC-02`.

* **Yang Perlu Dibahas:**

  * Ajak peserta menekan tombol `F12` di browser.

  * Praktikkan langsung pengujian Black Box sesuai halaman 2 PDF `SKPL_10`:

    1. **Uji `TC-01`:** Ketik `-50000` di kolom Nominal -> Klik Simpan -> Buktikan sistem menolak dan memunculkan pesan peringatan persis seperti syarat SKPL: ***"Nominal harus lebih dari 0"***.

    2. **Uji `TC-02`:** Pilih Jenis `Masuk`, ketik `20000`, keterangan `Iuran Kas Minggu 1` -> Klik Simpan -> Lihat tab Network muncul status hijau **`201 Created`** dan kartu Total Saldo bertambah **`Rp 20.000`**.

### Slide 11: Langkah 5B & 6 (Menit 90–120) — Live Hacking XSS & Auto-Deploy ke Vercel

* **Isi Visual:** Simulasi serangan XSS pada kolom `keterangan` + Alur Push GitHub & Deploy Vercel.

* **Yang Perlu Dibahas:**

  * **Momen Puncak (Live Hacking):** Buka file `demo_vibe_coding_vulnerable.html`. Masukkan transaksi kas dengan keterangan:
    `<img src="x" onerror="alert('Waduh! Kas Kelas Jebol Kena XSS!')">`
    Begitu disimpan, browser langsung meledak memunculkan pop-up alert! Tunjukkan penyebabnya di kode (karena memakai `.innerHTML`).

  * Buka versi kita yang sudah dipagari `RULES.md` (`demo_vibe_coding_secure.html`), masukkan payload hacker yang sama -> **Aman!** Script jahat hanya tampil sebagai teks biasa karena dirender dengan `.textContent`.

  * Lanjut ke **Langkah 6**: cek `.gitignore`, upload ke GitHub, dan sambungkan ke Vercel sampai keluar link `.vercel.app`.

### Slide 12: Penutup & Quotes Engineer

* **Isi Visual:** *"AI Menulis Kode Agar Cepat Jalan, Engineer Memastikannya Aman."*

* **Yang Perlu Dibahas:**

  * Berikan apresiasi kepada peserta lintas prodi dan buka sesi tanya jawab / *showcase* link Vercel peserta.

## BAGIAN 4: BANK PROMPT LENGKAP & KODE SIAP PAKAI (STUDI KASUS `SKPL_10`)

Bagian ini adalah **amunisi copy-paste** kamu saat live demo (dan bisa langsung dibagikan ke peserta di kolom chat Zoom).

### TAHAP 1: Prompt Ekstraksi SKPL Menjadi `PRD.md` & Prompt UI Stitch

*(Tempelkan prompt ini ke ChatGPT / Gemini / Claude / Copilot Chat)*

```
Saya adalah mahasiswa peserta Workshop Vibe Coding yang sedang membangun aplikasi web berdasarkan dokumen Spesifikasi Kebutuhan Perangkat Lunak (SKPL) berikut:

- Nama Aplikasi: Sistem Manajemen Kas Kelas (Cashflow Tracker) - SKPL_10
- Deskripsi: Aplikasi web untuk mencatat dan mengelola arus kas masuk (pemasukan/iuran) serta kas keluar (pengeluaran) secara transparan, serta menghitung sisa saldo akhir secara otomatis.
- Stack Teknis (Sesuai Ketentuan Narasumber Bab 6): HTML5, Tailwind CSS (via CDN), Vanilla JavaScript, dan Supabase (Cloud PostgreSQL). Seluruh aplikasi frontend berada dalam satu file `index.html`.
- Kebutuhan Fungsional (CRUD & Kalkulasi):
  1. REQ-01 (Create): Mencatat transaksi kas masuk dan keluar beserta tanggal, nominal, dan keterangan.
  2. REQ-02 (Summary): Menghitung dan menampilkan secara otomatis Total Pemasukan, Total Pengeluaran, dan Sisa Saldo Akhir (`Total Masuk - Total Keluar`).
  3. REQ-03 (Read & Filter): Menampilkan tabel riwayat transaksi kas secara rapi (terbaru di atas) dilengkapi fitur pencarian keterangan dan filter jenis transaksi (`Semua`, `Masuk`, `Keluar`).
  4. REQ-04 (Update & Delete): Mendukung fitur Edit transaksi dan Hapus transaksi beserta konfirmasi penghapusan, lalu mengupdate saldo secara real-time.
- Skema Tabel Database (`cash_transactions`):
  - `id`: BIGINT / INT Primary Key Auto Increment
  - `tanggal`: DATE (Tanggal transaksi kas)
  - `jenis_transaksi`: TEXT / ENUM ('Masuk', 'Keluar')
  - `nominal`: NUMERIC / DECIMAL(10,2)
  - `keterangan`: TEXT (Keterangan transaksi)
- Skenario Pengujian Wajib (Black Box Testing):
  - TC-01 (Input Nominal Negatif): Jika user menginput `Nominal: -50000` (atau angka <= 0), sistem WAJIB menolak penyimpanan dan menampilkan pesan peringatan persis: `"Nominal harus lebih dari 0"`.
  - TC-02 (Tambah Pemasukan Valid): Jika user menginput `Jenis: Masuk, Nominal: 20000`, data tersimpan ke database dan total saldo akhir bertambah sebesar `20.000`.

TUGAS KAMU:
1. Buatkan isi file `PRD.md` yang rapi, terstruktur, dan lengkap berdasarkan spesifikasi di atas sebagai acuan utama coding.
2. Buatkan 1 paragraf **Prompt Desain UI/UX** (dalam Bahasa Inggris dan Bahasa Indonesia) yang siap saya copy-paste ke **Google Stitch** untuk men-generate tampilan dashboard aplikasi Kas Kelas yang modern, bersih, dan responsif.

```

### TAHAP 2: Prompt untuk Ditempel ke Google Stitch (`stitch.withgoogle.com`)

*(Hasil dari Tahap 1 yang langsung bisa kamu tempel ke Google Stitch saat live demo)*

```
Design a modern, clean, and responsive Single-Page Web Application for "Sistem Manajemen Kas Kelas (Class Cashflow Tracker)" using Tailwind CSS. 
Include:
1. Top Header with the app title "Cashflow Tracker - Kas Kelas", subtitle "Transparansi Keuangan Kelas JKB PNC", and a connection status badge.
2. Three Summary Cards in a row: "Total Saldo Akhir" (Indigo/Primary highlight), "Total Kas Masuk" (Emerald Green), and "Total Kas Keluar" (Rose Red) formatted in Indonesian Rupiah (Rp).
3. Main Content Split Layout (2 columns on desktop):
   - Left Column (1/3 width): "Form Catat Transaksi" card containing inputs for Tanggal (Date picker), Jenis Transaksi (Select: Masuk / Keluar), Nominal Rp (Number input), Keterangan (Textarea), a hidden validation alert banner for error messages ("Nominal harus lebih dari 0"), and a primary "Simpan Transaksi" button.
   - Right Column (2/3 width): "Riwayat Mutasi Kas" card with a search bar, filter dropdown (Semua / Masuk / Keluar), and a clean data table showing Tanggal, Keterangan, Jenis Badge, Nominal, and Action buttons (Edit & Hapus).

```

### TAHAP 3: Isi File `RULES.md` (Simpan di Folder VS Code)

*(Minta peserta membuat file `RULES.md` di VS Code dan menempelkan aturan keamanan ini)*

```
# ATURAN PENGEMBANGAN & KEAMANAN (AGENT SKILLS / RULES.md)

Setiap AI Coding Assistant (Copilot / Cursor / Gemini / Claude) yang menulis atau memodifikasi kode di proyek ini WAJIB mematuhi aturan mutlak berikut:

1. **Kepatuhan Spesifikasi (`PRD.md`):**
   - Gunakan nama tabel `cash_transactions` dan nama kolom persis sesuai SKPL: `id`, `tanggal`, `jenis_transaksi`, `nominal`, `keterangan`.
   - Jangan menambah halaman baru; seluruh fitur berjalan di dalam satu file `index.html`.

2. **Aturan Validasi Input & Black Box Testing (`TC-01` & `TC-02`):**
   - Sebelum mengirim data ke Supabase, validasi bahwa `nominal` adalah angka valid dan `> 0`.
   - Jika `nominal <= 0` atau bernilai negatif (contoh: `-50000`), batalkan proses simpan dan tampilkan pesan peringatan di layar dengan teks persis: **"Nominal harus lebih dari 0"** (Sesuai `TC-01`).
   - Pastikan nilai `jenis_transaksi` hanya menerima `'Masuk'` atau `'Keluar'`.

3. **Aturan Secure Coding (Anti-XSS / Cross-Site Scripting):**
   - **DILARANG KERAS** merender data input dari user (khususnya kolom `keterangan`) ke dalam DOM menggunakan `.innerHTML` atau `outerHTML` tanpa sanitasi.
   - **WAJIB** menggunakan `document.createElement()` dan `.textContent` saat membuat baris tabel riwayat transaksi, atau gunakan fungsi `escapeHtml()` yang mengubah karakter `<`, `>`, `&`, `"`, dan `'` menjadi HTML entities.

4. **Aturan Kredensial & Privasi Data:**
   - Hanya gunakan `SUPABASE_URL` dan `SUPABASE_ANON_KEY` (Public Anon Key) pada sisi client.
   - **DILARANG KERAS** menuliskan `SUPABASE_SERVICE_ROLE_KEY` atau `STITCH_API_KEY` di dalam `index.html`.
   - Pastikan file `.gitignore` mengecualikan `.env`, `.vscode/mcp.json`, dan `.cursor/mcp.json`.

```

### TAHAP 4A: Script SQL Supabase (`cash_transactions` + Constraint `TC-01` + RLS)

*(Jalankan langsung di menu **SQL Editor** pada dashboard Supabase)*

```
-- 1. Buat tabel cash_transactions sesuai Modul 2 SKPL_10
CREATE TABLE IF NOT EXISTS public.cash_transactions (
    id BIGINT GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    tanggal DATE NOT NULL DEFAULT CURRENT_DATE,
    jenis_transaksi TEXT NOT NULL CHECK (jenis_transaksi IN ('Masuk', 'Keluar')),
    nominal NUMERIC(12, 2) NOT NULL CHECK (nominal > 0), -- Proteksi TC-01 di level Database!
    keterangan TEXT NOT NULL CHECK (char_length(trim(keterangan)) > 0),
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- 2. Aktifkan Row Level Security (RLS)
ALTER TABLE public.cash_transactions ENABLE ROW LEVEL SECURITY;

-- 3. Buat Policy agar aplikasi frontend bisa melakukan CRUD (Untuk keperluan Workshop/Demo)
CREATE POLICY "Izinkan akses publik CRUD kas kelas"
ON public.cash_transactions
FOR ALL
TO anon
USING (true)
WITH CHECK (nominal > 0);

-- 4. Insert 2 data awal untuk pengujian awal
INSERT INTO public.cash_transactions (tanggal, jenis_transaksi, nominal, keterangan)
VALUES 
  (CURRENT_DATE, 'Masuk', 150000, 'Saldo Awal Kas Kelas Semester Genap'),
  (CURRENT_DATE, 'Keluar', 25000, 'Beli Spidol Snowman & Penghapus Papan Tulis');

```

### TAHAP 4B: Prompt Menyambungkan `index.html` ke Supabase & Logika CRUD Aman

*(Tempelkan ke AI di VS Code setelah UI dari Google Stitch masuk ke `index.html`)*

```
Berdasarkan file `@PRD.md` dan aturan keamanan di `@RULES.md`, tolong lengkapi file `index.html` saya agar terhubung secara real-time ke database Cloud Supabase menggunakan CDN `@supabase/supabase-js@2`.

Detail Teknis yang Wajib Diimplementasikan:
1. Tambahkan script CDN `<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>` di bagian `<head>`.
2. Sediakan variabel konfigurasi `const SUPABASE_URL` dan `const SUPABASE_ANON_KEY` di bagian atas script.
3. Buat fungsi CRUD lengkap ke tabel `cash_transactions`:
   - `fetchTransactions()`: Mengambil seluruh data diurutkan dari tanggal/id terbaru, merender tabel menggunakan `.textContent` (Anti-XSS sesuai `RULES.md`), dan menghitung ulang kartu **Total Kas Masuk**, **Total Kas Keluar**, serta **Total Saldo Akhir** (`Masuk - Keluar`).
   - `handleSubmit(e)`: Memvalidasi input form. Jika `nominal <= 0` (contoh `-50000`), cegah request ke database dan tampilkan alert merah di form dengan kalimat persis: **"Nominal harus lebih dari 0"** (untuk memenuhi pengujian `TC-01`). Jika valid (`TC-02`), jalankan `insert` atau `update` ke Supabase.
   - `editTransaction(item)` & `deleteTransaction(id)`: Mengisi form untuk mode edit atau menghapus data berdasarkan `id`.
4. Tambahkan fitur filter real-time berdasarkan `jenis_transaksi` (`Semua`, `Masuk`, `Keluar`) dan pencarian kata kunci pada kolom `keterangan`.

```

### TAHAP 5: Skenario Pengujian DevTools & Payload Simulasi Live Hacking XSS

Saat masuk ke **Langkah 5 (Menit 75–105)**, gunakan urutan demo pengujian berikut di depan peserta:

1. **Uji Black Box `TC-01` (Input Nominal Negatif):**

   * **Input di Form:** Tanggal = Hari Ini | Jenis = `Masuk` | Nominal = `-50000` | Keterangan = `Tes Angka Minus`

   * **Hasil yang Ditunjukkan ke Peserta:** Sistem menolak menyimpan dan muncul kotak pesan merah bertuliskan ***"Nominal harus lebih dari 0"***. Status `TC-01` = **PASS**.

2. **Uji Black Box `TC-02` (Tambah Pemasukan Valid):**

   * **Input di Form:** Tanggal = Hari Ini | Jenis = `Masuk` | Nominal = `20000` | Keterangan = `Iuran Kas Budi`

   * **Hasil yang Ditunjukkan ke Peserta:** Buka **DevTools (`F12`) -> Tab Network (`Fetch/XHR`)**, tunjukkan baris `cash_transactions` berwarna hijau dengan status **`201 Created`**. Kartu Total Saldo Akhir otomatis bertambah **`Rp 20.000`**. Status `TC-02` = **PASS**.

3. **Simulasi Live Hacking `TC-03` (Uji Ketahanan Anti-XSS pada Kolom Keterangan):**

   * Copy-paste payload XSS berikut ke kolom **Keterangan** pada `demo_vibe_coding_vulnerable.html` vs `demo_vibe_coding_secure.html`:

     ```
     <img src="x" onerror="alert('HACKED! Saldo Kas Kelas Berhasil Dicuri via XSS!')">
     
     ```

   * **Di Aplikasi Tanpa `RULES.md` (Vulnerable):** Muncul pop-up alert berbahaya setiap kali halaman dibuka (Stored XSS) karena kode AI memakai `.innerHTML`.

   * **Di Aplikasi Kita (Secure):** Pop-up tidak muncul sama sekali, dan tag `<img src="x"...>` tampil sebagai teks biasa yang tidak berbahaya karena kode kita memakai `.textContent`.

### TAHAP 6: Isi File `.gitignore` & Checklist Deploy ke Vercel

Sebelum peserta mengunggah kode ke GitHub dan men-deploy ke Vercel di **Menit 105–120**, pastikan mereka membuat file **`.gitignore`**:

```
# Jangan pernah upload file environment & secret key ke GitHub!
.env
.env.local
.env.production

# Jangan upload konfigurasi MCP yang berisi STITCH_API_KEY!
.vscode/mcp.json
.cursor/mcp.json
claude_desktop_config.json

# File sistem OS
.DS_Store
Thumbs.db
node_modules/

```

## BAGIAN 5: IDE MODIFIKASI TAMBAHAN (OPSIONAL — KALAU KAMU MAU BIKIN LEBIH KEREN LAGI)

Kalau kamu ingin memodifikasi studi kasus `SKPL_10` ini sedikit agar terasa lebih "wah" saat didemokan di depan 5 prodi (terutama di depan anak-anak **Akuntansi ALKS** dan **Keamanan Siber RKS**), kita bisa menyisipkan 2 fitur kecil ini tanpa mengubah skema tabel database sama sekali:

1. **Indikator Kesehatan Kas otomatis (Buat memukau prodi Akuntansi/ALKS):**

   * Jika `Total Saldo Akhir < 0` (Defisit), warna kartu Saldo otomatis berubah jadi **Merah** dengan badge *"Defisit Kas!"*. Jika positif, warna kartu **Hijau/Indigo** dengan badge *"Kas Sehat"*.

2. **Tombol "Uji Simulasi TC-01 & Anti-XSS" 1-Klik di Pojok Atas (Buat memukau prodi RKS & TI):**

   * Kita bisa menambahkan bar kecil bertuliskan **"Panel Pengujian Otomatis (QA & Security Mode)"** yang punya tombol:

     * `[Uji TC-01 (-50.000)]` -> Otomatis mengisi form dengan `-50000` untuk mengetes validasi.

     * `[Uji TC-02 (+20.000)]` -> Otomatis mengisi form pemasukan `20000`.

     * `[Suntik Payload XSS]` -> Otomatis mengisi kolom keterangan dengan script `<img src=x onerror=alert(...)>` untuk membuktikan keamanan aplikasi.
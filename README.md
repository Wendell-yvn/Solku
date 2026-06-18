# SolKu 👟

> Temukan dan pesan tukang sol sepatu terpercaya di sekitarmu, kapan saja.

SolKu adalah aplikasi marketplace mobile dua sisi yang menghubungkan pembeli dengan penyedia jasa sol sepatu. Pembeli dapat menemukan penyedia jasa terdekat, melihat portfolio hasil kerja, dan melakukan booking slot waktu — sementara penyedia jasa mendapatkan saluran akuisisi pelanggan baru yang lebih luas dan konsisten.

---

## Status Proyek

| Layer | Status |
|---|---|
| UI/UX Design (Figma) | ✅ Selesai |
| Usability Testing (Maze) | ✅ Selesai |
| Dokumentasi Produk | ✅ Selesai |
| Frontend / Mobile | 🚧 In Progress |
| Backend | ⏳ Belum dimulai |

---

## Fitur yang Diimplementasikan (MVP)

- 📋 **Sistem Booking** — Pembeli memilih layanan dan slot waktu tersedia
- 📍 **Validasi Geolocation** — Radius layanan untuk penyedia jasa mobile
- 🔄 **Tracking Status Pesanan** — Update real-time dari Menunggu → Selesai
- 🖼️ **Portfolio Penyedia Jasa** — Foto hasil pekerjaan sebagai bukti kualitas
- 🔔 **Notifikasi** — Push notification setiap perubahan status order

---

## Tech Stack

| Kebutuhan | Teknologi |
|---|---|
| UI/UX Design | Figma, FigJam |
| Usability Testing | Maze |
| Mobile App | React Native *(direncanakan)* |
| Backend | Node.js + Express *(direncanakan)* |
| Database | PostgreSQL *(direncanakan)* |
| Geolocation | Google Maps API *(direncanakan)* |
| Push Notification | Firebase Cloud Messaging *(direncanakan)* |

---

## Struktur Repositori

```
solku/
├── README.md
├── docs/
│   ├── 01-requirements.md        # Klarifikasi kebutuhan (hasil grill-me)
│   ├── 02-prd.md                 # Product Requirements Document
│   ├── 03-vertical-slice-issues.md  # Breakdown implementasi
│   ├── 04-design.md              # Desain UI/UX dan arsitektur
│   ├── 05-tdd-and-testing.md     # TDD dan hasil pengujian
│   └── 06-reflection.md          # Refleksi penggunaan AI
├── assets/
│   └── screenshots/              # Screenshot UI dan hasil testing
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   └── vertical-slice.md
│   └── pull_request_template.md
└── src/                          # Source code (coming soon)
```

---

## Cara Menjalankan Aplikasi

> ⚠️ Backend belum diimplementasikan. Saat ini hanya tersedia desain UI/UX.

### Melihat Desain UI/UX

Akses prototype Figma: *(tambahkan link Figma di sini)*

### Melihat Hasil Usability Testing

Akses laporan Maze: *(tambahkan link Maze di sini)*

---

## Screenshots

> Tambahkan screenshot dari Figma / Maze di folder `assets/screenshots/`

---

## Dokumentasi Lengkap

| Dokumen | Deskripsi |
|---|---|
| [01 - Requirements](docs/01-requirements.md) | Klarifikasi kebutuhan produk hasil sesi grill-me |
| [02 - PRD](docs/02-prd.md) | Product Requirements Document lengkap |
| [03 - Vertical Slice Issues](docs/03-vertical-slice-issues.md) | Breakdown fitur menjadi implementation issues |
| [04 - Design](docs/04-design.md) | Desain UI/UX, alur pengguna, dan arsitektur sistem |
| [05 - TDD & Testing](docs/05-tdd-and-testing.md) | Laporan test-driven development dan pengujian |
| [06 - Reflection](docs/06-reflection.md) | Refleksi jujur penggunaan AI sepanjang proyek |

---

## Known Limitations

- Backend belum diimplementasikan — semua fitur masih dalam tahap desain
- Sistem pembayaran online tidak tersedia di MVP (pembayaran dilakukan offline)
- Sistem dispute/mediasi belum ada — resolusi konflik bergantung pada kepercayaan antarpihak
- Rating & review belum tersedia di versi pertama
- Validasi pasar masih terbatas pada 5 responden (2 penyedia jasa, 3 pembeli)

---

## Tentang Proyek

Proyek ini dikembangkan sebagai bagian dari tugas mata kuliah **Software Engineering** dengan pendekatan AI-Assisted Product Development. Seluruh proses dari klarifikasi kebutuhan hingga dokumentasi dilakukan dengan bantuan AI sebagai *software engineering assistant*.

Lihat [docs/06-reflection.md](docs/06-reflection.md) untuk refleksi lengkap penggunaan AI.

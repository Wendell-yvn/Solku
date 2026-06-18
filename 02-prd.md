# 02 — Product Requirements Document (PRD)

**Proyek:** SolKu
**Versi:** 1.0 (MVP)
**Status:** Draft
**Terakhir Diperbarui:** Juni 2026

---

## Product Overview

SolKu adalah platform marketplace mobile dua sisi yang menghubungkan pembeli dengan penyedia jasa sol sepatu melalui sistem booking berbasis slot waktu dan geolocation, dengan tracking status pekerjaan real-time.

> **One-liner:** *Temukan dan pesan tukang sol sepatu terpercaya di sekitarmu, kapan saja.*

Aplikasi mendukung dua model layanan:
- **Penyedia Tetap** — beroperasi di lokasi tetap (pasar/kios); pembeli datang ke lokasi
- **Penyedia Mobile** — beroperasi secara keliling; penyedia datang ke lokasi pembeli

---

## Goals

1. Menghubungkan pembeli dengan penyedia jasa sol sepatu secara lebih efisien
2. Memberikan penyedia jasa akses ke pelanggan yang lebih luas dan konsisten
3. Membangun kepercayaan pembeli melalui portfolio dan transparansi status pekerjaan
4. Menstandarkan proses pemesanan yang selama ini dilakukan manual via WhatsApp

---

## Non-Goals

- Sistem pembayaran online atau escrow (ditunda ke v1.2)
- Layanan antar-jemput pihak ketiga (ditunda ke v2.0)
- Rating & review (ditunda ke v1.1)
- Estimasi harga otomatis (ditunda ke v1.1)
- Chat in-app (ditunda ke v1.1)
- Dashboard admin (ditunda ke v1.2)
- Sistem mediasi dispute (ditunda ke v1.2)

---

## Target Users

### Pembeli
- Pria/wanita, 18–45 tahun, pengguna smartphone aktif
- Memiliki sepatu yang perlu diperbaiki 1–6 bulan sekali
- Pain point: sulit menemukan penyedia jasa berkualitas, tidak tahu harga di muka

### Penyedia Jasa
- Individu atau usaha kecil perbaikan sepatu
- Tipe: tetap (kios/pasar) atau mobile (keliling)
- Sudah familiar dengan smartphone dan WhatsApp
- Pain point: pelanggan tidak konsisten, promosi hanya dari mulut ke mulut

---

## User Stories

```
Sebagai pembeli, saya ingin melihat daftar penyedia jasa terdekat agar saya
dapat memilih yang paling sesuai dengan kebutuhan saya.

Sebagai pembeli, saya ingin melihat portfolio foto hasil pekerjaan agar saya
dapat menilai kualitas sebelum memesan.

Sebagai pembeli, saya ingin membuat pesanan dengan memilih slot waktu agar
jadwal pengerjaan sepatu saya terkoordinasi dengan baik.

Sebagai pembeli, saya ingin menerima notifikasi perubahan status pesanan agar
saya tidak perlu bertanya manual tentang progres pekerjaan.

Sebagai pembeli, saya ingin membatalkan pesanan yang belum dikonfirmasi agar
saya tidak terikat pada pesanan yang tidak lagi dibutuhkan.

Sebagai penyedia jasa, saya ingin mengatur profil dan area layanan agar calon
pelanggan dapat menemukan dan menilai layanan saya.

Sebagai penyedia jasa, saya ingin mengunggah foto hasil pekerjaan agar saya
dapat membangun kepercayaan calon pelanggan baru.

Sebagai penyedia jasa, saya ingin mengkonfirmasi atau menolak pesanan agar
saya memiliki kendali penuh atas pekerjaan yang saya terima.

Sebagai penyedia jasa, saya ingin memperbarui status pesanan agar pembeli
dapat memantau progres tanpa bertanya manual.
```

---

## Core Features

| # | Fitur | Prioritas |
|---|---|---|
| 1 | Autentikasi (registrasi, login, OTP) | Must Have |
| 2 | Profil penyedia jasa (lokasi, slot waktu, area layanan) | Must Have |
| 3 | Portfolio foto hasil pekerjaan | Must Have |
| 4 | Daftar & pencarian penyedia jasa berdasarkan lokasi | Must Have |
| 5 | Validasi geolocation radius (penyedia mobile) | Must Have |
| 6 | Sistem booking (pilih layanan + slot waktu) | Must Have |
| 7 | Konfirmasi order manual oleh penyedia jasa (HITL) | Must Have |
| 8 | Tracking status pesanan real-time | Must Have |
| 9 | Push notification perubahan status | Must Have |
| 10 | Riwayat transaksi | Must Have |

---

## Acceptance Criteria

### Booking
- [ ] Pembeli dapat memilih penyedia jasa, layanan, dan slot waktu
- [ ] Sistem memvalidasi radius sebelum booking (untuk penyedia mobile)
- [ ] Slot yang sudah dipesan tidak dapat dipilih pengguna lain
- [ ] Pembeli menerima konfirmasi setelah booking berhasil

### Tracking Status
- [ ] Alur status: Menunggu Konfirmasi → Dikonfirmasi → Sedang Dikerjakan → Selesai → Ditutup
- [ ] Status hanya bergerak maju (tidak bisa mundur)
- [ ] Pembeli menerima notifikasi setiap perubahan status
- [ ] Riwayat perubahan status tampil kronologis

### Portfolio
- [ ] Penyedia jasa dapat upload maks. 20 foto
- [ ] Foto tampil di halaman profil publik
- [ ] Penyedia jasa dapat menghapus foto

### Geolocation
- [ ] Sistem menghitung jarak antara lokasi pembeli dan titik operasional penyedia mobile
- [ ] Jika di luar radius: sistem menolak dan menyarankan penyedia lain

---

## Success Criteria

| Metrik | Target (3 Bulan Pertama) |
|---|---|
| Penyedia jasa aktif | ≥ 10 |
| Pembeli terdaftar | ≥ 200 |
| Total transaksi | ≥ 100 pesanan |
| Order completion rate | ≥ 70% |
| Rata-rata response time penyedia jasa | ≤ 2 jam |

---

## Risks

| # | Risiko | Tingkat | Mitigasi |
|---|---|---|---|
| 1 | Validasi pasar terbatas (5 responden) | 🔴 Tinggi | Survei ke ≥ 50 calon pengguna |
| 2 | Cold start — tidak ada penyedia jasa saat launch | 🔴 Tinggi | Akuisisi manual ≥ 10 penyedia jasa sebelum launch |
| 3 | Tidak ada perlindungan transaksi di MVP | 🟡 Sedang | Disclaimer + percepat rating di v1.1 |
| 4 | Literasi teknologi penyedia jasa rendah | 🟡 Sedang | Panduan onboarding video |
| 5 | Akurasi geolocation rendah di area tertentu | 🟢 Rendah | Izinkan koreksi lokasi manual |

---

## Out-of-Scope Items

| Fitur | Target Versi |
|---|---|
| Rating & Review | v1.1 |
| Estimasi Harga Otomatis | v1.1 |
| Chat In-App | v1.1 |
| Pembayaran Online / Escrow | v1.2 |
| Sistem Dispute / Mediasi | v1.2 |
| Dashboard Admin | v1.2 |
| Layanan Antar-Jemput Pihak Ketiga | v2.0 |

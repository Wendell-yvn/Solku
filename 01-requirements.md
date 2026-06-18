# 01 — Requirements Clarification

> Dokumen ini merupakan output dari sesi **grill-me** menggunakan AI sebagai interviewer teknis yang ketat. Pertanyaan diajukan satu per satu hingga ide produk cukup jelas untuk didokumentasikan sebagai requirements.

---

## Product Idea

Aplikasi marketplace mobile yang menghubungkan pembeli dengan penyedia jasa sol sepatu — baik penyedia jasa tetap (kios/pasar) maupun penyedia jasa mobile (keliling).

---

## Problem Statement

### Dari Sisi Penyedia Jasa
- Pelanggan tidak datang secara konsisten
- Jangkauan pasar terbatas secara geografis dan waktu (bergantung hari pasar / rute keliling)
- Promosi hanya dari mulut ke mulut
- Komunikasi dan negosiasi harga dilakukan manual via WhatsApp

### Dari Sisi Pembeli
- Sulit menemukan penyedia jasa yang terpercaya dan berkualitas
- Tidak tahu harga sebelum datang; harga sering berubah dari kesepakatan awal
- Tidak ada referensi kualitas yang objektif
- Harus menunggu tukang keliling atau pergi ke pasar

---

## Target Users

| Pengguna | Deskripsi |
|---|---|
| **Pembeli** | Pria/wanita 18–45 tahun, pengguna smartphone aktif, memiliki sepatu yang perlu diperbaiki 1–6 bulan sekali |
| **Penyedia Jasa Tetap** | Individu/usaha kecil yang beroperasi di lokasi tetap (pasar/kios) |
| **Penyedia Jasa Mobile** | Individu yang beroperasi secara keliling, mendatangi lokasi pelanggan |

---

## User Goals

### Pembeli
- Menemukan penyedia jasa terpercaya dengan mudah
- Mengetahui harga sebelum memesan
- Memantau status pekerjaan tanpa harus bertanya manual

### Penyedia Jasa
- Mendapatkan pelanggan baru yang lebih konsisten
- Tidak harus terus berkeliling atau bergantung pada hari pasar
- Mempromosikan kualitas kerja melalui portfolio

---

## Functional Requirements

| ID | Kebutuhan |
|---|---|
| FR-01 | Pengguna dapat mendaftar sebagai pembeli atau penyedia jasa |
| FR-02 | Penyedia jasa dapat mengatur profil, lokasi, area layanan, dan slot waktu |
| FR-03 | Penyedia jasa dapat mengunggah foto hasil pekerjaan ke portfolio |
| FR-04 | Pembeli dapat melihat daftar penyedia jasa berdasarkan lokasi |
| FR-05 | Sistem memvalidasi radius layanan sebelum booking (untuk penyedia mobile) |
| FR-06 | Pembeli dapat membuat pesanan dengan memilih layanan dan slot waktu |
| FR-07 | Penyedia jasa dapat mengkonfirmasi atau menolak pesanan secara manual |
| FR-08 | Penyedia jasa dapat memperbarui status pesanan secara bertahap |
| FR-09 | Sistem mengirim push notification setiap perubahan status |
| FR-10 | Pembeli dan penyedia jasa dapat melihat riwayat transaksi |

---

## Non-Functional Requirements

| ID | Kebutuhan |
|---|---|
| NFR-01 | Halaman daftar penyedia jasa termuat < 3 detik pada koneksi 4G |
| NFR-02 | Notifikasi status terkirim < 5 detik setelah perubahan |
| NFR-03 | Uptime sistem ≥ 99% per bulan |
| NFR-04 | Semua data dienkripsi (HTTPS, enkripsi penyimpanan) |
| NFR-05 | Aplikasi berjalan pada Android 8.0+ dan iOS 13+ |
| NFR-06 | Antarmuka dapat dioperasikan pengguna awam tanpa pelatihan khusus |

---

## Assumptions

- Pengguna memiliki smartphone dengan koneksi internet
- Penyedia jasa sudah familiar dengan smartphone (WhatsApp sudah digunakan)
- Pembayaran dilakukan sepenuhnya offline — aplikasi tidak mengelola dana
- Penyedia jasa bersedia meluangkan waktu untuk setup profil awal

---

## Constraints

- Backend belum diimplementasikan di tahap ini (MVP dimulai dari UI/UX)
- Tidak ada sistem dispute atau escrow di MVP
- Rating & review tidak tersedia di versi pertama
- Estimasi harga bersifat indikatif, bukan otomatis

---

## Open Questions

| # | Pertanyaan | Status |
|---|---|---|
| 1 | Apakah perlu sistem verifikasi identitas penyedia jasa? | Belum diputuskan |
| 2 | Bagaimana penanganan jika penyedia jasa tidak merespons order? | Ditentukan: reminder setelah 2 jam |
| 3 | Apakah perlu fitur chat in-app? | Ditunda ke v1.1 |
| 4 | Bagaimana model monetisasi platform? | Di luar scope MVP |

---

## Evidence: Sesi Grill-Me

Sesi klarifikasi kebutuhan dilakukan dalam format wawancara teknis ketat (product grilling) dengan AI sebagai interviewer. Total 10 pertanyaan diajukan, mencakup:

- Validasi pasar dan signifikansi masalah
- Strategi cold start (chicken-and-egg problem)
- Value proposition ke penyedia jasa
- Prioritas fitur MVP dengan justifikasi
- Alur transaksi end-to-end
- Model logistik kombinasi
- Implementasi validasi geolocation
- Mekanisme dispute dan trade-off

Lihat transkrip lengkap di: [`product-discovery-solku.md`](../product-discovery-solku.md) *(jika tersedia)*

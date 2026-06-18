# 03 — Vertical Slice Issues

> Setiap issue adalah potongan kerja yang dapat diimplementasikan, diuji, dan didemonstrasikan secara mandiri. Issue diurutkan berdasarkan dependency.

---

## Dependency Order

```
#01 Create Order
    ├── #02 View Orders
    │       └── #05 Search Order
    └── #03 Update Status
            └── #04 Cancel Order
#06 Confirm Order (HITL) ← bergantung pada #01
```

---

## Issue #01 — Pengguna dapat membuat pesanan baru

**Type:** AFK

**What to Build:**
Pembeli dapat membuat pesanan baru kepada penyedia jasa melalui form booking di halaman detail penyedia jasa. Sistem memvalidasi ketersediaan slot dan radius geolocation (untuk penyedia mobile) sebelum order dibuat.

**User Stories Covered:**
- Sebagai pembeli, saya ingin membuat pesanan baru agar saya dapat menjadwalkan layanan sol sepatu.

**Acceptance Criteria:**
- [ ] Form booking tersedia di halaman detail penyedia jasa
- [ ] Form memiliki field: pilihan layanan, slot waktu tersedia, dan catatan tambahan (opsional)
- [ ] Sistem memvalidasi ketersediaan slot sebelum order dibuat
- [ ] Sistem memvalidasi radius geolocation untuk penyedia mobile sebelum order dibuat
- [ ] Data pesanan tersimpan ke database setelah form disubmit
- [ ] Pesanan baru muncul di daftar order aktif milik pembeli
- [ ] Pesanan baru muncul di dashboard penyedia jasa sebagai pesanan masuk
- [ ] Pembeli menerima konfirmasi bahwa pesanan berhasil dibuat

**Blocked By:** None

**Testing Notes:**
1. Login sebagai pembeli
2. Buka halaman detail penyedia jasa
3. Pilih layanan dan slot waktu tersedia
4. Submit form booking
5. Verifikasi pesanan muncul di daftar order pembeli
6. Login sebagai penyedia jasa dan verifikasi pesanan muncul di dashboard

**AI Usage Notes:**
AI membantu memperluas acceptance criteria dan testing notes. Logika validasi radius dan slot harus diverifikasi manual.

---

## Issue #02 — Pengguna dapat melihat daftar pesanan

**Type:** AFK

**What to Build:**
Pembeli dapat melihat semua pesanan aktif dan riwayat pesanan. Penyedia jasa dapat melihat semua pesanan masuk. Setiap item menampilkan nama pihak lawan, jenis layanan, slot waktu, dan status terkini.

**User Stories Covered:**
- Sebagai pembeli, saya ingin melihat daftar pesanan saya agar saya dapat memantau semua pekerjaan yang sedang atau pernah saya pesan.

**Acceptance Criteria:**
- [ ] Halaman daftar pesanan tersedia untuk pembeli dan penyedia jasa
- [ ] Pembeli melihat semua pesanan miliknya (aktif dan riwayat)
- [ ] Penyedia jasa melihat semua pesanan yang masuk kepadanya
- [ ] Setiap item menampilkan: nama pihak lawan, jenis layanan, slot waktu, status terkini
- [ ] Pesanan diurutkan berdasarkan waktu terbaru
- [ ] Pesanan aktif dan selesai ditampilkan dalam tab atau seksi terpisah

**Blocked By:** #01 — Create Order

**Testing Notes:**
1. Login sebagai pembeli yang sudah memiliki pesanan
2. Buka halaman daftar pesanan
3. Verifikasi semua pesanan muncul dengan status yang benar
4. Login sebagai penyedia jasa dan verifikasi pesanan masuk tampil di dashboard

**AI Usage Notes:**
AI membantu merinci acceptance criteria untuk dua sudut pandang pengguna. Urutan tampilan dan pemisahan tab harus diverifikasi manual di UI.

---

## Issue #03 — Penyedia jasa dapat memperbarui status pesanan

**Type:** AFK

**What to Build:**
Penyedia jasa dapat memperbarui status pesanan secara bertahap melalui dashboard. Setiap perubahan status dikirimkan sebagai notifikasi real-time kepada pembeli. Status hanya bergerak maju.

**User Stories Covered:**
- Sebagai penyedia jasa, saya ingin memperbarui status pesanan agar pembeli dapat mengetahui progres tanpa bertanya manual.

**Acceptance Criteria:**
- [ ] Penyedia jasa dapat mengubah status pesanan melalui dashboard
- [ ] Alur status: `Menunggu Konfirmasi` → `Dikonfirmasi` → `Sedang Dikerjakan` → `Selesai` → `Ditutup`
- [ ] Status hanya bergerak maju — tidak bisa kembali ke status sebelumnya
- [ ] Setiap perubahan status tersimpan beserta timestamp
- [ ] Pembeli menerima push notification setiap kali status berubah
- [ ] Riwayat perubahan status tampil kronologis di halaman detail pesanan

**Blocked By:** #01 — Create Order, #02 — View Orders

**Testing Notes:**
1. Login sebagai penyedia jasa, buka pesanan dengan status `Menunggu Konfirmasi`
2. Ubah status ke `Dikonfirmasi` — verifikasi notifikasi terkirim ke pembeli
3. Ubah status ke `Sedang Dikerjakan`, kemudian `Selesai`
4. Verifikasi riwayat status tampil kronologis di sisi pembeli
5. Coba ubah status ke status sebelumnya — verifikasi sistem menolak

**AI Usage Notes:**
AI membantu mendefinisikan alur status dan constraint. Constraint "tidak bisa mundur" harus diuji secara manual dan dengan unit test.

---

## Issue #04 — Pengguna dapat membatalkan pesanan

**Type:** AFK

**What to Build:**
Pembeli dapat membatalkan pesanan selama status masih `Menunggu Konfirmasi`. Penyedia jasa dapat membatalkan pesanan yang sudah dikonfirmasi dengan memberikan alasan. Kedua pihak menerima notifikasi.

**User Stories Covered:**
- Sebagai pembeli, saya ingin membatalkan pesanan yang belum dikonfirmasi agar saya tidak terikat pada pesanan yang tidak lagi dibutuhkan.

**Acceptance Criteria:**
- [ ] Pembeli dapat membatalkan pesanan selama status masih `Menunggu Konfirmasi`
- [ ] Tombol batalkan tidak tersedia jika status sudah melewati `Menunggu Konfirmasi`
- [ ] Penyedia jasa dapat membatalkan pesanan `Dikonfirmasi` dengan mengisi alasan (wajib)
- [ ] Status pesanan berubah menjadi `Dibatalkan` setelah pembatalan berhasil
- [ ] Kedua pihak menerima push notification saat pesanan dibatalkan
- [ ] Pesanan yang dibatalkan tetap tersimpan di riwayat dengan status `Dibatalkan`

**Blocked By:** #01 — Create Order, #03 — Update Status

**Testing Notes:**
1. Batalkan pesanan saat status masih `Menunggu Konfirmasi` — verifikasi berhasil
2. Coba batalkan sebagai pembeli setelah `Dikonfirmasi` — verifikasi tombol tidak tersedia
3. Batalkan sebagai penyedia jasa tanpa isi alasan — verifikasi sistem menolak
4. Isi alasan dan batalkan — verifikasi notifikasi terkirim ke pembeli

**AI Usage Notes:**
AI membantu mengidentifikasi edge case pembatalan dari dua sisi pengguna. Semua edge case harus diuji manual.

---

## Issue #05 — Pengguna dapat mencari dan memfilter pesanan

**Type:** AFK

**What to Build:**
Pembeli dan penyedia jasa dapat mencari pesanan berdasarkan kata kunci dan memfilter berdasarkan status.

**User Stories Covered:**
- Sebagai penyedia jasa, saya ingin mencari pesanan tertentu agar saya tidak harus menggulir seluruh daftar.

**Acceptance Criteria:**
- [ ] Field pencarian tersedia di halaman daftar pesanan
- [ ] Pembeli dapat mencari berdasarkan nama penyedia jasa
- [ ] Penyedia jasa dapat mencari berdasarkan nama pembeli
- [ ] Filter berdasarkan status tersedia: Semua, Aktif, Selesai, Dibatalkan
- [ ] Hasil muncul real-time saat pengguna mengetik
- [ ] Jika kosong, tampilkan pesan "Tidak ada pesanan ditemukan"
- [ ] Pencarian dan filter dapat dikombinasikan

**Blocked By:** #02 — View Orders

**Testing Notes:**
1. Cari nama valid — verifikasi hasil muncul
2. Cari nama yang tidak ada — verifikasi pesan kosong tampil
3. Filter berdasarkan status `Selesai` — verifikasi hanya pesanan selesai muncul
4. Kombinasikan pencarian dan filter — verifikasi hasil akurat

**AI Usage Notes:**
AI membantu merinci kombinasi filter dan edge case. Perilaku real-time search harus diverifikasi di browser.

---

## Issue #06 — Penyedia jasa mengkonfirmasi atau menolak pesanan (HITL)

**Type:** HITL

**What to Build:**
Setelah pembeli membuat pesanan, penyedia jasa harus secara manual mengkonfirmasi atau menolak pesanan. Sistem tidak mengkonfirmasi pesanan secara otomatis.

**Why HITL:**
Konfirmasi tidak dapat diotomasi karena:
- Penyedia jasa perlu menilai kapasitas aktual pada hari tersebut
- Penyedia jasa mobile perlu mempertimbangkan kondisi cuaca dan jarak aktual
- Keputusan menerima pekerjaan bersifat subjektif dan tidak bisa diasumsikan sistem

**Human Decision Point:**

| Titik Keputusan | Siapa | Opsi |
|---|---|---|
| Pesanan baru masuk | Penyedia Jasa | Konfirmasi atau Tolak |

**User Stories Covered:**
- Sebagai penyedia jasa, saya ingin mengkonfirmasi atau menolak pesanan agar saya memiliki kendali penuh atas pekerjaan yang saya terima.

**Acceptance Criteria:**
- [ ] Penyedia jasa menerima push notification saat pesanan baru masuk
- [ ] Dashboard menampilkan tombol **Konfirmasi** dan **Tolak** pada pesanan baru
- [ ] Konfirmasi → status berubah ke `Dikonfirmasi`, notifikasi ke pembeli
- [ ] Tolak → wajib isi alasan → status `Dibatalkan`, notifikasi + alasan ke pembeli
- [ ] Jika tidak direspons dalam 2 jam → sistem kirim reminder ke penyedia jasa
- [ ] Pembeli dapat melihat alasan penolakan di halaman detail pesanan

**Blocked By:** #01 — Create Order

**Testing Notes:**
1. Buat pesanan baru sebagai pembeli
2. Login sebagai penyedia jasa — verifikasi notifikasi diterima
3. Tekan Konfirmasi — verifikasi status berubah dan notifikasi terkirim
4. Buat pesanan baru lagi, tekan Tolak tanpa isi alasan — verifikasi sistem menolak
5. Isi alasan dan tolak — verifikasi alasan tampil di sisi pembeli
6. Simulasikan 2 jam tanpa respons — verifikasi reminder terkirim

**AI Usage Notes:**
AI membantu mengidentifikasi titik keputusan manusia dan alasan mengapa konfirmasi tidak dapat diotomasi. Logika reminder 2 jam harus diuji dengan mock timer.

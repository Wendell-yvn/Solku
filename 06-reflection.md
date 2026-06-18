# 06 — Reflection on AI Usage

**Proyek:** SolKu — Aplikasi Marketplace Jasa Sol Sepatu
**Metode:** Design Thinking + AI-Assisted Product Development

---

## 1. Bagaimana AI digunakan saat klarifikasi kebutuhan?

AI digunakan sebagai **interviewer teknis yang ketat** melalui sesi *grill-me*. AI mengajukan satu pertanyaan per giliran dan menolak jawaban yang terlalu umum atau tidak didukung data.

Yang paling berguna: AI mengidentifikasi **chicken-and-egg problem** saat saya menjawab "dua-duanya sekarang" untuk pertanyaan prioritas akuisisi pengguna. Ini memaksa saya berpikir lebih dalam tentang strategi supply-side acquisition.

AI juga menantang justifikasi pasar saya yang hanya berdasarkan 5 responden, dan mencatat ini sebagai risiko utama.

**Yang saya verifikasi manual:** Validasi bahwa masalah dari 5 responden memang relevan dengan kondisi lapangan, berdasarkan observasi dan wawancara langsung sebagai bagian dari tahap *Empathize* Design Thinking.

---

## 2. Bagaimana AI digunakan saat pembuatan PRD?

AI membantu mengkonversi hasil sesi grill-me menjadi PRD terstruktur. AI menyarankan penambahan bagian **Non-Goals** yang awalnya tidak saya pikirkan — penting untuk menjaga scope MVP tetap terkendali.

AI juga membantu merumuskan **Success Criteria** yang terukur (angka konkret seperti "≥ 10 penyedia jasa aktif" dan "≥ 100 transaksi dalam 3 bulan").

**Yang saya verifikasi manual:** Kesesuaian user stories dengan temuan wawancara nyata dari 5 responden. Beberapa user story yang diusulkan AI tidak relevan dengan konteks lokal Manado dan saya hapus.

---

## 3. Bagaimana AI digunakan saat breakdown issue?

AI membantu mengidentifikasi bahwa konfirmasi pesanan harus dikategorikan sebagai **HITL** (bukan AFK), dengan alasan yang solid: penyedia jasa perlu menilai kapasitas aktual, kondisi cuaca untuk penyedia mobile, dan kelayakan permintaan — semua ini tidak bisa diotomasi.

AI juga membantu menyusun **dependency order** antar issue yang logis.

**Yang saya verifikasi manual:** Apakah setiap issue benar-benar "vertical" — menyentuh UI, logic, dan storage sekaligus, bukan hanya satu layer.

---

## 4. Bagaimana AI digunakan saat desain?

AI membantu membuat **data model** dan menyarankan pemisahan tabel `Order` dan `OrderStatusHistory` untuk mendukung riwayat perubahan status secara kronologis. AI juga membantu mendefinisikan architecture diagram dan menyarankan Firebase Cloud Messaging untuk push notification.

**Yang saya verifikasi manual:** Desain UI/UX dibuat sepenuhnya secara mandiri menggunakan Figma dan FigJam, berdasarkan temuan dari tahap Empathize dan Define. AI tidak terlibat dalam proses desain visual.

---

## 5. Bagaimana AI digunakan saat testing?

AI membantu merancang **TDD plan** dengan skenario test realistis untuk dua behavior kritis: validasi slot waktu dan validasi alur status. AI mengingatkan pentingnya menguji race condition pada booking slot.

Untuk usability testing, AI tidak terlibat — pengujian dilakukan secara mandiri menggunakan **Maze** dengan Task Scenario Testing dan **USE Questionnaire**.

**Hasil skor USE:**
- Pelanggan: **84.9%** (Sangat Bermanfaat)
- Penyedia Jasa: **81.78%** (Sangat Bermanfaat)

**Yang saya verifikasi manual:** Seluruh skenario task di Maze dirancang sendiri. Hasil USE Questionnaire diinterpretasikan sendiri tanpa bantuan AI.

---

## 6. Di mana AI membuat kesalahan atau memberikan saran yang lemah?

**Kesalahan 1 — Konteks lokal:** AI awalnya menyarankan fitur *escrow* dan *payment gateway* sebagai bagian MVP. Ini tidak realistis untuk konteks penyedia jasa sol sepatu di Manado yang rata-rata tidak familiar dengan transaksi digital. Saya menolak saran ini.

**Kesalahan 2 — Nama aplikasi:** AI menyebut nama "SolKu" tanpa diminta, dan saya mengikutinya tanpa benar-benar mempertimbangkan apakah nama ini tepat. Keputusan nama harusnya lebih disadari.

**Saran lemah:** AI mengusulkan React Native tanpa mempertimbangkan bahwa tim mungkin lebih familiar dengan Flutter. Ini perlu divalidasi ulang saat implementasi dimulai.

---

## 7. Apa yang diverifikasi secara manual?

| Area | Verifikasi Manual |
|---|---|
| Validasi pasar | Wawancara langsung dengan 5 responden (bukan dikarang AI) |
| Desain UI/UX | Dibuat sendiri di Figma berdasarkan temuan wawancara |
| Usability testing | Dilakukan langsung via Maze; AI tidak terlibat |
| Relevansi user stories | Dicocokkan dengan notulen wawancara asli |
| Prioritas fitur | Diputuskan sendiri dengan justifikasi yang dipertanggungjawabkan |
| Dependency order issue | Diperiksa ulang untuk memastikan setiap issue benar-benar vertikal |

---

## 8. Keputusan software engineering apa yang paling saya yakini?

**Keputusan untuk tidak mengotomasi konfirmasi pesanan (HITL).**

Setelah digali oleh AI di sesi grill-me, saya menyadari bahwa penyedia jasa perlu memiliki kendali penuh atas pekerjaan yang mereka terima. Konfirmasi otomatis akan memaksa penyedia jasa menerima semua pesanan tanpa bisa mempertimbangkan kapasitas aktual — ini justru merusak pengalaman mereka dan berpotensi meningkatkan pembatalan.

Keputusan ini konsisten dengan temuan *Empathize*: penyedia jasa menginginkan sistem yang membantu mereka, bukan yang mengontrol mereka.

---

## 9. Apa yang akan diperbaiki dengan lebih banyak waktu?

1. **Validasi pasar yang lebih kuat** — menambah responden dari 5 menjadi minimal 50 sebelum pengembangan penuh
2. **Implementasi backend** — saat ini baru sampai di UI/UX; backend belum dimulai
3. **TDD eksekusi nyata** — TDD plan sudah ada tapi belum bisa dieksekusi karena backend belum ada
4. **Iterasi desain** — Ease of Use mendapat skor terendah (74.3%) dalam USE Questionnaire; perlu iterasi pada navigasi yang masih membingungkan pengguna
5. **Responden penyedia jasa lebih banyak** — hanya 2 responden untuk USE Questionnaire sisi penyedia jasa, terlalu sedikit untuk kesimpulan yang valid

---

## Ringkasan Penggunaan AI

| Tahap | Kontribusi AI | Kontribusi Saya |
|---|---|---|
| Requirements | Mengajukan pertanyaan kritis, mengidentifikasi risiko | Menjawab, memvalidasi, memutuskan |
| PRD | Struktur dokumen, Non-Goals, Success Criteria | Konten berbasis wawancara nyata |
| Issues | Format, HITL identification, dependency order | Verifikasi vertikalitas setiap issue |
| Design | Data model, arsitektur, tech stack suggestion | UI/UX visual sepenuhnya mandiri |
| Testing | TDD plan, skenario test | Maze testing, USE Questionnaire, interpretasi hasil |

> **Prinsip yang saya pegang:** AI membantu saya bergerak lebih cepat, tetapi setiap keputusan produk adalah tanggung jawab saya sendiri.

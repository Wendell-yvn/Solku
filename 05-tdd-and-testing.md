# 05 — TDD and Testing Report

> Dokumen ini mencakup dua bagian: (1) rencana TDD untuk implementasi backend yang akan datang, dan (2) hasil pengujian usability yang sudah dilakukan menggunakan **Maze** dan **USE Questionnaire** sebagai bagian dari proses Design Thinking.

---

## Bagian A: Usability Testing (Maze + USE Questionnaire)

Pengujian dilakukan pada prototype high-fidelity yang dibuat di Figma, menggunakan platform **Maze** dengan dua metode: Task Scenario Testing dan USE Questionnaire.

---

### Responden

| Kelompok | Jumlah Responden |
|---|---|
| Pelanggan | 4–9 orang |
| Penyedia Jasa | 2 orang |

---

### Task Scenario Testing

Pengujian dilakukan melalui prototype interaktif di Maze. Setiap responden diminta menyelesaikan serangkaian task scenario berikut:

#### Skenario untuk Pelanggan

| No | Task Scenario |
|---|---|
| 1 | Masuk ke aplikasi dan membuka halaman beranda |
| 2 | Mengecek kategori layanan yang tersedia |
| 3 | Masuk ke bagian chat untuk berkomunikasi dengan penyedia jasa |
| 4 | Melihat jasa terdekat dari lokasi pengguna |
| 5 | Memilih jasa, layanan, dan melakukan pemesanan |
| 6 | Mengecek status dan detail pesanan |
| 7 | Memberikan rating dan ulasan setelah pesanan selesai |
| 8 | Melihat akun dan informasi alamat |
| 9 | Logout dari akun |

#### Skenario untuk Penyedia Jasa

| No | Task Scenario |
|---|---|
| 1 | Login ke aplikasi dan masuk ke halaman beranda |
| 2 | Melihat chat dari pelanggan |
| 3 | Melihat daftar pesanan, membuka detail, dan menerima pesanan |
| 4 | Mengubah status pesanan (diproses, selesai, dll.) |
| 5 | Melihat riwayat pesanan setelah selesai |
| 6 | Melihat informasi penghasilan |
| 7 | Melihat akun, informasi toko, dan ulasan pelanggan |
| 8 | Melakukan logout dari akun |

---

### Hasil USE Questionnaire

USE Questionnaire mengukur empat aspek: **Usefulness**, **Ease of Use**, **Ease of Learning**, dan **Satisfaction**.

#### Hasil — Pelanggan

| Aspek | Skor |
|---|---|
| Usefulness | 100.0 |
| Ease of Use | 74.3 |
| Ease of Learning | 80.1 |
| Satisfaction | 85.2 |
| **Total** | **84.9% — Kategori: Sangat Bermanfaat** |

#### Hasil — Penyedia Jasa

| Aspek | Skor |
|---|---|
| Usefulness | 88.0 |
| Ease of Use | 76.5 |
| Ease of Learning | 80.2 |
| Satisfaction | 82.4 |
| **Total** | **81.78% — Kategori: Sangat Bermanfaat** |

---

### Analisis Hasil

**Yang berjalan baik:**
- Usefulness mendapat skor tertinggi (100.0 dari pelanggan, 88.0 dari penyedia jasa) — pengguna merasakan manfaat nyata dari aplikasi
- Alur pemesanan dinilai mudah dipahami dan diselesaikan
- Pemisahan antarmuka pelanggan dan penyedia jasa terbukti meningkatkan kejelasan navigasi

**Yang perlu diperbaiki:**
- Ease of Use mendapat skor terendah di kedua kelompok (74.3 dan 76.5) — masih ada hambatan navigasi pada beberapa fitur
- Ditemukan kendala pada navigasi dan pemahaman fitur tertentu yang memerlukan iterasi desain
- Jumlah responden penyedia jasa masih terbatas (2 orang) sehingga data kurang representatif

---

## Bagian B: TDD Plan (Untuk Implementasi Backend)

> TDD berikut direncanakan untuk dieksekusi saat implementasi backend dimulai. Setiap siklus mengikuti pola **RED → GREEN → REFACTOR**.

---

### TDD Issue #1 — Sistem Booking: Validasi Slot Waktu

**Issue yang Diuji:** #01 — Pengguna dapat membuat pesanan baru

**Behavior Under Test:**
Sistem menolak booking jika slot waktu yang dipilih sudah dipesan oleh pengguna lain.

**Public Interface:**
Fungsi `createOrder(buyerId, providerId, slotId, serviceType)` pada Order Service.

#### RED
```javascript
// Test yang ditulis SEBELUM implementasi
test('createOrder should throw error if slot is already booked', async () => {
  // Arrange
  const slot = await createSlot({ isBooked: true });

  // Act & Assert
  await expect(
    createOrder({
      buyerId: 'buyer-1',
      providerId: 'provider-1',
      slotId: slot.id,
      serviceType: 'sol'
    })
  ).rejects.toThrow('Slot waktu tidak tersedia');
});
```
**Expected result saat RED:** Test gagal karena fungsi `createOrder` belum ada.

#### GREEN
```javascript
// Implementasi minimal untuk membuat test lulus
async function createOrder({ buyerId, providerId, slotId, serviceType }) {
  const slot = await TimeSlot.findById(slotId);
  if (slot.isBooked) {
    throw new Error('Slot waktu tidak tersedia');
  }
  // ... simpan order
}
```
**Expected result saat GREEN:** Test lulus.

#### REFACTOR
- Tambahkan database transaction untuk mencegah race condition (dua pengguna booking slot yang sama secara bersamaan)
- Pisahkan validasi ke `SlotValidator` class agar dapat digunakan ulang
- Tambahkan error code yang konsisten (`SLOT_UNAVAILABLE`) selain pesan teks

**Final Result:** ⏳ Menunggu implementasi backend

---

### TDD Issue #2 — Tracking Status: Validasi Alur Status

**Issue yang Diuji:** #03 — Penyedia jasa dapat memperbarui status pesanan

**Behavior Under Test:**
Sistem menolak perubahan status yang tidak mengikuti alur yang ditentukan (status tidak bisa mundur atau melompat).

**Public Interface:**
Fungsi `updateOrderStatus(orderId, newStatus, updatedBy)` pada Order Service.

#### RED
```javascript
// Test yang ditulis SEBELUM implementasi
test('updateOrderStatus should throw error if status moves backward', async () => {
  // Arrange
  const order = await createTestOrder({ status: 'in_progress' });

  // Act & Assert
  await expect(
    updateOrderStatus(order.id, 'confirmed', 'provider-1')
  ).rejects.toThrow('Perubahan status tidak valid');
});

test('updateOrderStatus should throw error if status skips a step', async () => {
  const order = await createTestOrder({ status: 'pending' });

  await expect(
    updateOrderStatus(order.id, 'done', 'provider-1')
  ).rejects.toThrow('Perubahan status tidak valid');
});
```
**Expected result saat RED:** Kedua test gagal karena fungsi belum ada.

#### GREEN
```javascript
const STATUS_FLOW = {
  pending: ['confirmed', 'cancelled'],
  confirmed: ['in_progress', 'cancelled'],
  in_progress: ['done'],
  done: ['closed'],
  closed: [],
  cancelled: []
};

async function updateOrderStatus(orderId, newStatus, updatedBy) {
  const order = await Order.findById(orderId);
  const allowedNextStatuses = STATUS_FLOW[order.status];

  if (!allowedNextStatuses.includes(newStatus)) {
    throw new Error('Perubahan status tidak valid');
  }

  order.status = newStatus;
  await order.save();
  await OrderStatusHistory.create({ orderId, status: newStatus, changedBy: updatedBy });
}
```
**Expected result saat GREEN:** Kedua test lulus.

#### REFACTOR
- Pindahkan `STATUS_FLOW` ke file konfigurasi terpisah (`orderStatusConfig.js`)
- Tambahkan validasi bahwa `updatedBy` memiliki hak untuk mengubah status (provider hanya bisa ubah ordernya sendiri)
- Tambahkan event emitter untuk trigger push notification setelah status berhasil diubah

**Final Result:** ⏳ Menunggu implementasi backend

---

## Bagian C: Browser Verification Plan

> Akan dilakukan saat implementasi frontend selesai.

### Checklist Verifikasi Browser

#### Happy Path
- [ ] Alur booking lengkap dari browse → pilih slot → konfirmasi berhasil
- [ ] Alur update status dari dashboard penyedia jasa
- [ ] Notifikasi muncul setelah status berubah

#### Edge Cases
- [ ] Booking slot yang sudah terisi → pesan error muncul
- [ ] Booking di luar radius (penyedia mobile) → pesan informatif muncul
- [ ] Submit form booking tanpa memilih slot → validasi muncul
- [ ] Tolak pesanan tanpa mengisi alasan → sistem menolak

#### Chrome DevTools Checks
- [ ] Console tidak ada unexpected errors
- [ ] Network requests API menggunakan HTTPS
- [ ] Layout responsif pada viewport mobile (375px)
- [ ] Push notification diterima dalam < 5 detik

---

*Catatan: Screenshot hasil Maze dan USE Questionnaire dapat ditambahkan di folder `assets/screenshots/` setelah ekspor dari platform Maze.*

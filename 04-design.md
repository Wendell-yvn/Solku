# 04 — Design Document

> Dokumen ini dibuat sebelum implementasi sebagai bukti bahwa desain dipikirkan sebelum coding. UI/UX telah dirancang menggunakan Figma dan FigJam, serta diuji menggunakan Maze.

---

## Technology Stack

| Layer | Teknologi | Alasan |
|---|---|---|
| UI/UX Design | Figma, FigJam | Standar industri, mendukung prototyping dan kolaborasi |
| Usability Testing | Maze | Terintegrasi dengan Figma, dapat mengukur task success rate |
| Mobile App | React Native *(direncanakan)* | Cross-platform (Android + iOS) dengan satu codebase |
| Backend | Node.js + Express *(direncanakan)* | Ekosistem JavaScript konsisten dengan frontend |
| Database | PostgreSQL *(direncanakan)* | Relational DB cocok untuk data transaksi terstruktur |
| Geolocation | Google Maps API *(direncanakan)* | Akurasi tinggi, sudah umum di Indonesia |
| Push Notification | Firebase Cloud Messaging *(direncanakan)* | Gratis untuk skala awal, mendukung Android & iOS |

**Trade-off:** React Native dipilih atas native (Swift/Kotlin) karena kecepatan development lebih tinggi untuk MVP, dengan konsekuensi performa sedikit lebih rendah untuk animasi berat.

---

## User Flow

### Alur Utama: Booking Pesanan

```
[PEMBELI]
Buka App → Login → Izinkan Lokasi → Lihat Daftar Penyedia Jasa
  → Buka Halaman Detail → Lihat Portfolio
  → Pilih Layanan + Slot Waktu
  → (Penyedia Mobile?) Validasi Radius Otomatis
      ├── Dalam Radius → Lanjut Booking
      └── Di Luar Radius → Tampilkan Pesan, Sarankan Penyedia Lain
  → Submit Form → Konfirmasi Berhasil → Tunggu Notifikasi

[PENYEDIA JASA]
Terima Notifikasi Pesanan Baru → Buka Dashboard
  → Lihat Detail Pesanan
  → Konfirmasi atau Tolak (+ alasan jika tolak)
  → Mulai Kerjakan → Update Status: Sedang Dikerjakan
  → Selesai → Update Status: Selesai
  → Order Ditutup → Tersimpan di Riwayat
```

### Alur Status Pesanan

```
[Menunggu Konfirmasi]
       │
       ├── Ditolak ──────────────────→ [Dibatalkan]
       │
       ▼
[Dikonfirmasi]
       │
       ├── Dibatalkan Penyedia ──────→ [Dibatalkan]
       │
       ▼
[Sedang Dikerjakan]
       │
       ▼
[Selesai]
       │
       ▼
[Ditutup]
```

---

## Component Breakdown

### Sisi Pembeli

```
App
├── AuthScreen
│   ├── RegisterScreen
│   └── LoginScreen
├── HomeScreen
│   ├── LocationPermissionPrompt
│   ├── ProviderList
│   │   └── ProviderCard (nama, foto, tipe, jarak)
│   └── FilterBar (tipe layanan, jenis layanan)
├── ProviderDetailScreen
│   ├── ProviderProfile
│   ├── PortfolioGallery
│   ├── ServiceList
│   └── BookingForm
│       ├── ServicePicker
│       ├── SlotPicker
│       └── NoteInput
├── OrderListScreen
│   ├── ActiveOrdersTab
│   └── HistoryTab
├── OrderDetailScreen
│   ├── StatusTimeline
│   └── CancelButton (kondisional)
└── NotificationHandler
```

### Sisi Penyedia Jasa

```
App
├── AuthScreen (sama)
├── DashboardScreen
│   ├── IncomingOrderList
│   └── OrderCard (konfirmasi / tolak)
├── ProfileSetupScreen
│   ├── BasicInfoForm
│   ├── LocationPicker
│   ├── ServiceTypeSelector (tetap / mobile)
│   ├── RadiusSlider (untuk mobile)
│   └── SlotScheduler
├── PortfolioScreen
│   ├── PhotoGrid
│   └── UploadButton
├── OrderManagementScreen
│   └── StatusUpdateButton
└── OrderHistoryScreen
```

---

## Data Model

```
User
├── id (UUID)
├── name (string)
├── phone (string, unique)
├── password_hash (string)
├── account_type (enum: buyer | provider)
└── created_at (timestamp)

ProviderProfile
├── id (UUID)
├── user_id (FK → User)
├── business_name (string)
├── description (text)
├── service_type (enum: fixed | mobile)
├── location_lat (float)
├── location_lng (float)
├── service_radius_km (float, null jika fixed)
└── is_active (boolean)

PortfolioPhoto
├── id (UUID)
├── provider_id (FK → ProviderProfile)
├── photo_url (string)
├── caption (string, nullable)
└── created_at (timestamp)

TimeSlot
├── id (UUID)
├── provider_id (FK → ProviderProfile)
├── date (date)
├── start_time (time)
├── end_time (time)
└── is_booked (boolean)

Order
├── id (UUID)
├── buyer_id (FK → User)
├── provider_id (FK → ProviderProfile)
├── slot_id (FK → TimeSlot)
├── service_type (string)
├── notes (text, nullable)
├── status (enum: pending | confirmed | in_progress | done | closed | cancelled)
├── cancel_reason (text, nullable)
└── created_at (timestamp)

OrderStatusHistory
├── id (UUID)
├── order_id (FK → Order)
├── status (enum)
├── changed_by (FK → User)
└── changed_at (timestamp)
```

---

## Architecture Diagram

```
┌─────────────────────────────────────────┐
│           Mobile App (React Native)      │
│  ┌──────────────┐  ┌──────────────────┐ │
│  │  Buyer Flow  │  │  Provider Flow   │ │
│  └──────────────┘  └──────────────────┘ │
└──────────────────┬──────────────────────┘
                   │ HTTPS / REST API
┌──────────────────▼──────────────────────┐
│         Backend (Node.js + Express)      │
│  ┌──────────┐ ┌──────────┐ ┌─────────┐ │
│  │  Auth    │ │  Orders  │ │  Geo    │ │
│  │  Module  │ │  Module  │ │  Module │ │
│  └──────────┘ └──────────┘ └─────────┘ │
└──────┬──────────────┬───────────────────┘
       │              │
┌──────▼──────┐ ┌─────▼──────────────────┐
│ PostgreSQL  │ │  External Services      │
│ (Database)  │ │  - Google Maps API      │
└─────────────┘ │  - Firebase FCM         │
                └────────────────────────┘
```

---

## UI/UX Design

Desain UI/UX dibuat menggunakan **Figma** dan **FigJam** sebelum implementasi backend.

- Prototype Figma: *(tambahkan link)*
- FigJam User Flow: *(tambahkan link)*
- Laporan Maze (Pembeli): *(tambahkan link)*
- Laporan Maze (Penyedia Jasa): *(tambahkan link)*

### Prinsip Desain yang Diterapkan

- **Minimum 3 langkah** untuk navigasi utama (sesuai NFR-06)
- **Informasi terpenting di atas** (nama, foto, jarak, status)
- **Status pesanan divisualkan** dengan timeline kronologis
- **Feedback langsung** setelah setiap aksi pengguna (konfirmasi, notifikasi)

---

## Important Trade-offs

| Keputusan | Dipilih | Ditolak | Alasan |
|---|---|---|---|
| Model logistik | Kombinasi tetap + mobile | Pihak ketiga antar-jemput | Kompleksitas integrasi logistik terlalu tinggi untuk MVP |
| Validasi radius | Otomatis oleh sistem | Negosiasi manual | Menghilangkan gesekan komunikasi antarpihak |
| Pembayaran | Offline (cash) | Escrow in-app | Payment gateway dan mediasi dispute di luar scope MVP |
| Konfirmasi order | Manual oleh penyedia (HITL) | Otomatis | Penyedia perlu menilai kapasitas aktual secara subjektif |
| Rating & review | Ditunda ke v1.1 | MVP | Belum ada cukup transaksi untuk data yang kredibel |

# Enhanced Alur End-to-End — TemuNiaga

> Blueprint untuk modul transaksi B2B. State machine, status transitions, dan flow dari buyer onboarding sampai SHU kembali ke petani.

---

## Fase 1: Petani → Kopdes (Procurement)

Sudah matang di dokumen sebelumnya. Ringkasan:

```
1. Registrasi anggota (hybrid: web / WA / datang langsung)
2. Lapor rencana pasokan (WA bot bertombol / web form)
3. Setor fisik (antar / jemput)
4. Grading MANUAL di titik terima (foto hanya info awal)
5. Sistem tampilkan HARGA ACUAN dari BPS/Bapanas
6. Petani tekan YA → DIBAYAR (penuh / talangan via simpan-pinjam)
7. Stok tercatat di ledger Kopdes
```

**State stok:** `reported → graded → paid → in_warehouse → allocated → shipped_out`

---

## Fase 2: Kopdes → Pembeli (Sales & Delivery)

### [E] Buyer Onboarding

```
Flow:
  1. Buyer akses halaman "Daftar" → isi form
  2. Operator TemuNiaga verifikasi manual
  3. Buyer verified → bisa login → akses dashboard pembeli

State: pending → verified → suspended

Data minimal:
  buyer_id | company_name | npwp | pic_name | phone | bank_account | status

MVP strategy:
  Seed 2 buyer di DB (PT Indofood, Bulog)
  Login: pilih buyer → langsung masuk (tanpa full registration)
  Kredensial: username | password untuk masing-masing buyer
```

### [A] PO Digital — State Machine

```
  draft  →  confirmed  →  in_preparation  →  shipped  →  delivered  →  completed
                                                                         |
                                                                    disputed (ke Fase [D])
```

**Transition rules:**

| Dari | Ke | Trigger | Aktor |
|---|---|---|---|
| -- | draft | Algoritma A match stok & RFQ → system generate PO | System (auto) |
| draft | confirmed | Buyer klik [KONFIRMASI PESANAN] | Buyer |
| confirmed | -- | (draft timeout? Bisa ditambahkan nanti) | -- |
| confirmed | in_preparation | Kopdes klik [SIAPKAN KIRIMAN] | Kopdes |
| in_preparation | shipped | Kopdes klik [KIRIM] + isi opsi pengiriman | Kopdes |
| shipped | delivered | Buyer klik [TERIMA] → QC pass | Buyer |
| shipped | disputed | Buyer klik [TOLAK] → masuk fase [D] | Buyer |
| delivered | completed | System auto setelah QC passed & escrow settled | System |
| disputed | negotiating | Buyer atau Kopdes ajukan resolusi | Buyer/Kopdes |
| negotiating | resolved | Kesepakatan tercapai (harga baru / return) | System |

**Data PO:**

```
po_id | buyer_id | kopdes_id | commodity | grade | volume_kg
unit_price | total_price | status | tolerance_clause
created_at | confirmed_at | prepared_at | shipped_at | delivered_at | completed_at
logistics_option | logistics_detail | driver_name
```

### [B] Logistik & Pengiriman

```
Opsi pengiriman (Kopdes pilih saat PO in_preparation):
  1. Kopdes antar (armada sendiri)
     - Input: tanggal kirim, nama driver
     - Status: ready → in_transit
  2. Buyer jemput (roadmap)
     - Input: tanggal pickup
  3. Pihak ketiga / ekspedisi (roadmap)
     - Input: nomor resi, nama ekspedisi

State logistik: packaging → ready → in_transit → delivered_by_courier
```

**Untuk MVP: opsi 1 saja (Kopdes antar).**

Tombol di dashboard Kopdes:
- [SIAPKAN KIRIMAN] → status PO: in_preparation
- [KIRIM SEKARANG] → input tanggal + driver → status PO: shipped

### [C] Verifikasi Kualitas Buyer

```
Flow:
  1. Barang sampai ke buyer
  2. Buyer buka dashboard → lihat PO status "shipped"
  3. Klik [TERIMA] atau [TOLAK]

JIKA [TERIMA]:
  - PO status → "delivered"
  - Sistem cek: escrow sudah deposited? → release → PO status → "completed"
  - Kopdes bisa proses SHU

JIKA [TOLAK]:
  - Wajib isi alasan teks (min 10 karakter)
  - Foto opsional (upload, max 5MB)
  - PO status → "disputed"
  - Masuk fase [D]
```

### [D] Dispute & Rejection

```
State dispute: opened → negotiating → resolved

Empat jalur resolusi:

JALUR 1: Penyesuaian Harga (MVP)
  1. Buyer input harga baru + alasan
  2. Sistem notifikasi ke Kopdes
  3. Kopdes: [SETUJU] atau [TIDAK SETUJU]
  4. JIKA SETUJU: PO diupdate harga baru → status "completed"
  5. JIKA TIDAK SETUJU: lanjut ke jalur 4 (arbitrase)

JALUR 2: Regrading & cari buyer lain (Roadmap)
  1. Barang kembali ke gudang Kopdes
  2. Stok di-relist ke buyer lain yang RFQ-nya cocok
  3. PO original → cancelled

JALUR 3: Return penuh + refund (Roadmap)
  1. Barang kembali ke Kopdes
  2. Escrow refund ke buyer (dikurangi biaya admin)
  3. PO → cancelled

JALUR 4: Arbitrase admin (Roadmap)
  1. Admin TemuNiaga (manusia) jadi mediator
  2. Bandingkan foto grading asli vs foto buyer
  3. Putuskan: penyesuaian harga atau return
  4. Keputusan final, tercatat di sistem
```

**Data dispute:**
```
dispute_id | po_id | opened_by | reason | suggested_price | status | resolved_by | resolution
```

---

## Fase 3: Settlement & SHU

Setelah PO status "completed":
```
1. Escrow release: deposited → released_to_kopdes
2. Fee deduction: total_price × fee_rate → TemuNiaga revenue
3. Logistics deduction: actual biaya logistik
4. Kopdes margin = revenue - farmer_payout - logistics - fee
5. SHU calculation: margin - bunga 6% - opex → surplus → SHU
6. SHU distribution: pro-rata based on farmer transaction volume
7. Ledger update: setiap petani bisa lihat harga jual & potongan
```

---

## State Machine Lengkap (Semua Fase)

```
[FASE 1: Procurement]
  stok petani: reported → graded → paid → in_warehouse

[FASE 2: Sales]
  PO: draft → confirmed → in_preparation → shipped → delivered
  Logistik: packaging → ready → in_transit → delivered
  QC: delivered → completed (accept)
               → disputed (reject)
  Dispute: opened → negotiating → resolved

[FASE 3: Settlement]
  Escrow: pending → deposited → released
  PO final: completed
  SHU: calculated → distributed → ledger_visible
```

---

## Route API (Rencana)

```
POST   /api/auth/login          - Buyer/Operator login
GET    /api/rfq                 - List RFQ (buyer view)
POST   /api/rfq                 - Post new RFQ (buyer)
GET    /api/rfq/:id/matches     - List matches for a RFQ (buyer)
POST   /api/po                  - Generate PO from match (system)
PUT    /api/po/:id/confirm      - Confirm PO (buyer)
PUT    /api/po/:id/ship         - Mark as shipped (kopdes)
PUT    /api/po/:id/deliver      - Mark as delivered (buyer accept)
PUT    /api/po/:id/dispute      - Open dispute (buyer reject)
PUT    /api/po/:id/resolve      - Resolve dispute (kopdes setuju)
GET    /api/po/:id              - PO detail with status history
GET    /api/dashboard/kopdes    - Kopdes dashboard: active POs, stok
GET    /api/dashboard/buyer     - Buyer dashboard: RFQs, POs
```

---

## Prioritas Build

| Layer | Komponen | Status |
|---|---|---|
| **Fase 1** | Procurement (petani → Kopdes) | Sudah matang, tinggal kode |
| **Fase 2 [A]** | PO digital | WAJIB di MVP |
| **Fase 2 [C]** | QC buyer | WAJIB di MVP |
| **Fase 2 [B]** | Logistik | Cukup 1 opsi (Kopdes antar) |
| **Fase 2 [D]** | Dispute | Cukup Jalur 1 (penyesuaian harga) |
| **Fase 2 [E]** | Buyer onboarding | Seed buyer, tanpa registration flow |
| **Fase 3** | Settlement & SHU | Sudah ada, tinggal kode |

---

*Blueprint ini menjadi acuan tunggal untuk pengembangan modul transaksi B2B TemuNiaga.*

# Konten Pitch Deck — TemuNiaga

> Untuk 11 slide Canva. Maksimal 12 slide sesuai TOR Bagian F. Slide 1-11 = isi, slide 12 opsional.
> Setiap slide: judul + 3-5 bullet point + 1 visual (diagram/foto). Tidak ada paragraf panjang.

---

## Slide 1: Judul

**TemuNiaga** — Aplikasi Dagang untuk Koperasi Desa Merah Putih

- *Temu*: pertemukan penjual & pembeli. *Niaga*: perdagangan desa ke pasar nasional.
- Tema 2: Optimalisasi Potensi Desa Melalui Koperasi
- Hackathon Digital Cooperatives Expo 2026

**Visual:** Logo TemuNiaga (dari assets/logo.svg)

---

## Slide 2: Masalah — Jurang Terdaftar vs Hidup

- 83.362 Kopdes terdaftar di SimkopDes. Yang bertransaksi: **hanya 795** (per Juni 2026)
- Petani dan UMKM terjebak **oligopsoni**: sedikit pengepul, petani jadi price-taker
- Rantai 7-9 lapis. Kopi Solok: bagian petani cuma **32%**. Margin Rp12.000/kg lari ke perantara
- Koperasi ditugaskan menyerap panen dan menekan tengkulak, **tapi belum punya alat**

**Visual:** Diagram jurang-terdaftar (dari assets/jurang-terdaftar.svg)

---

## Slide 3: Masalah — Data & Bukti

- NTP 127,73 (Mei 2026, BPS). Petani tidak "miskin", tapi **nilai tidak terdistribusi adil**
- Estimasi Kementan: **Rp313 triliun/th** mengendap di perantara (batas atas)
- Petani individu tidak bisa jual langsung ke pabrik: volume di bawah MOQ
- UMKM desa (64 juta unit) tidak punya akses ke ritel modern

**Visual:** Diagram rantai-pasok (dari assets/rantai-pasok.svg)

**Sumber:** BPS, Jurnal Jimanggis 2024, Jurnal Agristan, CNN Indonesia

---

## Slide 4: Solusi — TemuNiaga

- **Koperasi sebagai countervailing power** — agregasi volume, daya tawar kolektif
- 3 hal yang dikerjakan: internalisasi margin tengkulak, pangkas asimetri informasi, turunkan biaya transaksi
- Arah penjualan: **B2B keluar desa** (pabrik, ritel, Bulog, eksportir), bukan toko kelontong
- Dua kategori: komoditas tani mentah (agregasi) + produk UMKM olahan (etalase + payung hukum)

**Visual:** Diagram solusi — struktur lama vs baru

---

## Slide 5: Cara Kerja — 4 Fungsi Utama

1. **Mesin harga acuan** — data BPS/Bapanas, transparan, rentang + tanggal
2. **Pencocokan stok vs RFQ pembeli** — greedy matching + pooling antar-koperasi
3. **Ledger transparan** — tiap anggota lihat harga jual & potongan, cegah korupsi pengurus
4. **Asisten AI (RAG)** — WA bot + voice bot, jawab hanya dari data, near-zero halusinasi

**Visual:** Mockup WA bot + dashboard + portal (3 gambar kecil sejajar)

---

## Slide 6: Alur Operasional

Petani dibayar **sebelum** pembeli dicari — menyamai kecepatan tunai tengkulak.

1. Registrasi → 2. Lapor pasokan (WA bot) → 3. Setor fisik → 4. Grading manual → 5. Tampilkan harga acuan → 6. Petani tekan YA → **dibayar** → 7. Stok Kopdes → 8. Matching RFQ → 9. Jual B2B → 10. SHU pro-rata → 11. Ledger transparan

4 mode penyerapan: talangan, konsinyasi, forward, beli putus (default buyer-first)

**Visual:** Diagram alur (dari README)

---

## Slide 7: Dampak Terukur — Kopi sebagai Hero

| Komoditas | Baseline | Target | Petani | Dampak/th |
|---|---|---|---|---|
| Kopi Solok | Rp6.000/kg | Rp6.900/kg (+15%) | 500 x 800 kg | **+Rp360 jt** |
| Tembakau Sumenep | Rp30.000/kg | Rp32.400/kg (+8%) | 400 x 500 kg | +Rp480 jt |
| Padi Bangkalan | Rp4.200/kg | Rp6.600/kg (pindah saluran) | 600 x 2.000 kg | +Rp2,88 M |
| Singkong nasional | Rp1.200/kg | Rp1.400/kg (+17%) | 300 x 3.000 kg | +Rp180 jt |

**Format:** "500 petani kopi + harga naik 15% + dalam 6 bulan = Rp360 juta/tahun ke desa"

**Visual:** Diagram farmer's share comparison + unit economics flow

---

## Slide 8: Pasar — TAM / SAM / SOM

- **TAM**: 83.362 Kopdes berbadan hukum
- **SAM**: Kopdes dengan kesiapan digital dasar
- **SOM**: 10-50 Kopdes pilot multi-komoditas, penekanan kopi

Proyeksi: 10 (th 1) → 50 (th 2) → 500 (th 3) → 24.000 (th 5, 60% target operasional)
Dampak tahun ke-5: Rp19,3 T kenaikan farmgate + Rp9,9 T SHU

**Visual:** Diagram TAM/SAM/SOM + kurva proyeksi skala

---

## Slide 9: Model Bisnis & Keberlanjutan

- **Hybrid freemium + transaction fee B2B**: Kopdes kecil gratis, fee 1-2% per deal B2B
- Fee dipotong dari margin deal, **bukan dari kantong Kopdes**
- Benchmark: Alibaba Trade Assurance 0,5-3%, marketplace Indonesia 1-4%
- Revenue per Kopdes: Rp87 jt/th. Break-even: bulan ke-18
- Margin Kopdes: Rp844 jt/th → bisa bayar bunga 6% + cicilan pokok + SHU

**Visual:** Diagram aliran uang (unit-economics-kopi.svg)

---

## Slide 10: Mengapa Ini Berhasil — Tiga Pasak Tata Kelola

Tanpa 3 pasak, koperasi = "tengkulak berbadan hukum" bermodal pinjaman negara:

1. **SHU dibagi pro-rata nilai transaksi** — kunci di AD/ART, bukan per kepala
2. **Hak keluar murah** — petani bebas jual ke tengkulak. Tengkulak jadi yardstick
3. **Transparansi + audit** — ledger real-time, cegah elite capture

**Teori:** Petani = residual claimant (Cook/Royer). Penjual = pemilik = penerima SHU.

**Visual:** Diagram tiga pasak

---

## Slide 11: Tim & Visi

**Tim:** [nama, peran hacker/hustler/hipster, latar belakang singkat]

**Visi jangka panjang:**
- TemuNiaga sebagai fondasi data transaksi koperasi di level desa
- Interoperable dengan SimkopDes dan Satu Data Indonesia
- "Amul-nya Indonesia": agregasi petani kecil → daya tawar nasional (benchmark: Amul India, 3,6 jt peternak, Rp190 T)

**Pelajaran dari KUD:** 35% koperasi aktif rutin RAT. Gagal karena subsidi + tata kelola lemah. TemuNiaga memutus pola itu: ledger transparan, SHU pro-rata, mandiri secara finansial.

**Visual:** Foto tim + benchmark Amul

---

## Slide 12 (Opsional): Lampiran Sumber

- Jurnal Jimanggis 2024: FS kopi Solok 32%
- Larasati & Tamami 2024: FS tembakau Sumenep 75,8%
- Rojabi & Destiarni 2026: FS padi Bangkalan 60-96%
- BPS: NTP 127,73, harga GKP Rp6.925
- PMK 49/2025: pinjaman Rp3 M, bunga 6%
- SimkopDes per Juni 2026: 83.362 terdaftar, 795 bertransaksi
- Slide mentorship & TOR Hackathon Kemenkop 2026

---

*Dokumen ini adalah konten untuk pitch deck. Lakukan visualisasi di Canva dengan diagram dari folder assets/.*

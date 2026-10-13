# Panduan Belajar Kuis — Teori Optimasi (SD25-20006)

Isi panduan ini mencakup tiga modul:

| Bagian | Modul | Topik |
|---|---|---|
| **A** | Modul Minggu ke-5 (Tutorial 3) | Pemrograman Linear & Metode Grafik, Excel Solver, Python |
| **B** | Modul Minggu ke-6 (Tutorial 4) | Metode Simpleks & Analisis Sensitivitas |
| **C** | Modul Minggu ke-6 (Tutorial 5) | Optimasi Tanpa Kendala: Gradien, Hessian, Convex, Steepest Descent/Ascent, Newton |
| **D** | — | Rangkuman rumus cepat & daftar kesalahan/typo di modul |

> Semua soal latihan dikerjakan **langkah demi langkah**, dan semua angka sudah dicek ulang.
> Di bagian D ada daftar **kesalahan/typo di modul dan kunci jawaban** supaya kamu tidak ikut salah.

---

# BAGIAN A — PEMROGRAMAN LINEAR & METODE GRAFIK

## A1. Konsep Dasar Pemrograman Linear (LP)

**Pemrograman Linear (LP)** = metode matematis untuk mencari nilai **optimal (maksimum/minimum)** dari **fungsi tujuan linear** dengan **kendala berupa persamaan/pertidaksamaan linear**. Contoh: menentukan jumlah produksi agar laba maksimal dengan bahan baku/waktu produksi terbatas.

### Empat karakteristik LP

| # | Karakteristik | Arti | Contoh |
|---|---|---|---|
| 1 | **Proporsionalitas** | Perubahan fungsi tujuan/kendala sebanding dengan perubahan variabel | 1 unit laba Rp10 → 10 unit laba Rp100, tanpa biaya tersembunyi |
| 2 | **Aditivitas** | Kontribusi tiap variabel independen, total = jumlah kontribusi individu | Tidak ada laba ekstra khusus karena produk A + B dibuat bersama |
| 3 | **Divisibilitas** | Variabel boleh bernilai pecahan (kontinu) | Jika tidak masuk akal (orang, mobil, pesawat) → butuh **Integer Programming (IP)** / **Mixed Integer Programming (MIP)** |
| 4 | **Kepastian (Certainty)** | Semua parameter diketahui pasti | Tidak ada pertanyaan "bagaimana kalau harga berubah?" |

### Tiga komponen model LP
1. **Variabel keputusan** — yang ingin ditentukan.
2. **Fungsi objektif (tujuan)** — yang dimaksimalkan/diminimalkan.
3. **Kendala** — batasan yang harus dipenuhi.

## A2. Bentuk Standar LP

**Bentuk umum:**

```
minimize   z = c₁x₁ + c₂x₂ + … + cₙxₙ
subject to a₁₁x₁ + a₁₂x₂ + … + a₁ₙxₙ = b₁
           a₂₁x₁ + a₂₂x₂ + … + a₂ₙxₙ = b₂
           ⋮
           aₘ₁x₁ + aₘ₂x₂ + … + aₘₙxₙ = bₘ
           x₁ ≥ 0, x₂ ≥ 0, …, xₙ ≥ 0
```

**Notasi matriks-vektor:** `z = cᵀx`, `Ax = b`, `x ≥ 0`, dengan **b ≥ 0** (vektor non-negatif).

Contoh: c = [3, 5]ᵀ, x = [x₁, x₂]ᵀ → cᵀ = [3 5] → z = cᵀx = 3x₁ + 5x₂.

### Cara mengubah LP ke bentuk standar

| Kondisi awal | Cara mengubah |
|---|---|
| Kendala **≤** | **Tambah** variabel *slack* (s ≥ 0). Contoh: `2x₁ + x₂ ≤ 8` → `2x₁ + x₂ + s₁ = 8` |
| Kendala **≥** | **Kurangi** variabel *surplus* (e ≥ 0). Contoh: `x₁ + x₂ ≥ 3` → `x₁ + x₂ − e₁ = 3` |
| Fungsi tujuan **maksimum** (jika perlu bentuk minimum) | `max z = cᵀx` ⇔ `min (−z) = −cᵀx` |
| **RHS negatif** (b < 0) | Kalikan kedua ruas dengan **−1** (tanda pertidaksamaan **berbalik**) |
| Variabel **bebas** tanda (free) | Substitusi `x = x⁺ − x⁻`, dengan x⁺, x⁻ ≥ 0 |
| Variabel **x ≤ 0** | Substitusi `x = −x'`, dengan x' ≥ 0 |

> Syarat memakai simpleks (dari modul): (1) semua kendala pertidaksamaan jadi persamaan, (2) RHS tidak boleh negatif, (3) semua variabel non-negatif.

## A3. Metode Grafik (untuk 2 variabel)

Setiap kendala = garis di bidang koordinat. Area yang memenuhi semua kendala = **daerah solusi layak (feasible region)**. **Solusi optimal selalu berada di salah satu titik pojok (corner point)** daerah layak.

### Langkah-langkah
1. **Gambar kendala** — plot tiap kendala sebagai garis (ubah `≤` jadi `=` untuk menggambar), tentukan sisi yang valid.
   - Cari **intersep**: set x₂ = 0 untuk dapat titik di sumbu x₁, set x₁ = 0 untuk titik di sumbu x₂.
   - Uji titik (0,0): jika memenuhi kendala, daerah layak ada di sisi (0,0).
2. **Tentukan daerah feasible** — irisan semua kendala (termasuk x ≥ 0).
3. **Hitung titik pojok** — perpotongan dua garis kendala (selesaikan SPL 2×2) atau perpotongan garis dengan sumbu. **Pastikan titik itu memenuhi SEMUA kendala.**
4. **Evaluasi fungsi objektif** di tiap titik pojok.
5. **Pilih solusi optimal** — nilai Z terbesar (maksimasi) atau terkecil (minimasi).

> Kasus khusus: jika daerah layak **tak terbatas (unbounded)** dan Z bisa terus membesar → solusi **unbounded**. Jika tidak ada daerah layak → **infeasible**.

---

## A4. Contoh 1 (modul): Minimasi dengan Metode Grafik

```
minimize  z = −x₁ − 2x₂
subject to −2x₁ + x₂ ≤ 2
           −x₁  + x₂ ≤ 3
            x₁       ≤ 3
            x₁, x₂ ≥ 0
```

**Langkah 1 — Garis batas & sisi layak**

| Kendala | Garis | Titik pada sumbu | Sisi layak |
|---|---|---|---|
| (1) −2x₁ + x₂ ≤ 2 | x₂ = 2 + 2x₁ | (0, 2); (−1, 0) | di bawah garis |
| (2) −x₁ + x₂ ≤ 3 | x₂ = 3 + x₁ | (0, 3); (−3, 0) | di bawah garis |
| (3) x₁ ≤ 3 | x₁ = 3 | — | di kiri garis |

**Langkah 2 — Titik pojok**
- (0, 0): perpotongan sumbu.
- (3, 0): x₁ = 3 dengan sumbu x₁.
- (0, 2): x₁ = 0 dengan (1) (lebih ketat daripada (2) yang memberi 3).
- (1, 4): perpotongan (1) dan (2): −2x₁ + x₂ = 2 dan −x₁ + x₂ = 3 → kurangkan: −x₁ = −1 → x₁ = 1, x₂ = 4.
- (3, 6): x₁ = 3 dengan (2): x₂ = 3 + 3 = 6. (Cek (1): −6 + 6 = 0 ≤ 2 ✓)

**Langkah 3 — Evaluasi z = −x₁ − 2x₂**

| Titik | z |
|---|---|
| (0, 0) | 0 |
| (3, 0) | −3 |
| (0, 2) | −4 |
| (1, 4) | −1 − 8 = −9 |
| **(3, 6)** | −3 − 12 = **−15** |

**Kesimpulan:** minimum **z = −15** di **x₁ = 3, x₂ = 6** (titik pojok feasible).

---

## A5. Contoh 2 (modul): Bengkel Meja (M) & Kursi (K)

**Data:** meja = 4 jam perakitan + 2 jam pemolesan; kursi = 2 jam perakitan + 4 jam pemolesan. Perakitan maks 60 jam; pemolesan maks 48 jam. Laba meja Rp80.000 (= 8 satuan Rp10.000), kursi Rp60.000 (= 6 satuan).

**1) Fungsi objektif:** `Maksimalkan Z = 8M + 6K` (satuan Rp10.000)

**2) Kendala:**
```
4M + 2K ≤ 60   (perakitan)
2M + 4K ≤ 48   (pemolesan)
M ≥ 0, K ≥ 0
```

**3) Intersep garis kendala** (modul menyederhanakan dengan membagi 2)

- 4M + 2K = 60 ⇔ 2M + K = 30
  - K = 0 → M = 15 → titik **(15, 0)**
  - M = 0 → K = 30 → titik **(0, 30)**
- 2M + 4K = 48 ⇔ M + 2K = 24
  - K = 0 → M = 24 → titik **(24, 0)**
  - M = 0 → K = 12 → titik **(0, 12)**

**4) Titik pojok daerah feasible**
- (0, 0)
- (15, 0): garis perakitan dengan sumbu M (cek pemolesan: M + 2K = 15 ≤ 24 ✓)
- (0, 12): garis pemolesan dengan sumbu K (cek perakitan: 2·0 + 12 = 12 ≤ 30 ✓)
- **Perpotongan dua garis:**
  - 2M + K = 30 → K = 30 − 2M
  - Substitusi ke M + 2K = 24: M + 2(30 − 2M) = 24 → M + 60 − 4M = 24 → −3M = −36 → **M = 12**
  - K = 30 − 24 = **6** → titik **(12, 6)**

**5) Evaluasi Z = 8M + 6K**

| Titik | Hitungan | Z | Laba (× Rp10.000) |
|---|---|---|---|
| (0, 0) | 0 | 0 | Rp0 |
| (15, 0) | 8(15) + 6(0) | 120 | Rp1.200.000 |
| (0, 12) | 8(0) + 6(12) | 72 | Rp720.000 |
| **(12, 6)** | 8(12) + 6(6) = 96 + 36 | **132** | **Rp1.320.000** |

**Kesimpulan:** produksi **12 meja dan 6 kursi** → laba maksimum **Rp1.320.000**.

---

## A6. Contoh 3 (modul): Reddy Mikks

**Data**

| | Bahan baku per ton cat Eksterior | per ton cat Interior | Ketersediaan maks/hari (ton) |
|---|---|---|---|
| Bahan M1 | 6 | 4 | 24 |
| Bahan M2 | 1 | 2 | 6 |
| Profit per ton ($1000) | 5 | 4 | |

Tambahan: (i) produksi interior tidak boleh melebihi eksterior lebih dari 1 ton; (ii) permintaan interior maksimum 2 ton/hari.

**Variabel keputusan**
- x₁ = ton cat **eksterior** per hari
- x₂ = ton cat **interior** per hari

**Model lengkap**
```
Maximize z = 5x₁ + 4x₂
subject to
   6x₁ + 4x₂ ≤ 24   (1) bahan M1
    x₁ + 2x₂ ≤ 6    (2) bahan M2
   −x₁ +  x₂ ≤ 1    (3) batas pasar (x₂ − x₁ ≤ 1)
          x₂ ≤ 2    (4) batas permintaan
   x₁, x₂ ≥ 0       (5)
```

**Titik pojok daerah layak (A–F)**

| Titik | Diperoleh dari | Koordinat |
|---|---|---|
| A | Titik asal | (0, 0) |
| B | (1) dengan sumbu x₁: 6x₁ = 24 | (4, 0) |
| C | (1) ∩ (2): x₁ = 6 − 2x₂ → 6(6 − 2x₂) + 4x₂ = 24 → 36 − 8x₂ = 24 → x₂ = 1,5; x₁ = 3 | (3; 1,5) |
| D | (2) ∩ (4): x₂ = 2 → x₁ + 4 = 6 → x₁ = 2 | (2, 2) |
| E | (3) ∩ (4): x₂ = 2 → −x₁ + 2 = 1 → x₁ = 1 | (1, 2) |
| F | (3) dengan sumbu x₂: x₁ = 0 → x₂ = 1 | (0, 1) |

**Evaluasi z = 5x₁ + 4x₂**

| Titik | z |
|---|---|
| A (0, 0) | 0 |
| B (4, 0) | 20 |
| **C (3; 1,5)** | 15 + 6 = **21** |
| D (2, 2) | 10 + 8 = 18 |
| E (1, 2) | 5 + 8 = 13 |
| F (0, 1) | 4 |

**Kesimpulan:** optimum di **C: x₁ = 3 ton eksterior, x₂ = 1,5 ton interior, z = $21.000**.

> Cara cepat dengan **garis iso-profit**: garis z = 10, 15, 21 sejajar; geser ke arah z naik sampai garis terakhir menyentuh daerah layak (di titik C).

---

## A7. LATIHAN SOAL 1 — Teknik Grafis

```
maximize   x₁ + x₂
subject to −2x₁ + x₂ ≤ 1     (1)
            x₁ − x₂ ≤ 1     (2)
            x₁, x₂ ≥ 0
```

**Langkah 1 — Garis batas & sisi layak**

| Kendala | Garis | Titik pada sumbu | Sisi layak |
|---|---|---|---|
| (1) −2x₁ + x₂ ≤ 1 | x₂ = 1 + 2x₁ | x₁ = 0 → x₂ = 1 ; x₂ = 0 → x₁ = −1/2 | **di bawah** garis |
| (2) x₁ − x₂ ≤ 1 | x₂ = x₁ − 1 | x₁ = 0 → x₂ = −1 ; x₂ = 0 → x₁ = 1 | **di atas** garis |

Uji (0,0): (1) 0 ≤ 1 ✓ ; (2) 0 ≤ 1 ✓ → (0,0) layak.

**Langkah 2 — Perpotongan kedua garis**

```
 −2x₁ + x₂ = 1
   x₁ − x₂ = 1   +
 ─────────────────
   −x₁      = 2  → x₁ = −2, lalu x₂ = x₁ − 1 = −3
```
Perpotongan di (−2, −3) → **di luar kuadran I**, jadi bukan titik pojok daerah layak.

**Langkah 3 — Titik pojok yang benar-benar layak (dengan x₁, x₂ ≥ 0)**

| Titik | Asal | z = x₁ + x₂ |
|---|---|---|
| (0, 0) | titik asal | 0 |
| (0, 1) | (1) dengan sumbu x₂ | 1 |
| (1, 0) | (2) dengan sumbu x₁ | 1 |

Daerah layak **tidak tertutup (unbounded)**: terbuka ke arah kanan-atas, di antara garis x₂ = 1 + 2x₁ (atas) dan x₂ = x₁ − 1 (bawah).

**Langkah 4 — Periksa apakah z bisa dibuat tak terbatas**

Ambil titik (t, t) dengan t ≥ 0:
- Kendala (1): −2t + t = −t ≤ 1 ✓
- Kendala (2): t − t = 0 ≤ 1 ✓
- Z = t + t = 2t → **+∞** jika t → ∞

**Kesimpulan:** masalah **UNBOUNDED** — tidak ada solusi optimum terbatas, nilai maksimum = **+∞**.

> ⚠️ Pada kunci jawaban, tabel titik pojok memuat (−1/2, 0), (0, −1), (−2, −3) — titik-titik itu **melanggar x ≥ 0** sehingga bukan titik layak. Kesimpulan akhirnya (unbounded) tetap benar. Di kuis, sebut saja titik layak (0,0), (0,1), (1,0) lalu tunjukkan arah tak terbatas.

---

## A8. LATIHAN SOAL 2 — Reddy Mikks dengan Excel Solver & Python

Model sama dengan A6. Hasil yang diharapkan: **x₁ = 3, x₂ = 1,5, Profit maksimum = 21**.

### Cara 1: Excel Solver

**Langkah 1 — Siapkan tabel data**

| Sel | Isi |
|---|---|
| B5:C5 | Koefisien objektif: **5, 4** |
| B6:C6 | Bahan baku 1: **6, 4** |
| B7:C7 | Bahan baku 2: **1, 2** |
| B8:C8 | Batas pasar: **−1, 1** |
| B9:C9 | Batas permintaan: **0, 1** |
| F6:F9 | Limit (RHS): **24, 6, 1, 2** |
| E6:E9 | Simbol **<=** (opsional) |
| B13:C13 | **Sel variabel keputusan** (dikosongkan; Solver yang mengisi) |
| D13 | `=D5` (menampilkan nilai z; opsional) |

Struktur tabel:

| | A | B (x₁ Exterior) | C (x₂ Interior) | D (Totals) | E | F (Limits) |
|---|---|---|---|---|---|---|
| 5 | Objective | 5 | 4 | rumus | | |
| 6 | Raw Material 1 | 6 | 4 | rumus | <= | 24 |
| 7 | Raw Material 2 | 1 | 2 | rumus | <= | 6 |
| 8 | Market limit | −1 | 1 | rumus | <= | 1 |
| 9 | Demand limit | 0 | 1 | rumus | <= | 2 |
| 12 | (label) | x1 | x2 | z | | |
| 13 | solution | *(diisi Solver)* | *(diisi Solver)* | | | |

**Langkah 2 — Rumus di kolom Totals (SUMPRODUCT)**

```
D5:  =SUMPRODUCT(B5:C5, $B$13:$C$13)    ← total profit (fungsi tujuan)
D6:  =SUMPRODUCT(B6:C6, $B$13:$C$13)
D7:  =SUMPRODUCT(B7:C7, $B$13:$C$13)
D8:  =SUMPRODUCT(B8:C8, $B$13:$C$13)
D9:  =SUMPRODUCT(B9:C9, $B$13:$C$13)
```
> Jika Excel kamu memakai pengaturan regional Indonesia, pemisah argumen sering berupa **titik koma (`;`)**, mis. `=SUMPRODUCT(B5:C5;$B$13:$C$13)`. Kunci jawaban memakai kedua gaya.
> Tanda `$` (referensi absolut) penting agar sel variabel B13:C13 tetap saat rumus disalin.

**Langkah 3 — Buka Solver:** menu **Data → Solver**. Jika tidak ada: **File → Options → Add-ins → Manage: Excel Add-ins → Go → centang Solver Add-in**.

**Langkah 4 — Isi parameter Solver**

| Parameter | Isian |
|---|---|
| Set Objective | `$D$5` |
| To | **Max** |
| By Changing Variable Cells | `$B$13:$C$13` |
| Constraint 1 | `$D$6:$D$9 <= $F$6:$F$9` (semua batas bahan baku, pasar, permintaan) |
| Constraint 2 | `$B$13:$C$13 >= 0` (variabel tidak negatif) |
| Make Unconstrained Variables Non-Negative | ✔ dicentang |
| Select a Solving Method | **Simplex LP** (karena masalah linear) |

**Langkah 5 — Klik Solve** → pilih *Keep Solver Solution* → hasil: `x₁ = 3`, `x₂ = 1,5`, `Profit = 21`.

### Cara 2: Python (PuLP)

```python
# pip install pulp
import pulp

# 1. Definisikan problem (maximize)
model = pulp.LpProblem("Reddy_Mikks_Model", pulp.LpMaximize)

# 2. Variabel keputusan
x1 = pulp.LpVariable("x1", lowBound=0)   # x1 >= 0
x2 = pulp.LpVariable("x2", lowBound=0)   # x2 >= 0

# 3. Fungsi tujuan
model += 5 * x1 + 4 * x2, "Total_Profit"

# 4. Kendala
model += 6 * x1 + 4 * x2 <= 24    # Raw material 1
model += 1 * x1 + 2 * x2 <= 6     # Raw material 2
model += -1 * x1 + 1 * x2 <= 1    # Market limit  (x2 - x1 <= 1)
model += 0 * x1 + 1 * x2 <= 2     # Demand limit

# 5. Jalankan solver
model.solve()

# 6. Cetak hasil
print("Status:", pulp.LpStatus[model.status])
print("x1 =", x1.varValue)
print("x2 =", x2.varValue)
print("Optimal Profit =", pulp.value(model.objective))
```

Output:
```
Status: Optimal
x1 = 3.0
x2 = 1.5
Optimal Profit = 21.0
```

> ⚠️ Di kunci jawaban, kendala pasar tertulis `-1*x1 - 1*x2 <= -1` (artinya x₁ + x₂ ≥ 1). Itu **tidak sesuai** model, yang benar `-x₁ + x₂ ≤ 1` seperti di atas. Hasilnya kebetulan sama, tapi model yang benar adalah versi di atas. Begitu juga tabel Excel di kunci: baris Market limit yang benar adalah **(−1, 1)**.

## A9. Latihan Soal 3 [15 poin]
Pada modul, soal nomor 3 **tidak ada isinya** (hanya tertulis "3. [15 Poin-CPMK101]"). Jika dosen membagikannya, pola pengerjaannya sama dengan A4–A8.

---
---

# BAGIAN B — METODE SIMPLEKS & ANALISIS SENSITIVITAS

## B1. Konsep Dasar Simpleks

Simpleks adalah metode sistematis yang bergerak dari satu **solusi dasar layak (basic feasible solution)** ke solusi dasar layak lainnya, berulang-ulang (jumlah langkah terbatas), sampai tercapai solusi optimum. Pada tiap langkah nilai fungsi tujuan **lebih baik atau sama** dengan langkah sebelumnya. Ada **tes kriteria** yang menunjukkan kapan berhenti.

**Kelebihan dibanding grafik:** grafik hanya untuk 2 variabel; simpleks untuk variabel > 2.

**Slack variable** = variabel tambahan yang mewakili sisa kapasitas (sumber daya yang belum terpakai) pada kendala ≤.

**Istilah:** **NK** = Nilai Kanan persamaan (nilai setelah tanda "=").

**Tiga syarat bentuk simpleks:**
1. Semua kendala pertidaksamaan diubah jadi persamaan (tambah slack).
2. Ruas kanan tidak boleh negatif.
3. Semua variabel ≥ 0.

## B2. Langkah-langkah Simpleks (Maksimasi)

| Langkah | Kegiatan |
|---|---|
| 1 | Ubah fungsi tujuan: `Z = c₁x₁ + c₂x₂` → `Z − c₁x₁ − c₂x₂ = 0` |
| 2 | Ubah kendala ≤ jadi persamaan dengan **menambah slack** |
| 3 | Susun **tabel simpleks awal**; pilih **kolom kunci** = kolom dengan nilai **paling negatif** di baris Z |
| 4 | Pilih **baris kunci**: hitung **indeks = NK ÷ nilai kolom kunci** tiap baris (hanya yang kolom kuncinya **positif**); pilih indeks **positif terkecil** |
| 5 | **Angka kunci** = perpotongan kolom & baris kunci. **Bagi seluruh baris kunci dengan angka kunci**; ganti variabel dasar di baris itu dengan variabel kolom kunci |
| 6 | Ubah baris lain: **Baris baru = baris lama − (koefisien di kolom kunci × baris kunci baru)** |
| 7 | Ulangi langkah 3–6 sampai **baris Z tidak ada lagi yang bernilai negatif** (maksimasi) → optimal |

**Kriteria berhenti**
- **Maksimasi:** optimal jika semua koefisien baris Z **≥ 0**.
- **Minimasi (bentuk Z − cx = 0):** optimal jika semua koefisien baris Z ≤ 0.

**Cara membaca hasil akhir:** variabel yang ada di kolom "Variabel Dasar" bernilai sama dengan NK barisnya; variabel lain = 0. Nilai Z = NK baris Z.

**Catatan penting:** baris dengan nilai kolom kunci **0 atau negatif** tidak dihitung indeksnya (indeks "∞"/diabaikan).

---

## B3. Contoh (modul): Simpleks dengan 2 variabel

```
Maksimumkan Z = 3x₁ + 5x₂
Kendala: (1) 2x₁ ≤ 8
         (2) 3x₂ ≤ 15
         (3) 6x₁ + 5x₂ ≤ 30
```

**Langkah 1 — Fungsi tujuan:** `Z − 3x₁ − 5x₂ = 0`

**Langkah 2 — Tambah slack (x₃, x₄, x₅):**
```
2x₁ + x₃ = 8
3x₂ + x₄ = 15
6x₁ + 5x₂ + x₅ = 30
```

**Tabel simpleks pertama**

| Var. Dasar | Z | x₁ | x₂ | x₃ | x₄ | x₅ | NK |
|---|---|---|---|---|---|---|---|
| Z | 1 | −3 | **−5** | 0 | 0 | 0 | 0 |
| x₃ | 0 | 2 | 0 | 1 | 0 | 0 | 8 |
| x₄ | 0 | 0 | **3** | 0 | 1 | 0 | 15 |
| x₅ | 0 | 6 | 5 | 0 | 0 | 1 | 30 |

**Langkah 3 — Kolom kunci:** nilai paling negatif di baris Z = **−5** → kolom **x₂**.

**Langkah 4 — Baris kunci (indeks = NK ÷ kolom kunci)**

| Baris | Perhitungan | Indeks |
|---|---|---|
| x₃ | 8 / 0 | tak terdefinisi (∞), diabaikan |
| **x₄** | 15 / 3 | **5** ← terkecil |
| x₅ | 30 / 5 | 6 |

Baris kunci = **x₄**, angka kunci = **3**.

**Langkah 5 — Ubah baris kunci (bagi 3), variabel dasar x₄ diganti x₂**

```
x₂: [0, 0, 3/3, 0, 1/3, 0 | 15/3] = [0, 0, 1, 0, 1/3, 0 | 5]
```

**Langkah 6 — Ubah baris lain (baris baru = baris lama − koef. kolom kunci × baris kunci baru)**

*Baris Z* (koef. kolom kunci = −5): Z_baru = Z_lama − (−5)·x₂ = Z_lama + 5·x₂
```
Z lama:   [1, −3, −5, 0,   0,  0 | 0]
5·x₂:     [0,  0,  5, 0, 5/3,  0 | 25]
Z baru:   [1, −3,  0, 0, 5/3,  0 | 25]
```
*Baris x₃* (koef. kolom kunci = 0) → **tidak berubah**: `[0, 2, 0, 1, 0, 0 | 8]`

*Baris x₅* (koef. kolom kunci = 5): x₅_baru = x₅_lama − 5·x₂
```
x₅ lama:  [0, 6, 5, 0,    0, 1 | 30]
5·x₂:     [0, 0, 5, 0,  5/3, 0 | 25]
x₅ baru:  [0, 6, 0, 0, −5/3, 1 | 5]
```

**Tabel simpleks kedua**

| Var. Dasar | Z | x₁ | x₂ | x₃ | x₄ | x₅ | NK |
|---|---|---|---|---|---|---|---|
| Z | 1 | **−3** | 0 | 0 | 5/3 | 0 | 25 |
| x₃ | 0 | 2 | 0 | 1 | 0 | 0 | 8 |
| x₂ | 0 | 0 | 1 | 0 | 1/3 | 0 | 5 |
| x₅ | 0 | **6** | 0 | 0 | −5/3 | 1 | 5 |

Baris Z masih ada nilai negatif (−3) → **lanjut iterasi**.

**Iterasi 2**
- Kolom kunci: **x₁** (−3).
- Indeks: x₃ → 8/2 = **4**; x₂ → 5/0 = ∞ (diabaikan); x₅ → 5/6 = **0,833** ← terkecil.
- Baris kunci: **x₅**, angka kunci **6**.
- Baris kunci baru (bagi 6), variabel dasar jadi x₁: `[0, 1, 0, 0, −5/18, 1/6 | 5/6]`

*Baris Z* (koef. = −3): Z_baru = Z_lama + 3·x₁
```
Z lama:   [1, −3, 0, 0,   5/3,  0 | 25]
3·x₁:     [0,  3, 0, 0, −5/6, 1/2 | 5/2]
Z baru:   [1,  0, 0, 0,   5/6, 1/2 | 27½]
```
(5/3 − 5/6 = 10/6 − 5/6 = 5/6 ; 25 + 2,5 = 27,5)

*Baris x₃* (koef. = 2): x₃_baru = x₃_lama − 2·x₁
```
x₃ lama:  [0, 2, 0, 1,     0,    0 | 8]
2·x₁:     [0, 2, 0, 0, −5/9,  1/3 | 5/3]
x₃ baru:  [0, 0, 0, 1,   5/9, −1/3 | 19/3]
```
(8 − 5/3 = 19/3 = 6⅓)

*Baris x₂* (koef. = 0) → tidak berubah: `[0, 0, 1, 0, 1/3, 0 | 5]`

**Tabel simpleks final**

| Var. Dasar | Z | x₁ | x₂ | x₃ | x₄ | x₅ | NK |
|---|---|---|---|---|---|---|---|
| Z | 1 | 0 | 0 | 0 | 5/6 | 1/2 | 27½ |
| x₃ | 0 | 0 | 0 | 1 | 5/9 | −1/3 | 6⅓ |
| x₂ | 0 | 0 | 1 | 0 | 1/3 | 0 | 5 |
| x₁ | 0 | 1 | 0 | 0 | −5/18 | 1/6 | 5/6 |

Baris Z **tidak ada lagi yang negatif** → **OPTIMAL**.

**Hasil:** `x₁ = 5/6`, `x₂ = 5`, `Z_maks = 27½`.
Cek: 3(5/6) + 5(5) = 2,5 + 25 = 27,5 ✓. Cek kendala (3): 6(5/6) + 5(5) = 5 + 25 = 30 ✓ (aktif); kendala (2): 3(5) = 15 ✓ (aktif); kendala (1): 2(5/6) = 5/3 ≤ 8 (sisa x₃ = 19/3).

> Modul menulis "x₁ = 5/6, x₁ = 5" — yang kedua **typo**, seharusnya **x₂ = 5**.

---

## B4. Contoh soal modul (Perusahaan Air) — dikerjakan lengkap

Pada modul, soal ini hanya diformulasikan. Berikut penyelesaiannya (sebagai latihan tambahan).

**Data**

| Produk | Buruh (jam/unit) | Air (juta m³) | Keuntungan (Rp/unit) |
|---|---|---|---|
| Air Bersih (X₁) | 5 | 4 | 3 |
| Air Minum (X₂) | 2 | 6 | 5 |
| Listrik (X₃) | 4 | 3 | 2 |

Tersedia 240 jam kerja dan 400 (juta m³) bahan mentah.

**Model**
```
Maksimumkan Z = 3X₁ + 5X₂ + 2X₃
(1) 5X₁ + 2X₂ + 4X₃ ≤ 240   (jam buruh)
(2) 4X₁ + 6X₂ + 3X₃ ≤ 400   (bahan)
X₁, X₂, X₃ ≥ 0
```

**Bentuk simpleks:** `Z − 3X₁ − 5X₂ − 2X₃ = 0`; `5X₁ + 2X₂ + 4X₃ + S₁ = 240`; `4X₁ + 6X₂ + 3X₃ + S₂ = 400`.

**Tabel awal**

| Var. Dasar | Z | X₁ | X₂ | X₃ | S₁ | S₂ | NK | Indeks |
|---|---|---|---|---|---|---|---|---|
| Z | 1 | −3 | **−5** | −2 | 0 | 0 | 0 | |
| S₁ | 0 | 5 | 2 | 4 | 1 | 0 | 240 | 240/2 = 120 |
| S₂ | 0 | 4 | **6** | 3 | 0 | 1 | 400 | 400/6 = **66,67** ← |

Kolom kunci X₂; baris kunci S₂; angka kunci 6.

**Baris kunci baru (÷6), dasar → X₂:** `[0, 4/6, 1, 3/6, 0, 1/6 | 400/6]` = `[0, 2/3, 1, 1/2, 0, 1/6 | 200/3]`

**Z baru = Z lama + 5·X₂:**
```
X₁: −3 + 5(2/3) = 1/3
X₂: −5 + 5      = 0
X₃: −2 + 5(1/2) = 1/2
S₁: 0
S₂: 0 + 5(1/6)  = 5/6
NK: 0 + 5(200/3) = 1000/3
```
**S₁ baru = S₁ lama − 2·X₂:**
```
X₁: 5 − 2(2/3) = 11/3
X₃: 4 − 2(1/2) = 3
S₂: 0 − 2(1/6) = −1/3
NK: 240 − 2(200/3) = 320/3
```

**Tabel final**

| Var. Dasar | Z | X₁ | X₂ | X₃ | S₁ | S₂ | NK |
|---|---|---|---|---|---|---|---|
| Z | 1 | 1/3 | 0 | 1/2 | 0 | 5/6 | 1000/3 |
| S₁ | 0 | 11/3 | 0 | 3 | 1 | −1/3 | 320/3 |
| X₂ | 0 | 2/3 | 1 | 1/2 | 0 | 1/6 | 200/3 |

Semua koefisien baris Z ≥ 0 → **optimal**.

**Hasil:** `X₁ = 0`, `X₂ = 200/3 ≈ 66,67`, `X₃ = 0`, `Z_maks = 1000/3 ≈ 333,33`.
Hanya air minum yang diproduksi. Cek: jam buruh terpakai = 2(66,67) = 133,33 ≤ 240 (sisa S₁ = 106,67 = 320/3 ✓); bahan = 6(66,67) = 400 (habis).

---

## B5. Konsep Analisis Sensitivitas

**Analisis sensitivitas** menjelaskan **sejauh mana parameter model LP boleh berubah tanpa mengubah solusi optimal / basis optimal**. Dua pertanyaan utama:
1. Bagaimana perubahan **ruas kanan (RHS)** kendala memengaruhi solusi optimal?
2. Bagaimana perubahan **koefisien fungsi tujuan** memengaruhi solusi optimal?

**Harga bayangan (Shadow Price)** suatu kendala:

```
Shadow Price = (Z_baru − Z_lama) / (Perubahan RHS)  =  ΔZ / ΔRHS
```
Artinya: kenaikan nilai optimal Z untuk **setiap tambahan 1 unit kapasitas** kendala tersebut (berlaku selama **struktur basis tidak berubah**).

> Dalam tabel simpleks final, shadow price tiap kendala = **koefisien variabel slack kendala itu di baris Z**.

## B6. Contoh Sensitivitas (modul): Dua Mesin

| | Produk 1 | Produk 2 | Ketersediaan |
|---|---|---|---|
| Mesin 1 | 1 jam | 2 jam | 6 jam |
| Mesin 2 | 2 jam | 1 jam | 8 jam |
| Keuntungan | $2 | $3 | |

```
max z = 2x₁ + 3x₂
x₁ + 2x₂ ≤ 6     (Mesin 1)
2x₁ + x₂ ≤ 8     (Mesin 2)
x₁, x₂ ≥ 0
```

**Solusi grafik awal:** titik pojok A = perpotongan kedua kendala:
x₁ = 6 − 2x₂ → 2(6 − 2x₂) + x₂ = 8 → 12 − 3x₂ = 8 → x₂ = 4/3; x₁ = 10/3.
**A (10/3, 4/3)**, z_A = 2(10/3) + 3(4/3) = 20/3 + 4 = **32/3 = 10⅔**.

**Jika RHS Mesin 1 naik 6 → 7:** `x₁ + 2x₂ ≤ 7`
Titik baru B: x₁ = 7 − 2x₂ → 2(7 − 2x₂) + x₂ = 8 → 14 − 3x₂ = 8 → x₂ = 2; x₁ = 3. **B (3, 2)**, z_B = 6 + 6 = **12**.

**Shadow Price (Mesin 1)** = (z_B − z_A) / (7 − 6) = (12 − 10⅔) / 1 = **4/3** (≈ $1,33 per jam tambahan).

*(Tambahan, untuk Mesin 2: shadow price = 1/3. Dari dual: y₁ + 2y₂ = 2 dan 2y₁ + y₂ = 3 → y₁ = 4/3, y₂ = 1/3.)*

---

## B7. LATIHAN SOAL 1 — Simpleks

```
Maksimalkan Z = 6y₁ + 4y₂
Kendala:  2y₁ + y₂ ≤ 3
           y₁ + y₂ ≤ 2
           y₁, y₂ ≥ 0
```

**Langkah 1 — Fungsi tujuan:** `Z − 6y₁ − 4y₂ = 0`

**Langkah 2 — Slack s₁, s₂ ≥ 0:**
```
2y₁ + y₂ + s₁ = 3
 y₁ + y₂ + s₂ = 2
```

**Tabel simpleks pertama**

| Var. Dasar | Z | y₁ | y₂ | s₁ | s₂ | NK | Indeks |
|---|---|---|---|---|---|---|---|
| Z | 1 | **−6** | −4 | 0 | 0 | 0 | |
| s₁ | 0 | **2** | 1 | 1 | 0 | 3 | 3/2 = **1,5** ← |
| s₂ | 0 | 1 | 1 | 0 | 1 | 2 | 2/1 = 2 |

**Langkah 3 — Kolom kunci:** −6 paling negatif → **y₁**.
**Langkah 4 — Baris kunci:** indeks terkecil 1,5 → **s₁**; angka kunci = **2**.

**Langkah 5 — Baris kunci baru (÷2), dasar → y₁:**
`y₁: [0, 1, 1/2, 1/2, 0 | 3/2]`

**Langkah 6 — Baris lain**
- Z_baru = Z_lama + 6·y₁:
  - y₂: −4 + 6(1/2) = −1
  - s₁: 0 + 6(1/2) = 3
  - s₂: 0
  - NK: 0 + 6(3/2) = 9
- s₂_baru = s₂_lama − 1·y₁:
  - y₂: 1 − 1/2 = 1/2
  - s₁: 0 − 1/2 = −1/2
  - s₂: 1
  - NK: 2 − 3/2 = 1/2

**Tabel simpleks kedua**

| Var. Dasar | Z | y₁ | y₂ | s₁ | s₂ | NK | Indeks |
|---|---|---|---|---|---|---|---|
| Z | 1 | 0 | **−1** | 3 | 0 | 9 | |
| y₁ | 0 | 1 | 1/2 | 1/2 | 0 | 3/2 | (3/2)/(1/2) = 3 |
| s₂ | 0 | 0 | **1/2** | −1/2 | 1 | 1/2 | (1/2)/(1/2) = **1** ← |

Masih ada −1 di baris Z (maksimasi) → **lanjut**.

**Langkah 7 — Iterasi 2:** kolom kunci **y₂**; baris kunci **s₂**; angka kunci **1/2**.
- Baris kunci baru (÷1/2 = ×2), dasar → y₂: `y₂: [0, 0, 1, −1, 2 | 1]`
- Z_baru = Z_lama + 1·y₂: y₂: 0; s₁: 3 − 1 = **2**; s₂: 0 + 2 = **2**; NK: 9 + 1 = **10**
- y₁_baru = y₁_lama − (1/2)·y₂: y₂: 0; s₁: 1/2 + 1/2 = **1**; s₂: 0 − 1 = **−1**; NK: 3/2 − 1/2 = **1**

**Tabel simpleks final**

| Var. Dasar | Z | y₁ | y₂ | s₁ | s₂ | NK |
|---|---|---|---|---|---|---|
| Z | 1 | 0 | 0 | 2 | 2 | 10 |
| y₁ | 0 | 1 | 0 | 1 | −1 | 1 |
| y₂ | 0 | 0 | 1 | −1 | 2 | 1 |

Semua koefisien baris Z ≥ 0 → **OPTIMAL**.

**Jawaban:** **y₁ = 1, y₂ = 1, Z_maks = 10**.
(Cek: 6(1) + 4(1) = 10; 2(1)+1 = 3 ✓; 1+1 = 2 ✓ — kedua kendala aktif.)

**Bonus:** koefisien s₁ dan s₂ di baris Z, yaitu **2 dan 2**, adalah **shadow price** kendala 1 dan 2 (dipakai di soal 2).

---

## B8. LATIHAN SOAL 2 — Analisis Sensitivitas

Model awal: Z_maks = 10 di (y₁, y₂) = (1, 1), dari perpotongan:
```
2y₁ + y₂ = 3
 y₁ + y₂ = 2   → y₁ = 1, y₂ = 1  (substitusi y₂ = 2 − y₁: 2y₁ + 2 − y₁ = 3 → y₁ = 1)
```

### 2a) RHS kendala 1 berubah 3 → 4 (kendala 2 tetap)

Model baru:
```
Maks Z = 6y₁ + 4y₂
2y₁ + y₂ ≤ 4
 y₁ + y₂ ≤ 2
y₁, y₂ ≥ 0
```

**Langkah 1 — Titik potong kendala aktif**
```
2y₁ + y₂ = 4
 y₁ + y₂ = 2   → kurangkan: y₁ = 2, y₂ = 2 − 2 = 0
```
Titik **(2, 0)**.

**Langkah 2 — Titik pojok & evaluasi Z**

| Titik | Z = 6y₁ + 4y₂ |
|---|---|
| (0, 0) | 0 |
| (2, 0) | 12 |
| (0, 2) | 8 |

(Titik (0,4) tidak layak karena melanggar y₁ + y₂ ≤ 2.) → Optimum **(2, 0), Z = 12**.

**Langkah 3 — Perbandingan**

| Kondisi | y₁ | y₂ | Z_maks |
|---|---|---|---|
| Sebelum (RHS₁ = 3) | 1 | 1 | 10 |
| Sesudah (RHS₁ = 4) | 2 | 0 | 12 |

**Langkah 4 — Analisis**
- Daerah layak **melebar**; solusi pindah dari (1,1) ke (2,0): **y₁ naik** (1 → 2), **y₂ turun** (1 → 0), **Z naik** 10 → 12.
- ΔZ = 12 − 10 = **2**; ΔRHS = 4 − 3 = **1**
- **Shadow price kendala 1 = ΔZ / ΔRHS = 2**

**Interpretasi:** setiap penambahan 1 unit kapasitas (RHS) kendala 1 menaikkan keuntungan maksimum sebesar **2 unit** (selama struktur basis tidak berubah). Ini sesuai koefisien s₁ = 2 di tabel simpleks final.

**Rentang validitas (tambahan, memakai tabel final):** dengan RHS = (b₁, 2):
- y₁ = 1·b₁ − 1·2 = b₁ − 2 ≥ 0 → b₁ ≥ 2
- y₂ = −1·b₁ + 2·2 = 4 − b₁ ≥ 0 → b₁ ≤ 4

Jadi shadow price = 2 berlaku untuk **2 ≤ RHS₁ ≤ 4**. Pada RHS₁ = 4 tepat di batas atas (y₂ = 0, solusi degenerate), sehingga kenaikan di atas 4 tidak lagi memberi tambahan 2 per unit.

### 2b) RHS kendala 2 berubah 2 → 3 (kendala 1 tetap)

> Catatan: teks soal di modul berbunyi "RHS kendala 1 berubah menjadi 3 … y₁ + y₂ ≤ 2 … kendala 1 tetap" — itu **salah ketik**. Dari kunci jawaban, maksudnya: **RHS kendala 2** diubah menjadi 3 sehingga `y₁ + y₂ ≤ 3`, dan **kendala 1 tetap** (`2y₁ + y₂ ≤ 3`).

Model baru:
```
Maks Z = 6y₁ + 4y₂
2y₁ + y₂ ≤ 3
 y₁ + y₂ ≤ 3
y₁, y₂ ≥ 0
```

**Langkah 1 — Titik pojok**
- Sumbu y₁ (y₂ = 0): kendala 1 → y₁ = 1,5 → **(1,5; 0)** (kendala 2: 1,5 ≤ 3 ✓)
- Sumbu y₂ (y₁ = 0): kendala 1 → y₂ = 3; kendala 2 → y₂ = 3 → **(0, 3)**
- Perpotongan: 2y₁ + y₂ = 3 dan y₁ + y₂ = 3 → kurangkan: y₁ = 0 → kembali ke (0, 3)
- (0, 0)

**Langkah 2 — Evaluasi Z**

| Titik | Z |
|---|---|
| (0, 0) | 0 |
| (1,5; 0) | 9 |
| (0, 3) | 12 |

Optimum **(0, 3), Z = 12**.

**Langkah 3 — Perbandingan**

| Kondisi | y₁ | y₂ | Z_maks |
|---|---|---|---|
| Sebelum (RHS₂ = 2) | 1 | 1 | 10 |
| Sesudah (RHS₂ = 3) | 0 | 3 | 12 |

**Langkah 4 — Analisis**
- Solusi berpindah dari (1,1) ke (0,3): **y₁ turun** (1 → 0), **y₂ naik** (1 → 3), **Z naik** 10 → 12.
- ΔZ = 12 − 10 = **2**; ΔRHS = 3 − 2 = **1**
- **Shadow price kendala 2 = 2**

**Interpretasi:** setiap penambahan 1 unit kapasitas kendala 2 menaikkan keuntungan maksimum sebesar 2 unit (selama basis tidak berubah). Sesuai koefisien s₂ = 2 di tabel final.

**Rentang validitas (tambahan):** dengan RHS = (3, b₂):
- y₁ = 3 − b₂ ≥ 0 → b₂ ≤ 3
- y₂ = −3 + 2b₂ ≥ 0 → b₂ ≥ 1,5

Shadow price 2 berlaku untuk **1,5 ≤ RHS₂ ≤ 3**. Pada RHS₂ = 3, kendala 2 tepat mulai menjadi **redundan** (kendala 1 sudah membatasi daerah layak); di atas 3, menambah kapasitas kendala 2 tidak menambah Z lagi (shadow price jadi 0).

> Kunci jawaban punya kalimat yang saling bertentangan ("memberi ruang bagi y₁ meningkat" lalu "y₁ menurun"). Versi yang benar: y₁ **menurun**, y₂ **meningkat**.

### Tambahan: sensitivitas koefisien fungsi tujuan (jika ditanya)

Dari basis optimal (y₁, y₂), shadow price sebagai fungsi koefisien (c₁, c₂):
```
w₁ = c₁ − c₂ ≥ 0
w₂ = 2c₂ − c₁ ≥ 0      → c₂ ≤ c₁ ≤ 2c₂
```
- Dengan c₂ = 4 tetap: **4 ≤ c₁ ≤ 8** (c₁ = 6 sekarang, di dalam rentang).
- Dengan c₁ = 6 tetap: **3 ≤ c₂ ≤ 6** (c₂ = 4 sekarang, di dalam rentang).
Selama koefisien berada di rentang ini, titik optimal tetap (1, 1); Z berubah, tetapi solusinya sama.

---
---

# BAGIAN C — OPTIMASI TANPA KENDALA (UNCONSTRAINED)

## C1. Konsep Dasar

**Unconstrained optimization:** fungsi tujuan f(x) **tidak punya kendala**.
```
min f(x),   x ∈ ℝⁿ
```
Setiap x ∈ ℝⁿ adalah solusi feasible. Titik kandidat dicari dari syarat perlu **∇f(x) = 0**. Ada dua jenis solusi: **minimum lokal** dan **minimum global**.

Contoh dari modul: f(x₁, x₂) = 3x₁² + 2x₂² + 4x₁x₂ − 6x₁ − 8x₂ + 6; tujuan: cari x* yang meminimalkan f.

## C2. Gradien

Gradien = vektor turunan parsial:

```
∇f(x) = [ ∂f/∂x₁ , ∂f/∂x₂ , … , ∂f/∂xₙ ]ᵀ
```
- Menunjukkan **arah pertumbuhan tercepat** fungsi.
- −∇f = arah **penurunan tercepat**.
- Syarat perlu titik optimal: **∇f(x) = 0**.

## C3. Hessian

Matriks Hessian H(x) = ∇²f(x) = matriks turunan kedua:

```
H(x) = [ ∂²f/∂x₁²      …   ∂²f/∂x₁∂xₙ ]
       [    ⋮          ⋱       ⋮      ]
       [ ∂²f/∂xₙ∂x₁   …   ∂²f/∂xₙ²   ]
```
(Simetris jika turunan kedua kontinu.)

## C4. Kriteria Keoptimalan

### Syarat perlu (Necessary)
- **FONC (First-Order Necessary Condition):** jika x* minimum lokal → **∇f(x*) = 0**.
- Titik yang memenuhi ∇f(xₛ) = 0 disebut **titik stasioner**. Bisa **minimum, maksimum, atau saddle point** → FONC belum cukup.
- **SONC (Second-Order Necessary Condition):** jika x* minimum lokal → H(x*) **semidefinit positif**.

### Syarat cukup (Sufficient) — menentukan jenis titik stasioner
Jika ∇f(x*) = 0 dan:
| Sifat Hessian H(x*) | Kesimpulan |
|---|---|
| **Definit positif** | **Minimum lokal (strict)** |
| **Definit negatif** | **Maksimum lokal (strict)** |
| **Tidak definit (indefinit)** | **Saddle point** |
| Semidefinit | Tidak bisa disimpulkan (perlu analisis lain) |

### Definisi kedefinitan matriks M (n × n), dengan q(x) = xᵀMx

| Jenis | Definisi | Nilai eigen |
|---|---|---|
| Definit positif (PD) | xᵀMx > 0 untuk semua x ≠ 0 | semua λ **> 0** |
| Semidefinit positif (PSD) | xᵀMx ≥ 0 untuk semua x | semua λ **≥ 0** |
| Definit negatif (ND) | xᵀMx < 0 untuk semua x ≠ 0 | semua λ **< 0** |
| Semidefinit negatif (NSD) | xᵀMx ≤ 0 untuk semua x | semua λ **≤ 0** |
| Tidak definit (indefinit) | ada x, y: xᵀMx > 0 dan yᵀMy < 0 | ada λ positif **dan** negatif |

**Pertanyaan modul: "Apa hubungan dengan nilai eigen?"** → jawaban di kolom kanan tabel di atas (untuk matriks simetris). 

### Cara praktis untuk matriks 2×2 simetris [[a, b], [b, c]]
- PD ⇔ a > 0 **dan** det = ac − b² > 0
- ND ⇔ a < 0 **dan** det > 0
- Indefinit ⇔ det < 0
- Alternatif: cek **nilai eigen** (det(M − λI) = 0) atau **minor utama** (uji Sylvester).

## C5. Contoh Mencari Titik Stasioner (modul, Contoh 2.5)

f(x₁, x₂) = 3x₁² + 2x₂² + 4x₁x₂ − 6x₁ − 8x₂ + 6

**Langkah 1 — Gradien**
```
∇f = [ 6x₁ + 4x₂ − 6 ]
     [ 4x₁ + 4x₂ − 8 ]
```
**Langkah 2 — Syarat perlu ∇f = 0**
```
6x₁ + 4x₂ = 6
4x₁ + 4x₂ = 8     (dikurangkan)
─────────────────
2x₁ = −2  → x₁ = −1
```
Substitusi: 4(−1) + 4x₂ = 8 → x₂ = 3. Titik stasioner **x\* = (−1, 3)**.

**Langkah 3 — Hessian:** H = [[6, 4], [4, 4]] (konstan). det = 24 − 16 = 8 > 0 dan a = 6 > 0 → **definit positif** → **minimum lokal** (bahkan global karena H PD di mana-mana ⇒ f convex).

**Nilai:** f(−1, 3) = 3 + 18 − 12 + 6 − 24 + 6 = **−3**.

## C6. Contoh Menentukan Kedefinitan Matriks (modul)

### Contoh A: M = [[2, −1], [−1, 2]]

**Cara pertama — lewat bentuk kuadratik q(x) = xᵀMx**
```
Mx = [ 2x₁ − x₂ ; −x₁ + 2x₂ ]
xᵀ(Mx) = x₁(2x₁ − x₂) + x₂(−x₁ + 2x₂)
       = 2x₁² − x₁x₂ − x₁x₂ + 2x₂²
       = 2x₁² − 2x₁x₂ + 2x₂²
       = 2(x₁² − x₁x₂ + x₂²)
```
Karena x₁² − x₁x₂ + x₂² = ½[(x₁ − x₂)² + x₁² + x₂²] ≥ 0, maka q(x) ≥ 0 untuk semua x. Nilai 0 hanya jika x₁ − x₂ = 0, x₁ = 0, x₂ = 0, yaitu x = 0. Jadi q(x) > 0 untuk x ≠ 0 → **definit positif**.

Aturan umum:
- Semua x ≠ 0 memberi hasil positif → definit positif.
- Ada x ≠ 0 yang memberi nol (sisanya ≥ 0) → semidefinit positif.
- Ada x yang memberi negatif dan ada yang positif → indefinit.

**Cara kedua — nilai eigen**
```
det(M − λI) = (2 − λ)² − 1 = λ² − 4λ + 3 = (λ − 1)(λ − 3) = 0
λ₁ = 1, λ₂ = 3
```
Semua eigenvalue > 0 → **definit positif**. ✓ (konsisten)

### Contoh B (modul)
- **[[−2, 0], [0, −1]]**: matriks diagonal → eigen = −2, −1 (semua < 0) → **definit negatif**. (Cek: q = −2x₁² − x₂² < 0 untuk x ≠ 0.)
- **[[8, −1], [−1, 1]]**: a = 8 > 0, det = 8 − 1 = 7 > 0 → **definit positif**. (Eigen: λ² − 9λ + 7 = 0 → λ = (9 ± √53)/2 ≈ 8,14 dan 0,86, keduanya positif.)

### Contoh C (modul): klasifikasi titik stasioner dengan Hessian

f(x) = ½x₁² + x₁x₂ + 2x₂² − 4x₁ − 4x₂ − x₂³

```
∇f = ( x₁ + x₂ − 4 ,  x₁ + 4x₂ − 4 − 3x₂² )ᵀ
∇²f = [ 1      1     ]
      [ 1   4 − 6x₂  ]
```
**Titik stasioner:** dari persamaan 1: x₁ = 4 − x₂. Substitusi ke persamaan 2:
(4 − x₂) + 4x₂ − 4 − 3x₂² = 0 → 3x₂ − 3x₂² = 0 → 3x₂(1 − x₂) = 0 → x₂ = 0 atau x₂ = 1.
Titik: **(4, 0)** dan **(3, 1)**.

| Titik | Hessian | Analisis | Jenis |
|---|---|---|---|
| (4, 0) | [[1, 1], [1, 4]] | a = 1 > 0, det = 4 − 1 = 3 > 0 | **Definit positif → minimum lokal** |
| (3, 1) | [[1, 1], [1, −2]] | det = −2 − 1 = −3 < 0 | **Tidak definit → saddle point** |

## C7. Fungsi Convex dan Non-Convex

**Definisi (Boyd, 2004):** f: ℝⁿ → ℝ convex jika
```
f(θx + (1−θ)y) ≤ θf(x) + (1−θ)f(y),   ∀ x, y ∈ ℝⁿ, θ ∈ [0, 1]
```
Artinya grafik fungsi berada **di bawah atau tepat pada garis lurus** yang menghubungkan dua titik di grafik.

**Sifat penting**
- Jika f convex, **setiap minimum lokal adalah minimum global**.
- Jika **Hessian ∇²f(x) ⪰ 0** (PSD) untuk semua x, maka f convex.
- **Non-convex:** tidak memenuhi syarat itu; bisa punya banyak minimum/maksimum lokal sehingga sulit menemukan optimum global.

### Contoh soal 1: f(x) = x² (convex)

1. Ambil sembarang x, y ∈ ℝ dan θ ∈ [0, 1].
2. Uji: f(θx + (1−θ)y) ≤ θf(x) + (1−θ)f(y).
3. Kiri: (θx + (1−θ)y)² = θ²x² + 2θ(1−θ)xy + (1−θ)²y².
4. Kanan: θx² + (1−θ)y².
5. Cek angka (modul): x = 0, y = 2, θ = 0,5 → kiri = (0 + 1)² = 1; kanan = 0 + 0,5(4) = 2 → 1 ≤ 2 ✓.
6. **Bukti umum (aljabar):**
   ```
   kanan − kiri = θx² + (1−θ)y² − (θx + (1−θ)y)²
                = θ(1−θ)x² + θ(1−θ)y² − 2θ(1−θ)xy
                = θ(1−θ)(x − y)²  ≥ 0
   ```
   Karena θ(1−θ) ≥ 0 dan (x−y)² ≥ 0, ketaksamaan benar untuk **semua** x, y, θ. → **f(x) = x² convex**.
7. **Cara cepat dengan Hessian:** f''(x) = 2 ≥ 0 → convex.

### Contoh soal 2: f(x) = −x² (bukan convex)

1. Ambil x = 0, y = 2, θ = 0,5.
2. Kiri: f(θx + (1−θ)y) = f(1) = −1.
3. Kanan: 0,5·f(0) + 0,5·f(2) = 0,5(0) + 0,5(−4) = −2.
4. Bandingkan: −1 ≤ −2 → **SALAH** (karena −1 > −2).

Jadi f(x) = −x² **bukan convex** (ia **concave**). Cara cepat: f''(x) = −2 < 0.

> Menunjukkan *satu* contoh yang melanggar sudah cukup untuk **membuktikan bukan convex**. Tetapi satu contoh yang memenuhi **tidak cukup** untuk membuktikan convex — harus bukti umum (seperti langkah 6) atau uji Hessian PSD.

## C8. Metode Steepest Descent (Gradient Descent)

Mulai dari tebakan awal x⁽⁰⁾. Iterasi:
```
x⁽ᵏ⁺¹⁾ = x⁽ᵏ⁾ − λ ∇f(x⁽ᵏ⁾)
```
Nilai λ (ukuran langkah) dari **line search**: minimalkan fungsi satu variabel
```
φ(λ) = f( x⁽ᵏ⁾ − λ ∇f(x⁽ᵏ⁾) )   →   φ'(λ) = 0
```

**Langkah kerja tiap iterasi**
1. Hitung ∇f(x⁽ᵏ⁾).
2. Bentuk titik parametrik x(λ) = x⁽ᵏ⁾ − λ∇f(x⁽ᵏ⁾).
3. Substitusi ke f → φ(λ).
4. Turunkan, φ'(λ) = 0 → λ.
5. Hitung x⁽ᵏ⁺¹⁾ dan f(x⁽ᵏ⁺¹⁾).

**Shortcut untuk fungsi kuadratik** f(x) = ½xᵀHx − bᵀx (Hessian konstan H) dengan g = ∇f(x⁽ᵏ⁾):
```
λ = (gᵀg) / (gᵀHg)
```
(Dipakai untuk mengecek hasil line search.)

### Contoh modul: min f(x₁, x₂) = x₁² − x₁x₂ + x₂², x⁽⁰⁾ = (1, ½)

f(x⁽⁰⁾) = 1 − ½ + ¼ = **3/4**. ∇f = (2x₁ − x₂, −x₁ + 2x₂). Hessian H = [[2, −1], [−1, 2]].

**Iterasi 1**
- ∇f(x⁽⁰⁾) = (2 − ½, −1 + 1) = **(3/2, 0)**
- x(λ) = (1 − (3/2)λ, ½)
- φ(λ) = (1 − 1,5λ)² − (1 − 1,5λ)(½) + (½)²
- φ'(λ) = 2(1 − 1,5λ)(−1,5) + ¾ = 0 → −3(1 − 1,5λ) + 0,75 = 0 → 1 − 1,5λ = 0,25 → **λ = ½**
- x⁽¹⁾ = (1 − ¾, ½) = **(¼, ½)**
- f(x⁽¹⁾) = 1/16 − 1/8 + 1/4 = **3/16**
- *Cek shortcut:* gᵀg = 2,25; gᵀHg = (1,5)(2)(1,5) = 4,5 → λ = 0,5 ✓

**Iterasi 2**
- ∇f(x⁽¹⁾) = (2(0,25) − 0,5, −0,25 + 2(0,5)) = **(0; 0,75)**
- x(λ) = (0,25; 0,5 − 0,75λ)
- φ(λ) = 9/16 λ² − 9/16 λ + 3/16
- φ'(λ) = 9/8 λ − 9/16 = 0 → **λ = ½**
- x⁽²⁾ = (0,25; 0,5 − 0,375) = **(0,25; 0,125)**
- f(x⁽²⁾) = 0,0625 − 0,03125 + 0,015625 = **0,046875**

**Iterasi 3**
- ∇f(x⁽²⁾) = (0,5 − 0,125; −0,25 + 0,25) = **(0,375; 0)**
- x(λ) = (0,25 − 0,375λ; 0,125)
- φ(λ) = 0,046875 − 0,140625λ + 0,140625λ²
- φ'(λ) = −0,140625 + 0,28125λ = 0 → **λ = 0,5**
- x⁽³⁾ = (0,25 − 0,1875; 0,125) = **(0,0625; 0,125)**
- f(x⁽³⁾) = 0,00390625 − 0,0078125 + 0,015625 = **0,01171875**

> ⚠️ Modul menulis f(x⁽³⁾) = 0,015625 — itu **keliru**. Nilai yang benar **0,01171875 (= 3/256)**. Pola turun: 0,75 → 0,1875 → 0,046875 → 0,0117 (setiap iterasi ÷4). Minimum sebenarnya di (0,0), f = 0.

### Kriteria penghentian iterasi
1. **Batas iterasi maksimum** (mis. 100 kali) — agar tidak berjalan terlalu lama.
2. **Norma gradien kecil:** ‖∇f(x⁽ᵏ⁾)‖ < ε (toleransi, mis. 10⁻⁶ atau 10⁻¹⁶).
3. **∇f(x⁽ᵏ⁾) = 0** tepat → titik stasioner (berharap minimum; cek Hessian).

## C9. Steepest Ascent

Arah menjadi **positif** (untuk **maksimasi**):
```
x⁽ᵏ⁺¹⁾ = x⁽ᵏ⁾ + λ ∇f(x⁽ᵏ⁾)
```
dan λ dicari dengan **memaksimalkan** φ(λ).

### Contoh modul: max f(x₁, x₂, x₃) = −x₁² − x₂² − x₃², x⁽⁰⁾ = (1, 1, 1)

1. ∇f = (−2x₁, −2x₂, −2x₃).
2. f(x⁽⁰⁾) = −3; ∇f(x⁽⁰⁾) = (−2, −2, −2).
3. Arah ascent d = ∇f = (−2, −2, −2).
4. x(λ) = (1, 1, 1) + λ(−2, −2, −2) = (1 − 2λ, 1 − 2λ, 1 − 2λ).
5. f(x(λ)) = −3(1 − 2λ)².
6. Maksimalkan: −3(1−2λ)² maksimum (= 0) saat 1 − 2λ = 0 → **λ = ½**.
7. x⁽¹⁾ = (0, 0, 0), f(x⁽¹⁾) = **0** (maksimum global; ∇f(0) = 0).

## C10. Metode Newton

Memakai Hessian, **tanpa λ**:
```
x⁽ᵏ⁺¹⁾ = x⁽ᵏ⁾ − H(x⁽ᵏ⁾)⁻¹ ∇f(x⁽ᵏ⁾)
```
Invers matriks 2×2: untuk H = [[a, b], [c, d]], H⁻¹ = (1/(ad − bc)) · [[d, −b], [−c, a]].

### Contoh modul: min f(x₁, x₂) = x₁ − x₂ + 2x₁² + 2x₁x₂ + x₂², x⁽⁰⁾ = (0, 0)

1. f(x⁽⁰⁾) = 0.
2. ∇f = (1 + 4x₁ + 2x₂, −1 + 2x₁ + 2x₂) → ∇f(x⁽⁰⁾) = **(1, −1)**.
3. H = [[4, 2], [2, 2]]; det = 8 − 4 = 4 → H⁻¹ = ¼[[2, −2], [−2, 4]] = **[[½, −½], [−½, 1]]**.
4. H⁻¹∇f = [[½, −½], [−½, 1]] · (1, −1)ᵀ = (½ + ½, −½ − 1) = **(1, −3/2)**.
5. x⁽¹⁾ = (0, 0) − (1, −3/2) = **(−1, 3/2)**.

**Cek:** ∇f(−1; 1,5) = (1 − 4 + 3, −1 − 2 + 3) = (0, 0) ✓ → titik stasioner; H PD (a = 4 > 0, det = 4 > 0) → minimum. Untuk fungsi **kuadratik**, Newton sampai optimum **dalam 1 iterasi**.

> Modul menulis "= (1, −1) = (−1, 3/2)" — bagian "(1, −1)" adalah typo; yang benar langkah antara adalah (1, −3/2).

---

## C11. LATIHAN SOAL 1 — Titik stasioner f(x, y) = x² + xy + y² − 2x − 3y

**Langkah 1 — Gradien**
```
∂f/∂x = 2x + y − 2
∂f/∂y = x + 2y − 3
```
**Langkah 2 — Syarat perlu ∇f = 0**
```
2x + y = 2   →  y = 2 − 2x
x + 2y = 3   →  x + 2(2 − 2x) = 3 → x + 4 − 4x = 3 → −3x = −1 → x = 1/3
              y = 2 − 2/3 = 4/3
```
**Titik stasioner: (1/3, 4/3).**

**Langkah 3 — Hessian**
```
fₓₓ = 2,  f_yy = 2,  fₓᵧ = 1   →   H = [[2, 1], [1, 2]]
```
**Langkah 4 — Klasifikasi**
- Minor: 2 > 0; det = 4 − 1 = 3 > 0 → **definit positif**.
- Eigen: λ² − 4λ + 3 = 0 → λ = 1, 3 (keduanya > 0) ✓.

**Kesimpulan:** (1/3, 4/3) adalah **minimum lokal (strict)**; karena H PD di semua titik (f convex), ia juga **minimum global**.

**Nilai minimum:** f(1/3, 4/3) = 1/9 + 4/9 + 16/9 − 2/3 − 4 = 21/9 − 6/9 − 36/9 = **−21/9 = −7/3 ≈ −2,333**.

---

## C12. LATIHAN SOAL 2 — Perbedaan FONC dan SONC (syarat perlu vs syarat cukup)

**FONC (First-Order Necessary Condition)**
- Jika x* minimum lokal dan f diferensiabel, maka **∇f(x\*) = 0**.
- Hanya syarat **perlu**: titik yang memenuhinya (titik stasioner) bisa minimum, maksimum, atau saddle.

**SONC (Second-Order Necessary Condition)**
- Jika x* minimum lokal, maka **∇f(x\*) = 0** dan **H(x\*) semidefinit positif**.
- Masih syarat **perlu**: dipakai untuk **menyingkirkan** kandidat (mis. Hessian punya eigen negatif → pasti bukan minimum).

**Syarat cukup (SOSC — Second-Order Sufficient Condition)** — yang di modul disebut "syarat cukup"
- Jika **∇f(x\*) = 0** dan **H(x\*) definit positif**, maka x* **minimum lokal strict**.
- Dipakai untuk **memastikan** jenis titik.

**Ringkasan perbedaan**

| | FONC | SONC | SOSC (syarat cukup) |
|---|---|---|---|
| Isi | ∇f(x*) = 0 | ∇f(x*) = 0 dan H(x*) ⪰ 0 | ∇f(x*) = 0 dan H(x*) ≻ 0 |
| Sifat | Perlu | Perlu | **Cukup** |
| Fungsi | Mencari kandidat | Menyaring kandidat | Menjamin minimum |

**Contoh yang menunjukkan perbedaannya**

1. **f(x) = x³** — di x = 0: f'(0) = 0 (FONC terpenuhi), f''(0) = 0 (SONC juga terpenuhi karena 0 ≥ 0), tetapi x = 0 **bukan** minimum maupun maksimum (titik belok; f negatif di kiri, positif di kanan). → Syarat perlu **tidak cukup**.
2. **f(x, y) = x² − y²** — di (0,0): ∇f = (2x, −2y) = 0 (FONC terpenuhi). H = diag(2, −2) **indefinit** → SONC **gagal** → (0,0) saddle point, bukan minimum.
3. **f(x) = x²** — f'(0) = 0 dan f''(0) = 2 > 0 → SOSC terpenuhi → x = 0 pasti minimum lokal strict.
4. **f(x) = x⁴** — x = 0 adalah minimum, tapi f''(0) = 0 (H hanya semidefinit) sehingga SOSC **tidak** terpenuhi. → Syarat cukup **tidak selalu perlu** (kegagalan SOSC tidak berarti bukan minimum).

---

## C13. LATIHAN SOAL 3 — Kedefinitan A = [[4, 2], [2, 3]]

### a) Dengan nilai eigen
```
det(A − λI) = (4 − λ)(3 − λ) − (2)(2)
            = 12 − 7λ + λ² − 4
            = λ² − 7λ + 8 = 0
λ = (7 ± √(49 − 32)) / 2 = (7 ± √17) / 2
λ₁ ≈ (7 + 4,123)/2 ≈ 5,562
λ₂ ≈ (7 − 4,123)/2 ≈ 1,438
```
Cek: λ₁ + λ₂ = 7 = trace ✓; λ₁ · λ₂ = 8 = det ✓.
Kedua eigenvalue **> 0** → **A definit positif**.

### b) Dengan xᵀAx
```
Ax = [4x₁ + 2x₂ ; 2x₁ + 3x₂]
xᵀAx = x₁(4x₁ + 2x₂) + x₂(2x₁ + 3x₂)
     = 4x₁² + 4x₁x₂ + 3x₂²
```
Lengkapi kuadrat: 4x₁² + 4x₁x₂ + x₂² + 2x₂² = **(2x₁ + x₂)² + 2x₂²** ≥ 0.

Sama dengan nol hanya jika **x₂ = 0** dan **2x₁ + x₂ = 0 → x₁ = 0**, yaitu x = 0. Jadi xᵀAx > 0 untuk semua x ≠ 0 → **A definit positif**. ✓

(Cek cepat: a = 4 > 0 dan det = 12 − 4 = 8 > 0 → PD.)

**Kesimpulan: A definit positif** (bukan semidefinit saja, bukan indefinit).

---

## C14. LATIHAN SOAL 4 — Satu iterasi Newton, f(x, y) = x² + xy + y² − 6x − 9y, (x₀, y₀) = (0, 0)

**Langkah 1 — Nilai awal:** f(0, 0) = 0.

**Langkah 2 — Gradien**
```
∇f = (2x + y − 6 , x + 2y − 9)
∇f(0, 0) = (−6, −9)
```
**Langkah 3 — Hessian & inversnya**
```
H = [[2, 1], [1, 2]],  det = 4 − 1 = 3
H⁻¹ = (1/3) [[2, −1], [−1, 2]]
```
**Langkah 4 — H⁻¹∇f**
```
H⁻¹ ∇f = (1/3) [ 2(−6) − 1(−9) ,  −1(−6) + 2(−9) ]
       = (1/3) [ −12 + 9 ,  6 − 18 ]
       = (1/3) [ −3 , −12 ] = (−1, −4)
```
**Langkah 5 — Update**
```
x⁽¹⁾ = x⁽⁰⁾ − H⁻¹∇f = (0, 0) − (−1, −4) = (1, 4)
```
**Cek:** ∇f(1, 4) = (2 + 4 − 6, 1 + 8 − 9) = (0, 0) ✓.
**Nilai:** f(1, 4) = 1 + 4 + 16 − 6 − 36 = **−21** (turun dari 0).

**Jawaban: x⁽¹⁾ = (1, 4), f = −21** — sudah titik minimum (fungsi kuadratik, H PD, Newton langsung konvergen dalam 1 iterasi).

---

## C15. LATIHAN SOAL 5 — Steepest Descent 1 iterasi, f(x, y) = x² + 2y² − 2x − 4y, (x₀, y₀) = (0, 0)

**Langkah 1 — Nilai awal:** f(0, 0) = 0.

**Langkah 2 — Gradien**
```
∇f = (2x − 2 , 4y − 4)
∇f(0, 0) = (−2, −4)
```
**Langkah 3 — Titik parametrik** (x − λ∇f)
```
x(λ) = (0 + 2λ , 0 + 4λ) = (2λ, 4λ)
```
**Langkah 4 — Substitusi ke f**
```
φ(λ) = (2λ)² + 2(4λ)² − 2(2λ) − 4(4λ)
     = 4λ² + 32λ² − 4λ − 16λ
     = 36λ² − 20λ
```
**Langkah 5 — Minimalkan φ(λ)**
```
φ'(λ) = 72λ − 20 = 0  →  λ = 20/72 = 5/18
```
(Cek shortcut: gᵀg = 4 + 16 = 20; H = diag(2, 4) → gᵀHg = 2(4) + 4(16) = 72 → λ = 20/72 ✓.)

**Langkah 6 — Titik baru**
```
x⁽¹⁾ = (2 · 5/18 , 4 · 5/18) = (5/9 , 10/9) ≈ (0,556 ; 1,111)
f(x⁽¹⁾) = 36(25/324) − 20(5/18) = 25/9 − 50/9 = −25/9 ≈ −2,778
```
**Jawaban: x⁽¹⁾ = (5/9, 10/9), f ≈ −2,778** (turun dari 0).

> Catatan: titik minimum sebenarnya adalah (1, 1) dengan f = −3 (dari ∇f = 0: x = 1, y = 1). Setelah 1 iterasi belum sampai: ∇f(x⁽¹⁾) = (−8/9, 4/9) ≠ 0, jadi iterasi bisa dilanjutkan.

---

## C16. LATIHAN SOAL 6 — Steepest Ascent, f(x, y) = 3x² + 2xy + y², titik awal (1, 1)

> **Perhatian pada soal:** tertulis "Steepest *Ascent* untuk mencari titik *minimum*". Ascent (naik) dipakai untuk **maksimum**, sedangkan minimum dicari dengan **descent**. Fungsi ini punya Hessian PD → ia **convex tanpa maksimum** (tak terbatas ke atas), dan minimumnya di (0, 0). Berikut dijelaskan kedua tafsir; yang menghasilkan jawaban bermakna untuk "mencari minimum" adalah arah **−∇f**.

**Langkah 1 — Nilai awal & gradien**
```
f(1, 1) = 3 + 2 + 1 = 6
∇f = (6x + 2y , 2x + 2y)  →  ∇f(1, 1) = (8, 4)
H = [[6, 2], [2, 2]], det = 12 − 4 = 8 > 0, 6 > 0 → PD (convex)
```

**Tafsir 1 — Literal steepest ascent: x = x⁽⁰⁾ + λ∇f = (1 + 8λ, 1 + 4λ)**
```
φ(λ) = 6 + 80λ + 272λ²   → terus naik tanpa batas untuk λ → ∞
```
Tidak ada λ yang memaksimalkan: fungsi **tak terbatas ke atas** (tidak punya maksimum). Maka ascent **tidak** memberi titik minimum — ia justru menjauhi minimum.

**Tafsir 2 — Untuk mencari MINIMUM (arah −∇f, steepest descent)**

Titik parametrik: x(λ) = (1 − 8λ , 1 − 4λ)
```
φ(λ) = 3(1 − 8λ)² + 2(1 − 8λ)(1 − 4λ) + (1 − 4λ)²
     = (3 − 48λ + 192λ²) + (2 − 24λ + 64λ²) + (1 − 8λ + 16λ²)
     = 6 − 80λ + 272λ²
φ'(λ) = −80 + 544λ = 0  →  λ = 80/544 = 5/34 ≈ 0,147
```
(Cek: gᵀg = 80; Hg = (56, 24); gᵀHg = 8·56 + 4·24 = 544 → λ = 80/544 ✓.)

```
x⁽¹⁾ = (1 − 40/34 , 1 − 20/34) = (−3/17 , 7/17) ≈ (−0,176 ; 0,412)
f(x⁽¹⁾) = 6 − 80(5/34) + 272(25/1156) = 6 − 100/17 = 2/17 ≈ 0,118
```
Cek langsung: 3(9/289) + 2(−21/289) + 49/289 = 34/289 = 2/17 ✓.

**Iterasi 2 (opsional)**
```
∇f(x⁽¹⁾) = (−4/17 , 8/17);   gᵀg = 80/289;   Hg = (−8/17 , 8/17);   gᵀHg = 96/289
λ = 80/96 = 5/6
x⁽²⁾ = (1/51 , 1/51) ≈ (0,0196 ; 0,0196),   f(x⁽²⁾) = 6/2601 ≈ 0,0023
```
Terlihat barisan mendekati minimum **(0, 0)**, f = 0.

**Jawaban:** minimum fungsi di **(0, 0)** dengan f = 0 (dari ∇f = 0 → 6x + 2y = 0 dan 2x + 2y = 0 → x = y = 0). Setelah 1 iterasi memakai arah −∇f: **x⁽¹⁾ = (−3/17, 7/17), f = 2/17**. Jika dipaksa murni *ascent*, nilai f membesar tanpa batas (tidak ada maksimum).

---
---

# BAGIAN D — RANGKUMAN CEPAT & CATATAN KOREKSI

## D1. Cheat sheet rumus

**LP & Grafik**
- Bentuk standar: min cᵀx, Ax = b, x ≥ 0, b ≥ 0. ≤ → +slack; ≥ → −surplus.
- Optimum LP ada di **titik pojok**. Unbounded jika Z bisa → ∞ di daerah tak terbatas.

**Simpleks (maks)**
- Kolom kunci: koef. baris Z **paling negatif**. Baris kunci: **NK ÷ kolom kunci**, positif terkecil.
- Baris kunci baru = baris kunci ÷ angka kunci. Baris lain: **baru = lama − (koef. kolom kunci × baris kunci baru)**.
- Berhenti saat baris Z **tidak ada yang negatif**.
- Shadow price = koef. slack di baris Z (akhir) = ΔZ/ΔRHS.

**Unconstrained**
- ∇f = 0 → titik stasioner. Hessian PD → min; ND → max; indefinit → saddle.
- Eigen: PD (semua > 0), PSD (≥ 0), ND (< 0), NSD (≤ 0), indefinit (campur).
- Convex ⇔ Hessian PSD di mana-mana; convex ⇒ min lokal = min global.
- Steepest descent: x⁺ = x − λ∇f; λ dari φ'(λ) = 0 (kuadratik: λ = gᵀg / gᵀHg).
- Steepest ascent: x⁺ = x + λ∇f (maksimasi).
- Newton: x⁺ = x − H⁻¹∇f (kuadratik → 1 langkah).

## D2. Daftar kesalahan/typo yang ditemukan di modul (hati-hati di kuis)

| # | Lokasi | Masalah | Yang benar |
|---|---|---|---|
| 1 | Modul 5, Steepest Descent iterasi 3 | f(x⁽³⁾) ditulis 0,015625 | **0,01171875** (3/256) |
| 2 | Modul 5, Newton | "= (1, −1) = (−1, 3/2)" | Langkah antara (1, −3/2), hasil x⁽¹⁾ = (−1, 3/2) |
| 3 | Modul 5, bukti convex x² | Menyebut "Cauchy-Schwarz" dan hanya cek angka x = 0, y = 2 | Bukti umum: selisih kanan − kiri = θ(1−θ)(x−y)² ≥ 0 (atau f'' = 2 ≥ 0) |
| 4 | Modul 5, soal 6 | "Steepest Ascent … mencari titik minimum" | Minimum dicari dengan **descent** (−∇f); ascent untuk maksimum |
| 5 | Modul 5, soal 2 | SONC disebut "syarat cukup" | SONC = syarat **perlu**; syarat **cukup** = SOSC (H definit positif) |
| 6 | Modul 4, hasil akhir contoh | "x₁ = 5/6, x₁ = 5" | x₁ = 5/6, **x₂ = 5**, Z = 27½ |
| 7 | Modul 4, soal 2b | "RHS kendala 1 berubah jadi 3 … kendala 1 tetap" | Maksudnya **RHS kendala 2** → 3 (y₁ + y₂ ≤ 3), kendala 1 tetap |
| 8 | Modul 4, kunci 2b | Kalimat "y₁ meningkat" lalu "y₁ menurun" | y₁ **menurun** (1→0), y₂ **naik** (1→3) |
| 9 | Modul 3, kunci soal 1 | Tabel titik pojok memuat titik dengan koordinat negatif | Titik layak hanya (0,0), (0,1), (1,0); kesimpulan unbounded tetap benar |
| 10 | Modul 3, kunci soal 2 (Excel) | Baris Market limit tertulis (−1, −1) di salah satu tabel | Yang benar **(−1, 1)** |
| 11 | Modul 3, kunci soal 2 (Python) | `-1*x1 - 1*x2 <= -1` | Yang benar `-1*x1 + 1*x2 <= 1` |
| 12 | Modul 3, soal 3 | Soal kosong | — |

## D3. Strategi cepat saat kuis
1. **Grafik:** selalu cek tiap titik pojok memenuhi **semua** kendala + non-negatif sebelum menghitung Z.
2. **Simpleks:** tulis tabel rapi; hitung indeks hanya untuk kolom kunci **> 0**; cek akhir dengan substitusi ke fungsi tujuan & kendala.
3. **Sensitivitas:** shadow price = ΔZ/ΔRHS; sebutkan "selama basis tidak berubah".
4. **Stasioner:** ∇f = 0 → Hessian → klasifikasi (gunakan det & trace untuk 2×2).
5. **Steepest descent kuadratik:** gunakan λ = gᵀg/gᵀHg untuk mengecek line search.
6. **Newton fungsi kuadratik:** hasil 1 iterasi = titik optimum; cek ∇f = 0 sebagai verifikasi.

Semoga sukses kuisnya! 🍀

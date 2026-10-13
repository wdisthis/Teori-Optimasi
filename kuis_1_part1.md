# Panduan Belajar Lengkap — Teori Optimasi (SD25-20006)
**Optimasi Numeris Satu Dimensi: Golden Section Search, Bisection, Interval Halving, Exhaustive Search**

> Sumber: Modul Tutorial 1 (dengan kunci jawaban) dan Modul Tutorial 2.
> Semua angka dalam panduan ini sudah dihitung ulang dengan program untuk memastikan benar.
> Format angka memakai koma desimal seperti di modul.

---

## DAFTAR ISI

1. Konsep dasar optimasi
2. Jenis-jenis optimasi
3. Optimasi satu variabel: titik stasioner, uji turunan kedua, lokal vs global
4. Mengapa perlu metode numeris
5. Exhaustive Search (langkah + soal)
6. Interval Halving (langkah + soal)
7. Golden Section Search (teori, langkah, contoh, soal)
8. Metode Bisection (teori, langkah, contoh)
9. Perbandingan metode
10. Pemodelan optimasi (formulasi soal cerita)
11. Jawaban lengkap Latihan Tutorial 2 (G.1, G.2, G.3)
12. Catatan koreksi pada modul (hati-hati saat kuis)
13. Cheat sheet rumus dan kesalahan umum

---

# 1. KONSEP DASAR OPTIMASI

**Optimasi** = proses mencari kondisi paling menguntungkan: **memaksimalkan** (misal keuntungan) atau **meminimalkan** (misal biaya, error).

Tiga unsur yang selalu ada:

| Unsur | Arti | Contoh |
|---|---|---|
| **Fungsi objektif / fungsi tujuan** f(x) | Fungsi yang dimaksimumkan / diminimumkan | Keuntungan, biaya, MSE |
| **Variabel keputusan** x | Besaran yang boleh kita pilih | Jumlah produksi |
| **Kendala** | Batasan yang harus dipenuhi | a ≤ x ≤ b, bahan baku terbatas |

**Hubungan minimasi dan maksimasi (penting!)**
Jika f(x) minimum di x*, maka −f(x) maksimum di x* yang sama. Jadi:

```
maksimumkan f(x)   ≡   minimumkan −f(x)
```

Karena itu cukup satu versi algoritma (minimasi); untuk maksimasi bisa memakai −f(x) atau membalik arah perbandingan.

**Bentuk umum masalah optimasi (Rao, pers. 1.1):**

```
Cari X = (x1, x2, ..., xn)  yang meminimumkan f(X)
dengan kendala:
    g_j(X) ≤ 0,   j = 1, 2, ..., m     (kendala pertidaksamaan / inequality)
    l_j(X) = 0,   j = 1, 2, ..., p     (kendala persamaan / equality)
```

- X = vektor desain (design vector) = variabel keputusan
- f(X) = fungsi tujuan
- g_j = kendala pertidaksamaan, l_j = kendala persamaan

Untuk satu variabel: `maks_x / min_x f(x)` dengan `a ≤ x ≤ b`.

---

# 2. JENIS-JENIS OPTIMASI

## 2.1 Tanpa kendala vs dengan kendala
- **Unconstrained**: fungsi tujuan bergantung pada variabel real tanpa batas nilai. Contoh: regresi linear. Masalah terkendala bisa dibuat "seolah tak terkendala" memakai **fungsi penalti**.
- **Constrained**: ada batasan (sumber daya, biaya, waktu, kapasitas). Cara penyelesaian: **Substitusi** (kendala dimasukkan ke fungsi tujuan) atau **Lagrange** (pengali Lagrange).

## 2.2 Program linear vs nonlinear
- **Program linear (LP)**: fungsi tujuan **dan** semua kendala linear.
  ```
  minimize  c1x1 + c2x2 + ... + cnxn
  subject to a11x1 + ... + a1nxn = b1
             ...
             x1 ≥ 0, ..., xn ≥ 0
  ```
- **Program nonlinear (NLP)**: fungsi tujuan nonlinear, kendala nonlinear atau linear.
  Contoh: struktur mekanis (jembatan, rangka batang), fungsi produksi, kurva biaya.

## 2.3 Kontinu, diskrit, campuran
- **Kontinu**: variabel bisa bernilai tak terhingga banyak (misal antara −2 dan 2).
  - Tujuan + kendala linear → pemrograman linear
  - Ada yang nonlinear → pemrograman nonlinear
- **Diskrit**: sebagian/semua variabel dari himpunan diskrit:
  - **Bilangan bulat linear**: variabel integer, tujuan dan kendala linear
  - **Bilangan bulat nonlinear**: integer, ada yang nonlinear
  - **Bilangan bulat biner**: x ∈ {0, 1}, untuk keputusan ya/tidak, pilih/tidak, aktif/nonaktif
- **Campuran**: gabungan variabel kontinu dan diskrit.

## 2.4 Contoh optimasi di Sains Data

| Kasus | Fungsi tujuan | Kendala |
|---|---|---|
| **Regresi linear** (prediksi harga rumah) | min Σ (yᵢ − ŷᵢ)² (MSE) | Biasanya tidak ada (unconstrained) |
| **K-Means** (kelompok pelanggan) | min Σᵢ Σₖ ‖xᵢ − μₖ‖² | Tiap data hanya masuk satu cluster |
| **Training Neural Network** (klasifikasi gambar) | Loss function (misal cross-entropy), diminimalkan dengan SGD/Adam | learning rate, kapasitas memori, regularisasi dropout |

---

# 3. OPTIMASI SATU VARIABEL

## 3.1 Definisi
Fungsi satu variabel memetakan x ∈ ℝ ke f(x). Dalam optimasi disebut fungsi objektif. Tujuan: cari x* yang meminimumkan/memaksimumkan f pada domain tertentu.
Kasus satu dimensi adalah dasar optimasi multivariabel, karena hampir semua algoritma kompleks memakai **line search** (pencarian sepanjang garis) yang pada dasarnya optimasi satu variabel.

Contoh: f(x) = x² + 4x + 5 (domain seluruh bilangan real).
- f'(x) = 2x + 4 = 0 → x* = −2
- f''(x) = 2 > 0 → minimum, f(−2) = 4 − 8 + 5 = 1

## 3.2 Titik stasioner (kondisi perlu)
```
f'(x*) = 0
```
Titik ini hanya **kandidat** optimum. Bisa juga titik infleksi.

## 3.3 Uji turunan kedua
| Kondisi | Kesimpulan |
|---|---|
| f''(x*) > 0 | minimum lokal |
| f''(x*) < 0 | maksimum lokal |
| f''(x*) = 0 | tidak bisa disimpulkan; pakai turunan orde lebih tinggi atau uji grafik |

## 3.4 Optimum lokal dan global
- **Lokal**: terbaik di lingkungan kecil sekitar x*.
- **Global**: terbaik di seluruh domain.

**Contoh (Rao, 2009):** f(x) = x³ − 3x² + 2
1. f'(x) = 3x² − 6x = 3x(x − 2) → titik stasioner x = 0 dan x = 2
2. f''(x) = 6x − 6
3. f''(0) = −6 < 0 → **maksimum lokal**, f(0) = 2
4. f''(2) = 6 > 0 → **minimum lokal**, f(2) = 8 − 12 + 2 = −2

---

# 4. MENGAPA PERLU METODE NUMERIS (DIRECT SEARCH)

Cara analitik (f'(x) = 0) hanya cocok jika fungsi mudah diturunkan dan akar turunannya mudah dicari. Banyak fungsi nyata: turunannya rumit, akarnya tak punya bentuk tertutup, atau fungsinya hanya berupa data hasil pengukuran. Maka dipakai **metode numeris yang hanya memakai nilai fungsi, tanpa turunan** (Fletcher: *search methods without derivatives*, disebut juga **direct search**).

Metode yang dibahas: Exhaustive Search, Interval Halving, Golden Section Search (mencari optimum), dan Bisection (mencari akar).

---

# 5. EXHAUSTIVE SEARCH (PENCARIAN LENGKAP)

## 5.1 Langkah-langkah (Fletcher, 2013)
1. Tentukan interval pencarian [a, b] dan jumlah pembagi n.
2. Hitung lebar langkah: **Δx = (b − a) / n**
3. Bentuk titik uji: x₀ = a, x₁ = a + Δx, x₂ = a + 2Δx, ..., xₙ = b. Rumus umum: **xᵢ = a + i·Δx**, i = 0, 1, ..., n. (Dengan n pembagi akan ada **n + 1 titik**.)
4. Hitung f(xᵢ) untuk semua titik.
5. Bandingkan:
   - Minimasi: f(x*) = min f(xᵢ)
   - Maksimasi: f(x*) = max f(xᵢ)
6. Pilih xᵢ terbaik sebagai x*.
7. Jika akurasi kurang: **tambah jumlah titik** atau **persempit interval** di sekitar x* lalu ulangi.

## 5.2 Interval ketidakpastian (menurut kunci Soal 6, Rao pers. 5.2)
Minimum sejati berada di sekitar titik terbaik, yaitu di antara tetangga kiri dan kanannya: **[xᵢ₋₁, xᵢ₊₁]**, lebar = 2Δx.
Rumus: **Lₙ = 2/(n+1) · L₀**, dengan n = banyak titik uji interior (di luar a dan b).
Untuk 20 pembagi ada 19 titik interior sehingga Lₙ = 2/20 × 10 = 1,0.

## 5.3 Kelebihan dan kekurangan
| Kelebihan | Kekurangan |
|---|---|
| Paling sederhana | Boros perhitungan |
| Cocok untuk fungsi diskrit | Akurasi bergantung pada jumlah titik |
| Tidak perlu turunan | Kurang efisien untuk interval lebar |

## 5.4 SOAL 3 — Exhaustive Search (20 poin)
**f(x) = (x − 3,5)² + 2 pada [0, 8], n = 8 (titik x = 0, 1, ..., 8)**

**Langkah 1.** [a, b] = [0, 8], n = 8
**Langkah 2.** Δx = (8 − 0)/8 = 1
**Langkah 3.** Titik uji xᵢ = 0 + i·1 → x = 0, 1, 2, 3, 4, 5, 6, 7, 8
**Langkah 4.** Evaluasi fungsi:

| x | (x − 3,5)² | f(x) |
|---|---|---|
| 0 | 12,25 | 14,25 |
| 1 | 6,25 | 8,25 |
| 2 | 2,25 | 4,25 |
| 3 | 0,25 | **2,25** |
| 4 | 0,25 | **2,25** |
| 5 | 2,25 | 4,25 |
| 6 | 6,25 | 8,25 |
| 7 | 12,25 | 14,25 |
| 8 | 20,25 | 22,25 |

**Langkah 5.** Nilai terkecil = 2,25, muncul **dua kali** di x = 3 dan x = 4 (fungsi simetris terhadap 3,5).
**Kesimpulan:** x* ≈ 3 atau 4, f(x*) ≈ 2,25.
**Verifikasi analitik:** f'(x) = 2(x − 3,5) = 0 → x* = 3,5, f = 2. Minimum sejati berada di antara kedua titik terbaik, karena 3,5 bukan titik uji.

## 5.5 SOAL 6 — Exhaustive Search (15 poin)
**f(x) = x³ − 12x² + 36x + 10 (biaya, x dalam ratusan unit), interval [0, 10], N = 20**

**Langkah 1.** Δx = (10 − 0)/20 = 0,5. Titik uji xᵢ = 0 + i·0,5, i = 0..20 → **21 titik evaluasi**: 0; 0,5; 1; 1,5; ...; 10.

**Langkah 2.** Hitung f(x):

| No | x | Perhitungan x³ − 12x² + 36x + 10 | f(x) |
|---|---|---|---|
| 1 | 0 | 0 − 0 + 0 + 10 | 10 |
| 2 | 0,5 | 0,125 − 3 + 18 + 10 | 25,125 |
| 3 | 1 | 1 − 12 + 36 + 10 | 35 |
| 4 | 1,5 | 3,375 − 27 + 54 + 10 | 40,375 |
| 5 | 2 | 8 − 48 + 72 + 10 | 42 |
| 6 | 2,5 | 15,625 − 75 + 90 + 10 | 40,625 |
| 7 | 3 | 27 − 108 + 108 + 10 | 37 |
| 8 | 3,5 | 42,875 − 147 + 126 + 10 | 31,875 |
| 9 | 4 | 64 − 192 + 144 + 10 | 26 |
| 10 | 4,5 | 91,125 − 243 + 162 + 10 | 20,125 |
| 11 | 5 | 125 − 300 + 180 + 10 | 15 |
| 12 | 5,5 | 166,375 − 363 + 198 + 10 | 11,375 |
| 13 | 6 | 216 − 432 + 216 + 10 | **10** |
| 14 | 6,5 | 274,625 − 507 + 234 + 10 | 11,625 |
| 15 | 7 | 343 − 588 + 252 + 10 | 17 |
| 16 | 7,5 | 421,875 − 675 + 270 + 10 | 26,875 |
| 17 | 8 | 512 − 768 + 288 + 10 | 42 |
| 18 | 8,5 | 614,125 − 867 + 306 + 10 | 63,125 |
| 19 | 9 | 729 − 972 + 324 + 10 | 91 |
| 20 | 9,5 | 857,375 − 1083 + 342 + 10 | 126,375 |
| 21 | 10 | 1000 − 1200 + 360 + 10 | 170 |

**Langkah 3.** Nilai terkecil f_min = **10**, muncul dua kali: x = 0 dan x = 6. Keduanya benar-benar sama (bukan pembulatan): f(0) = 10 dan f(6) = 216 − 432 + 216 + 10 = 10.

**Langkah 4. Menafsirkan.**
- x = 0 berarti tidak berproduksi. Biaya 10 di situ adalah **biaya tetap** yang tetap keluar. Secara matematis titik minimum, tetapi tidak menjawab persoalan perusahaan.
- Titik yang bermakna: **x* = 6 → 600 unit, biaya minimum f(x*) = 10**.

**Langkah 5. Interval ketidakpastian:** Lₙ = 2/(n+1) · L₀ = 2/20 × 10 = 1,0. Minimum sejati pasti berada di **[5,5 ; 6,5]** (tetangga kiri dan kanan x = 6).

**Langkah 6. Verifikasi analitik:**
- f'(x) = 3x² − 24x + 36 = 3(x − 2)(x − 6) = 0 → x = 2 dan x = 6
- f''(x) = 6x − 24
- f''(2) = −12 < 0 → maksimum lokal, f(2) = 42
- f''(6) = +12 > 0 → **minimum lokal**, f(6) = 10

Exhaustive search menemukan x = 6 tepat tanpa galat, karena 6 kebetulan termasuk titik uji.

---

# 6. INTERVAL HALVING (BAGI DUA INTERVAL)

## 6.1 Ide
Tiap iterasi memakai **tiga titik** yang membagi interval [a, b] menjadi empat bagian sama panjang, lalu membuang separuh interval. Panjang interval **menjadi tepat setengahnya** tiap iterasi (karena itu disebut *halving*). Syarat: fungsi unimodal pada interval.

## 6.2 Langkah-langkah (Rao, 2009)
Untuk **minimasi**:

1. Ambil interval awal [a, b].
2. Hitung titik tengah: **x₀ = (a + b)/2**
3. Hitung dua titik uji:
   - **x₁ = (a + x₀)/2**
   - **x₂ = (x₀ + b)/2**
4. Hitung f₁ = f(x₁), f₀ = f(x₀), f₂ = f(x₂).
5. Bandingkan, ada tiga kasus:

| Kasus | Kondisi | Yang dibuang | Interval baru | Titik tengah baru |
|---|---|---|---|---|
| **1** | f₁ < f₀ < f₂ | (x₀, b] | [a, x₀] | x₁ lama |
| **2** | f₂ < f₀ < f₁ | [a, x₀) | [x₀, b] | x₂ lama |
| **3** | f₁ > f₀ dan f₂ > f₀ | [a, x₁) dan (x₂, b] | [x₁, x₂] | x₀ tetap |

6. Hitung panjang interval baru **L = b − a**.
   - Jika **L ≤ ε** → berhenti. Jika **L > ε** → ulangi dari langkah 2.
7. Hasil akhir: **x\* = titik tengah interval terakhir**, f(x\*) = nilai fungsinya.

**Tips efisiensi:** titik tengah baru sama dengan salah satu titik lama (lihat kolom terakhir), jadi nilai fungsinya tidak perlu dihitung ulang. Hanya 2 evaluasi baru per iterasi (iterasi pertama 3 evaluasi). Untuk maksimasi, balik arah perbandingannya.

| Kelebihan | Kekurangan |
|---|---|
| Tidak perlu turunan | Konvergensi lambat |
| Mudah dipahami dan diimplementasikan | Kurang efisien untuk interval lebar |

## 6.2b Estimasi cepat
Panjang awal L₀ = b − a. Setelah k iterasi, panjang = L₀ / 2ᵏ. Berhenti saat L ≤ ε.
Soal 5: 6 → 3 → 1,5 → 0,75 → 0,375 (4 iterasi). Soal 4: 6 → 3 → 1,5 → 0,75 → 0,375 (4 iterasi).

## 6.3 SOAL 4 — Interval Halving (10 poin)
**f(x) = (x − 4)² + 2, interval [2, 8], ε = 0,5**

**Iterasi 1**
1. [a, b] = [2, 8]
2. x₀ = (2 + 8)/2 = 5
3. x₁ = (2 + 5)/2 = 3,5 ; x₂ = (5 + 8)/2 = 6,5
4. f(3,5) = (3,5 − 4)² + 2 = 0,25 + 2 = 2,25
   f(5) = (5 − 4)² + 2 = 1 + 2 = 3
   f(6,5) = (6,5 − 4)² + 2 = 6,25 + 2 = 8,25
5. f₁ < f₀ < f₂ (2,25 < 3 < 8,25) → **Kasus 1**, buang (x₀, b]. Interval baru [a, x₀] = **[2, 5]**
6. L = 5 − 2 = 3 > 0,5 → lanjut

**Iterasi 2**
1. [a, b] = [2, 5]
2. x₀ = (2 + 5)/2 = 3,5
3. x₁ = (2 + 3,5)/2 = 2,75 ; x₂ = (3,5 + 5)/2 = 4,25
4. f(2,75) = (2,75 − 4)² + 2 = 1,5625 + 2 = 3,5625
   f(3,5) = 2,25
   f(4,25) = (4,25 − 4)² + 2 = 0,0625 + 2 = 2,0625
5. f₂ < f₀ < f₁ (2,0625 < 2,25 < 3,5625) → **Kasus 2**, buang [a, x₀). Interval baru [x₀, b] = **[3,5 ; 5]**
6. L = 5 − 3,5 = 1,5 > 0,5 → lanjut

**Iterasi 3**
1. [a, b] = [3,5 ; 5]
2. x₀ = (3,5 + 5)/2 = 4,25
3. x₁ = (3,5 + 4,25)/2 = 3,875 ; x₂ = (4,25 + 5)/2 = 4,625
4. f(3,875) = (3,875 − 4)² + 2 = 0,015625 + 2 = 2,015625
   f(4,25) = 0,0625 + 2 = 2,0625
   f(4,625) = (4,625 − 4)² + 2 = 0,390625 + 2 = 2,390625
5. f₁ < f₀ < f₂ (2,015625 < 2,0625 < 2,390625) → **Kasus 1**. Interval baru [a, x₀] = **[3,5 ; 4,25]**
6. L = 4,25 − 3,5 = 0,75 > 0,5 → lanjut

**Iterasi 4**
1. [a, b] = [3,5 ; 4,25]
2. x₀ = (3,5 + 4,25)/2 = 3,875
3. x₁ = (3,5 + 3,875)/2 = 3,6875 ; x₂ = (3,875 + 4,25)/2 = 4,0625
4. f(3,6875) = (3,6875 − 4)² + 2 = 0,09765625 + 2 = 2,09765625
   f(3,875) = 2,015625
   f(4,0625) = (4,0625 − 4)² + 2 = 0,00390625 + 2 = 2,00390625
5. f₂ < f₀ < f₁ (2,00390625 < 2,015625 < 2,09765625) → **Kasus 2**. Interval baru [x₀, b] = **[3,875 ; 4,25]**
6. L = 4,25 − 3,875 = 0,375 ≤ 0,5 → **BERHENTI**

**Hasil akhir:**
x\* = (3,875 + 4,25)/2 = **4,0625**, f(x\*) = (4,0625 − 4)² + 2 = **2,00390625**
Verifikasi: minimum sejati x = 4, f = 2.

| Iter | Interval awal | x₀ | x₁ | x₂ | f₁ | f₀ | f₂ | Kasus | Interval baru | L |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | [2 ; 8] | 5 | 3,5 | 6,5 | 2,25 | 3 | 8,25 | 1 | [2 ; 5] | 3 |
| 2 | [2 ; 5] | 3,5 | 2,75 | 4,25 | 3,5625 | 2,25 | 2,0625 | 2 | [3,5 ; 5] | 1,5 |
| 3 | [3,5 ; 5] | 4,25 | 3,875 | 4,625 | 2,015625 | 2,0625 | 2,390625 | 1 | [3,5 ; 4,25] | 0,75 |
| 4 | [3,5 ; 4,25] | 3,875 | 3,6875 | 4,0625 | 2,09766 | 2,015625 | 2,00391 | 2 | [3,875 ; 4,25] | 0,375 |

## 6.4 SOAL 5 — Interval Halving (15 poin)
**f(x) = (x − 3)² + 2, interval [0, 6], ε = 0,5**

**Iterasi 1**
1. [a, b] = [0, 6]
2. x₀ = (0 + 6)/2 = 3
3. x₁ = (0 + 3)/2 = 1,5 ; x₂ = (3 + 6)/2 = 4,5
4. f(1,5) = (1,5 − 3)² + 2 = 2,25 + 2 = 4,25
   f(3) = 0 + 2 = 2
   f(4,5) = (4,5 − 3)² + 2 = 2,25 + 2 = 4,25
5. f₁ > f₀ dan f₂ > f₀ (4,25 > 2 dan 4,25 > 2) → **Kasus 3**, kedua tepi dibuang. Interval baru [x₁, x₂] = **[1,5 ; 4,5]**
6. L = 4,5 − 1,5 = 3 > 0,5 → lanjut

**Iterasi 2**
1. [a, b] = [1,5 ; 4,5]
2. x₀ = 3
3. x₁ = (1,5 + 3)/2 = 2,25 ; x₂ = (3 + 4,5)/2 = 3,75
4. f(2,25) = (2,25 − 3)² + 2 = 0,5625 + 2 = 2,5625
   f(3) = 2 (sudah diketahui)
   f(3,75) = (3,75 − 3)² + 2 = 2,5625
5. Kasus 3. Interval baru [x₁, x₂] = **[2,25 ; 3,75]**
6. L = 1,5 > 0,5 → lanjut

**Iterasi 3**
1. [a, b] = [2,25 ; 3,75]
2. x₀ = 3
3. x₁ = (2,25 + 3)/2 = 2,625 ; x₂ = (3 + 3,75)/2 = 3,375
4. f(2,625) = (2,625 − 3)² + 2 = 0,140625 + 2 = 2,140625
   f(3) = 2
   f(3,375) = (3,375 − 3)² + 2 = 2,140625
5. Kasus 3. Interval baru = **[2,625 ; 3,375]**
6. L = 0,75 > 0,5 → lanjut

**Iterasi 4**
1. [a, b] = [2,625 ; 3,375]
2. x₀ = 3
3. x₁ = (2,625 + 3)/2 = 2,8125 ; x₂ = (3 + 3,375)/2 = 3,1875
4. f(2,8125) = (2,8125 − 3)² + 2 = 0,03515625 + 2 = 2,03515625
   f(3) = 2
   f(3,1875) = (3,1875 − 3)² + 2 = 2,03515625
5. Kasus 3. Interval baru = **[2,8125 ; 3,1875]**
6. L = 3,1875 − 2,8125 = 0,375 ≤ 0,5 → **BERHENTI**

**Hasil akhir:** x\* = (2,8125 + 3,1875)/2 = **3**, f(x\*) = (3 − 3)² + 2 = **2** (sama dengan nilai eksak).

---

# 7. GOLDEN SECTION SEARCH (GSS)

## 7.1 Konsep
- Mencari **titik optimum (minimum/maksimum)** dengan mempersempit interval berulang kali.
- Tiap iterasi ada **dua titik uji** di dalam interval. Bagian yang pasti tak memuat optimum dibuang.
- **Syarat: fungsi UNIMODAL** pada interval awal (hanya satu titik ekstrem). Tanpa ini aturan pembuangan tidak sah dan optimum bisa ikut terbuang.
- Hanya butuh nilai fungsi (tanpa turunan).

## 7.2 Turunan golden ratio
Golden section: ruas garis dibagi sehingga (seluruh ruas : bagian besar) = (bagian besar : bagian kecil).

```
(a + b)/a = a/b = φ
```
Tetapkan bagian kecil = 1, bagian besar = φ:
```
(φ + 1)/φ = φ/1   →   φ² = φ + 1   →   φ² − φ − 1 = 0
φ = (1 ± √5)/2     (rumus abc)
```
Akar negatif ditolak karena φ adalah perbandingan panjang. Jadi **φ = (1 + √5)/2 = 1,618**.

| Besaran | Nilai eksak | Hampiran | Peran |
|---|---|---|---|
| φ | (1 + √5)/2 | 1,6180 | Golden ratio; tidak dipakai langsung di rumus |
| **1/φ** | (√5 − 1)/2 | **0,6180** | Faktor penyusutan interval, dipakai menghitung d |
| **1/φ²** | (3 − √5)/2 | **0,3820** | Faktor pada rumus error |

**Ingat:** φ hanya punya satu nilai, yaitu 1,618. Angka 0,618 dan 0,382 bukan φ yang lain, tetapi **1/φ** dan **1/φ²**. Sifat khas: **φ − 1 = 1/φ** dan **1/φ² = 1 − 1/φ** (0,382 = 1 − 0,618).

## 7.3 Mengapa faktornya 0,618, bukan 0,5? (inti jawaban G.3 no. 1)
Kita mau dua titik uji x₁ = xᵤ − r·L dan x₂ = x_l + r·L, dan setelah membuang sebagian interval, salah satu titik lama **dipakai ulang** sebagai titik baru (hemat 1 evaluasi fungsi per iterasi).

Contoh Kondisi 1 (buang kanan): interval baru [x_l, x₂], panjang rL. Titik uji kedua baru = x_l + r·(rL) = x_l + r²L. Titik ini harus sama dengan x₁ lama = x_l + (1 − r)L. Maka:

```
r² = 1 − r   →   r² + r − 1 = 0   →   r = (√5 − 1)/2 = 0,618 = 1/φ
```
Itulah sifat **1/φ² = 1 − 1/φ**. Jika r = 0,5 maka x₁ = x₂ (kedua titik menyatu di tengah) sehingga tidak ada yang bisa dibandingkan; metode dua titik tidak jalan.

## 7.4 Langkah perhitungan MINIMASI

**Step 1.** Tentukan batas awal [x_l, xᵤ], hitung jarak:
```
d = 0,618 · (xᵤ − x_l)       (tepatnya (√5 − 1)/2 · (xᵤ − x_l))
```
**Step 2.** Tentukan dua titik uji (selalu x_l < x₁ < x₂ < xᵤ):
```
x₁ = xᵤ − d
x₂ = x_l + d
```
**Step 3.** Hitung f(x₁) dan f(x₂).

**Step 4.** Bandingkan:

| | Kondisi 1: f(x₁) < f(x₂) | Kondisi 2: f(x₁) > f(x₂) |
|---|---|---|
| Artinya | minimum dekat x₁ | minimum dekat x₂ |
| Yang dibuang | daerah kanan x₂ | daerah kiri x₁ |
| Pembaruan | **xᵤ = x₂** (x_l tetap) | **x_l = x₁** (xᵤ tetap) |
| Interval baru | [x_l, x₂] | [x₁, xᵤ] |
| x_opt | **x₁** | **x₂** |

**Step 5. Kriteria berhenti.** Hitung error relatif:
```
εₐ = (1 − 1/φ) · |(xᵤ − x_l) / x_opt| × 100%  = 0,382 · |(xᵤ − x_l)/x_opt| × 100%
```
(0,382 = 2 − φ). Jika **εₐ < εₛ** (toleransi) → berhenti. Jika masih lebih besar → bentuk interval baru, hitung ulang d, x₁, x₂.
Atau berhenti saat panjang interval sudah sangat kecil.

> Catatan: xᵤ dan x_l di rumus error adalah batas interval **di awal iterasi tersebut** (sebelum diperbarui). Itulah yang dipakai di semua tabel.

**Tabel ringkas minimasi**
| Perbandingan | Daerah minimum | Pembaruan batas | Interval baru | x_opt |
|---|---|---|---|---|
| f(x₁) < f(x₂) | dekat x₁ | xᵤ = x₂, x_l tetap | [x_l, x₂] | x₁ |
| f(x₁) > f(x₂) | dekat x₂ | x_l = x₁, xᵤ tetap | [x₁, xᵤ] | x₂ |

## 7.5 Kasus MAKSIMASI
Langkah sama persis, **arah perbandingan dibalik**:

| Perbandingan | Daerah maksimum | Pembaruan batas | Interval baru | x_opt |
|---|---|---|---|---|
| f(x₁) > f(x₂) | dekat x₁ | xᵤ = x₂, x_l tetap | [x_l, x₂] | x₁ |
| f(x₁) < f(x₂) | dekat x₂ | x_l = x₁, xᵤ tetap | [x₁, xᵤ] | x₂ |

Cara lain: minimumkan **−f(x)**, hasilnya x* sama, nilai fungsi dikembalikan dengan mengalikan −1.

**Cara mengingat:** titik yang nilainya *lebih baik* menjadi x_opt dan **sisi yang jauh dari titik itu dibuang**.
- Titik terbaik x₁ (sisi kiri) → buang kanan (xᵤ = x₂).
- Titik terbaik x₂ (sisi kanan) → buang kiri (x_l = x₁).

## 7.6 Perkiraan jumlah iterasi
Panjang interval menyusut 0,618 tiap iterasi, jadi dapat diperkirakan di muka:
```
n ≥ 1 + ln(L_akhir / L₀) / ln(0,618)
```
Contoh: L₀ = 6, L_akhir = 0,05 → ln(0,05/6) = −4,787; ln 0,618 = −0,4812; hasil 9,95; n ≥ 1 + 9,95 = 10,95 → **n = 11**. Sesuai hasil tabel.

(Konvensi modul: iterasi ke-n adalah iterasi yang interval awalnya sudah ≤ L_akhir.)

## 7.7 CONTOH — Minimasi (dari modul)
**Minimumkan f(x) = x² − 4x + 5 pada [0, 6], εₛ = 1%**

**Iterasi 1**
```
d  = 0,618 (6 − 0) = 3,7082
x1 = 6 − 3,7082 = 2,2918      f(x1) = 1,0851
x2 = 0 + 3,7082 = 3,7082      f(x2) = 3,9180
Cek: 1,0851 < 3,9180 → BENAR → Kondisi 1, x_opt = x1
Maka: xu = x2 = 3,7082 ; xl = 0 tetap
Error: ea = 0,3820 × |(6 − 0)/2,2918| × 100% = 100,00%
100,00% > 1% → lanjut, interval baru [0 ; 3,7082]
```
**Iterasi 2**
```
d  = 0,618 (3,7082 − 0) = 2,2918
x1 = 3,7082 − 2,2918 = 1,4164     f(x1) = 1,3406
x2 = 0 + 2,2918 = 2,2918          f(x2) = 1,0851
Cek: 1,3406 < 1,0851 → SALAH → Kondisi 2, x_opt = x2
Maka: xl = x1 = 1,4164 ; xu = 3,7082 tetap
Error: ea = 0,3820 × |(3,7082 − 0)/2,2918| × 100% = 61,80%
61,80% > 1% → lanjut, interval baru [1,4164 ; 3,7082]
```
**Iterasi 3**
```
d  = 0,618 (3,7082 − 1,4164) = 1,4164
x1 = 3,7082 − 1,4164 = 2,2918     f(x1) = 1,0851   (= x2 iterasi 2, dipakai ulang)
x2 = 1,4164 + 1,4164 = 2,8328     f(x2) = 1,6936
Cek: 1,0851 < 1,6936 → BENAR → Kondisi 1, x_opt = x1 = 2,2918
Maka: xu = x2 = 2,8328 ; xl = 1,4164 tetap
Error: ea = 0,3820 × |(3,7082 − 1,4164)/2,2918| × 100% = 38,20%
```
Pola yang sama dilanjutkan. **Tabel lengkap:**

| k | x_l | x₁ | x₂ | xᵤ | f(x₁) | f(x₂) | Kond. | x_opt | εₐ |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 0,0000 | 2,2918 | 3,7082 | 6,0000 | 1,0851 | 3,9180 | 1 | 2,2918 | 100,00% |
| 2 | 0,0000 | 1,4164 | 2,2918 | 3,7082 | 1,3406 | 1,0851 | 2 | 2,2918 | 61,80% |
| 3 | 1,4164 | 2,2918 | 2,8328 | 3,7082 | 1,0851 | 1,6936 | 1 | 2,2918 | 38,20% |
| 4 | 1,4164 | 1,9574 | 2,2918 | 2,8328 | 1,0018 | 1,0851 | 1 | 1,9574 | 27,64% |
| 5 | 1,4164 | 1,7508 | 1,9574 | 2,2918 | 1,0621 | 1,0018 | 2 | 1,9574 | 17,08% |
| 6 | 1,7508 | 1,9574 | 2,0851 | 2,2918 | 1,0018 | 1,0072 | 1 | 1,9574 | 10,56% |
| 7 | 1,7508 | 1,8785 | 1,9574 | 2,0851 | 1,0148 | 1,0018 | 2 | 1,9574 | 6,52% |
| 8 | 1,8785 | 1,9574 | 2,0062 | 2,0851 | 1,0018 | 1,0000 | 2 | 2,0062 | 3,93% |
| 9 | 1,9574 | 2,0062 | 2,0364 | 2,0851 | 1,0000 | 1,0013 | 1 | 2,0062 | 2,43% |
| 10 | 1,9574 | 1,9876 | 2,0062 | 2,0364 | 1,0002 | 1,0000 | 2 | 2,0062 | 1,50% |
| 11 | 1,9876 | 2,0062 | 2,0177 | 2,0364 | 1,0000 | 1,0003 | 1 | 2,0062 | 0,93% |

**Berhenti pada k = 11** karena εₐ = 0,93% < 1%.
Hasil: **x\* ≈ 2,0062, f(x\*) ≈ 1,0000**. Minimum sejati terkurung di [1,9876 ; 2,0364] (interval pada iterasi 11, atau [1,9876 ; 2,0177] setelah pembaruan Kondisi 1).
Verifikasi analitik: f'(x) = 2x − 4 = 0 → x* = 2, f(2) = 1. Selisih numerik terhadap eksak hanya 0,0062.

## 7.8 CONTOH — Maksimasi (penerapan nyata: volume kotak)
Kotak dari karton persegi sisi 20 cm. Di tiap sudut dipotong persegi kecil sisi x cm, sisi dilipat ke atas. Cari x pada 0 ≤ x ≤ 10 yang memaksimumkan volume.

**Model:** V(x) = x (20 − 2x)². Maksimasi → pakai aturan Subbab 7.5. εₛ = 1%.

**Iterasi 1**
```
d  = 0,618 (10 − 0) = 6,1803
x1 = 10 − 6,1803 = 3,8197      V(x1) = 583,592
x2 = 0 + 6,1803 = 6,1803       V(x2) = 360,680
Cek: V(x1) > V(x2) → BENAR → Kondisi 1, x_opt = x1
Maka: xu = x2 = 6,1803 ; xl = 0 tetap
Error: ea = 0,3820 × |(10 − 0)/3,8197| × 100% = 100,00%
```
**Tabel lengkap:**

| k | x_l | x₁ | x₂ | xᵤ | V(x₁) | V(x₂) | Kond. | x_opt | εₐ |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 0,0000 | 3,8197 | 6,1803 | 10,0000 | 583,592 | 360,680 | 1 | 3,8197 | 100,00% |
| 2 | 0,0000 | 2,3607 | 3,8197 | 6,1803 | 551,070 | 583,592 | 2 | 3,8197 | 61,80% |
| 3 | 2,3607 | 3,8197 | 4,7214 | 6,1803 | 583,592 | 526,225 | 1 | 3,8197 | 38,20% |
| 4 | 2,3607 | 3,2624 | 3,8197 | 4,7214 | 592,390 | 583,592 | 1 | 3,2624 | 27,64% |
| 5 | 2,3607 | 2,9180 | 3,2624 | 3,8197 | 585,405 | 592,390 | 2 | 3,2624 | 17,08% |
| 6 | 2,9180 | 3,2624 | 3,4752 | 3,8197 | 592,390 | 591,799 | 1 | 3,2624 | 10,56% |
| 7 | 2,9180 | 3,1308 | 3,2624 | 3,4752 | 590,919 | 592,390 | 2 | 3,2624 | 6,52% |
| 8 | 3,1308 | 3,2624 | 3,3437 | 3,4752 | 592,390 | 592,588 | 2 | 3,3437 | 3,93% |
| 9 | 3,2624 | 3,3437 | 3,3939 | 3,4752 | 592,588 | 592,447 | 1 | 3,3437 | 2,43% |
| 10 | 3,2624 | 3,3126 | 3,3437 | 3,3939 | 592,575 | 592,588 | 2 | 3,3437 | 1,50% |
| 11 | 3,3126 | 3,3437 | 3,3629 | 3,3939 | 592,588 | 592,558 | 1 | 3,3437 | 0,93% |

Berhenti pada k = 11. **x\* ≈ 3,3437 cm, V(x\*) ≈ 592,59 cm³.**
Verifikasi analitik: V'(x) = (20 − 2x)(20 − 6x) = 0 → x = 10 atau x = 10/3.
x = 10 memberi V = 0 (bukan maksimum). Jawaban benar x\* = 10/3 ≈ 3,3333, V\* = 16.000/27 ≈ 592,593 cm³. Selisih numerik hanya 0,0104 cm.

**Fakta penting:** deret εₐ identik pada contoh minimasi dan maksimasi (100%, 61,80%, 38,20%, 27,64%, ...). Penyusutan interval tetap (0,618 per iterasi) dan tidak bergantung bentuk fungsi.

---

# 8. METODE BISECTION

## 8.1 Konsep
- Mencari **akar persamaan f(x) = 0** (bukan titik optimum!) dengan membagi dua interval berulang.
- Dasar: **Teorema Nilai Antara**. Jika f kontinu pada [a, b] dan nilai f di kedua ujung **berbeda tanda**, maka ada **minimal satu akar** di antaranya.
- **Syarat awal wajib: f kontinu dan f(a) · f(b) < 0**
- Berkembang dari metode tabel; pada bisection pembagian selalu tepat dua bagian sama besar.
- Sederhana, stabil, konvergensi **terjamin**, tetapi lajunya hanya linear.
- Bisection bisa dipakai untuk optimasi bila diterapkan pada **turunan** f'(x), karena titik ekstrem memenuhi f'(x) = 0.

## 8.2 Langkah perhitungan
1. Definisikan f(x) kontinu pada interval.
2. Tentukan batas bawah a, batas atas b, toleransi ε, dan jumlah iterasi maksimum N.
3. **Periksa syarat awal f(a)·f(b) < 0.** Jika tidak terpenuhi, metode tidak boleh dipakai; ganti interval.
4. Hitung titik tengah **c = (a + b)/2**, hitung f(c).
5. Jika f(c) = 0 → berhenti, akar tepat ditemukan.
6. Jika **f(a)·f(c) < 0** → akar di [a, c], **b diganti c**.
7. Selain itu → akar di [c, b], **a diganti c**.
8. Ulangi langkah 4–7 sampai **|b − a| < ε** atau iterasi mencapai N.
9. Akar hampiran **x ≈ c** pada iterasi terakhir.

## 8.3 Perkiraan jumlah iterasi
Lebar interval menyusut setengah tiap iterasi: lebar setelah n iterasi = (b − a)/2ⁿ. Maka:
```
n ≥ log₂( (b − a) / ε )
```
Dibulatkan **ke atas**.

## 8.4 CONTOH TERBIMBING
**Tentukan akar f(x) = x³ − x − 2 pada [1, 2], ε = 0,01**

**Cek syarat awal**
```
f(1) = 1 − 1 − 2 = −2      (negatif)
f(2) = 8 − 2 − 2 = +4      (positif)
f(a)·f(b) = (−2)(4) = −8 < 0   → syarat terpenuhi
```
**Perkiraan iterasi**
```
n ≥ log2( (2 − 1)/0,01 ) = log2(100) = 6,64  → 7 iterasi
```
**Iterasi 1**
```
c = (1 + 2)/2 = 1,5
f(1,5) = 1,5³ − 1,5 − 2 = −0,125     (negatif)
Cek: f(a)·f(c) = (−2)(−0,125) = +0,25 < 0 ?  → SALAH
Maka: f(c)·f(b) = (−0,125)(4) = −0,5 < 0 → BENAR
Akar di [c, b], a diganti c → [1,5 ; 2]
Lebar = 0,5 > 0,01 → lanjut
```
**Iterasi 2**
```
a = 1,5 ; b = 2 ; c = 1,75
f(1,75) = 5,359375 − 1,75 − 2 = +1,60938
f(a)·f(c) = (−0,125)(1,60938) < 0 → BENAR → akar di [a, c], b diganti c → [1,5 ; 1,75]
Lebar = 0,25 > 0,01 → lanjut
```
Dan seterusnya. **Tabel lengkap:**

| Iter | a | b | c = (a+b)/2 | f(c) | Interval baru | Lebar interval awal |
|---|---|---|---|---|---|---|
| 1 | 1,0000 | 2,0000 | 1,500000 | −0,12500 | [1,500000 ; 2,000000] | 1,0000 |
| 2 | 1,5000 | 2,0000 | 1,750000 | +1,60938 | [1,500000 ; 1,750000] | 0,5000 |
| 3 | 1,5000 | 1,7500 | 1,625000 | +0,66602 | [1,500000 ; 1,625000] | 0,2500 |
| 4 | 1,5000 | 1,6250 | 1,562500 | +0,25220 | [1,500000 ; 1,562500] | 0,1250 |
| 5 | 1,5000 | 1,5625 | 1,531250 | +0,05911 | [1,500000 ; 1,531250] | 0,0625 |
| 6 | 1,5000 | 1,5313 | 1,515625 | −0,03405 | [1,515625 ; 1,531250] | 0,0313 |
| 7 | 1,5156 | 1,5313 | 1,523438 | +0,01225 | [1,515625 ; 1,523438] | 0,0156 |

Setelah iterasi ke-7 lebar interval = 0,0078 < 0,01 → berhenti. **Akar hampiran x ≈ 1,5234.** Jumlah iterasi tepat 7, sesuai perkiraan. Akar sejati 1,52138, galat hanya sekitar 0,002.

**Cara menentukan interval baru (jangan sampai tertukar):**
- f(a) dan f(c) berbeda tanda (hasil kali negatif) → akar di sisi kiri → **b = c**
- f(a) dan f(c) sama tanda (hasil kali positif) → akar di sisi kanan → **a = c**

---

# 9. PERBANDINGAN METODE

## 9.1 GSS vs Bisection

| Aspek | Golden Section Search | Bisection |
|---|---|---|
| Yang dicari | Titik minimum/maksimum | Akar persamaan f(x) = 0 |
| Syarat awal | Fungsi unimodal pada [a, b] | f kontinu dan f(a)·f(b) < 0 |
| Informasi dibutuhkan | Nilai fungsi saja | Nilai fungsi saja |
| Titik uji per iterasi | Dua titik di dalam interval | Satu titik tengah |
| Evaluasi fungsi per iterasi | Satu (setelah iterasi pertama, karena titik dipakai ulang) | Satu |
| Faktor penyusutan | 0,618 | 0,500 |
| Dasar keputusan | Perbandingan nilai fungsi | Perbandingan **tanda** nilai fungsi |
| Kriteria berhenti | εₐ < εₛ atau interval kecil | \|b − a\| < ε |
| Perkiraan iterasi | n ≥ 1 + ln(L_akhir/L₀)/ln 0,618 | n ≥ log₂((b − a)/ε) |
| Kelemahan | Gagal bila fungsi tidak unimodal | Gagal bila kedua ujung bertanda sama |

Bisection tampak menyusut lebih cepat (0,5 vs 0,618), tetapi keduanya **tidak bisa dibandingkan langsung** karena menjawab pertanyaan berbeda. Perbandingan adil hanya jika bisection diterapkan pada f'(x).

## 9.2 Perbandingan metode pencarian sederhana (Tutorial 1)

| Metode | Titik uji | Penyusutan | Catatan |
|---|---|---|---|
| Exhaustive Search | n + 1 titik merata, semua dicek | tidak menyusut; interval ketidakpastian 2L₀/(n+1) | Paling sederhana, boros, cocok untuk fungsi diskrit |
| Interval Halving | 3 titik (x₁, x₀, x₂) | 0,5 per iterasi | 2 evaluasi baru per iterasi, konvergensi lambat |
| Golden Section | 2 titik | 0,618 per iterasi | 1 evaluasi baru per iterasi (paling efisien), butuh unimodal |

---

# 10. PEMODELAN OPTIMASI (SOAL CERITA / FORMULASI)

## 10.1 Langkah umum membuat model
1. Tentukan **variabel keputusan** (biasanya jumlah produk yang dibuat).
2. Susun **fungsi tujuan** (maks keuntungan atau min biaya).
3. Tulis **kendala** (sumber daya terbatas, permintaan minimum).
4. Tambahkan **kendala non-negatif**.

## 10.2 SOAL 1 — Pabrik Pupuk (10 poin)
**Diketahui harga bahan baku ($/ton):** Nitrat 1.500; Fosfat 500; Kalium (Potash) 1.000; Kapur inert 100.

| Pupuk | Biaya produksi ($/ton) | Harga jual ($/ton) | Nitrat | Fosfat | Potash | Kapur inert |
|---|---|---|---|---|---|---|
| A | 100 | 350 | 5% | 10% | 5% | 80% |
| B | 150 | 550 | 5% | 15% | 10% | 70% |
| C | 200 | 450 | 10% | 20% | 10% | 60% |
| D | 250 | 700 | 15% | 5% | 15% | 65% |

Ketersediaan per minggu: maks 1.000 ton nitrat, 2.000 ton fosfat, 1.500 ton kalium. Wajib memasok minimal 5.000 ton A dan 4.000 ton D. Selain itu bebas.

**Langkah 1. Variabel keputusan**
x_A, x_B, x_C, x_D ≥ 0 = jumlah (ton) pupuk A, B, C, D yang diproduksi per minggu.

**Langkah 2. Komposisi (ton bahan per ton produk)** — ubah persen menjadi desimal
| Pupuk | Nitrat | Fosfat | Potash | Kapur inert |
|---|---|---|---|---|
| A | 0,05 | 0,10 | 0,05 | 0,80 |
| B | 0,05 | 0,15 | 0,10 | 0,70 |
| C | 0,10 | 0,20 | 0,10 | 0,60 |
| D | 0,15 | 0,05 | 0,15 | 0,65 |

(Artinya 1 ton pupuk A = 0,05 ton nitrat + 0,10 ton fosfat + 0,05 ton potash + 0,80 ton kapur inert.)

**Langkah 3. Biaya bahan baku per ton produk**
- A: (0,05×1500) + (0,10×500) + (0,05×1000) + (0,80×100) = 75 + 50 + 50 + 80 = **255**
- B: (0,05×1500) + (0,15×500) + (0,10×1000) + (0,70×100) = 75 + 75 + 100 + 70 = **320**
- C: (0,10×1500) + (0,20×500) + (0,10×1000) + (0,60×100) = 150 + 100 + 100 + 60 = **410**
- D: (0,15×1500) + (0,05×500) + (0,15×1000) + (0,65×100) = 225 + 25 + 150 + 65 = **465**

**Langkah 4. Total biaya per ton = biaya produksi + biaya bahan**
- A: 100 + 255 = 355
- B: 150 + 320 = 470
- C: 200 + 410 = 610
- D: 250 + 465 = 715

**Langkah 5. Keuntungan per ton = harga jual − total biaya**
- A: 350 − 355 = **−5** (rugi)
- B: 550 − 470 = **+80** (untung)
- C: 450 − 610 = **−160** (rugi)
- D: 700 − 715 = **−15** (rugi)

Hanya pupuk B yang memberi keuntungan positif per ton.

**Langkah 6. Fungsi tujuan**
```
maks Z = −5 x_A + 80 x_B − 160 x_C − 15 x_D
```
**Langkah 7. Kendala**
```
0,05 x_A + 0,05 x_B + 0,10 x_C + 0,15 x_D ≤ 1000    (nitrat)
0,10 x_A + 0,15 x_B + 0,20 x_C + 0,05 x_D ≤ 2000    (fosfat)
0,05 x_A + 0,10 x_B + 0,10 x_C + 0,15 x_D ≤ 1500    (potash)
x_A ≥ 5000,   x_D ≥ 4000                            (permintaan minimum)
x_A, x_B, x_C, x_D ≥ 0                              (non-negatif)
```
Model akhir = fungsi tujuan + seluruh kendala di atas.
(Kunci jawaban menulis non-negatif sebagai "x_A, x_A, x_A, x_A ≥ 0"; itu salah ketik, seharusnya x_A, x_B, x_C, x_D.)

## 10.3 SOAL 2 — Pengrajin Kursi dan Meja (10 poin)
Kursi (X): 2 jam kerja, 3 unit kayu, untung Rp200 ribu. Meja (Y): 4 jam kerja, 2 unit kayu, untung Rp300 ribu. Tersedia 40 jam kerja dan 30 unit kayu per minggu.

**Langkah 1. Variabel:** x = jumlah kursi, y = jumlah meja.
**Langkah 2. Fungsi tujuan:** `maks Z = 200x + 300y` (dalam ribu rupiah)
**Langkah 3. Kendala:**
```
Tenaga kerja:  2x + 4y ≤ 40
Kayu:          3x + 2y ≤ 30
Non-negatif:   x ≥ 0, y ≥ 0
```
**Model akhir:**
```
maks Z = 200x + 300y
s.t.  2x + 4y ≤ 40
      3x + 2y ≤ 30
      x, y ≥ 0
```

*(Tambahan, tidak diminta soal; untuk latihan.)* Penyelesaian grafik: titik sudut (0, 0) → Z = 0; (10, 0) → 2000; (0, 10) → 3000; perpotongan 2x + 4y = 40 dan 3x + 2y = 30 → x = 5, y = 7,5 → **Z = 3250** (Rp3.250.000). Jika produk harus bulat, solusi terbaik (4, 8) dengan Z = 3200.

---

# 11. JAWABAN LENGKAP LATIHAN TUTORIAL 2

> **Konvensi yang dipakai:** untuk GSS dengan syarat panjang interval, berhenti pada iterasi yang interval awalnya (xᵤ − x_l) sudah ≤ batas (konsisten dengan rumus n ≥ 1 + ln(L_akhir/L₀)/ln 0,618 di modul). Jika nilai f(x₁) = f(x₂) persis sama, optimum berada di antara x₁ dan x₂ sehingga salah satu sisi boleh dibuang; tabel di bawah memakai Kondisi 2 (karena "f(x₁) < f(x₂)" tidak terpenuhi pada minimasi, dan "f(x₁) > f(x₂)" tidak terpenuhi pada maksimasi).
>
> Rumus error: εₐ = 0,3820 · |(xᵤ − x_l)/x_opt| × 100%.
> Rumus tetap: d = 0,618034 (xᵤ − x_l); x₁ = xᵤ − d; x₂ = x_l + d.

## G.1 — Golden Section Search

### G.1 No. 1 — Maksimum f(x) = 2 sin x − x²/10 pada [0, 4], toleransi 1%
(Catatan: teks soal pada PDF rusak; saya membaca sebagai 2 sin x, soal klasik ini. Memakai radian.)

**Iterasi 1**
```
d  = 0,618034 (4 − 0) = 2,4721
x1 = 4 − 2,4721 = 1,5279     f(x1) = 2 sin(1,5279) − (1,5279)²/10 = 1,7647
x2 = 0 + 2,4721 = 2,4721     f(x2) = 2 sin(2,4721) − (2,4721)²/10 = 0,6300
Maksimasi: f(x1) > f(x2) → Kondisi 1, x_opt = x1 = 1,5279
xu = x2 = 2,4721 ; xl = 0 tetap
ea = 0,3820 × |(4 − 0)/1,5279| × 100% = 100,00% > 1% → lanjut
```
**Iterasi 2**
```
d  = 0,618034 (2,4721 − 0) = 1,5279
x1 = 2,4721 − 1,5279 = 0,9443     f(x1) = 1,5310
x2 = 0 + 1,5279 = 1,5279          f(x2) = 1,7647
Maksimasi: f(x1) < f(x2) → Kondisi 2, x_opt = x2 = 1,5279
xl = x1 = 0,9443 ; xu = 2,4721 tetap
ea = 0,3820 × |2,4721/1,5279| × 100% = 61,80% > 1% → lanjut
```
**Tabel lengkap**
| k | x_l | x₁ | x₂ | xᵤ | f(x₁) | f(x₂) | Kond. | x_opt | εₐ |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 0,0000 | 1,5279 | 2,4721 | 4,0000 | 1,76472 | 0,62997 | 1 | 1,5279 | 100,00% |
| 2 | 0,0000 | 0,9443 | 1,5279 | 2,4721 | 1,53098 | 1,76472 | 2 | 1,5279 | 61,80% |
| 3 | 0,9443 | 1,5279 | 1,8885 | 2,4721 | 1,76472 | 1,54322 | 1 | 1,5279 | 38,20% |
| 4 | 0,9443 | 1,3050 | 1,5279 | 1,8885 | 1,75945 | 1,76472 | 2 | 1,5279 | 23,61% |
| 5 | 1,3050 | 1,5279 | 1,6656 | 1,8885 | 1,76472 | 1,71358 | 1 | 1,5279 | 14,59% |
| 6 | 1,3050 | 1,4427 | 1,5279 | 1,6656 | 1,77547 | 1,76472 | 1 | 1,4427 | 9,55% |
| 7 | 1,3050 | 1,3901 | 1,4427 | 1,5279 | 1,77420 | 1,77547 | 2 | 1,4427 | 5,90% |
| 8 | 1,3901 | 1,4427 | 1,4752 | 1,5279 | 1,77547 | 1,77324 | 1 | 1,4427 | 3,65% |
| 9 | 1,3901 | 1,4226 | 1,4427 | 1,4752 | 1,77570 | 1,77547 | 1 | 1,4226 | 2,29% |
| 10 | 1,3901 | 1,4102 | 1,4226 | 1,4427 | 1,77540 | 1,77570 | 2 | 1,4226 | 1,41% |
| 11 | 1,4102 | 1,4226 | 1,4303 | 1,4427 | 1,77570 | 1,77572 | 2 | 1,4303 | 0,87% |

**Berhenti pada k = 11** (εₐ = 0,87% < 1%).
**Kesimpulan:** x\* ≈ **1,4303**, f(x\*) ≈ **1,7757**.
Verifikasi analitik: f'(x) = 2 cos x − x/5 = 0 → x ≈ 1,4276, f ≈ 1,7757. Selisih x hanya 0,0027.

### G.1 No. 2 — Minimum f(x) = (x − 3)⁴ + 2(x − 3)² + 13 pada [2, 4], hingga panjang interval ≤ 0,05

Perkiraan iterasi: n ≥ 1 + ln(0,05/2)/ln 0,618 = 1 + 7,67 = 8,67 → **9 iterasi**.

**Iterasi 1**
```
d  = 0,618034 (4 − 2) = 1,2361
x1 = 4 − 1,2361 = 2,7639     f(x1) = (−0,2361)⁴ + 2(−0,2361)² + 13 = 0,0031 + 0,1115 + 13 = 13,11456
x2 = 2 + 1,2361 = 3,2361     f(x2) = (0,2361)⁴ + 2(0,2361)² + 13 = 13,11456
f(x1) = f(x2) (fungsi simetris terhadap x = 3) → minimum di antara x1 dan x2
Dipakai Kondisi 2: x_opt = x2 ; xl = x1 = 2,7639 ; xu = 4 tetap
Panjang interval awal = 2 > 0,05 → lanjut
```
**Iterasi 2**
```
d  = 0,618034 (4 − 2,7639) = 0,7639
x1 = 4 − 0,7639 = 3,2361     f(x1) = 13,11456   (dipakai ulang)
x2 = 2,7639 + 0,7639 = 3,5279     f(x2) = 13,63492
f(x1) < f(x2) → Kondisi 1, x_opt = x1 = 3,2361
xu = x2 = 3,5279 ; xl = 2,7639 tetap
Panjang interval awal = 1,2361 > 0,05 → lanjut
```
**Tabel lengkap**
| k | x_l | x₁ | x₂ | xᵤ | f(x₁) | f(x₂) | Kond. | x_opt | Panjang (xᵤ − x_l) |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 2,0000 | 2,7639 | 3,2361 | 4,0000 | 13,11456 | 13,11456 | 2 (sama) | 3,2361 | 2,0000 |
| 2 | 2,7639 | 3,2361 | 3,5279 | 4,0000 | 13,11456 | 13,63492 | 1 | 3,2361 | 1,2361 |
| 3 | 2,7639 | 3,0557 | 3,2361 | 3,5279 | 13,00622 | 13,11456 | 1 | 3,0557 | 0,7639 |
| 4 | 2,7639 | 2,9443 | 3,0557 | 3,2361 | 13,00622 | 13,00622 | 2 (sama) | 3,0557 | 0,4721 |
| 5 | 2,9443 | 3,0557 | 3,1246 | 3,2361 | 13,00622 | 13,03130 | 1 | 3,0557 | 0,2918 |
| 6 | 2,9443 | 3,0132 | 3,0557 | 3,1246 | 13,00035 | 13,00622 | 1 | 3,0132 | 0,1803 |
| 7 | 2,9443 | 2,9868 | 3,0132 | 3,0557 | 13,00035 | 13,00035 | 2 (sama) | 3,0132 | 0,1115 |
| 8 | 2,9868 | 3,0132 | 3,0294 | 3,0557 | 13,00035 | 13,00173 | 1 | 3,0132 | 0,0689 |
| 9 | 2,9868 | 3,0031 | 3,0132 | 3,0294 | 13,00002 | 13,00035 | 1 | 3,0031 | 0,0426 |

Pada k = 9, panjang interval 0,0426 ≤ 0,05 → **berhenti**. Interval setelah pembaruan Kondisi 1: [2,9868 ; 3,0132] (panjang 0,0263).
**Kesimpulan:** x\* ≈ **3,0031**, f(x\*) ≈ **13,0000**.
Verifikasi analitik: f'(x) = 4(x − 3)³ + 4(x − 3) = 4(x − 3)[(x − 3)² + 1] = 0 → hanya x = 3. f(3) = 13. Selisih 0,0031.

### G.1 No. 3 — Minimum f(x) = (x − 2)² + 3 pada [0, 5], toleransi 1%

**Iterasi 1**
```
d  = 0,618034 (5 − 0) = 3,0902
x1 = 5 − 3,0902 = 1,9098     f(x1) = (1,9098 − 2)² + 3 = 3,00813
x2 = 0 + 3,0902 = 3,0902     f(x2) = (3,0902 − 2)² + 3 = 4,18847
3,00813 < 4,18847 → Kondisi 1, x_opt = x1 = 1,9098
xu = x2 = 3,0902 ; xl = 0 tetap
ea = 0,3820 × |5/1,9098| × 100% = 100,00% → lanjut
```
**Iterasi 2**
```
d  = 0,618034 (3,0902 − 0) = 1,9098
x1 = 3,0902 − 1,9098 = 1,1803     f(x1) = (1,1803 − 2)² + 3 = 3,67184
x2 = 0 + 1,9098 = 1,9098          f(x2) = 3,00813
3,67184 < 3,00813 ? SALAH → Kondisi 2, x_opt = x2 = 1,9098
xl = x1 = 1,1803 ; xu = 3,0902 tetap
ea = 0,3820 × |3,0902/1,9098| × 100% = 61,80% → lanjut
```
**Tabel lengkap**
| k | x_l | x₁ | x₂ | xᵤ | f(x₁) | f(x₂) | Kond. | x_opt | εₐ |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 0,0000 | 1,9098 | 3,0902 | 5,0000 | 3,00813 | 4,18847 | 1 | 1,9098 | 100,00% |
| 2 | 0,0000 | 1,1803 | 1,9098 | 3,0902 | 3,67184 | 3,00813 | 2 | 1,9098 | 61,80% |
| 3 | 1,1803 | 1,9098 | 2,3607 | 3,0902 | 3,00813 | 3,13009 | 1 | 1,9098 | 38,20% |
| 4 | 1,1803 | 1,6312 | 1,9098 | 2,3607 | 3,13602 | 3,00813 | 2 | 1,9098 | 23,61% |
| 5 | 1,6312 | 1,9098 | 2,0820 | 2,3607 | 3,00813 | 3,00673 | 2 | 2,0820 | 13,38% |
| 6 | 1,9098 | 2,0820 | 2,1885 | 2,3607 | 3,00673 | 3,03552 | 1 | 2,0820 | 8,27% |
| 7 | 1,9098 | 2,0163 | 2,0820 | 2,1885 | 3,00026 | 3,00673 | 1 | 2,0163 | 5,28% |
| 8 | 1,9098 | 1,9756 | 2,0163 | 2,0820 | 3,00059 | 3,00026 | 2 | 2,0163 | 3,26% |
| 9 | 1,9756 | 2,0163 | 2,0414 | 2,0820 | 3,00026 | 3,00171 | 1 | 2,0163 | 2,02% |
| 10 | 1,9756 | 2,0007 | 2,0163 | 2,0414 | 3,00000 | 3,00026 | 1 | 2,0007 | 1,26% |
| 11 | 1,9756 | 1,9911 | 2,0007 | 2,0163 | 3,00008 | 3,00000 | 2 | 2,0007 | 0,78% |

**Berhenti pada k = 11** (0,78% < 1%).
**Kesimpulan numerik:** x\* ≈ **2,0007**, f(x\*) ≈ **3,0000**.
**Penyelesaian analitik:** f'(x) = 2(x − 2) = 0 → x\* = 2; f''(x) = 2 > 0 (minimum); f(2) = 3.
**Perbandingan:** selisih x = 0,0007, selisih nilai fungsi ≈ 0,0000005. Hasil numerik sangat dekat dengan eksak.

### G.1 No. 4 — Maksimum f(x) = −0,3(x − 5)² + 4 pada [2, 8], toleransi 1%
(Aturan perbandingan terbalik dibanding minimasi.)

**Iterasi 1**
```
d  = 0,618034 (8 − 2) = 3,7082
x1 = 8 − 3,7082 = 4,2918     f(x1) = −0,3(4,2918 − 5)² + 4 = 3,84953
x2 = 2 + 3,7082 = 5,7082     f(x2) = −0,3(5,7082 − 5)² + 4 = 3,84953
f(x1) = f(x2) (simetris terhadap x = 5) → maksimum di antara keduanya
Dipakai Kondisi 2: x_opt = x2 = 5,7082 ; xl = x1 = 4,2918 ; xu = 8 tetap
ea = 0,3820 × |(8 − 2)/5,7082| × 100% = 40,15% > 1% → lanjut
```
**Iterasi 2**
```
d  = 0,618034 (8 − 4,2918) = 2,2918
x1 = 8 − 2,2918 = 5,7082     f(x1) = 3,84953
x2 = 4,2918 + 2,2918 = 6,5836     f(x2) = −0,3(1,5836)² + 4 = 3,24767
Maksimasi: f(x1) > f(x2) → Kondisi 1, x_opt = x1 = 5,7082
xu = x2 = 6,5836 ; xl = 4,2918 tetap
ea = 0,3820 × |3,7082/5,7082| × 100% = 24,81% → lanjut
```
**Tabel lengkap**
| k | x_l | x₁ | x₂ | xᵤ | f(x₁) | f(x₂) | Kond. | x_opt | εₐ |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 2,0000 | 4,2918 | 5,7082 | 8,0000 | 3,84953 | 3,84953 | 2 (sama) | 5,7082 | 40,15% |
| 2 | 4,2918 | 5,7082 | 6,5836 | 8,0000 | 3,84953 | 3,24767 | 1 | 5,7082 | 24,81% |
| 3 | 4,2918 | 5,1672 | 5,7082 | 6,5836 | 3,99161 | 3,84953 | 1 | 5,1672 | 16,94% |
| 4 | 4,2918 | 4,8328 | 5,1672 | 5,7082 | 3,99161 | 3,99161 | 2 (sama) | 5,1672 | 10,47% |
| 5 | 4,8328 | 5,1672 | 5,3738 | 5,7082 | 3,99161 | 3,95807 | 1 | 5,1672 | 6,47% |
| 6 | 4,8328 | 5,0395 | 5,1672 | 5,3738 | 3,99953 | 3,99161 | 1 | 5,0395 | 4,10% |
| 7 | 4,8328 | 4,9605 | 5,0395 | 5,1672 | 3,99953 | 3,99953 | 2 (sama) | 5,0395 | 2,53% |
| 8 | 4,9605 | 5,0395 | 5,0883 | 5,1672 | 3,99953 | 3,99766 | 1 | 5,0395 | 1,57% |
| 9 | 4,9605 | 5,0093 | 5,0395 | 5,0883 | 3,99997 | 3,99953 | 1 | 5,0093 | 0,97% |

**Berhenti pada k = 9** (0,97% < 1%).
**Kesimpulan:** x\* ≈ **5,0093**, f(x\*) ≈ **3,99997 ≈ 4**.
Verifikasi analitik: f'(x) = −0,6(x − 5) = 0 → x = 5; f''(x) = −0,6 < 0 (maksimum); f(5) = 4.

## G.2 — Metode Bisection

### G.2 No. 1 — Akar f(x) = cos(x) − x pada [0, 1], berhenti bila panjang interval < 0,1
(Catatan: nama fungsi pada PDF rusak; saya membaca sebagai cos(x) − x, soal klasik. Prosedurnya sama untuk fungsi lain.)

**Cek syarat awal**
```
f(0) = cos 0 − 0 = +1
f(1) = cos 1 − 1 = 0,5403 − 1 = −0,4597
f(0)·f(1) = −0,4597 < 0 → syarat terpenuhi
```
**Perkiraan iterasi:** n ≥ log₂((1 − 0)/0,1) = log₂ 10 = 3,32 → **4 iterasi**.

| Iter | a | b | c | f(a) | f(c) | f(a)·f(c) | Interval baru | Lebar baru |
|---|---|---|---|---|---|---|---|---|
| 1 | 0,0000 | 1,0000 | 0,5000 | +1,00000 | +0,37758 | + | [0,5 ; 1] (a = c) | 0,5000 |
| 2 | 0,5000 | 1,0000 | 0,7500 | +0,37758 | −0,01831 | − | [0,5 ; 0,75] (b = c) | 0,2500 |
| 3 | 0,5000 | 0,7500 | 0,6250 | +0,37758 | +0,18596 | + | [0,625 ; 0,75] (a = c) | 0,1250 |
| 4 | 0,6250 | 0,7500 | 0,6875 | +0,18596 | +0,08533 | + | [0,6875 ; 0,75] (a = c) | 0,0625 |

Setelah iterasi 4 lebar = 0,0625 < 0,1 → **berhenti**.
**Kesimpulan:** akar hampiran x ≈ c = **0,6875** (aturan modul: c pada iterasi terakhir). Titik tengah interval akhir, 0,71875, lebih dekat. Akar sejati 0,739085 (galat 0,0516 untuk 0,6875). Interval akhir [0,6875 ; 0,75] memang memuat akar sejati.

### G.2 No. 2 — f(x) = (x − 16/3)(5 − 12x + 20x²) pada [0, 1], hingga lebar interval < 0,10

**Langkah wajib pertama: cek syarat awal.**
```
f(0) = (0 − 5,3333)(5 − 0 + 0)      = (−5,3333)(5)  = −26,667
f(1) = (1 − 5,3333)(5 − 12 + 20)    = (−4,3333)(13) = −56,333
f(0)·f(1) = (−26,667)(−56,333) = +1502 > 0   → SYARAT TIDAK TERPENUHI
```
**Kesimpulan: bisection tidak boleh dilanjutkan pada [0, 1].**
Alasan: faktor 5 − 12x + 20x² punya diskriminan 144 − 400 = −256 < 0, jadi selalu positif (tidak punya akar real). Satu-satunya akar real f adalah x = 16/3 ≈ 5,3333, yang berada di luar [0, 1]. Pada [0, 1] fungsi bertanda negatif terus sehingga tidak ada pergantian tanda.

**Yang harus dilakukan:** ganti interval sampai f(a)·f(b) < 0 (misal dengan metode tabel/grafik). Contoh interval yang valid: **[5, 6]**.

**Demonstrasi pada [5, 6]**
```
f(5) = (5 − 5,3333)(5 − 60 + 500) = (−0,3333)(445) = −148,333
f(6) = (6 − 5,3333)(5 − 72 + 720) = (0,6667)(653)  = +435,333
f(5)·f(6) < 0 → syarat terpenuhi
Perkiraan: n ≥ log2(1/0,1) = 3,32 → 4 iterasi
```
| Iter | a | b | c | f(a) | f(c) | f(a)·f(c) | Interval baru | Lebar baru |
|---|---|---|---|---|---|---|---|---|
| 1 | 5,0000 | 6,0000 | 5,5000 | −148,333 | +90,667 | − | [5 ; 5,5] (b = c) | 0,5000 |
| 2 | 5,0000 | 5,5000 | 5,2500 | −148,333 | −41,104 | + | [5,25 ; 5,5] (a = c) | 0,2500 |
| 3 | 5,2500 | 5,5000 | 5,3750 | −41,104 | +21,596 | − | [5,25 ; 5,375] (b = c) | 0,1250 |
| 4 | 5,2500 | 5,3750 | 5,3125 | −41,104 | −10,535 | + | [5,3125 ; 5,375] (a = c) | 0,0625 |

Lebar 0,0625 < 0,1 → berhenti. Akar hampiran **x ≈ 5,3125**; akar sejati 16/3 = 5,3333.

### G.2 No. 3 — Perkiraan iterasi vs iterasi sebenarnya
Rumus: n ≥ log₂((b − a)/ε), dibulatkan ke atas.

| Soal | Interval | ε | Perkiraan | Sebenarnya |
|---|---|---|---|---|
| No. 1 (cos x − x) | [0, 1] | 0,1 | log₂ 10 = 3,32 → **4** | **4** (lebar 0,0625 < 0,1) |
| No. 2 (pada [0, 1]) | [0, 1] | 0,1 | log₂ 10 = 3,32 → 4 | **Tidak dapat dijalankan** (f(0)·f(1) > 0) |
| No. 2 (interval pengganti [5, 6]) | [5, 6] | 0,1 | 3,32 → **4** | **4** (lebar 0,0625 < 0,1) |

Perkiraan sama persis dengan kenyataan, karena lebar interval menyusut tepat setengah tiap iterasi (hanya bergantung pada b − a dan ε, bukan bentuk fungsi). Hati-hati: perkiraan hanya valid jika syarat awal f(a)·f(b) < 0 terpenuhi.

## G.3 — Soal Analisis

### G.3 No. 1 — Mengapa faktor penyusutan GSS = 0,618, bukan 0,5?
GSS memakai dua titik uji x₁ = xᵤ − r·L dan x₂ = x_l + r·L, dan ingin **memakai ulang** satu titik lama pada iterasi berikutnya agar hanya ada 1 evaluasi baru per iterasi.

Jika xᵤ dibuang (Kondisi 1), interval baru panjangnya r·L. Titik uji kedua yang baru adalah x_l + r·(rL) = x_l + r²L. Agar sama dengan x₁ lama = x_l + (1 − r)L, haruslah:
```
r² = 1 − r   →   r² + r − 1 = 0   →   r = (√5 − 1)/2 = 0,618 = 1/φ
```
Ini sama dengan sifat **1/φ² = 1 − 1/φ** (0,382 = 1 − 0,618). Dengan r = 0,5 kedua titik uji akan bertepatan di tengah (x₁ = x₂), sehingga tidak ada dua nilai yang dapat dibandingkan dan tidak ada titik lama yang bisa dipakai ulang. Karena itu r = 0,5 tidak bisa; 0,618 adalah satu-satunya nilai yang membuat penggunaan ulang titik terjadi.

### G.3 No. 2 — Fungsi dengan dua lembah (tidak unimodal)
**Yang mungkin terjadi:** aturan pembuangan GSS (bandingkan f(x₁) dan f(x₂)) hanya sah bila ada satu minimum. Pada fungsi dua lembah, perbandingan dua titik bisa menyesatkan, sehingga daerah yang memuat minimum global ikut terbuang. GSS tetap konvergen, tetapi hanya ke **salah satu minimum lokal**, mungkin yang bukan global, tanpa memberi peringatan. Hasil bergantung pada interval awal.

**Langkah pengaman:**
1. Plot atau pindai fungsi dulu (misal Exhaustive Search kasar) untuk melihat lembah-lembahnya.
2. Pecah interval menjadi sub-interval yang masing-masing unimodal, jalankan GSS di tiap sub-interval, lalu bandingkan nilai optimum yang diperoleh untuk memilih yang terbaik (global).
3. Alternatif: mulai dari beberapa interval awal berbeda (multi-start) dan bandingkan hasilnya; atau cek tanda turunan untuk memastikan hanya ada satu titik stasioner pada interval.

### G.3 No. 3 — Bisection dengan f(a) dan f(b) sama-sama positif
**Yang harus dilakukan:** hentikan, jangan lanjut. Cari interval baru [a, b] yang menghasilkan f(a)·f(b) < 0 (lewat plot, metode tabel, atau mencoba beberapa titik), sambil memeriksa f kontinu.

**Mengapa tidak boleh dilanjutkan:** jaminan keberadaan akar berasal dari Teorema Nilai Antara yang butuh perbedaan tanda di kedua ujung. Jika keduanya positif, interval bisa tidak memuat akar sama sekali, atau memuat **jumlah genap akar** (misal dua akar, atau akar kembar yang menyentuh sumbu x). Aturan memilih setengah interval berdasarkan tanda tidak punya dasar: nilai f(c) tidak akan menunjukkan sisi yang benar, sehingga iterasi akan memberi angka yang tidak bermakna (konvergen ke titik yang bukan akar, atau tidak konvergen).
Jika masalahnya akar kembar (fungsi tidak berganti tanda), pakai metode lain.

---

# 12. CATATAN KOREKSI PADA MODUL (hati-hati saat kuis)

Beberapa salah ketik di modul. Dengan mengetahuinya Anda tidak bingung saat membandingkan:

| Lokasi | Tertulis | Seharusnya |
|---|---|---|
| Tutorial 2, contoh minimasi, iterasi 2 | d = 2,2917 | 2,2918 (selisih pembulatan) |
| Tutorial 2, contoh minimasi, interval akhir | "terkurung dalam [1,9876 ; 2,0364]" | itu interval iterasi 11 sebelum pembaruan; setelah Kondisi 1 menjadi [1,9876 ; 2,0177] |
| Tutorial 1, kunci Soal 1 (non-negatif) | x_A, x_A, x_A, x_A ≥ 0 | x_A, x_B, x_C, x_D ≥ 0 |
| Tutorial 1, kunci Soal 1 (baris total biaya) | "A: 100+255=355100 + 255 = 355100+255=355" | cukup 100 + 255 = 355 (teks terduplikasi) |
| Tutorial 1, kunci Soal 6, butir 3 | f(0) = 0 − 0 + 0 + 10 = **1** | f(0) = **10** |
| Tutorial 1, kunci Soal 6, butir 5 | Lₙ = 2/(n+1)·L₀ = 2/20 × 10 | n di sini = 19 titik interior (21 titik − 2 ujung), sehingga n + 1 = 20 |
| Tutorial 1, Soal 4 (kunci) | Iterasi 1, "Cek kriteria berhenti" kosong | L = 5 − 2 = 3 > 0,5 → lanjut |
| Tutorial 2, soal G.1/G.2 | Nama fungsi trigonometri rusak | Dibaca 2 sin x (G.1 no. 1) dan cos x − x (G.2 no. 1) |
| Tutorial 2, G.2 no. 2 | Interval [0, 1] | Tidak memenuhi f(a)·f(b) < 0 (lihat jawaban); kemungkinan soal sengaja menguji pemeriksaan syarat |

Tabel rumus tutorial 2 pada PDF banyak yang tampil rusak (huruf hilang); rumus yang benar adalah yang ada di panduan ini.

---

# 13. CHEAT SHEET

## 13.1 Rumus inti
```
Golden ratio:         φ = (1+√5)/2 = 1,618
Faktor penyusutan:    1/φ = 0,618       1/φ² = 0,382 = 1 − 0,618
GSS:                  d = 0,618 (xu − xl);  x1 = xu − d;  x2 = xl + d
Error GSS:            ea = 0,382 · |(xu − xl)/x_opt| × 100%
Iterasi GSS:          n ≥ 1 + ln(Lakhir/L0)/ln 0,618
Bisection:            c = (a+b)/2;   syarat f(a)·f(b) < 0
Iterasi bisection:    n ≥ log2((b − a)/ε)
Interval halving:     x0 = (a+b)/2; x1 = (a+x0)/2; x2 = (x0+b)/2; L baru = L/2
Exhaustive search:    Δx = (b − a)/n;  xi = a + i·Δx;  Ln = 2/(n+1) · L0 (n = titik interior)
Uji turunan kedua:    f'' > 0 → min;  f'' < 0 → maks
```

## 13.2 Aturan keputusan (hafalan cepat)

**GSS minimasi:** f(x₁) < f(x₂) → x_opt = x₁, **xᵤ = x₂** (Kondisi 1) | f(x₁) > f(x₂) → x_opt = x₂, **x_l = x₁** (Kondisi 2)
**GSS maksimasi:** kebalikannya (f(x₁) > f(x₂) → Kondisi 1)
**Bisection:** f(a)·f(c) < 0 → **b = c** | selain itu → **a = c**
**Interval halving (minimasi):** f₁ < f₀ < f₂ → [a, x₀] | f₂ < f₀ < f₁ → [x₀, b] | f₁ > f₀ dan f₂ > f₀ → [x₁, x₂]

## 13.3 Kesalahan umum yang sering membuat nilai hilang
1. **Tertukar tujuan:** GSS mencari **optimum**, Bisection mencari **akar**.
2. **Tertukar nilai:** φ = 1,618 saja. 0,618 = 1/φ, 0,382 = 1/φ². Tulis dengan tanda bagi.
3. **Lupa cek syarat awal** (unimodal untuk GSS; f(a)·f(b) < 0 untuk bisection).
4. **Arah perbandingan salah pada maksimasi.** Tulis tabel keputusan dulu sebelum mulai.
5. **Salah pasang batas:** Kondisi 1 → **xᵤ** berubah menjadi x₂. Kondisi 2 → **x_l** berubah menjadi x₁.
6. **Rumus error memakai batas lama** (xᵤ − x_l pada awal iterasi), bukan interval baru.
7. **Tidak memakai ulang titik lama** pada GSS (boleh dihitung ulang, hasilnya sama, tetapi membuang waktu).
8. **Pembulatan:** pakai 4 desimal (GSS) dan simpan nilai penuh saat menghitung agar error tidak menumpuk.
9. **Lupa verifikasi analitik** di akhir (f'(x) = 0) dan lupa menyebut selisih hasil numerik dengan nilai eksak.
10. **Exhaustive search:** n pembagi = n + 1 titik (x = 0 sampai 8 untuk n = 8 adalah 9 titik). Bila ada nilai minimum kembar, sebutkan keduanya dan tafsirkan (Soal 3 dan 6).
11. **Bisection:** jumlah iterasi dibulatkan **ke atas**.
12. **Tafsir hasil di soal cerita:** x = 0 pada biaya adalah biaya tetap, bukan jawaban bermakna (Soal 6); x = 10 pada volume kotak memberi V = 0 (bukan maksimum).

## 13.4 Template jawaban saat kuis (urutan menulis)
1. Tulis jenis soal (min/maks, akar) dan metode.
2. Tulis syarat awal dan cek (unimodal / f(a)·f(b) < 0).
3. Tulis rumus yang dipakai.
4. Hitung iterasi 1 secara rinci (semua angka), lalu iterasi berikutnya dengan pola sama.
5. Setiap iterasi: nilai fungsi → keputusan (kondisi/kasus) → batas baru → error/panjang interval → lanjut atau berhenti.
6. Sajikan tabel ringkasan.
7. Tulis kesimpulan (x\* dan f(x\*)) lalu verifikasi dengan solusi analitik.

---

*Selamat belajar dan semoga kuisnya lancar!*

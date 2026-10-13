# Soal & Cara Pengerjaan: Optimasi Multivariabel Tanpa Kendala
*Persiapan kuis, Teori Optimasi, Pertemuan 6*

---

## 0. Alat yang Dipakai (Cheat Sheet)

**Langkah baku mencari & mengklasifikasi titik optimum:**

1. Hitung gradien: $\nabla f(x) = \left[\frac{\partial f}{\partial x_1}, \dots, \frac{\partial f}{\partial x_n}\right]^T$
2. Selesaikan $\nabla f(x) = 0$ → dapat **titik stasioner** (kandidat optimum, ini hanya syarat *perlu*).
3. Hitung Hessian: $H(x) = \nabla^2 f(x)$ (matriks turunan kedua).
4. Substitusi titik stasioner ke Hessian, lalu cek **kedefinitan**.
5. Klasifikasikan:

| Hessian di titik stasioner | Kesimpulan |
|---|---|
| Definit positif (semua $\lambda > 0$) | **Minimum lokal** |
| Definit negatif (semua $\lambda < 0$) | **Maksimum lokal** |
| Indefinit (ada $\lambda > 0$ dan $\lambda < 0$) | **Titik pelana (saddle)**, bukan optimum |
| Semidefinit positif / negatif (ada $\lambda = 0$) | **Belum bisa disimpulkan** (min/maks lokal atau pelana) |

**Cara cepat cek kedefinitan matriks simetris 2×2** $\begin{pmatrix} a & b \\ b & d \end{pmatrix}$:

| Kondisi | Hasil |
|---|---|
| $\det > 0$ dan $a > 0$ | Definit positif |
| $\det > 0$ dan $a < 0$ | Definit negatif |
| $\det < 0$ | Indefinit |
| $\det = 0$ | Semidefinit (cek lebih lanjut) |

**Cara cek lewat nilai eigen:** selesaikan $\det(M - \lambda I) = 0$.
Semua $\lambda > 0$ → PD; semua $\lambda < 0$ → ND; campuran → indefinit; ada $\lambda = 0$ → semidefinit.

**Cara cek lewat bentuk kuadrat:** hitung $q(x) = x^T M x$, lalu lihat tandanya untuk semua $x \neq 0$.

**Convexity:**
- Definisi: $f(\theta x + (1-\theta)y) \le \theta f(x) + (1-\theta) f(y)$, untuk semua $x,y$ dan $\theta \in [0,1]$.
- Uji Hessian: $\nabla^2 f(x) \succeq 0$ (semidefinit positif) untuk **semua** $x$ → $f$ convex.
- Jika $f$ convex, setiap minimum lokal = minimum global.

---

## 1. Mencari Titik Stasioner (Contoh 1 di Slide)

**Soal:** Tentukan semua titik stasioner dari
$$f(x_1,x_2) = x_1^2 + 4x_1x_2 - x_2^2$$

**Pengerjaan:**

1. Gradien:
$$\nabla f(x) = \begin{bmatrix} 2x_1 + 4x_2 \\ 4x_1 - 2x_2 \end{bmatrix}$$
2. Samakan dengan nol:
   - $2x_1 + 4x_2 = 0 \Rightarrow x_1 = -2x_2$
   - Substitusi ke persamaan kedua: $4(-2x_2) - 2x_2 = -10x_2 = 0 \Rightarrow x_2 = 0$, lalu $x_1 = 0$.
3. **Titik stasioner: $(0,0)$** (satu-satunya).

**(Lanjutan, klasifikasi):**
Hessian: $H = \begin{pmatrix} 2 & 4 \\ 4 & -2 \end{pmatrix}$, $\det = (2)(-2) - 16 = -20 < 0$ → **indefinit** → $(0,0)$ adalah **titik pelana**.
(Cek eigen: $\lambda^2 - 20 = 0 \Rightarrow \lambda = \pm\sqrt{20}$, ada positif dan negatif.)

---

## 2. Titik Optimum + Klasifikasi (Contoh 2.5 di Slide)

**Soal:** Cari dan klasifikasikan titik optimum dari
$$f(x) = 3x_1^2 + 2x_2^2 + 4x_1x_2 - 6x_1 - 8x_2 + 6$$

**Pengerjaan:**

1. Gradien:
$$\nabla f(x) = \begin{bmatrix} 6x_1 + 4x_2 - 6 \\ 4x_1 + 4x_2 - 8 \end{bmatrix}$$
2. Set $\nabla f = 0$:
   - (i) $6x_1 + 4x_2 = 6$
   - (ii) $4x_1 + 4x_2 = 8$
   - Kurangkan (i) − (ii): $2x_1 = -2 \Rightarrow x_1 = -1$
   - Substitusi ke (ii): $-4 + 4x_2 = 8 \Rightarrow x_2 = 3$
   - **Titik stasioner: $x^* = (-1, 3)$**
3. Hessian (konstan):
$$H = \begin{pmatrix} 6 & 4 \\ 4 & 4 \end{pmatrix}$$
4. Cek: $a = 6 > 0$, $\det = 24 - 16 = 8 > 0$ → **definit positif**.
5. **Kesimpulan:** $x^* = (-1,3)$ adalah **titik minimum**.
6. Nilai fungsi:
$$f(-1,3) = 3(1) + 2(9) + 4(-1)(3) - 6(-1) - 8(3) + 6 = 3 + 18 - 12 + 6 - 24 + 6 = -3$$

> **Catatan:** karena Hessian-nya PD di *semua* titik (konstan), $f$ convex sehingga ini minimum **global**.
> *(Di slide tertulis "3+8−12+..." , seharusnya $3+18-12+...$; hasil akhirnya tetap $-3$.)*

---

## 3. Mencari Nilai Eigen (Contoh di Slide)

**Soal:** Cari nilai eigen $M = \begin{pmatrix} 2 & -1 \\ -1 & 2 \end{pmatrix}$ dan tentukan kedefinitannya.

**Pengerjaan:**

1. $M - \lambda I = \begin{pmatrix} 2-\lambda & -1 \\ -1 & 2-\lambda \end{pmatrix}$
2. Determinan = 0:
$$(2-\lambda)^2 - 1 = 0 \Rightarrow \lambda^2 - 4\lambda + 3 = 0 \Rightarrow (\lambda-1)(\lambda-3) = 0$$
3. $\lambda_1 = 1,\ \lambda_2 = 3$
4. Keduanya positif → **$M$ definit positif**.

---

## 4. Cek Kedefinitan dengan Bentuk Kuadrat (Contoh di Slide)

**Soal:** Tunjukkan $M = \begin{pmatrix} 2 & -1 \\ -1 & 2 \end{pmatrix}$ definit positif lewat $q(x) = x^T M x$.

**Pengerjaan:**

1. $Mx = \begin{bmatrix} 2x_1 - x_2 \\ -x_1 + 2x_2 \end{bmatrix}$
2. $q(x) = x_1(2x_1 - x_2) + x_2(-x_1 + 2x_2) = 2x_1^2 - 2x_1x_2 + 2x_2^2 = 2(x_1^2 - x_1x_2 + x_2^2)$
3. Trik kuadrat sempurna:
$$x_1^2 - x_1x_2 + x_2^2 = \tfrac{1}{2}\left[(x_1 - x_2)^2 + x_1^2 + x_2^2\right] \ge 0$$
4. Bernilai 0 **hanya jika** $x_1 = x_2 = 0$. Untuk $x \ne 0$ selalu $> 0$ → **definit positif** (bukan sekadar semidefinit).

**Aturan ingat:**
- Semua $x \neq 0$ memberi hasil $> 0$ → definit positif
- Ada $x \neq 0$ yang memberi $0$ (sisanya $\ge 0$) → semidefinit positif
- Ada yang memberi positif **dan** ada yang negatif → indefinit

---

## 5. Definiteness: Dua Contoh Matriks (Slide "Buktikan")

**(a)** $\begin{pmatrix} -2 & 0 \\ 0 & -1 \end{pmatrix}$ adalah definit negatif.

Pembuktian: $q(x) = -2x_1^2 - x_2^2 < 0$ untuk semua $x \neq 0$. ✔
(Cara eigen: matriks diagonal → eigen = diagonal = $-2, -1$, keduanya negatif.)

**(b)** $\begin{pmatrix} 8 & -1 \\ -1 & 1 \end{pmatrix}$ adalah definit positif.

Pembuktian: $q(x) = 8x_1^2 - 2x_1x_2 + x_2^2 = (x_1 - x_2)^2 + 7x_1^2$.
Nol hanya jika $x_1 = 0$ dan $x_1 = x_2$, yaitu $x = 0$. Jadi $q(x) > 0$ untuk $x \ne 0$. ✔
(Cara cepat: $a = 8 > 0$, $\det = 8 - 1 = 7 > 0$.)

---

## 6. Dua Titik Stasioner dengan Jenis Berbeda (Slide "Definiteness")

**Soal:** Untuk
$$f(x) = \tfrac{1}{2}x_1^2 + x_1x_2 + 2x_2^2 - 4x_1 - 4x_2 - x_2^3$$
cari semua titik stasioner dan klasifikasikan.

**Pengerjaan:**

1. Gradien:
$$\nabla f(x) = \begin{bmatrix} x_1 + x_2 - 4 \\ x_1 + 4x_2 - 4 - 3x_2^2 \end{bmatrix}$$
2. Dari persamaan pertama: $x_1 = 4 - x_2$. Substitusi ke persamaan kedua:
$$(4 - x_2) + 4x_2 - 4 - 3x_2^2 = 0 \Rightarrow 3x_2 - 3x_2^2 = 0 \Rightarrow 3x_2(1 - x_2) = 0$$
   - $x_2 = 0 \Rightarrow x_1 = 4$ → titik $(4,0)$
   - $x_2 = 1 \Rightarrow x_1 = 3$ → titik $(3,1)$
3. Hessian:
$$\nabla^2 f(x) = \begin{pmatrix} 1 & 1 \\ 1 & 4 - 6x_2 \end{pmatrix}$$
4. Klasifikasi:

| Titik | Hessian | Cek | Kesimpulan |
|---|---|---|---|
| $(4,0)$ | $\begin{pmatrix} 1 & 1 \\ 1 & 4 \end{pmatrix}$ | $a=1>0$, $\det = 4-1 = 3 > 0$ | Definit positif → **minimum lokal** |
| $(3,1)$ | $\begin{pmatrix} 1 & 1 \\ 1 & -2 \end{pmatrix}$ | $\det = -2-1 = -3 < 0$ | Indefinit → **titik pelana** |

> Fungsi ini **non-convex** karena Hessian-nya bergantung pada $x_2$ (tidak PSD di semua titik).

---

## 7. Membuktikan Fungsi Convex (Contoh Soal 1 di Slide)

**Soal:** Apakah $f(x) = x^2$ convex?

**Cara 1: Uji Hessian (tercepat, dipakai saat kuis):**
$f'(x) = 2x$, $f''(x) = 2 > 0$ untuk semua $x$ → **convex**.

**Cara 2: Dari definisi:**

1. Ruas kiri: $f(\theta x + (1-\theta)y) = (\theta x + (1-\theta) y)^2$
2. Ruas kanan: $\theta x^2 + (1-\theta) y^2$
3. Selisih (kanan − kiri):
$$\theta x^2 + (1-\theta)y^2 - (\theta x + (1-\theta)y)^2 = \theta(1-\theta)(x-y)^2 \ge 0$$
   karena $\theta \in [0,1]$ dan kuadrat selalu $\ge 0$. Jadi pertidaksamaan convex terpenuhi → **convex**.

> **Peringatan:** mencoba satu angka saja (mis. $x=0, y=2, \theta=0.5$ → $1 \le 2$) hanya *memeriksa*, bukan *membuktikan*. Untuk membuktikan convex harus berlaku untuk **semua** $x,y,\theta$.

---

## 8. Membuktikan Fungsi TIDAK Convex (Contoh Soal di Slide)

**Soal:** Apakah $f(x) = -x^2$ convex?

**Pengerjaan (cukup satu contoh penyangkal / counterexample):**

1. Ambil $x = 0,\ y = 2,\ \theta = 0.5$.
2. Ruas kiri: $f(0.5 \cdot 0 + 0.5 \cdot 2) = f(1) = -1$
3. Ruas kanan: $0.5 f(0) + 0.5 f(2) = 0.5(0) + 0.5(-4) = -2$
4. Syarat convex menuntut $-1 \le -2$ → **SALAH**.
5. **Kesimpulan:** $f(x) = -x^2$ **tidak convex** (ia **concave**).

(Cara Hessian: $f''(x) = -2 < 0$ → tidak PSD → tidak convex.)

---

## 9. Latihan Tambahan (Kemungkinan Tipe Soal Kuis)

### Latihan 1
Cari dan klasifikasikan titik stasioner: $f(x_1,x_2) = x_1^2 + x_2^2 - 2x_1 - 4x_2 + 5$.

<details><summary>Jawaban</summary>

- $\nabla f = (2x_1 - 2,\ 2x_2 - 4) = 0 \Rightarrow (1, 2)$
- $H = \begin{pmatrix} 2 & 0 \\ 0 & 2 \end{pmatrix}$ → eigen $2, 2 > 0$ → definit positif
- **Minimum lokal di $(1,2)$**, $f(1,2) = 1 + 4 - 2 - 8 + 5 = 0$. Karena $H$ konstan PD → minimum **global**.
</details>

### Latihan 2
Klasifikasikan titik stasioner: $f(x_1,x_2) = x_1^2 - x_2^2$.

<details><summary>Jawaban</summary>

- $\nabla f = (2x_1,\ -2x_2) = 0 \Rightarrow (0,0)$
- $H = \begin{pmatrix} 2 & 0 \\ 0 & -2 \end{pmatrix}$ → eigen $2$ dan $-2$ → indefinit
- **Titik pelana di $(0,0)$.**
</details>

### Latihan 3
Klasifikasikan titik stasioner: $f(x_1,x_2) = -2x_1^2 - x_2^2 + 4x_1 - 2x_2$.

<details><summary>Jawaban</summary>

- $\nabla f = (-4x_1 + 4,\ -2x_2 - 2) = 0 \Rightarrow (1,-1)$
- $H = \begin{pmatrix} -4 & 0 \\ 0 & -2 \end{pmatrix}$ → definit negatif
- **Maksimum lokal di $(1,-1)$**, $f(1,-1) = -2 - 1 + 4 + 2 = 3$.
</details>

### Latihan 4
Tentukan kedefinitan $A = \begin{pmatrix} 1 & 2 \\ 2 & 1 \end{pmatrix}$.

<details><summary>Jawaban</summary>

- $\det(A - \lambda I) = (1-\lambda)^2 - 4 = \lambda^2 - 2\lambda - 3 = (\lambda - 3)(\lambda + 1) = 0$
- $\lambda = 3$ dan $\lambda = -1$ → **indefinit**
- (Cek cepat: $\det = 1 - 4 = -3 < 0$.)
</details>

### Latihan 5
Convex atau tidak? (a) $f(x) = x^4$  (b) $f(x) = x^3$  (c) $f(x_1,x_2) = x_1^2 + x_1x_2 + x_2^2$  (d) $f(x_1,x_2) = x_1x_2$

<details><summary>Jawaban</summary>

- (a) $f'' = 12x^2 \ge 0$ untuk semua $x$ → **convex**.
- (b) $f'' = 6x$, negatif untuk $x < 0$ → **tidak convex**.
- (c) $H = \begin{pmatrix} 2 & 1 \\ 1 & 2 \end{pmatrix}$, eigen $1, 3 > 0$ → PD → **convex**.
- (d) $H = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$, eigen $\pm 1$ → indefinit → **tidak convex**.
</details>

---

## 10. Konsep Teori yang Sering Keluar (Singkat)

- **Syarat perlu orde-1 (FONC):** jika $x^*$ minimum lokal, maka $\nabla f(x^*) = 0$. Ini **perlu tapi belum cukup**: titik itu bisa min, maks, atau pelana.
- **Syarat orde-2:** Hessian di $x^*$ harus semidefinit positif (perlu). Jika definit positif (cukup), maka $x^*$ minimum lokal.
- **Gradien** menunjuk arah pertumbuhan tercepat fungsi.
- **Titik stasioner:** titik $x_s$ dengan $\nabla f(x_s) = 0$.
- **Non-convex:** bisa punya banyak minimum/maksimum lokal, sehingga sulit menemukan optimum global.
- **Convex:** setiap minimum lokal adalah minimum global.
- Jika Hessian semidefinit (ada $\lambda=0$), uji orde-2 **tidak konklusif**.

---

## 11. Strategi Cepat Saat Kuis

1. **Soal "cari optimum":** gradien → set 0 → selesaikan sistem → Hessian → cek $a$ dan $\det$ → tulis kesimpulan.
2. **Soal "kedefinitan matriks":** untuk 2×2 pakai $a$ dan $\det$; jika diminta cara eigen, tulis persamaan karakteristik lalu akar-akarnya.
3. **Soal "convex atau bukan":** hitung Hessian. PSD untuk semua $x$ → convex. Kalau diminta pakai definisi dan Anda yakin *tidak* convex, cari **satu contoh penyangkal**.
4. **Jangan lupa** menyebut jenis titik (minimum lokal / maksimum lokal / pelana) dan nilai $f(x^*)$ bila diminta.

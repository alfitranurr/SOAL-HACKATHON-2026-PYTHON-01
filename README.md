# HACK-2026-PYTHON-01: Retail Crisis & Recovery Analysis System 🛒📉📈

[![Python Version](https://img.shields.io/badge/python-3.9%20%7C%203.10%20%7C%203.11-blue.svg)](https://www.python.org)
[![Data Analysis](https://img.shields.io/badge/analytics-Pandas%20%7C%20Numpy-orange.svg)]()
[![Machine Learning](https://img.shields.io/badge/ML-Apriori%20%7C%20Association%20Rules-green.svg)]()

Sistem analisis cerdas berbasis Python yang dirancang untuk mengatasi fenomena penurunan penjualan (*Retail Crisis*) pada **DQFresh Mart Retail**. Proyek ini berfokus pada otomatisasi pencarian produk potensial baru (*Rising Star*) dan rekomendasi bundling produk (*Potential Packaging*) untuk memicu pemulihan omzet toko (*Recovery*).

---

## 1. Pengenalan Project 📌

### Latar Belakang

DQFresh Mart Retail mengalami penurunan total nilai penjualan yang signifikan selama 6 bulan terakhir akibat berkurangnya volume pengunjung. Strategi awal manajemen yang hanya mempertahankan produk *bestseller* tradisional terbukti gagal menahan laju penurunan profit.

Project ini hadir untuk membuktikan hipotesis bahwa **terdapat produk-produk non-bestseller yang justru menunjukkan tren pertumbuhan konsisten di tengah krisis (Rising Star)** serta mendeteksi **pola kombinasi pembelian tersembunyi dari konsumen (Potential Packaging)** untuk dioptimalkan sebagai strategi *cross-selling*.

### Tujuan Utama

* **Otomatisasi Deteksi Rising Star:** Mengidentifikasi produk dengan performa penjualan yang tumbuh konsisten berdasarkan algoritma run-streak harian dan pertumbuhan persentase nilai penjualan.
* **Optimasi Market Basket Analysis (MBA):** Menemukan kombinasi produk strategis menggunakan algoritma Apriori untuk dijadikan paket promosi (*bundling*).
* **Visualisasi Performa Eksekutif:** Menghasilkan grafik tren penjualan aktual dan performa indeks berbasis 100 untuk keperluan pengambilan keputusan manajemen.

---

## 2. Arsitektur Sistem 🏗️

Sistem ini dibangun dengan pendekatan *High-Performance Pure Vectorization* menggunakan komponen arsitektur modular sebagai berikut:

```text
                                ┌──────────────────────┐
                                │  data_penjualan.csv  │
                                └──────────┬───────────┘
                                           │
                                           ▼
                         ┌─────────────────────────────────┐
                         │   Data Preprocessing Engine     │
                         │ (DateTime Parsing & Memory Opt) │
                         └────────────────┬────────────────┘
                                           │
                  ┌───────────────────────┴───────────────────────┐
                  ▼                                               ▼
    ┌───────────────────────────┐                   ┌───────────────────────────┐
    │     Rising Star Engine     │                   │Potential Packaging Engine │
    ├───────────────────────────┤                   ├───────────────────────────┤
    │ - Daily Rolling Aggregate │                   │ - Basket Matrix Pivot     │
    │ - Run-Streak Vectorization│                   │ - Apriori Algorithm       │
    │ - Growth YoY/MoM Metrics  │                   │ - Association Rules Extr. │
    └─────────────┬─────────────┘                   └─────────────┬─────────────┘
                  │                                               │
                  ▼                                               ▼
    ┌───────────────────────────┐                   ┌───────────────────────────┐
    │    Visualization Engine   │                   │        Export Engine      │
    ├───────────────────────────┤                   ├───────────────────────────┤
    │ - rising_star_actual.png  │                   │ - retail_insight.xlsx     │
    │ - rising_star_index.png   │                   │   (Multi-Sheet Report)    │
    └───────────────────────────┘                   └───────────────────────────┘

```

### Teknologi & Library yang Digunakan

* **Pandas & NumPy:** Komputasi matriks dan manipulasi data terstruktur berskala besar.
* **Mlxtend (Apriori & Association Rules):** Penambangan pola frekuensi tinggi (*Frequent Pattern Mining*) untuk analisis keranjang belanja.
* **Matplotlib:** Generator grafik analitis berkualitas tinggi.
* **Openpyxl:** Engine penulisan laporan akhir berformat spreadsheet Excel (`.xlsx`).

---

## 3. Struktur Repositori 📁

```text
.
├── data_penjualan.csv                 # Dataset transaksi mentah (input utama)
├── solusi-retail.py                   # Script engine analisis utama berbasis Python
├── retail_insight.xlsx                # Output laporan Excel (Generated)
│   ├── Sheet: Rising Star
│   └── Sheet: Potential Packaging
├── rising_star_actual.png             # Grafik tren nilai penjualan asli (Generated)
├── rising_star_index.png              # Grafik tren performa indeks base 100 (Generated)
├── SOAL-HACKATHON.pdf                 # Dokumen spesifikasi dan acuan studi kasus
└── README.md                          # Dokumentasi teknis proyek

```

---

## 4. Dataset & Fitur 📊

Sistem membaca data historis penjualan langsung dari file **`data_penjualan.csv`**. Format struktur dataset terdiri dari kolom-kolom berikut:

| Nama Kolom | Tipe Data | Deskripsi |
| --- | --- | --- |
| `nomor_struk` | Object / String | ID Unik untuk setiap transaksi struk belanja |
| `tgl_transaksi` | Object / String | Tanggal terjadinya transaksi pembelian |
| `kode_produk` | Object / String | Kode SKU unik untuk tiap item barang |
| `nama_produk` | Object / String | Nama deskriptif produk |
| `jumlah_terjual` | Integer | Kuantitas produk yang dibeli dalam satu struk |
| `harga` | Integer / Float | Harga satuan produk |
| `total_nilai` | Integer / Float | Total nominal pendapatan (`jumlah_terjual` $\times$ `harga`) |

---

## 5. Metodologi Analisis 🧠

### A. Algoritma Rising Star

Kriteria produk diklasifikasikan sebagai *Rising Star* apabila memenuhi kondisi empiris berikut:

1. **Consecutive Growth Days (Run-Streak):** Dihitung menggunakan teknik vektorisasi *cumsum* bergulir untuk mendeteksi berapa hari berturut-turut penjualan harian produk bernilai positif (lebih besar dari 0).
2. **Growth Percentage:** Total nilai penjualan periode berjalan dibandingkan dengan basis performa awal untuk melihat akselerasi minat pasar.

### B. Market Basket Analysis (Potential Packaging)

Menggunakan algoritma **Apriori** untuk ekstraksi aturan asosiasi (*Association Rules Mapping*) dengan parameter kontrol kualitas:

* **Support:** Mengukur seberapa sering kombinasi produk muncul dalam keseluruhan transaksi.
* **Confidence:** Mengukur seberapa kuat hubungan antar produk (jika membeli Produk A, seberapa besar probabilitas membeli Produk B).
* **Lift Ratio:** Jika Nilai Lift lebih besar dari 1, menunjukkan bahwa hubungan antar item valid dan bukan merupakan kebetulan belaka.

---

## 6. Cara Menjalankan Project 🚀

### 1. Kloning Repositori

```bash
git clone [https://github.com/alfitranurr/SOAL-HACKATHON-2026-PYTHON-01.git](https://github.com/alfitranurr/SOAL-HACKATHON-2026-PYTHON-01.git)
cd SOAL-HACKATHON-2026-PYTHON-01

```

### 2. Instalasi Dependensi

Pastikan Anda telah menginstal pustaka Python yang diperlukan sebelum mengeksekusi sistem:

```bash
pip install pandas numpy matplotlib mlxtend openpyxl

```

### 3. Eksekusi Script Utama

Jalankan engine analisis untuk memproses data transaksi dan memproduksi hasil rekomendasi:

```bash
python solusi-retail.py

```

Setelah proses selesai, sistem otomatis memproduksi berkas output `retail_insight.xlsx`, `rising_star_actual.png`, dan `rising_star_index.png` pada direktori aktif Anda.

---

## 7. Hasil & Visualisasi 📈

### A. Evaluasi Metrik Rising Star

Berdasarkan hasil pemrosesan data, sistem berhasil mengidentifikasi **Top 4 Produk Rising Star** yang memiliki resiliensi tinggi dan performa pertumbuhan konsisten di tengah periode krisis makro retail:

| Peringkat | Nama Produk | Run-Streak Tertinggi | Tren Pertumbuhan | Status Rekomendasi |
| --- | --- | --- | --- | --- |
| **1** | Wajan Enamel Anti Lengket | 24 Hari | Sangat Agresif | Promosi Utama / Display Depan |
| **2** | Sabun Cuci Cair 1.5L | 24 Hari | Konsisten | Penjaga Stabilitas Volume |
| **3** | Beras Premium 5kg | 20 Hari | Stabil Naik | Komoditas Pengikat Pelanggan |
| **4** | Minyak Goreng Refill 1L | 16 Hari | Akselerasi Baru | Target Bundling Strategis |

---

### B. Analisis Grafik Visualisasi

#### 1. Tren Nilai Penjualan Aktual (`rising_star_actual.png`)

Grafik ini memetakan fluktuasi nominal pendapatan harian riil (dalam Rupiah) antara produk retail tradisional (*Top Sales Legacy*) dengan produk *Rising Star* terpilih (*Minyak Goreng Refill 1L*).

> 💡 **Insight Analitis (Actual Trend):**
> Meskipun secara nominal harian produk *Top Sales Legacy* masih menghasilkan angka yang besar, grafiknya menunjukkan tren *slope* negatif yang terus menurun (pembusukan pasar). Sebaliknya, *Minyak Goreng Refill 1L* merangkak naik secara pasti dari dasar dengan volatilitas yang minim. Ini membuktikan bahwa produk penopang lama sudah jenuh, dan manajemen harus segera mengalihkan modal kerja ke produk komoditas esensial baru ini.

#### 2. Tren Performa Indeks Skala Base 100 (`rising_star_index.png`)

Untuk menghilangkan bias ukuran nominal akibat perbedaan harga barang, visualisasi di bawah ini menggunakan transformasi indeks berbasis 100 (titik awal performa disamakan di angka 100).

> 💡 **Insight Analitis (Index Performance):**
> Melalui kacamata Indeks Base 100, terlihat jelas deviasi performa yang masif. Produk *Top Sales Legacy* mengalami penurunan performa indeks hingga ke bawah **80%** dari kapasitas awalnya. Di sisi lain, *Minyak Goreng Refill 1L* melesat konsisten hingga menembus indeks **180%** (Pertumbuhan lebih dari 80%). Transformasi ini menegaskan bahwa akselerasi kecepatan tumbuh *Rising Star* jauh mengungguli daya tahan produk lama.

---

### C. Strategi Potential Packaging (Market Basket Analysis)

Hasil ekstraksi algoritma Apriori pada file `retail_insight.xlsx` memunculkan *pola belanja tersembunyi* yang sangat kuat. Kombinasi aturan asosiasi terbaik yang ditemukan adalah:

**{Minyak Goreng Refill 1L} → {Beras Premium 5kg}**

* **Nilai Support:** 0.14 (Muncul di 14% dari total seluruh struk belanja).
* **Nilai Confidence:** 0.78 (78% konsumen yang membeli Minyak Goreng dipastikan akan membeli Beras Premium).
* **Lift Ratio:** 2.45 (Angka lebih besar dari 1 menandakan hubungan asosiasi ini bersifat kausalitas kuat, bukan kebetulan).

> 🛒 **Rekomendasi Aksi Korporasi (Recovery Strategy):**
> 1. **Cross-Selling Bundling:** Buat paket hemat "Sembako Pemulihan" yang menyatukan *Minyak Goreng Refill 1L* dan *Beras Premium 5kg* dengan potongan harga tipis untuk menstimulus peningkatan *Average Transaction Value* (ATV).
> 2. **Layouting Restrukturisasi:** Tempatkan produk *Wajan Enamel Anti Lengket* (Streak 24 Hari) di koridor yang sama atau berdekatan dengan rak *Minyak Goreng* untuk memicu *impulse buying* (pembelian spontan).
> 
> 

---

## 8. Identitas Pengembang / Peneliti 👤

* **Nama Pengembang:** Al Fitra Nur Ramadhani
* **Peran/Posisi:** Data Scientist & Core System Architect
* **Kontak GitHub:** [@alfitranurr](https://www.google.com/search?q=https://github.com/alfitranurr)
* **Afiliasi/Sertifikasi:** DQLab Hackathon Python Solution Challenger

---

*Dokumentasi ini dibuat sebagai standar resmi reproduksibilitas riset data retail crisis 2026.*
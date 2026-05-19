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
    │    Rising Star Engine     │                   │Potential Packaging Engine │
    ├───────────────────────────┤                   ├───────────────────────────┤
    │ - Daily Rolling Aggregate │                   │ - Basket Matrix Pivot     │
    │ - Run-Streak Vectorization│                   │ - Apriori Algorithm       │
    │ - Growth YoY/MoM Metrics  │                   │ - Association Rules Extr. │
    └─────────────┬─────────────┘                   └─────────────┬─────────────┘
                  │                                               │
                  ▼                                               ▼
    ┌───────────────────────────┐                   ┌───────────────────────────┐
    │   Visualization Engine    │                   │      Export Engine        │
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
| `tgl_transaksi` | Date (DD-MM-YYYY) | Tanggal terjadinya transaksi pembelian |
| `kode_produk` | Object / String | Kode SKU unik untuk tiap item barang |
| `nama_produk` | Object / String | Nama deskriptif produk |
| `jumlah_terjual` | Integer | Kuantitas produk yang dibeli dalam satu struk |
| `harga` | Integer / Float | Harga satuan produk |
| `total_nilai` | Integer / Float | Total nominal pendapatan (`jumlah_terjual` $\times$ `harga`) |

---

## 5. Metodologi Analisis 🧠

### A. Algoritma Rising Star

Kriteria produk diklasifikasikan sebagai *Rising Star* apabila memenuhi kondisi empiris berikut:

1. **Consecutive Growth Days (Run-Streak):** Dihitung menggunakan teknik vektorisasi *cumsum* bergulir untuk mendeteksi berapa hari berturut-turut penjualan harian produk bernilai positif (> 0).
2. **Growth Percentage:** Total nilai penjualan periode berjalan dibandingkan dengan basis performa awal untuk melihat akselerasi minat pasar.

### B. Market Basket Analysis (Potential Packaging)

Menggunakan algoritma **Apriori** untuk ekstraksi aturan asosiasi (*Association Rules Mapping*) dengan parameter kontrol kualitas:

* **Support:** Mengukur seberapa sering kombinasi produk muncul dalam keseluruhan transaksi.
* **Confidence:** Mengukur seberapa kuat hubungan antar produk (jika membeli Produk A, seberapa besar probabilitas membeli Produk B).
* **Lift Ratio:** Jika Nilai $\text{Lift} > 1$, menunjukkan bahwa hubungan antar item valid dan bukan merupakan kebetulan belaka.

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

### Ringkasan Insight Hasil Eksekusi:

* **Top 4 Produk Rising Star Terpilih:**
1. *Wajan Enamel Anti Lengket* (Streak tertinggi: 24 Hari)
2. *Sabun Cuci Cair 1.5L* (Streak tertinggi: 24 Hari)
3. *Beras Premium 5kg* (Streak tertinggi: 20 Hari)
4. *Minyak Goreng Refill 1L* (Streak tertinggi: 16 Hari)



### Output Visualisasi Grafik:

Sistem menghasilkan dua buah visualisasi tren komparatif antara produk kompetitor tradisional (*Top Sales Bestseller*) dengan produk *Rising Star* utama (*Minyak Goreng Refill 1L*):

1. **`rising_star_actual.png`** : Menampilkan visualisasi nominal pendapatan riil harian, memperlihatkan fluktuasi omzet nyata di lapangan.
2. **`rising_star_index.png`** : Menampilkan indeks performa (Skala Base 100), memberikan visualisasi standarisasi pertumbuhan yang adil tanpa terpengaruh perbedaan nominal harga dasar produk.

---

## 8. Identitas Pengembang / Peneliti 👤

* **Nama Pengembang:** Al Fitra Nur Ramadhani
* **Peran/Posisi:** Data Scientist & Core System Architect
* **Kontak GitHub:** [@alfitranurr](https://www.google.com/search?q=https://github.com/alfitranurr)
* **Afiliasi/Sertifikasi:** DQLab Hackathon Python Solution Challenger

---

*Dokumentasi ini dibuat sebagai standar reproduksibilitas riset data retail crisis 2026.*
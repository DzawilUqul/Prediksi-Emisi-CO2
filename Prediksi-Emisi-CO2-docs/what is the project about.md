# Apa Isi Proyek Ini? (What is the Project About)

## Ringkasan

Proyek ini adalah aplikasi web berbasis **Streamlit** bernama **CO₂ Emissions Estimation** yang berfungsi untuk **menganalisis dan memprediksi emisi karbon dioksida (CO₂)** dari provinsi-provinsi di **China**. Aplikasi ini memadukan data sosio-ekonomi dengan teknik *machine learning* untuk memahami faktor pendorong emisi serta memperkirakan tren emisi di masa depan.

Aplikasi ini disusun sebagai bagian dari **uji sertifikasi LSP (Lembaga Sertifikasi Profesi) Universitas Dian Nuswantoro** oleh:

- **Dzawil Uqul** — NIM A11.2022.14141
- **Najma Amira Mumtaz** — NIM A11.2022.14708

## Latar Belakang

Di tengah urgensi krisis iklim global, emisi CO₂ menjadi salah satu penyumbang terbesar perubahan iklim. Untuk merumuskan solusi yang tepat, pemahaman terhadap faktor-faktor pendorong emisi perlu dikaji tidak hanya di tingkat global, tetapi juga pada skala regional tempat kebijakan dapat diterapkan langsung.

Analisis ini berfokus pada provinsi-provinsi di China untuk memahami bagaimana **pertumbuhan ekonomi, dinamika kependudukan, dan perubahan struktur industri** secara bersama-sama membentuk jejak karbon suatu wilayah.

## Tujuan

1. Menganalisis faktor-faktor sosio-ekonomi yang memengaruhi emisi CO₂.
2. Memprediksi emisi CO₂ berdasarkan skenario faktor tertentu (*what-if*).
3. Memperkirakan tren emisi masa depan tiap provinsi berdasarkan data historis.
4. Menyajikan hasil analisis melalui dashboard interaktif yang mudah dipahami.

## Dataset

- **Sumber**: [Data for: Spatial Characteristics and Future Forecasting of Carbon Dioxide Emissions in China: A Provincial-Level Analysis](https://data.mendeley.com/datasets/rp3f7mdjxz/1)
- **DOI**: [10.17632/rp3f7mdjxz.1](https://doi.org/10.17632/rp3f7mdjxz.1)
- **Cakupan**: 31 provinsi di China, periode **1999–2019** (21 tahun).
- **Jumlah data**: 651 baris (31 provinsi × 21 tahun).

### Kolom Dataset

| Kolom | Deskripsi |
| --- | --- |
| `Name` | Nama provinsi di China |
| `Year` | Tahun pencatatan (1999–2019) |
| `per capita gdp(yuan)` | GDP per kapita (Yuan China) |
| `total population(million)` | Total populasi (juta jiwa) |
| `urbanization rate(%)` | Persentase penduduk perkotaan |
| `proportion of primary industry(%)` | Kontribusi sektor primer terhadap GDP |
| `proportion of secondary industry(%)` | Kontribusi sektor sekunder terhadap GDP |
| `proportion of the tertiary industry(%)` | Kontribusi sektor tersier terhadap GDP |
| `coal proportion(%)` | Persentase penggunaan batu bara dalam konsumsi energi |
| `Total carbon dioxide emissions (million tons)` | **Target prediksi** — total emisi CO₂ (juta ton) |

## Metode yang Digunakan

Aplikasi menggunakan **dua pendekatan utama**:

### 1. Regresi (Gradient Boosting Regressor)
- Model dilatih menggunakan **seluruh dataset** dari semua provinsi.
- Mempelajari hubungan kompleks antara emisi CO₂ dan faktor sosio-ekonomi (GDP, populasi, urbanisasi, struktur industri, penggunaan batu bara).
- Memungkinkan pembuatan skenario *what-if*: bagaimana perubahan kebijakan/ekonomi memengaruhi emisi.
- Notebook: [Prediksi_Emisi_CO₂_Regresi.ipynb](https://colab.research.google.com/drive/1gKiGprGtOf1U0LJpDtPk0fLd7gZTRHrb?usp=sharing)

### 2. Forecasting Time-Series (ARIMA)
- Model **ARIMA** dilatih **per provinsi** secara individual (31 model).
- Fokus pada tren emisi historis tiap provinsi.
- Memberikan perkiraan "dasar" arah emisi jika momentum historis berlanjut tanpa perubahan eksternal signifikan.
- Notebook: [Prediksi_Emisi_CO₂_Forecasting.ipynb](https://colab.research.google.com/drive/141thc0NK_SIjM0JK--x4013xN38LXvmA?usp=sharing)

## Fitur Aplikasi

### Beranda
- **Tab Deskripsi**: latar belakang, deskripsi proyek, dan penjelasan metode.
- **Tab Gambaran Umum Dataset**: sumber data, deskripsi kolom, pratinjau data mentah & bersih, langkah pra-pemrosesan, serta visualisasi data (heatmap korelasi, tren emisi nasional, tren per provinsi, distribusi fitur).

### Prediksi
- **Prediksi Berdasarkan Faktor (Regresi)**: pengguna mengisi nilai GDP, populasi, urbanisasi, proporsi industri, dan batu bara, lalu mendapat estimasi emisi CO₂ beserta penjelasan level tiap faktor (L/M/H).
- **Prediksi Tren Historis (ARIMA)**: pengguna memilih provinsi dan jumlah tahun (1–5), lalu melihat grafik prediksi dengan interval kepercayaan 95%.

### Tentang Penyusun
- Profil tim penyusun beserta tautan LinkedIn.

## Manfaat

Aplikasi ini membantu pengguna (akademisi, pembuat kebijakan, atau masyarakat umum) untuk:
- Memahami keterkaitan faktor ekonomi dan emisi karbon.
- Menyusun skenario kebijakan berbasis data.
- Melihat proyeksi tren emisi tiap provinsi China hingga beberapa tahun ke depan.

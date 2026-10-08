# Alur Proyek (Flow Project)

Dokumen ini menjelaskan alur kerja aplikasi **Prediksi Emisi CO₂**, mulai dari pemuatan berkas hingga penyajian hasil prediksi ke pengguna. Aplikasi dibangun dengan **Streamlit** (`app.py`).

## 1. Diagram Alur Umum

```
Mulai
  │
  ▼
Pemuatan Konfigurasi Halaman (set_page_config)
  │
  ▼
Pemuatan Data & Model (dengan cache)
  │
  ├── dataset.csv            → data mentah
  ├── cleaned_dataset.csv    → data bersih
  ├── gradient_boosting_model.pkl      → model regresi
  └── province_models_*.pkl  → 31 model ARIMA
  │
  ▼
Perhitungan Kuartil (Q1, Q3) untuk kategori Level (L/M/H)
  │
  ▼
Navigasi Sidebar
  │
  ├── Beranda
  ├── Prediksi
  └── Tentang Penyusun
  │
  ▼
Selesai
```

## 2. Tahap Pemuatan Data dan Model

Semua pemuatan menggunakan decorator cache agar efisien:

| Fungsi | Tipe Cache | Berkas | Keterangan |
| --- | --- | --- | --- |
| `load_raw_data` | `@st.cache_data` | `dataset.csv` | Delimiter `;` |
| `load_cleaned_data` | `@st.cache_data` | `cleaned_dataset.csv` | Data siap model |
| `load_model` | `@st.cache_resource` | `.pkl` (regresi & ARIMA) | Memuat objek model |

## 3. Perhitungan Level (L / M / H)

- Aplikasi menghitung **kuartil 25% (Q1)** dan **75% (Q3)** dari dataset bersih.
- Fungsi `get_level(value, column)` mengklasifikasikan sebuah nilai:
  - **L (Low)** → nilai < Q1
  - **M (Medium)** → Q1 ≤ nilai ≤ Q3
  - **H (High)** → nilai > Q3
- Klasifikasi ini dipakai untuk menjelaskan hasil prediksi secara kontekstual.

## 4. Struktur Navigasi

Navigasi menggunakan `st.session_state.page` dan tombol di sidebar:

```
Sidebar
 ├── Beranda
 │     ├── Tab "Deskripsi"           → latar belakang, deskripsi proyek, metode
 │     └── Tab "Gambaran Umum Dataset"
 │           ├── Tentang Dataset (sumber + deskripsi kolom)
 │           ├── Pratinjau Dataset Mentah
 │           ├── Langkah Pra-pemrosesan
 │           ├── Pratinjau Dataset Bersih
 │           └── Visualisasi Data
 │                 ├── Heatmap Korelasi
 │                 ├── Tren Emisi Nasional
 │                 ├── Tren Emisi per Provinsi
 │                 └── Distribusi Fitur
 │
 ├── Prediksi
 │     ├── Tab "Prediksi Berdasarkan Faktor" (Regresi)
 │     └── Tab "Prediksi Tren Historis" (ARIMA / Forecasting)
 │
 └── Tentang Penyusun                  → profil tim
```

## 5. Alur Prediksi Berdasarkan Faktor (Regresi)

```
Pengguna mengisi form:
  Province, Year, GDP per kapita, Populasi,
  Urbanisasi, % Industri Primer/Sekunder/Tersier,
  % Batu Bara
          │
          ▼
Data input diubah menjadi DataFrame
          │
          ▼
regression_model.predict(input_data_df)
          │
          ▼
Hasil prediksi (juta ton) ditampilkan
          │
          ▼
Penjelasan otomatis:
  - Setiap faktor diberi Level (L/M/H)
  - Hasil prediksi dibandingkan dengan data historis
```

## 6. Alur Prediksi Tren Historis (ARIMA)

```
Pengguna memilih provinsi & jumlah tahun (1–5)
          │
          ▼
Memuat model ARIMA sesuai provinsi
(province_models_fixed_order/arima_model_<Provinsi>.pkl)
          │
          ▼
province_ts = data historis (year, total_emissions)
          │
          ▼
forecast = arima_model.get_forecast(steps=n)
   - mean (hasil prediksi)
   - mean_ci_lower / mean_ci_upper (interval kepercayaan 95%)
          │
          ▼
Nilai negatif di-clip ke 0
          │
          ▼
Visualisasi Plotly:
  - Garis historis
  - Garis prediksi (putus-putus)
  - Area interval kepercayaan 95%
          │
          ▼
Tabel data prediksi ditampilkan
```

## 7. Alur Pra-pemrosesan Dataset (Ringkasan)

1. **Standarisasi nama kolom** → format `snake_case`.
2. **Penanganan missing value** pada kolom `name` → forward-fill (`ffill`).
3. **Konversi tipe data** → koma diganti titik, string menjadi `float`.
4. **Penanganan outlier ekstrem** → nilai anomali diganti rata-rata provinsi.
5. Hasil akhir disimpan sebagai `cleaned_dataset.csv`.

## 8. Ringkasan Teknologi

| Komponen | Teknologi |
| --- | --- |
| Antarmuka Web | Streamlit |
| Manipulasi Data | pandas, numpy |
| Model Regresi | Gradient Boosting Regressor (scikit-learn) |
| Model Time-Series | ARIMA (statsmodels) |
| Visualisasi | Plotly (graph_objects, express, subplots) |

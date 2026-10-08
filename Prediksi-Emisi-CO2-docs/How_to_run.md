# Cara Menjalankan Proyek (How to Run)

Aplikasi ini adalah aplikasi web berbasis **Streamlit** untuk analisis dan prediksi emisi CO₂ provinsi-provinsi di China. Ikuti langkah berikut untuk menjalankannya di komputer lokal.

## 1. Prasyarat

- **Python 3.12** (disarankan) sudah terpasang.
- **pip** sudah tersedia.
- Git (opsional, untuk mengkloning repositori).

Cek versi Python:

```bash
python --version
```

## 2. Dapatkan Kode Proyek

Jika belum punya, kloning repositori lalu masuk ke foldernya:

```bash
git clone <url-repositori>
cd Prediksi-Emisi-CO2
```

## 3. (Disarankan) Buat Virtual Environment

**Windows (Git Bash / CMD / PowerShell):**

```bash
python -m venv venv
venv\Scripts\activate
```

**Linux / macOS:**

```bash
python -m venv venv
source venv/bin/activate
```

## 4. Instal Dependensi

Semua dependensi sudah dicantumkan di `requirements.txt`:

```bash
pip install -r requirements.txt
```

Isi `requirements.txt`:

| Paket | Versi |
| --- | --- |
| streamlit | (terbaru) |
| scikit-learn | 1.6.1 |
| pandas | 2.2.2 |
| statsmodels | 0.14.5 |
| plotly | 5.24.1 |
| numpy | 2.3.2 |

## 5. Jalankan Aplikasi

Dari folder proyek, jalankan:

```bash
streamlit run app.py
```

Setelah berjalan, Streamlit akan menampilkan alamat seperti:

```
Local URL: http://localhost:8501
```

Buka alamat tersebut di browser. Untuk menghentikan server tekan `Ctrl + C`.

## 6. Opsi Menjalankan (Tambahan)

```bash
# Menentukan port tertentu
streamlit run app.py --server.port 8501

# Mode headless (tanpa membuka browser otomatis)
streamlit run app.py --server.headless true
```

## 7. Struktur File Penting

```
Prediksi-Emisi-CO2/
├── app.py                          # Aplikasi utama Streamlit
├── requirements.txt                # Daftar dependensi
├── .streamlit/config.toml          # Konfigurasi tema
├── dataset.csv                     # Dataset mentah
├── cleaned_dataset.csv             # Dataset hasil pembersihan
├── gradient_boosting_model.pkl     # Model regresi Gradient Boosting
├── province_models_fixed_order/    # 31 model ARIMA (per provinsi)
├── Header_Streamlit.png            # Aset gambar header
├── Regression.png                  # Aset gambar
├── forecasting.jpg                 # Aset gambar
├── dzawil.jpg / najma.jpg          # Foto penyusun
└── Prediksi-Emisi-CO2-docs/        # Dokumentasi proyek
    ├── How_to_run.md
    ├── flow_project.md
    └── what is the project about.md
```

## 8. Troubleshooting

- **`streamlit: command not found`** → pastikan virtual environment aktif dan dependensi telah diinstal, atau jalankan dengan `python -m streamlit run app.py`.
- **Error memuat file/dataset** → pastikan seluruh file (`dataset.csv`, `cleaned_dataset.csv`, `gradient_boosting_model.pkl`, folder `province_models_fixed_order/`) berada di direktori yang sama dan dijalankan dari folder root proyek.
- **Konflik versi** → gunakan versi paket sesuai `requirements.txt` agar model `.pkl` dapat dimuat dengan benar.
- **Error `MT19937 is not a known BitGenerator module` (model gagal dimuat)** → model `.pkl` dibuat dengan **numpy 2.x**. Pastikan numpy yang terpasang sesuai, yaitu `numpy==2.3.2`:
  ```bash
  pip install "numpy==2.3.2"
  ```
  Jika numpy versi lama (1.x) tetap terpasang karena dipakai proyek lain, gunakan **virtual environment** khusus proyek ini agar tidak saling mengganggu.

# Time Series Analysis: ARIMA and XGBoost Model for Predicting Monthly Exchange Rate of the Yuan Against Rupiah (CNY/IDR)

[cite_start]Proyek ini berfokus pada analisis deret waktu (*time series analysis*) dan pemodelan prediktif untuk meramalkan pergerakan nilai tukar bulanan mata uang Yuan (CNY) terhadap Rupiah (IDR)[cite: 702]. [cite_start]Nilai tukar yang stabil dan terprediksi sangat krusial mengingat eratnya hubungan perdagangan serta investasi bilateral antara Indonesia dan China[cite: 716].

## Latar Belakang
[cite_start]Hubungan perdagangan, aktivitas ekspor-impor, serta arus investasi asing dari China ke Indonesia terus mengalami dinamika dari tahun ke tahun[cite: 716]. [cite_start]Fluktuasi nilai tukar CNY/IDR secara langsung memengaruhi berbagai aspek ekonomi, mulai dari penetapan harga produk, nilai riil investasi, anggaran pariwisata, hingga volume peredaran uang[cite: 716]. [cite_start]Proyek ini membandingkan pendekatan statistik klasik (**ARIMA**) dengan pendekatan *machine learning* berbasis *tree-boosting* (**XGBoost**) untuk mendapatkan akurasi prediksi terbaik[cite: 758, 958].

## Metodologi & Alur Kerja

### 1. Data Pre-Processing
* [cite_start]**Sumber Data:** Data historis nilai tukar bulanan diambil melalui Google Finance[cite: 760].
* [cite_start]**Pembersihan Data:** Identifikasi serta penghapusan *outliers* dan pengujian efek musiman (*seasonality test*)[cite: 760].
* [cite_start]**Pembagian Data:** Data dibagi menjadi 80% untuk *Training Set* dan 20% untuk *Test Set*[cite: 761].

### 2. Pemodelan ARIMA
* [cite_start]Pengujian stasioneritas data menggunakan **Augmented Dickey-Fuller (ADF) Test**[cite: 763].
* [cite_start]Hasil ADF Test sebelum *differencing* menunjukkan data tidak stasioner ($p\text{-value} = 0.181$)[cite: 831, 859]. [cite_start]Setelah proses *differencing* pertama ($d=1$), data terbukti stasioner ($p\text{-value} = 3.05 \times 10^{-17}$)[cite: 853, 858].
* [cite_start]Identifikasi lag melalui plot ACF (*Autocorrelation Function*) dan PACF (*Partial Autocorrelation Function*) menghasilkan model terbaik **ARIMA (0,1,0)** dengan nilai AIC terendah[cite: 764, 861, 862].
* [cite_start]Evaluasi residu menggunakan **Ljung-Box Test** menghasilkan $p\text{-value} > 0.05$, membuktikan residu bebas dari autokorelasi signifikan (*white noise*)[cite: 873, 876, 877].

### 3. Pemodelan XGBoost Regression
* [cite_start]Transformasi data deret waktu menjadi fitur berbasis tanggal (*date-based features* seperti *year*, *month*, dan *dayofyear*)[cite: 768].
* [cite_start]Melakukan **Hyperparameter Tuning** dengan metode *Cross-Validation* (total 6.912 kombinasi fit)[cite: 905].
* [cite_start]Parameter optimal yang ditemukan: `colsample_bytree: 0.8`, `gamma: 0.2`, `learning_rate: 0.1`, `max_depth: 7`, `n_estimators: 1000`, `subsample: 0.8`[cite: 905].

---

## Hasil Evaluasi & Perbandingan Model

### Performa Validasi XGBoost (Data Training):
* [cite_start]**MAE:** 1.2244 [cite: 908]
* [cite_start]**MSE:** 3.3054 [cite: 908]
* [cite_start]**RMSE:** 1.8180 [cite: 908]
* [cite_start]**R² Score:** 0.9996 [cite: 908]

### Feature Importance (XGBoost)
Berdasarkan kontribusi fitur terhadap model XGBoost, urutan fitur yang paling memengaruhi prediksi adalah:
1. [cite_start]**Year** (Tahun) — Kontribusi tertinggi[cite: 909, 912].
2. [cite_start]**Dayofyear** (Hari ke-n dalam tahun)[cite: 910, 912].
3. [cite_start]**Month** (Bulan)[cite: 911, 912].

### Kesimpulan Akhir
[cite_start]Penelitian ini berhasil meramalkan nilai tukar bulanan Yuan terhadap Rupiah secara akurat[cite: 958]. [cite_start]Secara keseluruhan, **XGBoost memberikan hasil prediksi yang lebih presisi dan adaptif** dibandingkan model statistik ARIMA (0,1,0), dibuktikan oleh metrik evaluasi MSE yang lebih kecil, nilai $R^2$ yang tinggi, serta visualisasi kurva prediksi yang paling mendekati data aktual (*actual vs predicted*)[cite: 959].

---

## Struktur Repositori
* `dataset/` : Menyimpan data mentah historis kurs CNY/IDR dari Google Finance.
* `notebooks/` : Jupyter Notebook berisi proses Preprocessing, ADF Test, plot ACF/PACF, tuning XGBoost, dan plot validasi.
* `src/` : Kode sumber Python untuk fungsi *forecasting* mandiri.

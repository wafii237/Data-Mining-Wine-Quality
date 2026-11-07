# Klasifikasi Kualitas Anggur (Wine Quality)

Proyek UTS (Take Home Test) mata kuliah **Data Mining**, D3 Statistika Terapan dan Komputasi, Universitas Negeri Semarang (UNNES), semester Ganjil 2026/2027.

**Disusun oleh:** Muhammad Faeruz Wafi Abidin
**Dosen pengampu:** Nur Achmey Selgi Harwanti, M.Stat

## Tujuan

Membangun model klasifikasi untuk memprediksi `quality` (kualitas anggur) berdasarkan 11 fitur kimiawi, menggunakan `data_training.csv`, lalu memprediksi `quality` pada `data_testing.csv`.

## Isi Repository

| File | Keterangan |
|---|---|
| `UTS_DataMining_WineQuality.ipynb` | Notebook analisis lengkap beserta penjelasan dan interpretasi setiap langkah |
| `hasilprediksi_028.csv` | Hasil prediksi data testing (hanya kolom `Id` dan `Quality`) |
| `model_wine_quality.joblib` | Model terlatih yang disimpan untuk deployment pada data testing |

## Dataset

- **Data training:** 857 sampel, 11 fitur kimiawi + target `quality` (rentang 3 sampai 8).
- **Data testing:** 286 sampel, tanpa label `quality`.
- **Fitur:** `fixed acidity`, `volatile acidity`, `citric acid`, `residual sugar`, `chlorides`, `free sulfur dioxide`, `total sulfur dioxide`, `density`, `pH`, `sulphates`, `alcohol`.
- **Distribusi target:** tidak seimbang. Kelas 5 (362) dan kelas 6 (341) mendominasi, sedangkan kelas 3 (6), 4 (26), 7 (109), dan 8 (13) lebih sedikit.

## Tahapan Analisis

1. **Persiapan data:** memuat data, mengeluarkan kolom `Id` dari fitur, dan eksplorasi (distribusi target, distribusi fitur, korelasi).
2. **Pembersihan data:** pemeriksaan missing value dan duplikat, imputasi median di dalam pipeline, pemeriksaan outlier, dan `StandardScaler` untuk model yang sensitif terhadap skala.
3. **Pembuatan model:** perbandingan 6 model dengan 5-fold cross-validation, tuning model terbaik dengan `RandomizedSearchCV`, lalu evaluasi dengan akurasi, classification report, dan confusion matrix.
4. **Prediksi data uji:** model dilatih ulang pada seluruh data training, disimpan dengan `joblib`, dimuat kembali, lalu dipakai memprediksi data testing.
5. **Pengumpulan hasil:** file `hasilprediksi_<3 digit NIM>.csv` berisi `Id` dan `Quality`.

## Model yang Dibandingkan

| Model | Akurasi CV (5-fold) |
|---|---|
| **Extra Trees** | **0,6336** |
| Random Forest | 0,6292 |
| Gradient Boosting | 0,6161 |
| Logistic Regression | 0,5854 |
| KNN | 0,5328 |
| Decision Tree | 0,5226 |

## Hasil

- **Model terpilih:** Extra Trees dengan parameter terbaik `n_estimators=600`, `max_depth=25`, `max_features=0.5`, `min_samples_split=2`, `min_samples_leaf=2`.
- **Akurasi CV setelah tuning:** 0,6467.
- **Akurasi pada data validasi (20% data training):** 0,62.
- **Fitur terpenting:** `alcohol` (0,186), `sulphates` (0,126), `volatile acidity` (0,112), `total sulfur dioxide` (0,096), `citric acid` (0,083).
- **Performa per kelas:** baik pada kelas 5 (F1 0,72) dan kelas 6 (F1 0,58), lemah pada kelas minoritas (kelas 3 dan 4 tidak berhasil diprediksi) akibat ketidakseimbangan data.

## Keterbatasan

Ketidakseimbangan kelas membuat model cenderung memprediksi kelas mayoritas, sehingga prediksi data uji hanya berisi kelas 5, 6, dan 7. Perbaikan yang dapat dicoba: *class weighting*, oversampling (SMOTE), menggabungkan kelas ekstrem, dan rekayasa fitur.

## Cara Menjalankan

1. Buka `UTS_DataMining_WineQuality.ipynb` di [Google Colab](https://colab.research.google.com) (**File → Upload notebook**).
2. Unduh dataset dari https://bit.ly/datasetwine, lalu upload `data_training.csv` dan `data_testing.csv` ke panel *Files* di Colab.
3. Pada sel pengaturan, isi `NIM3` dengan 3 digit terakhir NIM.
4. Jalankan semua sel (**Runtime → Run all**).

**Library:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `joblib` (sudah tersedia di Google Colab).

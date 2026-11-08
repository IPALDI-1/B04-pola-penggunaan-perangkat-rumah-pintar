# B04 - Pola Penggunaan Perangkat Rumah Pintar

Repository ini memuat seluruh artefak **UTS Milestone 1** mata kuliah **IF2116 Pembelajaran Mesin**.

| | |
|---|---|
| **Project ID** | B04 |
| **Nama Kelompok** | Kelompok 10 |
| **Kelas** | IF-B PAGI |
| **Anggota** | Ipaldi Sembiring (231112081), Mikkael Raynala Tampubolon (231112313), Muhammad Dhani Irvansyah (231112747) |

## Deskripsi Masalah
Pola penggunaan perangkat listrik pada sebuah rumah pintar berbeda-beda menurut waktu dan kondisi cuaca, tetapi pola tersebut belum terlihat jelas dari data konsumsi mentah. Project ini mengelompokkan jam-jam penggunaan listrik ke dalam cluster dengan karakteristik konsumsi perangkat yang serupa, lalu menafsirkannya sebagai pola penggunaan perangkat.

- **Jenis masalah:** unsupervised learning (clustering), tanpa target atau label.
- **Metode:** K-Means (baseline), Gaussian Mixture Model dan Agglomerative Clustering (kandidat), PCA (uji reduksi dimensi).
- **Unit observasi:** satu jam (agregasi rata-rata dari data per menit), 8.399 observasi dan 22 fitur.
- **Evaluasi:** hold-out 80/20 dan 5-Fold Cross-Validation dengan Silhouette, Davies-Bouldin, dan Calinski-Harabasz. Semua hasil memakai `random_state = 42`.

## Isi Repository
| Berkas atau folder | Keterangan |
|---|---|
| `B04_PolaPenggunaanPerangkatRumahPintar_UTS_Milestone1.ipynb` | Notebook eksperimen lengkap (bagian A sampai W) |
| `report/Laporan_UTS_Milestone1_B04.pdf` | Laporan UTS Milestone 1 |
| `docs/DATASET.md` | Dokumentasi sumber dan karakteristik dataset |
| `results/` | Tabel hasil eksperimen awal (CSV) dan grafik (`results/figures/`) |
| `requirements.txt` | Versi library yang tercatat saat eksperimen |

## Petunjuk Penggunaan
1. Buka notebook di **Google Colab** (unggah file `.ipynb` lewat *File, Upload notebook*, atau buka dari tab GitHub).
2. Pilih **Runtime, Run all**. Tidak perlu mengunggah dataset karena sel kode pertama mengunduhnya otomatis dari Kaggle lewat `kagglehub` (butuh koneksi internet).
3. Eksekusi penuh membutuhkan beberapa menit. Angka pada notebook, laporan, dan folder `results/` berasal dari eksekusi yang sama.

Untuk menjalankan secara lokal: `pip install -r requirements.txt`, lalu jalankan notebook dengan Jupyter.

## Sumber Dataset
*Smart Home Dataset with Weather Information* (Kaggle, pengunggah taranvee), file `HomeC.csv`:
https://www.kaggle.com/datasets/taranvee/smart-home-dataset-with-weather-information

Dataset tidak disertakan dalam repository (ukuran sekitar 125 MB). Cara memperolehnya dijelaskan di `docs/DATASET.md`.

## Ringkasan Hasil Awal
Silhouette pada 5-Fold Cross-Validation (rata-rata ± simpangan baku antar fold). Tabel lengkap ada di `results/tabel_hasil_eksperimen.csv`.

| Model | k = 2 | k = 3 |
|---|---|---|
| K-Means (baseline) | 0,100 ± 0,006 | 0,099 ± 0,005 |
| GMM (kandidat 1) | 0,100 ± 0,019 | 0,051 ± 0,042 |
| Agglomerative (kandidat 2) | 0,169 ± 0,023 | 0,138 ± 0,028 |

- Tidak ada model yang unggul pada semua metrik, dan semua nilai Silhouette rendah, sehingga struktur cluster belum tegas.
- Ketiga model membentuk kelompok yang sangat berbeda (K-Means memisahkan musim dan pemanas, GMM memisahkan jam aktif dan jam sepi, Agglomerative memisahkan jam berawan).
- Model final belum diputuskan pada Milestone 1.

## Keterbatasan
- Interval 1 baris = 1 menit dan zona waktu UTC-5 adalah asumsi dari pola data, bukan keterangan resmi dataset.
- Pembagian data dan Cross-Validation dilakukan secara acak pada data deret waktu.
- Evaluasi data uji Agglomerative memakai centroid terdekat sehingga tidak sepenuhnya sebanding dengan model lain.
- Tidak ada label kebenaran, sehingga evaluasi hanya memakai metrik internal dan interpretasi manual.

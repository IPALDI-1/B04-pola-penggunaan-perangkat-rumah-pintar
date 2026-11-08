# Dokumentasi Sumber Dataset

## Identitas
- **Nama:** Smart Home Dataset with Weather Information
- **Platform dan pengunggah:** Kaggle, taranvee
- **URL:** https://www.kaggle.com/datasets/taranvee/smart-home-dataset-with-weather-information
- **File yang dipakai:** `HomeC.csv`
- **Lisensi dan ketentuan penggunaan:** tidak dicantumkan di dokumen ini. Periksa halaman Kaggle di atas sebelum mendistribusikan ulang data.

## Cara Memperoleh Data
Dataset tidak disertakan di repository. Notebook mengunduhnya otomatis pada sel kode pertama:

```python
import kagglehub
path = kagglehub.dataset_download("taranvee/smart-home-dataset-with-weather-information")
df_raw = pd.read_csv(path + "/HomeC.csv", low_memory=False)
```

Alternatif manual: unduh `HomeC.csv` dari halaman Kaggle di atas, lalu ganti `path + "/HomeC.csv"` dengan lokasi file.

## Karakteristik (hasil pemeriksaan pada notebook)
- 503.911 baris dan 32 kolom (503.910 baris valid), sekitar 125 MB.
- Satu baris diasumsikan satu menit (asumsi dari pola data), mencakup sekitar 350 hari (1 Januari sampai 15 Desember 2016 pada waktu lokal berdasarkan asumsi UTC-5).
- Tidak ada target atau label.

## Fitur yang Dipakai (22)
- **Perangkat (13), kW:** Dishwasher, Furnace 1, Furnace 2, Home office, Fridge, Wine cellar, Garage door, Kitchen 12, Kitchen 14, Barn, Well, Microwave, Living room.
- **Cuaca (6):** temperature, humidity, windSpeed, cloudCover, pressure, visibility (satuan adalah dugaan dari rentang nilai).
- **Waktu (3), hasil feature engineering:** hour_sin, hour_cos, weekend.

Kolom yang tidak dipakai: `use` dan `gen` (identik dengan `House overall` dan `Solar`), `Kitchen 38` (hampir selalu nol), `apparentTemperature`, `windBearing`, `precipIntensity`, `dewPoint`, `precipProbability`, `icon`, dan `summary`.

## Masalah Kualitas Data yang Ditemukan
1. Kolom `time` berisi Unix timestamp yang naik 1 per baris sehingga perlu direkonstruksi.
2. Satu baris terakhir tidak valid (semua kolom kosong).
3. Kolom `cloudCover` bertipe teks karena 58 baris berisi kata "cloudCover".
4. `use` identik dengan `House overall`, dan `gen` identik dengan `Solar`.
5. `Kitchen 38` hampir selalu nol.
6. Tidak ada data duplikat.
7. Jumlah konsumsi semua perangkat hanya berkorelasi 0,66 dengan `use`, sehingga tidak semua beban rumah tercatat per perangkat.

# Clean the Netflix Dataset

Pembersihan dataset Netflix Movies and TV Shows menggunakan Python dan Pandas. Proyek ini merupakan bagian dari latihan Data Analyst di [roadmap.sh](https://roadmap.sh/projects/cleaning-netflix-dataset).

## Ringkasan

Dataset mentah memiliki data kosong, kolom tanggal yang bertipe teks, kolom durasi yang mencampur angka dengan satuan, nilai yang salah kolom, dan format negara yang rusak. Proyek ini membersihkannya dengan keputusan yang disengaja untuk setiap kolom, lalu mengekspor hasilnya ke CSV.

## Dataset

| Aspek | Keterangan |
|---|---|
| Sumber | [Netflix Movies and TV Shows (Kaggle)](https://www.kaggle.com/datasets/shivamb/netflix-shows) |
| File mentah | `netflix_titles.csv` |
| File hasil | `netflix_titles_cleaned.csv` |
| Ukuran awal | 8.807 baris, 12 kolom |
| Ukuran akhir | 8.797 baris, 16 kolom |
| Isi | 6.131 film dan 2.666 serial TV |

## Alur Pembersihan

| Tahap | Yang dilakukan |
|---|---|
| 1 | Inspeksi data dengan `head()`, `info()`, dan `describe()` |
| 2 | Mengukur data kosong per kolom |
| 3 | Memeriksa nilai yang salah kolom pada `rating` |
| 4 | Menangani data kosong kolom demi kolom |
| 5 | Memecah kolom `duration` menjadi angka dan satuan |
| 6 | Mengubah `date_added` menjadi datetime |
| 7 | Memeriksa duplikat dan spasi berlebih |
| 8 | Merapikan format kolom `country` |
| 9 | Verifikasi akhir dan ekspor ke CSV |

## Keputusan Pembersihan

Kolom "Jumlah" menunjukkan kondisi sebelum pembersihan.

| Kolom | Masalah | Jumlah | Keputusan | Alasan |
|---|---|---|---|---|
| director | Data kosong | 2.634 (29,91%) | Isi "Unknown" | Membuang hampir sepertiga data akan menghilangkan banyak judul, dan nama sutradara tidak dapat diperkirakan |
| country | Data kosong | 831 (9,44%) | Isi "Unknown" | Mengisi dengan nilai terbanyak akan menggeser distribusi negara |
| cast | Data kosong | 825 (9,37%) | Isi "Unknown" | Nama pemeran tidak dapat diperkirakan, dan sebagian judul memang tidak mencantumkannya |
| date_added | Data kosong dan bertipe teks | 10 (0,11%) | Hapus baris, ubah ke datetime | Jumlahnya sangat kecil dan tanggal tidak dapat diperkirakan |
| rating | Tiga nilai salah kolom dan empat data kosong | 3 dan 4 | Pindahkan tiga nilai ke duration, isi 7 baris kosong dengan "Unknown" | Tiga nilai itu adalah durasi film yang salah masuk kolom, jadi dipindahkan, bukan dibuang |
| duration | Tiga data kosong dan campuran angka dengan satuan | 3 | Isi dari rating, pecah menjadi angka dan satuan | Angka dan satuan terpisah sehingga bisa dihitung dan difilter |
| country | Format rusak (koma di awal atau akhir) | 7 | Hapus koma berlebih | Koma berlebih akan menghasilkan nilai negara kosong saat data dipisah per negara |

## Temuan Penting

- Tiga film stand-up Louis C.K. memiliki durasi ("66 min", "74 min", "84 min") yang salah masuk ke kolom `rating`. Nilainya dipindahkan, bukan dibuang.
- Sepuluh baris yang dihapus (`date_added` kosong) seluruhnya serial TV, sehingga jumlah film tetap 6.131.
- Tujuh baris `country` memiliki koma berlebih (misalnya ", South Korea"). Masalah ini berasal dari data asli dan sudah dirapikan.
- Tidak ditemukan duplikat pada baris, `show_id`, maupun pasangan judul dan tipe.
- Kategori rating `NR` dan `UR` dibiarkan apa adanya.
- Ada 14 judul dengan `release_year` lebih besar dari tahun `date_added`. Ini dibiarkan karena Netflix dapat menambahkan judul ke katalog sebelum tanggal rilis resminya.

## Hasil Akhir

| Aspek | Sebelum | Sesudah |
|---|---|---|
| Jumlah baris | 8.807 | 8.797 |
| Jumlah kolom | 12 | 16 |
| Data kosong | 6 kolom bermasalah | 0 |
| `date_added` | Teks | datetime |
| `duration` | Teks campuran angka dan satuan | Dipecah menjadi `duration_value` dan `duration_unit` |
| Duplikat | - | 0 |

### Kolom Baru

| Kolom | Isi |
|---|---|
| duration_value | Angka durasi (bilangan bulat) |
| duration_unit | `min` untuk film, `season` untuk serial TV |
| year_added | Tahun judul ditambahkan ke Netflix |
| month_added | Bulan judul ditambahkan ke Netflix |

## Keterbatasan

- Nilai "Unknown" pada `director`, `cast`, dan `country` berarti informasi tidak tersedia, bukan berarti memang tidak ada. Analisis lanjutan perlu memperlakukannya secara terpisah.
- Kolom `country` dan `listed_in` masih berisi beberapa nilai dalam satu sel (dipisah koma). Analisis per negara atau per genre perlu memecahnya terlebih dahulu.
- Kolom `duration` asli dipertahankan sebagai arsip di samping kolom hasil pemecahan.

## Struktur Repositori

```
Clean-Netflix-Dataset/
├── data/
│   └── netflix_titles_cleaned.csv
├── clean-the-netflix-dataset.ipynb
└── README.md
```

## Cara Menjalankan

| Langkah | Yang dilakukan |
|---|---|
| 1 | Unduh `netflix_titles.csv` dari Kaggle ke folder `data/` |
| 2 | Instal library: `pip install pandas jupyter` |
| 3 | Buka notebook dengan `jupyter notebook`, lalu ubah path pada sel pemuatan data menjadi `data/netflix_titles.csv` |
| 4 | Jalankan semua sel dari atas ke bawah |

Jika dijalankan di Kaggle, tambahkan dataset melalui **Add Input** dan gunakan path dari `/kaggle/input/`.

## Tautan

| Item | Tautan |
|---|---|
| Notebook di Kaggle | [isi tautan notebook Kaggle] |
| Halaman proyek | https://roadmap.sh/projects/cleaning-netflix-dataset |

## Penulis

[Nama kamu] · [tautan GitHub atau LinkedIn]

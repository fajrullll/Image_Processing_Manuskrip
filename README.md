# Segmentasi Citra Tulisan Jawi dengan Graph, FCC, TSP, dan B-Spline

Proyek ini mengimplementasikan pipeline **pengolahan citra tulisan Jawi/Arab** untuk melakukan segmentasi rasm (badan huruf), mendeteksi fitur topologi, menelusuri skeleton, dan membentuk kurva karakter.

Metode utama yang digunakan:

- Skeletonisasi dan pemodelan skeleton sebagai graf
- Deteksi endpoint, intersection, turn, dan loop
- **Freeman Chain Code (FCC)** untuk membantu menentukan titik potong
- **Travelling Salesman Problem (TSP)** untuk mengurutkan titik skeleton
- **B-Spline** untuk membentuk kurva halus karakter
- Evaluasi hasil segmentasi dan curve fitting

## Contoh Hasil

### Segmentasi Utama
![Visualisasi segmentasi utama](HASIL/visualisasi_utama_segmentasi.png)

### Deteksi Key Point
![Visualisasi key point](HASIL/visualisasi_key_point_detection.png)

### Freeman Chain Code
![Visualisasi Freeman Chain Code](HASIL/visualisasi_freeman_region_1.png)

### Jalur TSP
![Visualisasi total jarak TSP](HASIL/visualisasi_total_jarak_tsp.png)

## Fitur

- Pra-pemrosesan, binarisasi, dan penghapusan noise
- Estimasi lebar goresan secara dinamis
- Pemisahan rasm dan diakritik atas/bawah
- Pembentukan graph 8-neighbor dari skeleton
- Deteksi endpoint, intersection, belokan, dan loop
- Segmentasi berbasis topologi graph dan Freeman Chain Code
- Normalisasi arah FCC untuk tulisan kanan-ke-kiri
- Penelusuran skeleton menggunakan TSP
- Curve fitting menggunakan B-Spline
- Evaluasi accuracy, precision, recall, dan F1-score
- Evaluasi kompresi, RMSE, MAE, maximum error, dan smoothness kurva
- Penyimpanan otomatis potongan karakter dan visualisasi

## Alur Pemrosesan

```text
Citra masukan
    ↓
Grayscale dan binarisasi
    ↓
Pembersihan noise dan estimasi skala
    ↓
Identifikasi rasm dan diakritik
    ↓
Skeletonisasi dan ekstraksi fitur graph
    ↓
Pemotongan rasm berbasis graph + FCC
    ↓
Penelusuran skeleton dengan TSP
    ↓
Curve fitting B-Spline
    ↓
Evaluasi dan penyimpanan visualisasi
```

## Teknologi

- Python
- NumPy
- OpenCV
- scikit-image
- SciPy
- NetworkX
- Matplotlib

## Persyaratan

- Python 3.9 atau lebih baru
- `pip`

## Konfigurasi

Sebelum menjalankan program, buka `SATU_CITRA_GRAPH_FCC.py`, lalu sesuaikan lokasi citra masukan dan folder keluaran:

```python
image_path = r"C:\lokasi\proyek\CITRA\nama_citra.png"
OUTPUT_FOLDER = r"C:\lokasi\proyek\HASIL"
```

> **Catatan:** Versi saat ini masih menggunakan path absolut yang harus disesuaikan dengan lokasi repository pada komputer pengguna.

Freeman Chain Code dapat diaktifkan atau dinonaktifkan melalui:

```python
FCC_AKTIF = True   # Aktif
FCC_AKTIF = False  # Nonaktif
```

Parameter target segmentasi, rentang koordinat FCC, dan jumlah ground truth rasm juga dapat disesuaikan langsung di dalam script agar cocok dengan citra yang dianalisis.

## Menjalankan Program

```bash
python SATU_CITRA_GRAPH_FCC.py
```

Program akan membaca citra, melakukan segmentasi, menjalankan analisis TSP dan B-Spline, mencetak tabel evaluasi di terminal, lalu menyimpan hasil ke folder `HASIL`.

## Struktur Proyek

```text
Image_Processing_PerangJohor/
├── CITRA/
│   └── dengan syafaat rasul.png
├── HASIL/
│   ├── potongan_huruf_*.png
│   ├── visualisasi_biner_tsp_huruf_*.png
│   ├── visualisasi_bspline_huruf_*.png
│   ├── visualisasi_freeman_region_*.png
│   ├── visualisasi_key_point_detection.png
│   ├── visualisasi_total_jarak_tsp.png
│   └── visualisasi_utama_segmentasi.png
├── SATU_CITRA_GRAPH_FCC.py
└── README.md
```

## Keluaran

| Pola nama file | Keterangan |
|---|---|
| `potongan_huruf_*.png` | Citra biner setiap hasil segmentasi karakter |
| `visualisasi_utama_segmentasi.png` | Segmentasi lengkap, bounding box, dan garis potong |
| `visualisasi_key_point_detection.png` | Skeleton, endpoint, intersection, loop, dan diakritik |
| `visualisasi_freeman_region_*.png` | Jalur, kode arah, kandidat, dan titik potong FCC |
| `visualisasi_biner_tsp_huruf_*.png` | Perbandingan citra biner dengan skeleton dan jalur TSP |
| `visualisasi_bspline_huruf_*.png` | Control point, control polygon, dan kurva B-Spline |
| `visualisasi_total_jarak_tsp.png` | Jalur dan total jarak TSP setiap karakter |

## Evaluasi

Program mengevaluasi:

- **Segmentasi rasm:** GT, DT, TP, FP, FN, accuracy, precision, recall, dan F1-score
- **TSP:** jumlah piksel skeleton dan total jarak rute
- **B-Spline:** control point, kompresi, panjang kurva, RMSE, MAE, maximum error, dan smoothness

## Catatan

- Pipeline saat ini disetel untuk satu citra contoh dan mungkin memerlukan penyesuaian parameter untuk citra lain.
- Nilai ground truth rasm ditentukan secara manual di dalam script.
- Kualitas segmentasi dipengaruhi resolusi, ketebalan goresan, noise, jarak antarhuruf, dan posisi diakritik.
- Gunakan citra dengan kontras yang baik untuk hasil optimal.

## Lisensi

Belum ada lisensi yang ditentukan. Tambahkan file `LICENSE` apabila proyek akan didistribusikan atau digunakan oleh pihak lain.

## Kontributor

Dikembangkan oleh [fajrullll](https://github.com/fajrullll).

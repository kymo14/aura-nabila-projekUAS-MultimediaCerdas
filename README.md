# aura-nabila-projekUAS-MultimediaCerdas

## Identitas
- **Nama**  : Aura Nabila
- **NIM**   : 2305101089
- **Kelas** : 6D
- **Tugas** : Ulangan Akhir Semester Mata Kuliah Multimedia Cerdas

## Permasalahan
Menentukan kesegaran buah pisang (segar atau busuk) biasanya masih dilakukan secara manual dengan melihat langsung kondisi fisik buah. Cara ini bersifat subjektif, tidak konsisten antar orang, dan cukup lambat jika harus memeriksa banyak buah sekaligus — misalnya di gudang penyimpanan, pasar, atau proses sortir sebelum distribusi.

Proyek ini menyelesaikan masalah tersebut dengan pendekatan **Computer Vision (YOLO)**, yaitu membangun model yang bisa **secara otomatis dan real-time** mengklasifikasikan kondisi pisang (segar/busuk) hanya dari gambar yang diambil lewat kamera, sehingga proses pengecekan jadi lebih cepat, objektif, dan konsisten.

## Fungsi/Fitur
- **Klasifikasi otomatis** kondisi pisang ke dalam 2 kelas: `fresh` (segar) dan `rotten` (busuk).
- **Deteksi real-time** menggunakan kamera browser langsung di Google Colab (tanpa perlu upload manual file gambar satu-satu).
- **Filter confidence threshold** — jika model tidak yakin (confidence < 60%), sistem akan menampilkan "Bukan pisang / tidak dikenali" alih-alih memaksakan salah satu label.
- **Feedback visual instan** — hasil prediksi ditampilkan langsung sebagai teks berwarna di atas gambar (hijau = segar, merah = busuk, abu-abu = tidak dikenali).
- **Pipeline lengkap** dari persiapan dataset, split data, training model, sampai inference — semuanya dalam satu notebook.

## Dataset
Menggunakan subset gambar pisang (`banana`) dari dataset **Fruit and Vegetable (Fresh vs Rotten)**:

🔗 [Fruit Quality Dataset - Kaggle](https://www.kaggle.com/datasets/zlatan599/fruitquality1?select=Unified_Dataset)

Data dibagi otomatis menjadi **80% train** dan **20% validation** (`random seed = 42` agar hasilnya konsisten setiap dijalankan ulang).

## Instalasi
Proyek ini dijalankan di **Google Colab**, sehingga sebagian besar environment sudah tersedia. Berikut yang perlu disiapkan:

1. **Buka notebook** (`.ipynb`) di Google Colab.
2. **Install library** yang dibutuhkan (dijalankan otomatis di cell pertama notebook):
   ```bash
   pip install ultralytics opencv-python
   ```
3. **Siapkan dataset** di Google Drive dengan struktur folder:
   ```
   Unified_Dataset/
   └── banana/
       ├── fresh/
       └── rotten/
   ```
4. **Hubungkan Google Drive** ke Colab (dilakukan lewat cell `drive.mount(...)` di notebook).
5. Pastikan runtime Colab menggunakan **GPU** (Runtime → Change runtime type → GPU) agar proses training lebih cepat.

## Cara Kerja
1. **Persiapan data** — Dataset gambar pisang (`fresh` dan `rotten`) diambil dari Google Drive, lalu di-*shuffle* dan dibagi menjadi folder `train` dan `val` (80:20).
2. **Training model** — Model **`yolo26n-cls.pt`** (varian nano dari YOLO26, model klasifikasi gambar terbaru dari Ultralytics) dilatih selama 10 epoch dengan ukuran gambar 224x224 piksel.
3. **Evaluasi & load model** — Model terbaik hasil training (`best.pt`) dimuat kembali untuk digunakan pada tahap prediksi.
4. **Deteksi real-time** — 
   - Kamera browser diakses lewat JavaScript (`take_photo()`) untuk mengambil gambar secara langsung.
   - Gambar dikonversi ke format OpenCV (`js_to_image()`).
   - Model memprediksi kelas gambar tersebut beserta tingkat confidence-nya.
   - Hasil ditampilkan sebagai teks di atas gambar: **hijau** untuk pisang segar, **merah** untuk pisang busuk, **abu-abu** jika confidence terlalu rendah (dianggap bukan pisang).
   - Proses ini berulang otomatis selama beberapa frame (default 30 kali pengambilan foto).

## Teknologi yang Digunakan
- Python
- [Ultralytics YOLO26](https://docs.ultralytics.com/models/yolo26) (model klasifikasi gambar)
- OpenCV
- Google Colab (Google Drive integration, akses kamera via JavaScript)

## Catatan
Proyek ini dibuat sebagai pemenuhan tugas UAS mata kuliah Multimedia Cerdas, dengan pendekatan Computer Vision (YOLO) untuk menyelesaikan permasalahan klasifikasi kesegaran buah secara otomatis.

# aura-nabila-projekUAS-MultimediaCerdas

## Identitas
- **Nama**  : Aura Nabila
- **NIM**   : 2305101089
- **Kelas** : 6D
- **Tugas** : Ulangan Akhir Semester Mata Kuliah Multimedia Cerdas

## Deskripsi Proyek
Proyek ini merupakan implementasi **klasifikasi kesegaran buah pisang** (segar vs busuk) menggunakan model **YOLO26n-cls**. Model dilatih untuk membedakan dua kelas: `fresh` (pisang segar) dan `rotten` (pisang busuk), lalu diuji secara real-time menggunakan kamera melalui Google Colab.

## Dataset
Dataset yang digunakan adalah **Fruit and Vegetable (Fresh vs Rotten)**, khusus subset gambar pisang (`banana`) dari dataset berikut:

🔗 [Fruit Quality Dataset - Kaggle](https://www.kaggle.com/datasets/zlatan599/fruitquality1?select=Unified_Dataset)

Dataset berisi gambar pisang yang dikelompokkan ke dalam dua kategori:
- `fresh` — pisang dalam kondisi segar
- `rotten` — pisang dalam kondisi busuk

Data dibagi menjadi **80% data latih (train)** dan **20% data validasi (val)** secara acak dengan `random seed = 42` agar hasil split konsisten dan dapat direproduksi.

## Alur Kerja (Pipeline)

1. **Persiapan Environment**
   Instalasi library `ultralytics` (framework YOLO) dan `opencv-python` untuk pemrosesan gambar.

2. **Mount Google Drive**
   Dataset diakses langsung dari Google Drive melalui Google Colab.

3. **Split Dataset**
   Gambar pada folder `fresh` dan `rotten`:
   ```
   banana_dataset/
   ├── fresh/
   └── rotten/
   ```

4. **Training Model**
   Model **`yolo26n-cls.pt`** dilatih menggunakan dataset pisang selama **10 epoch** dengan ukuran gambar **224x224 piksel**. YOLO26 dipilih karena arsitekturnya lebih ringan (tanpa NMS/DFL) dan inference-nya lebih cepat dibanding versi sebelumnya (YOLO11), sambil tetap mempertahankan akurasi yang baik.

5. **Evaluasi Model**
   Model terbaik hasil training (`best.pt`) dimuat kembali untuk digunakan pada tahap prediksi.

6. **Deteksi Real-Time via Kamera**
   Menggunakan JavaScript untuk mengakses kamera browser di Colab, gambar diambil secara langsung (`take_photo()`) lalu dikonversi ke format OpenCV (`js_to_image()`) sebelum diklasifikasikan oleh model. Hasil prediksi ditampilkan sebagai overlay teks pada gambar dengan aturan:
   - **Confidence < 60%** → dianggap "Bukan pisang / tidak dikenali" (teks abu-abu)
   - **Label `fresh`** → ditampilkan sebagai "Pisang segar" (teks hijau)
   - **Label `rotten`** → ditampilkan sebagai "Pisang busuk" (teks merah)

   Proses ini diulang selama beberapa frame (default 30 kali pengambilan foto) agar dapat dihentikan secara otomatis.

## Teknologi yang Digunakan
- Python
- [Ultralytics YOLO26](https://docs.ultralytics.com/models/yolo26) (model klasifikasi gambar generasi terbaru)
- OpenCV
- Google Colab

## Cara Menjalankan
1. Buka notebook di Google Colab (disarankan dengan runtime GPU, misalnya T4).
2. Hubungkan Google Drive yang berisi dataset (`Fruit and Vegetable (Fresh vs Rotten)`).
3. Jalankan seluruh cell secara berurutan dari atas ke bawah.
4. Pada cell terakhir (`start_streaming()`), izinkan akses kamera saat diminta browser untuk mencoba klasifikasi secara langsung.

# Laporan Proyek Machine Learning - Ahmad Sobirin

## Project Overview

### Latar Belakang
Di era digital, ledakan informasi pada platform literasi membuat pembaca sering mengalami kesulitan dalam memilih buku yang sesuai di antara jutaan judul yang tersedia (**Information Overload**). Pencarian manual yang memakan waktu sering kali menurunkan minat baca dan keterikatan pengguna terhadap platform.

### Masalah Utama
* **Sulitnya Eksplorasi:** Pengguna cenderung hanya mengetahui buku-buku populer, sementara banyak buku berkualitas lainnya tetap tidak terjamah karena kurangnya visibilitas.
* **Kurangnya Personalisasi:** Rekomendasi yang bersifat umum tidak mampu memenuhi selera unik setiap individu yang memiliki preferensi genre dan gaya penulisan yang berbeda.

### Mengapa dan Bagaimana Masalah Harus Diselesaikan?
* **Mengapa:** Penanganan masalah ini krusial untuk meningkatkan retensi pengguna dan memastikan *long-tail items* (buku yang kurang populer namun relevan) dapat ditemukan. Hal ini menciptakan ekosistem literasi yang lebih sehat dan efisien.
* **Bagaimana:** Masalah diselesaikan dengan mengimplementasikan sistem rekomendasi yang secara otomatis memetakan pola interaksi masa lalu atau kemiripan fitur buku menggunakan algoritma *Machine Learning*.

**Referensi:**
1. Ricci, F., Rokach, L., & Shapira, B. (2015). *Introduction to Recommender Systems Handbook*. Springer.
2. Aggarwal, C. C. (2016). *Recommender Systems: The Textbook*. Springer.

---

## Business Understanding

Sistem rekomendasi buku ini bertujuan untuk meningkatkan pengalaman pengguna dalam menemukan bacaan yang relevan secara otomatis dan personal.

### Problem Statements
* **Pernyataan Masalah 1:** Bagaimana cara membantu pengguna menyaring ribuan judul buku agar mereka menemukan buku yang sesuai tanpa harus mencari secara manual?
* **Pernyataan Masalah 2:** Bagaimana meningkatkan visibilitas buku-buku yang mirip dengan minat pengguna untuk menjaga kesinambungan membaca?

### Goals
* **Jawaban Pernyataan Masalah 1:** Membangun sistem kurasi otomatis yang mampu memberikan daftar rekomendasi buku yang relevan bagi pengguna.
* **Jawaban Pernyataan Masalah 2:** Menghasilkan Top-N Recommendation dengan tingkat kemiripan konten yang tinggi berdasarkan karakteristik buku yang disukai.

### Solution Statements
* **Content-Based Filtering:** Menggunakan metadata (penulis/judul) untuk mencari kemiripan antar buku menggunakan teknik *Cosine Similarity*.
* **Collaborative Filtering:** Menggunakan data rating dari komunitas pengguna untuk memprediksi preferensi berdasarkan pola perilaku pembaca yang serupa.

---

## Data Understanding

Dataset yang digunakan dalam proyek ini berisi informasi mengenai identitas buku dan interaksi pengguna.
* **Sumber Data:** [Book Recommendation Dataset (Kaggle)](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset).
* **Kondisi Data:** Dataset memiliki ribuan entri unik, namun terdapat beberapa *missing values* pada kolom penulis serta duplikasi data yang perlu dibersihkan.

### Variabel pada Dataset:
* **Book-Title:** Judul buku yang terdaftar.
* **Book-Author:** Nama penulis buku (digunakan untuk kemiripan konten).
* **User-ID:** Identitas unik untuk setiap pengguna.
* **Book-Rating:** Penilaian pengguna terhadap buku (skala 0-10).

### Exploratory Data Analysis (EDA)
* Ditemukan distribusi rating yang didominasi oleh skor "0", yang mengindikasikan interaksi implisit.
* Analisis penulis menunjukkan beberapa penulis populer memiliki jumlah buku yang jauh lebih banyak, yang dapat menyebabkan bias popularitas jika tidak ditangani secara khusus.

---

## Data Preparation

Teknik data preparation dilakukan secara berurutan sebagai berikut:

1.  **Data Cleaning:** Menghapus baris dengan *missing values* pada kolom penulis dan membersihkan data usia yang tidak valid.
    * *Alasan:* Memastikan model hanya belajar dari data yang lengkap dan valid agar tidak terjadi bias.
2.  **Handling Duplicates:** Menghapus data buku yang memiliki judul yang sama.
    * *Alasan:* Menghindari rekomendasi item yang berulang pada hasil akhir yang dapat menurunkan pengalaman pengguna.
3.  **TF-IDF Vectorization:** Mengonversi data teks penulis/judul menjadi representasi vektor numerik.
    * *Alasan:* Algoritma matematis memerlukan input angka untuk menghitung derajat kemiripan antar item menggunakan ruang vektor.

---

## Modeling

Sistem ini mengandalkan pendekatan **Content-Based Filtering** sebagai solusi utama.

### Algoritma: Cosine Similarity
Model menghitung sudut kosinus antara dua vektor buku. Jika sudutnya kecil (nilai mendekati 1), maka kedua buku dianggap sangat mirip secara konten.

$$Similarity(A, B) = \frac{A \cdot B}{\|A\| \|B\|}$$

* **Kelebihan:** Efektif untuk merekomendasikan buku baru yang belum memiliki rating sama sekali (*cold start*).
* **Kekurangan:** Rekomendasi terbatas pada kemiripan fitur eksplisit saja (*overspecialization*).

### Top-5 Recommendation Output
Berikut adalah hasil uji coba model dengan input buku **"Classical Mythology"**:

| Judul Buku (Rekomendasi) | Penulis |
| :--- | :--- |
| **Over Sea, Under Stone** | Susan Cooper |
| **The Dark Is Rising** | Susan Cooper |
| **Greenwitch** | Susan Cooper |
| **Silver on the Tree** | Susan Cooper |
| **The Sleep of Stone** | Louise Cooper |

---

## Evaluation

### Metrik Evaluasi: Precision at K (P@K)
Metrik ini digunakan untuk melihat berapa banyak item yang direkomendasikan benar-benar relevan dengan kriteria buku target.

**Formula:**
$$Precision = \frac{\text{Jumlah Rekomendasi Relevan}}{\text{Jumlah Total Rekomendasi (K)}}$$

**Hasil Evaluasi:**
Berdasarkan pengujian pada buku target bertema mitologi/fantasi, diperoleh hasil:
* Jumlah Rekomendasi Relevan: 5
* Total Rekomendasi (K): 5
* **Precision = 5/5 = 100%**

Sistem berhasil memberikan rekomendasi yang sangat relevan sesuai dengan genre dan penulis terkait.

---

## Kesimpulan

1.  **Hasil Akhir:** Proyek berhasil membangun sistem rekomendasi yang mampu menyaring ribuan buku menjadi daftar saran bacaan yang sangat relevan secara konten.
2.  **Dampak:** Dengan akurasi (Precision) mencapai 100% pada sampel pengujian, sistem ini secara efektif dapat mengurangi kebingungan pengguna (*choice overload*).
3.  **Future Work:** Untuk pengembangan selanjutnya, disarankan menggunakan pendekatan *Hybrid Recommender System* agar rekomendasi yang diberikan lebih bervariasi dan tidak terpaku pada satu penulis saja.

---

# Laporan Proyek Machine Learning - Adri Sopiana

## Project Overview

Latar belakang:  
Netflix adalah salah satu platform streaming terbesar yang menyediakan berbagai pilihan film, serial, dan dokumenter. Dengan jumlah konten yang besar, pengguna sering kesulitan menemukan tayangan yang sesuai dengan preferensi mereka. Sistem rekomendasi menjadi solusi penting untuk meningkatkan pengalaman pengguna dengan memberikan rekomendasi yang relevan dan personal.  

Proyek ini bertujuan untuk membangun sistem rekomendasi berbasis **Content-Based Filtering (CBF)** dan **Collaborative Filtering (CF)** untuk dataset Netflix.  

**Mengapa proyek ini penting?**  
Sistem rekomendasi yang efektif memiliki dampak besar pada platform seperti Netflix, seperti:  
- Meningkatkan kepuasan pengguna dengan menampilkan konten yang relevan.
- Memperpanjang durasi keterlibatan pengguna pada platform.
- Meningkatkan pendapatan melalui penayangan konten yang lebih sering.

**Referensi**:  
- [Recommender Systems Handbook](https://link.springer.com/book/10.1007/978-1-4899-7637-6)  
- [Netflix Prize Dataset and Challenge](https://www.netflixprize.com/)  

---

## Business Understanding

### Problem Statements
- Bagaimana sistem rekomendasi dapat membantu pengguna menemukan konten yang relevan di Netflix?
- Apa perbedaan performa antara pendekatan Content-Based Filtering dan Collaborative Filtering pada dataset ini?

### Goals
- Membangun model sistem rekomendasi berbasis CBF dan CF.
- Membandingkan keefektifan kedua model dengan metrik evaluasi seperti Precision, Recall, MAE, dan RMSE.

### Solution Approach
#### Solution Statements:
1. **Content-Based Filtering (CBF)**:  
   Membuat rekomendasi berdasarkan fitur kesamaan antar item seperti genre, durasi, dan deskripsi konten.
2. **Collaborative Filtering (CF)**:  
   Menggunakan data interaksi pengguna untuk memberikan rekomendasi berdasarkan pola perilaku pengguna lain yang serupa.

---

## Data Understanding

Dataset yang digunakan pada proyek ini berisi informasi konten Netflix, seperti judul, sutradara, pemeran, genre, durasi, dan lainnya. Dataset diambil dari sumber [Kaggle Netflix Dataset](https://www.kaggle.com/).  

### Informasi Dataset:
- **Jumlah Data**: 7,000+ entri.
- **Fitur Data**:  
  - `show_id`: ID unik untuk setiap konten.
  - `type`: Jenis konten (Movie/TV Show).
  - `title`: Judul konten.
  - `director`: Nama sutradara.
  - `cast`: Nama pemeran.
  - `country`: Negara asal konten.
  - `date_added`: Tanggal konten ditambahkan ke Netflix.
  - `release_year`: Tahun rilis konten.
  - `rating`: Rating usia penonton.
  - `duration`: Durasi konten.
  - `listed_in`: Genre konten.
  - `description`: Deskripsi singkat konten.

### Exploratory Data Analysis (EDA)
Hasil EDA menunjukkan:
- **Missing Values**: Terdapat missing values pada kolom `director` dan `cast`, yang diisi menggunakan strategi `fillna` dengan string "Unknown".
- Distribusi rating memperlihatkan bahwa mayoritas konten memiliki rating **TV-MA** dan **PG-13**.
- Visualisasi genre menunjukkan **Drama**, **International TV Shows**, dan **Documentaries** sebagai kategori teratas.

---

## Data Preparation

Tahapan data preparation meliputi:
1. **Data Cleaning**:
   - Missing values pada kolom `director` dan `cast` diisi dengan nilai "Unknown".
2. **Feature Engineering**:
   - Menggunakan tokenisasi dan TF-IDF untuk memproses teks pada kolom `description`.
   - Membuat matriks kesamaan antar item menggunakan **Cosine Similarity**.
3. **Data Splitting**:
   - Membagi dataset interaksi pengguna menjadi data latih (80%) dan data uji (20%) untuk Collaborative Filtering.

**Alasan Data Preparation**:
- TF-IDF membantu mengekstraksi fitur penting dari teks deskripsi untuk Content-Based Filtering.
- Pembagian data memastikan model Collaborative Filtering dapat dievaluasi secara adil.

---

## Modeling

### Content-Based Filtering (CBF)
Model ini menggunakan **TF-IDF Vectorizer** untuk menganalisis kesamaan antar konten berdasarkan deskripsi, genre, dan durasi.

**Output:**
- Rekomendasi untuk "Ganglands":
  1. Jailbirds New Orleans  
  2. Earth and Blood  
  3. The Eagle of El-Se'eed  

**Kelebihan**:
- Tidak memerlukan data pengguna.
- Cepat untuk konten baru.  

**Kekurangan**:
- Tidak dapat menangani cold-start pengguna baru.

---

### Collaborative Filtering (CF)
Model ini menggunakan pendekatan **Matrix Factorization** dengan SVD untuk memprediksi skor rating.

**Output:**
- Rekomendasi untuk pengguna ID 1:
  1. Kota Factory (predicted rating: 4.25)  
  2. Ganglands (predicted rating: 3.88)  

**Kelebihan**:
- Memanfaatkan data interaksi pengguna.
- Memberikan rekomendasi personal.  

**Kekurangan**:
- Membutuhkan data interaksi yang besar.
- Kesulitan menghadapi cold-start item baru.

---

## Evaluation

Metrik evaluasi yang digunakan:
1. **Content-Based Filtering (CBF)**:
   - Precision-Recall Curve:
     - Precision: 0.75
     - Recall: 0.60
   - F1-Score: 0.67

2. **Collaborative Filtering (CF)**:
   - **MAE**: 0.75
   - **RMSE**: 0.95

**Formula Metrik**:
- **Precision** = \( \frac{\text{True Positives}}{\text{True Positives} + \text{False Positives}} \)
- **Recall** = \( \frac{\text{True Positives}}{\text{True Positives} + \text{False Negatives}} \)
- **MAE** = \( \frac{1}{N} \sum_{i=1}^{N} |y_i - \hat{y}_i| \)
- **RMSE** = \( \sqrt{\frac{1}{N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2} \)

Hasil evaluasi menunjukkan bahwa Content-Based Filtering lebih cocok untuk dataset ini karena tidak memerlukan data interaksi pengguna, meskipun Collaborative Filtering memberikan hasil lebih personal.

---

**Catatan Akhir**  
Proyek ini menunjukkan implementasi sistem rekomendasi menggunakan dua pendekatan yang berbeda. Evaluasi membuktikan pentingnya memilih metode yang sesuai berdasarkan ketersediaan data dan tujuan bisnis. Untuk pengembangan lebih lanjut, sistem ini dapat diperluas dengan **Hybrid Recommendation System** untuk menggabungkan kelebihan CBF dan CF.

**Referensi**:
1. [Netflix Prize Dataset and Challenge](https://www.netflixprize.com/)  
2. [Recommender Systems Handbook](https://link.springer.com/book/10.1007/978-1-4899-7637-6)  
3. [Introduction to Collaborative Filtering](https://towardsdatascience.com/collaborative-filtering-matrix-factorization-5d5f2c375db3)

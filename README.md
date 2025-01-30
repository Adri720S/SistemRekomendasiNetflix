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

Dataset yang digunakan pada proyek ini berisi informasi konten Netflix, seperti judul, sutradara, pemeran, genre, durasi, dan lainnya. Dataset diambil dari sumber [Kaggle Netflix Dataset]( https://www.kaggle.com/datasets/shivamb/netflix-shows).  

### Informasi Dataset:
- **Jumlah Data**: 8807 entri.
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
![alt text](https://github.com/Adri720S/SistemRekomendasiNetflix/blob/main/download.png?raw=true)
- Distribusi rating memperlihatkan bahwa mayoritas konten memiliki rating **TV-MA** dan **TV-14**.
![alt text](https://github.com/Adri720S/SistemRekomendasiNetflix/blob/main/download%20(2).png?raw=true)
- Visualisasi genre menunjukkan **International Movies**, **Dramas**, dan **Comedies** sebagai kategori teratas.

---

## Data Preparation

Tahapan data preparation meliputi:
1. **Mengubah Format date_added**:
   - Kolom date_added diubah menjadi format datetime untuk analisis waktu.
2. **Penggabungan Fitur**:
   - Kolom cast, listed_in, dan description digabungkan menjadi satu kolom combined_features untuk digunakan dalam model content-based filtering.

---

## Modeling

### a. Content-Based Filtering (CBF)
Model ini merekomendasikan konten berdasarkan kesamaan fitur antar konten:
- TF-IDF Vectorization: Representasi teks menggunakan teknik TF-IDF untuk menghitung skor pentingnya kata dalam combined_features.
- Cosine Similarity: Mengukur kesamaan antar konten untuk memberikan rekomendasi.
- Hasil: Contoh rekomendasi diberikan untuk film "Ganglands", dengan film yang memiliki kesamaan konten tertinggi.
Rekomendasi untuk "Ganglands":
  1. Jailbirds New Orleans  
  2. Earth and Blood  
  3. The Eagle of El-Se'eed  

**Kelebihan**:
- Tidak memerlukan data pengguna.
- Cepat untuk konten baru.  

**Kekurangan**:
- Tidak dapat menangani cold-start pengguna baru.

---

### b. Collaborative Filtering (CF)
Model ini merekomendasikan konten berdasarkan pola interaksi pengguna:
- Matriks User-Item: Membuat matriks dari data interaksi pengguna, mengisi nilai kosong dengan rata-rata rating pengguna.
- Cosine Similarity Antar Pengguna: Menghitung kesamaan antar pengguna untuk menemukan pengguna serupa.
- Prediksi Rating: Menggunakan rata-rata tertimbang dari pengguna serupa untuk memprediksi rating film yang belum ditonton.
- Hasil: Contoh rekomendasi diberikan untuk user_id = 1, dengan film yang memiliki skor prediksi tertinggi.
Rekomendasi untuk pengguna ID 1:
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

Tahap ini mengevaluasi performa sistem rekomendasi menggunakan metrik evaluasi:

- CBF Evaluation:
  - Precision (0.33), Recall (0.33), dan F1-Score (0.33): Mengukur akurasi rekomendasi.
  - Precision-Recall Curve: Visualisasi trade-off antara precision dan recall.
     ![alt text](https://github.com/Adri720S/SistemRekomendasiNetflix/blob/main/download%20(3).png?raw=true)
  - Heatmap Cosine Similarity: Menampilkan kesamaan antar konten dalam bentuk heatmap.
     ![alt text](https://github.com/Adri720S/SistemRekomendasiNetflix/blob/main/download%20(4).png?raw=true)
- CF Evaluation:
  - MAE (0.25) dan RMSE (0.29): Mengukur error antara rating aktual dan prediksi.
    ![alt text](https://github.com/Adri720S/SistemRekomendasiNetflix/blob/main/download%20(5).png?raw=true)
  - nDCG dan MRR: Mengukur kualitas rekomendasi dengan mempertimbangkan relevansi.
    ![alt text](https://github.com/Adri720S/SistemRekomendasiNetflix/blob/main/download%20(6).png?raw=true)
  - Heatmap User Similarity: Menampilkan kesamaan antar pengguna dalam bentuk heatmap.
    ![alt text](https://github.com/Adri720S/SistemRekomendasiNetflix/blob/main/download%20(7).png?raw=true)
  - Visualisasi Rekomendasi: Menampilkan film rekomendasi dengan skor prediksi tertinggi.
    ![alt text](https://github.com/Adri720S/SistemRekomendasiNetflix/blob/main/download%20(8).png?raw=true)
    
## Summary and Insights

- CBF: Rekomendasi berbasis konten efektif dalam menemukan film yang serupa, terutama dalam genre atau deskripsi.
- CF: Rekomendasi berbasis kolaborasi mampu memanfaatkan pola interaksi pengguna dengan baik, tetapi membutuhkan data interaksi yang cukup banyak.
- Evaluasi: Model CBF dan CF memiliki performa yang baik, dengan hasil yang dapat diukur menggunakan metrik seperti precision, recall, MAE, dan RMSE.

---

**Catatan Akhir**  
Proyek ini menunjukkan implementasi sistem rekomendasi menggunakan dua pendekatan yang berbeda. Evaluasi membuktikan pentingnya memilih metode yang sesuai berdasarkan ketersediaan data dan tujuan bisnis. Untuk pengembangan lebih lanjut, sistem ini dapat diperluas dengan **Hybrid Recommendation System** untuk menggabungkan kelebihan CBF dan CF.

# Laporan Proyek Machine Learning - Adri Sopiana

## Project Overview

**Latar belakang:**  
Netflix adalah salah satu platform streaming terbesar yang menyediakan berbagai pilihan film, serial, dan dokumenter. Dengan jumlah konten yang besar, pengguna sering kesulitan menemukan tayangan yang sesuai dengan preferensi mereka. Sistem rekomendasi menjadi solusi penting untuk meningkatkan pengalaman pengguna dengan memberikan rekomendasi yang relevan dan personal.  

Proyek ini bertujuan untuk membangun sistem rekomendasi berbasis **Content-Based Filtering (CBF)** dan **Collaborative Filtering (CF)** untuk dataset Netflix.  

**Mengapa proyek ini penting?**  
Sistem rekomendasi yang efektif memiliki dampak besar pada platform seperti Netflix, seperti:  
- Meningkatkan kepuasan pengguna dengan menampilkan konten yang relevan.
- Memperpanjang durasi keterlibatan pengguna pada platform.
- Meningkatkan pendapatan melalui penayangan konten yang lebih sering.

**Referensi**:  
- [Recommender Systems Handbook](https://link.springer.com/book/10.1007/978-1-4899-7637-6)  


## Business Understanding

### Problem Statements
- Bagaimana sistem rekomendasi dapat membantu pengguna menemukan konten yang relevan di Netflix?
- Apa perbedaan antara model Content-Based Filtering dengan Collaborative Filtering pada dataset ini?

### Goals
- Membangun model sistem rekomendasi berbasis CBF dan CF.
- Menganalisis perbedaan cara kerja model CBF dan CF.

### Solution Approach
#### Solution Statements:
1. **Content-Based Filtering (CBF)**:  
   Membuat rekomendasi berdasarkan fitur kesamaan antar item seperti pemeran, genre dan deskripsi konten.
2. **Collaborative Filtering (CF)**:  
   Menggunakan data interaksi pengguna (rating usia) untuk memberikan rekomendasi berdasarkan pola perilaku pengguna lain yang serupa.


## 1. Data Understanding

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

### 2. Exploratory Data Analysis (EDA)

![alt text](https://github.com/Adri720S/SistemRekomendasiNetflix/blob/main/download.png?raw=true)
- Distribusi rating memperlihatkan bahwa mayoritas konten memiliki rating **TV-MA** dan **TV-14**.
  
![alt text](https://github.com/Adri720S/SistemRekomendasiNetflix/blob/main/download%20(2).png?raw=true)
- Visualisasi genre menunjukkan **International Movies**, **Dramas**, dan **Comedies** sebagai kategori teratas.


## 3. Data Preparation

Tahapan data preparation meliputi:

- Penanganan missing value menggunakan metode forward fill
- Penanganan duplikat data
- Mengubah Format date_added
  - Kolom date_added diubah menjadi format datetime untuk analisis waktu.

**Preparation for CBF**
- Penggabungan Fitur:
  - Kolom cast, listed_in, dan description digabungkan menjadi satu kolom combined_features untuk digunakan dalam model content-based filtering.
- TF-IDF Vectorization:
  - TF-IDF digunakan untuk merepresentasikan fitur dari item (cast, listed_in, dan description) dalam bentuk vektor numerik berdasarkan deskripsi teksnya. Ini membantu dalam mengukur kesamaan antar item untuk memberikan rekomendasi yang lebih relevan.

**Preparation for CF**
- Pivot data interaksi pengguna
- User-item matrix berdasarkan rating


## 4. Modeling and Result

### a. Content-Based Filtering (CBF)
Model CBF menghasilkan rekomendasi dengan mengukur kesamaan antara konten berdasarkan fitur-fitur yang ada, seperti genre, deskripsi, dan pemeran. Dengan menggunakan cosine similarity, kesamaan antar item konten dapat dihitung, memungkinkan model untuk memberikan rekomendasi berdasarkan kedekatan antara konten yang ada.

Hasil dari model ini menghasilkan rekomendasi konten yang memiliki kesamaan tinggi dengan konten yang telah dipilih oleh pengguna. Sebagai contoh, jika pengguna menonton film "End Game", model ini akan merekomendasikan film lain yang memiliki kesamaan dengan konten tersebut, seperti:

1. End Game
2. Extremis
3. How to Be a Player

### b. Collaborative Filtering (CF)
Pada model CF, rekomendasi diberikan berdasarkan interaksi pengguna dengan konten. Hal ini memungkinkan model untuk memberikan rekomendasi berdasarkan pola perilaku pengguna untuk preferensi. Sebagai contoh, untuk pengguna dengan rating usia "PG-13", rekomendasi yang diberikan meliputi:

1. 22-Jul (Dramas, Thrillers)
2. 9-Feb (International TV Shows, TV Dramas)
3. 15-Aug (Comedies, Dramas, Independent Movies)

Dengan menggunakan pendekatan ini, rekomendasi didasarkan pada pola interaksi pengguna lain dalam kelompok usia yang sama, sehingga dapat meningkatkan relevansi dan pengalaman pengguna dengan konten yang lebih sesuai.

## 5. Evaluation

**Problem Statement**
1. Bagaimana sistem rekomendasi dapat membantu pengguna menemukan konten yang relevan di Netflix?
- **CBF** memberikan solusi yang efektif dalam menemukan konten yang relevan berdasarkan kesamaan fitur konten, yaitu pemeran, genre atau deskripsi. Model ini sangat berguna untuk menemukan konten yang sesuai dengan preferensi pengguna tanpa memerlukan data pengguna.
- **CF** memungkinkan sistem memberikan rekomendasi yang lebih personal berdasarkan pola perilaku pengguna lain dengan preferensi yang serupa. Hal ini membantu dalam menemukan konten yang relevan berdasarkan interaksi pengguna sebelumnya.

2. Apa perbedaan antara model Content-Based Filtering dengan Collaborative Filtering pada dataset ini?
- **CBF** mengandalkan kesamaan antar konten, sedangkan **CF** berfokus pada kesamaan antara pengguna berdasarkan interaksi mereka dengan konten. Model **CBF** dapat bekerja dengan baik meskipun tanpa data pengguna, sementara **CF** memerlukan data interaksi yang lebih banyak agar dapat memberikan rekomendasi yang lebih akurat.

**Goal Evaluation**
1. Membangun model sistem rekomendasi berbasis CBF dan CF.
- Tujuan ini berhasil tercapai dengan pengembangan model CBF dan CF yang masing-masing menggunakan cosine similarity untuk menghitung kesamaan antara konten dan antar pengguna.
2. Menganalisis perbedaan cara kerja model CBF dan CF.
- Analisis mengenai perbedaan kedua model telah dilakukan dengan mengidentifikasi bahwa CBF berfokus pada kesamaan konten, sedangkan CF mengandalkan data interaksi pengguna untuk memberikan rekomendasi yang lebih personal.

**Solution Statements Impact**
1. Content-Based Filtering (CBF):
- Model CBF terbukti efektif dalam memberikan rekomendasi konten berdasarkan kesamaan antar konten. Model ini memberikan solusi yang tepat untuk meningkatkan relevansi rekomendasi tanpa memerlukan data interaksi pengguna, yang menjawab masalah bagi konten yang baru atau kurang diketahui.
2. Collaborative Filtering (CF):
- Model CF memberikan rekomendasi yang personal dengan memanfaatkan data interaksi pengguna untuk menemukan pola yang relevan.

**Kelebihan dan Kekurangan**
- Content-Based Filtering:
  - Kelebihan: Dapat memberikan rekomendasi berdasarkan konten yang sudah diketahui, bahkan untuk pengguna baru.
  - Kekurangan: Tidak dapat memberikan rekomendasi yang sangat beragam karena hanya berdasarkan pada satu jenis data (misalnya, kategori produk).
- Collaborative Filtering:
  - Kelebihan: Memberikan rekomendasi berdasarkan pola interaksi pengguna lain, cocok untuk sistem dengan banyak pengguna dan data interaksi.
  - Kekurangan: Kesulitan dalam memberikan rekomendasi untuk pengguna baru (masalah cold-start) atau produk yang jarang di-interaksi.

## Evaluation Metrics and Results
Evaluasi dilakukan dengan menggunakan dua metrik utama:

1. Precision@N untuk Content-Based Filtering
2. Diversity@N untuk Collaborative Filtering
   
**a. CBF Evaluation - Precision@N**

Precision digunakan untuk mengukur seberapa relevan rekomendasi yang diberikan oleh model CBF dibandingkan dengan konten yang sebenarnya relevan.

- Metode perhitungan Precision@N:

$$ 
Precision@N = \frac{\text{Jumlah rekomendasi yang relevan}}{\text{N}} 
$$

- ​Output:

$$  
Precision@3 = 0.67
$$

Artinya, dari 3 rekomendasi yang diberikan, 67% di antaranya memiliki genre yang sesuai dengan film referensi.

**b. CF Evaluation - Diversity@N**

Diversity digunakan untuk mengukur seberapa beragam rekomendasi yang diberikan oleh model CF. Metrik ini mengidentifikasi jumlah kategori (genre, tema, atau atribut lain) yang berbeda dalam rekomendasi.

- Metode perhitungan Diversity@N:


$$ 
Diversity@N = \frac{\text{Jumlah kategori unik}}{\text{N}} 
$$
​
- Output:

$$
Diversity@3 = 2.00
$$

Artinya, dari 3 rekomendasi yang diberikan, rata-rata tingkat perbedaan antar item yang direkomendasikan adalah 2.00. Hal ini menunjukkan bahwa rekomendasi cukup beragam, tetapi masih ada kemiripan antar beberapa item.

## Final Evaluation & Business Impact

Berdasarkan hasil evaluasi menggunakan metrik Precision@N dan Diversity@N, sistem rekomendasi yang dibangun mampu memberikan hasil yang efektif:

1. Content-Based Filtering (CBF) terbukti dapat memberikan rekomendasi yang relevan dengan tingkat precision 0.67. Ini menunjukkan bahwa model berhasil merekomendasikan film berdasarkan kesamaan fitur konten, yang cocok untuk pengguna baru atau film yang kurang populer.
2. Collaborative Filtering (CF) memberikan rekomendasi yang cukup beragam dengan Diversity@3 sebesar 2.00. Hal ini menunjukkan bahwa rekomendasi dari model CF tidak hanya relevan tetapi juga memiliki variasi dalam genre yang ditampilkan.

Dari sisi Business Understanding, sistem rekomendasi ini dapat meningkatkan pengalaman pengguna dengan memberikan rekomendasi yang lebih relevan (CBF) serta beragam dan lebih personal (CF). Ini bisa meningkatkan watch time pengguna di Netflix serta meningkatkan kepuasan pengguna terhadap layanan streaming.


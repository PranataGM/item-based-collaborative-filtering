# Simple Movie Recommender System

---

## Gambaran Umum Proyek

Pernah bingung mau nonton film apa lagi? Nah, proyek ini solusinya! Saya membangun sistem rekomendasi film sederhana yang bisa menyarankan film baru berdasarkan selera Anda atau, lebih tepatnya, berdasarkan pola rating dari pengguna lain yang memiliki selera serupa. Ini seperti punya teman yang tahu film-film keren yang mungkin Anda lewatkan!

Proyek ini fokus pada implementasi **Item-Based Collaborative Filtering**, sebuah teknik yang banyak digunakan dalam sistem rekomendasi di platform besar.

---

## Dataset

Dataset yang digunakan adalah **MovieLens ml-latest-small**, salah satu dataset standar untuk penelitian dan pengembangan sistem rekomendasi. Dataset ini berisi koleksi rating film yang diberikan oleh pengguna.

* **Sumber**: [MovieLens Latest Datasets](https://grouplens.org/datasets/movielens/latest/)
* **File Utama**:
    * `ratings.csv`: Berisi data rating pengguna terhadap film (`userId`, `movieId`, `rating`, `timestamp`).
    * `movies.csv`: Berisi informasi film (`movieId`, `title`, `genres`).

---

## Alur Kerja Data Science

Proyek ini mengikuti tahapan dasar pembangunan model Machine Learning:

### 1. Pengumpulan & Pemahaman Data
* Memuat `ratings.csv` dan `movies.csv` ke dalam Pandas DataFrame.
* Melakukan inspeksi awal data untuk memahami struktur, tipe data, dan keberadaan nilai yang hilang.

### 2. Pra-pemrosesan Data
* **Menggabungkan Data**: Menggabungkan `ratings_df` dan `movies_df` berdasarkan `movieId` untuk mendapatkan informasi judul film pada setiap baris rating.
* **Membuat Matriks Pengguna-Item**: Mengubah data yang digabungkan menjadi matriks pivot (`user_movie_matrix`) di mana baris mewakili `userId`, kolom mewakili `title` (judul film), dan nilai sel adalah `rating` yang diberikan. Sel yang kosong diisi dengan `NaN` (Not a Number) karena pengguna belum memberikan rating pada film tersebut.

### 3. Pembangunan Model Rekomendasi (Item-Based Collaborative Filtering)
* **Transpose Matriks**: Mengubah `user_movie_matrix` menjadi `movie_user_matrix` (baris adalah film, kolom adalah pengguna) agar memudahkan perhitungan kesamaan antar film.
* **Menghitung Kesamaan Item**: Menggunakan **Cosine Similarity** untuk menghitung tingkat kesamaan antara setiap pasang film berdasarkan pola rating pengguna. Nilai `NaN` diisi dengan 0 sebelum perhitungan kesamaan. Hasilnya adalah `item_similarity_df`, sebuah matriks di mana setiap sel menunjukkan kesamaan antara dua film.

### 4. Fungsi Rekomendasi
* Membuat fungsi `get_similar_movies()` yang menerima `movie_title` (judul film yang disukai) dan `num_recommendations` sebagai input.
* Fungsi ini mencari film-film yang paling mirip dengan film input dari `item_similarity_df` dan mengembalikan daftar rekomendasi teratas.

### 5. Evaluasi & Contoh Hasil

Evaluasi sistem rekomendasi sederhana ini dilakukan secara kualitatif dengan melihat relevansi rekomendasi yang dihasilkan.

* **Contoh Rekomendasi 'Toy Story (1995)':**
    ```
    Jika Anda menyukai 'Toy Story (1995)', mungkin Anda juga menyukai:
    title
    Toy Story 2 (1999)                                   0.572601
    Jurassic Park (1993)                                 0.565637
    Independence Day (a.k.a. ID4) (1996)                 0.564262
    Star Wars: Episode IV - A New Hope (1977)            0.557388
    Forrest Gump (1994)                                  0.547096
    Lion King, The (1994)                                0.541145
    Star Wars: Episode VI - Return of the Jedi (1983)    0.541089
    Mission: Impossible (1996)                           0.538913
    Groundhog Day (1993)                                 0.534169
    Back to the Future (1985)                            0.530381
    Name: Toy Story (1995), dtype: float64
    ```

* **Contoh Rekomendasi 'Finding Nemo (2003)':**
    ```
    Jika Anda menyukai 'Finding Nemo (2003)', mungkin Anda juga menyukai:
    title
    Incredibles, The (2004)                                          0.726374
    Shrek (2001)                                                     0.701435
    Monsters, Inc. (2001)                                            0.697340
    Pirates of the Caribbean: The Curse of the Black Pearl (2003)    0.652265
    Shrek 2 (2004)                                                   0.625074
    Catch Me If You Can (2002)                                       0.594174
    Lord of the Rings: The Return of the King, The (2003)            0.587321
    Lord of the Rings: The Two Towers, The (2002)                    0.568078
    Harry Potter and the Prisoner of Azkaban (2004)                  0.560267
    Ocean's Eleven (2001)                                            0.556801
    Name: Finding Nemo (2003), dtype: float64
    ```

* **Wawasan**: Rekomendasi yang dihasilkan cukup relevan, seringkali menyarankan sekuel, film dengan genre serupa, atau film-film populer lain yang ditonton oleh demografi penonton yang sama. Ini menunjukkan bahwa pendekatan Collaborative Filtering Item-Based efektif dalam menemukan pola kesamaan selera di antara pengguna.

---

## Bagaimana Menjalankan Program Ini?

1.  **Clone repositori ini:**
    ```bash
    git clone [https://github.com/YourGitHubUsername/simple-movie-recommender.git](https://github.com/YourGitHubUsername/simple-movie-recommender.git)
    cd simple-movie-recommender
    ```
2.  **Unduh Dataset:**
    Unduh file `ml-latest-small.zip` dari [MovieLens Latest Datasets](https://grouplens.org/datasets/movielens/latest/). Ekstrak file ZIP tersebut, dan Anda akan menemukan `ratings.csv` serta `movies.csv`.
3.  **Tempatkan Dataset:**
    Unggah kedua file CSV (`ratings.csv` dan `movies.csv`) ke Google Drive Anda. **Sesuaikan `base_path`** dalam notebook Colab Anda agar menunjuk ke folder tempat Anda menyimpan dataset.
4.  **Buka di Google Colab:**
    Buka file `Simple_Recommendation_System.ipynb` di Google Colab. Pastikan Anda sudah *mount* Google Drive Anda di Colab.
5.  **Jalankan Sel-sel Kode:**
    Jalankan setiap sel kode secara berurutan (`Runtime > Run all`) untuk mereplikasi seluruh proses pembangunan sistem rekomendasi.

---

## Teknologi yang Digunakan

* **Python**
* **Pandas**
* **NumPy**
* **Scikit-learn** (untuk `cosine_similarity`)
* **Matplotlib** (opsional, untuk visualisasi jika ditambahkan)
* **Seaborn** (opsional, untuk visualisasi jika ditambahkan)
* **Google Colab**

---

## Kontributor

* [Nama Lengkap Anda] ([Link Profil GitHub Anda](https://github.com/YourGitHubUsername)) - (Pengembang Utama)

---

### Langkah Final Anda di GitHub:

1.  **Buat Repositori Baru:**
    * Nama Repositori: `simple-movie-recommender`
    * Deskripsi: `A basic movie recommendation system built using Item-Based Collaborative Filtering. Recommends movies based on user rating patterns.`
2.  **Edit `README.md`:** Salin seluruh teks `README.md` di atas, tempelkan ke `README.md` di repositori Anda. Pastikan untuk mengisi `[Nama Lengkap Anda]` dan `[Link Profil GitHub Anda]`.
3.  **Unggah File Proyek:**
    * `Simple_Recommendation_System.ipynb`
    * `ratings.csv` dan `movies.csv` (Anda bisa membuat folder `data/` di GitHub lalu mengunggahnya ke sana).
    * `requirements.txt` (buat dari Colab dengan `!pip freeze > requirements.txt` lalu unduh dan unggah).

Dengan ini, proyek sistem rekomendasi Anda siap dipamerkan! Ini adalah demonstrasi yang sangat bagus untuk portofolio AI Anda.

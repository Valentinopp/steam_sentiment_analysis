# Steam Sentiment Analysis

Steam Sentiment Analysis merupakan proyek analisis sentimen berbasis Natural Language Processing (NLP) yang bertujuan untuk mengklasifikasikan ulasan pengguna pada platform Steam ke dalam sentimen positif dan negatif. Proyek ini memanfaatkan representasi fitur teks menggunakan Term Frequency–Inverse Document Frequency (TF-IDF) serta beberapa algoritma machine learning untuk menganalisis persepsi pengguna terhadap gim digital secara otomatis.

---

## Sumber dan Karakteristik Data

Data yang digunakan berasal dari dataset publik Steam Reviews yang diperoleh melalui platform Kaggle, link dataset : https://www.kaggle.com/datasets/andrewmvd/steam-reviews?resource=download . Jumlah keseluruhan datanya adalah 6,4 juta, untuk mengurangi beban komputasi selama proses analisis dan pemodelan, digunakan sebanyak 350.000 data yang dipilih dengan mempertimbangkan keseimbangan distribusi kelas sentimen.

Dataset disimpan dalam basis data PostgreSQL dan dideploy menggunakan layanan Supabase, sehingga memungkinkan pengelolaan data berskala besar serta koneksi langsung dengan Python melalui library psycopg2.

Atribut utama yang digunakan dalam penelitian ini meliputi:
- user id: identitas unik pengguna Steam
- app_id: identitas unik aplikasi atau gim
- app_name: nama aplikasi atau gim
- review_text: teks ulasan pengguna
- review_score: kelas sentimen ulasan (-1 untuk negatif, 1 untuk positif)


## Preprocessing Data

Preprocessing dilakukan menggunakan pendekatan batch processing dengan ukuran batch sebesar 5.000 data untuk menjaga efisiensi penggunaan memori dan waktu komputasi. Tahapan preprocessing meliputi:
- Case folding
- Penghapusan karakter tidak perlu seperti newline, URL, dan spasi berlebih
- Penghapusan karakter non-alfanumerik
- Normalisasi kata slang menggunakan kamus slang bahasa Inggris
- Penghapusan stopwords bahasa Inggris
- Stemming menggunakan Porter Stemmer

Setelah seluruh tahapan preprocessing diterapkan, diperoleh sebanyak 337.609 data ulasan bersih yang digunakan pada tahap selanjutnya.

## Ekstraksi Fitur

Teks ulasan hasil preprocessing direpresentasikan ke dalam bentuk numerik menggunakan metode TF-IDF. Ekstraksi fitur dilakukan dengan TfidfVectorizer menggunakan parameter utama:
- max_features: 5.000
- ngram_range: (1,2)
- min_df: 5
- max_df: 0,9

Hasil proses ini berupa matriks fitur TF-IDF berdimensi n_dokumen × 5.000.

## Algoritma Klasifikasi

Penelitian ini membandingkan tiga algoritma klasifikasi, yaitu:
- Logistic Regression sebagai model baseline
- Random Forest sebagai pendekatan ensemble non-linear
- Support Vector Machine (SVM) dengan kernel linear

Pemilihan beberapa algoritma bertujuan untuk membandingkan performa model linear dan non-linear dalam menangani representasi TF-IDF berdimensi tinggi.

---

## Cara Menjalankan Notebook

### 1. Persiapan Environment

Pastikan Python telah terpasang pada sistem (disarankan Python versi 3.8 atau lebih baru). Selanjutnya, lakukan *clone* repositori ini:

git clone https://github.com/Valentinopp/steam_sentiment_analysis.git<br>
cd steam-sentiment-analysis

---
### 2. Install Dependensi

pip install -r requirements.txt

### 3. Jalankan notebook dari atas hingga bawah secara berurutan

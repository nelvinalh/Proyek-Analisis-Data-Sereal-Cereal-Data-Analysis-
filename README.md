# 🥣 Analisis Data Sereal & Nutrisi (Cereal Data Analysis)

## 📋 Deskripsi Proyek
Proyek ini bertujuan untuk melakukan analisis data eksploratif (*Exploratory Data Analysis*) terhadap dataset **Cereals**. Menggunakan **KNIME Analytics Platform**, proyek ini memproses data mentah, melakukan normalisasi nutrisi, dan memvisualisasikan hubungan antara kandungan nutrisi (seperti gula dan kalori) dengan kepuasan konsumen (*rating*).

## 🎯 Tujuan Utama
1.  **Data Preparation:** Membersihkan dan menstandarisasi data numerik.
2.  **Insight & Visualisasi:** Menemukan faktor nutrisi apa yang paling mempengaruhi *rating* sereal.
3.  **Statistik:** Melihat distribusi data nutrisi secara menyeluruh.

---

## 🛠️ Alur Kerja KNIME (Workflow)

Proyek ini dibangun menggunakan *workflow* KNIME dengan tahapan sebagai berikut:

### 1. Data Ingestion & Preparation
* **CSV Reader:** Membaca dataset `Cereals.csv`.
* **Column Filter:** Menyeleksi fitur yang relevan untuk analisis dengan mengecualikan (exclude) kolom name, shelf, weight, dan cups, serta mempertahankan seluruh kolom data lainnya.

### 2. Data Processing
* **Normalizer:** Mengubah skala semua kolom numerik (*calories, protein, fat, sugars, rating*, dll.) menjadi rentang **0 hingga 1 (Min-Max Normalization)**. Langkah ini penting agar perbandingan antar nutrisi menjadi adil dan visualisasi lebih akurat.

### 3. Visualisasi & Analisis
Output dari proses normalisasi dihubungkan ke beberapa *node* visualisasi secara paralel:
* **Scatter Plot:**Menganalisis korelasi antara dua variabel (Gula vs Rating).
* **Histogram:** Melihat distribusi frekuensi *rating* sereal.
* **Bar Chart:** Membandingkan rata-rata kalori per produk/produsen.
* **Statistics:** Menghasilkan ringkasan statistik deskriptif (Mean, Median, Standard Deviation).

---

## 📊 Hasil Analisis (Key Insights)
Berdasarkan hasil visualisasi dari *workflow* KNIME, ditemukan beberapa *insight* penting:

### 📉 1. Korelasi Gula dan Rating (Scatter Plot)
* **Temuan:** Terdapat **korelasi negatif yang kuat** antara kandungan gula (*sugars*) dan *rating*.
* **Interpretasi:** Titik-titik data bergerak menurun dari kiri atas ke kanan bawah. Artinya, **semakin tinggi kandungan gula, semakin rendah rating sereal tersebut.** Konsumen cenderung memberikan nilai lebih tinggi pada sereal yang lebih sehat (rendah gula).

### 📊 2. Distribusi Rating (Histogram)
* **Temuan:** Mayoritas sereal memiliki *rating* di kisaran menengah ke bawah. Hanya sedikit sereal yang mencapai *rating* sangat tinggi (>80), yang biasanya merupakan sereal dengan serat tinggi dan gula rendah.

### 🍫 3. Kandungan Kalori (Bar Chart)
* **Temuan:** Visualisasi ini membantu mengidentifikasi sereal mana yang paling padat energi. Sereal dengan kalori tinggi sering kali tidak berbanding lurus dengan *rating* tinggi.

---

## 📝 Kesimpulan
Faktor nutrisi, terutama **kandungan gula**, memainkan peran krusial dalam penentuan *rating* sereal. Produsen sereal yang ingin meningkatkan persepsi konsumen sebaiknya fokus pada pengurangan gula dan peningkatan serat, sebagaimana dibuktikan oleh korelasi data yang kuat.

---

## 🌟 Bonus: Tahap Klasifikasi (Machine Learning)

Sebagai langkah lanjutan untuk memprediksi kategori *rating*, alur kerja diperluas dengan algoritma **Decision Tree**:

1.  **Numeric Binner:** Mengelompokkan kolom *rating* (numerik) menjadi 3 kategori: *Low, Medium, High*.
2.  **One to Many:** Mengubah data kategori (Produsen/Type) menjadi angka biner.
3.  **Table Partitioner:** Membagi data menjadi **80% Data Latih** dan **20% Data Uji**.
4.  **Decision Tree Learner & Predictor:** Melatih model untuk memprediksi kategori *rating* berdasarkan fitur nutrisi.
5.  **Scorer:** Mengevaluasi akurasi prediksi model.

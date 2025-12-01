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
* **Scatter Plot:** Menganalisis korelasi antara dua variabel (Gula vs Rating).
* **Histogram:** Melihat distribusi frekuensi *rating* sereal.
* **Bar Chart:** Membandingkan rata-rata kalori per produk/produsen.
* **Statistics:** Menghasilkan ringkasan statistik deskriptif (Mean, Median, Standard Deviation).

### 4. Klasifikasi
Pada tahap ini dilakukan proses **klasifikasi kesehatan sereal** menggunakan node **Rule Engine** di KNIME. Klasifikasi dibuat berdasarkan kandungan nutrisi untuk menentukan apakah suatu sereal termasuk **sehat** atau **tidak sehat**.
#### 1. Membuat Label Kesehatan (`health_class`) — Rule Engine
Node **Rule Engine** digunakan untuk membuat kolom baru bernama `health_class`.
Aturan yang digunakan:
$fiber$ >= 5 AND $sugars$ <= 6 => "Healthy"
TRUE => "Not Healthy"
Penjelasan aturan:
- Sereal dianggap **Healthy** jika **serat ≥ 5** dan **gula ≤ 6**.  
- Selain itu dikategorikan sebagai **Not Healthy**.
#### 2. Visualisasi Hasil — Pie Chart
Node **Pie Chart** digunakan untuk menampilkan distribusi kategori `health_class`.
Pengaturan pada Pie Chart:
- **Category Dimension:** `health_class`
- **Aggregation:** Occurrence count
- **Aggregate small categories:** Off
Pie chart menampilkan proporsi antara sereal **Healthy** dan **Not Healthy**.

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

### 🥗 4. Distribusi Kesehatan Sereal (Klasifikasi)
Hasil klasifikasi menggunakan Rule Engine menunjukkan bahwa sebagian besar produk dalam dataset tidak memenuhi kriteria sereal sehat.  
- **Healthy:** 5.19%  
- **Not Healthy:** 94.81%  
Hal ini menunjukkan bahwa mayoritas sereal pada dataset memiliki kandungan serat rendah dan/atau gula yang cukup tinggi, sehingga tidak memenuhi standar nutrisi yang ditetapkan.

---

## 📝 Kesimpulan
Faktor nutrisi, terutama **kandungan gula**, memainkan peran krusial dalam penentuan *rating* sereal. Produsen sereal yang ingin meningkatkan persepsi konsumen sebaiknya fokus pada pengurangan gula dan peningkatan serat, sebagaimana dibuktikan oleh korelasi data yang kuat.


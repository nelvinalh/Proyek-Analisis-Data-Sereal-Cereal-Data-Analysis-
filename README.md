# 🥣 Analisis Data Sereal & Nutrisi (Cereal Data Analysis)

## 📋 Deskripsi Proyek
Proyek ini bertujuan untuk melakukan **Analisis Data Eksploratif (Exploratory Data Analysis)** terhadap dataset **Cereals** menggunakan **KNIME Analytics Platform**.

Analisis dilakukan secara menyeluruh mulai dari pembersihan data (*data cleaning*), normalisasi, visualisasi pola nutrisi, hingga klasifikasi tingkat kesehatan sereal berdasarkan kandungan gula dan serat.

## 🎯 Tujuan Utama
1.  **Membersihkan dan menstandarisasi data** untuk memastikan kualitas data yang baik sebelum dianalisis.
2.  **Melakukan visualisasi distribusi nutrisi** agar dapat memahami pola dan karakteristik dataset.
3.  **Melakukan klasifikasi** (*Healthy / Not Healthy*) dan tingkat gula (*Sugar Level*) berdasarkan ambang batas nutrisi yang valid.
4.  **Mencari insight** tentang hubungan nutrisi (terutama serat & gula) terhadap kualitas sereal (*rating*).

---

## 🛠️ KNIME Workflow
Workflow KNIME dibangun dengan tahapan sistematis sebagai berikut:

### 1. 📥 Data Ingestion
* **Node:** `CSV Reader`
    * Membaca dataset `Cereals.csv` sebagai data mentah.

### 2. 🧹 Data Cleaning & Preparation
* **Node:** `Column Filter`
    * Menghapus kolom yang tidak dibutuhkan: `name`, `shelf`, `weight`, `cups`.
    * Mempertahankan kolom nutrisi dan rating.
* **Node:** `Missing Value`
    * Menangani nilai hilang.
    * **Strategi:** Kolom numerik (integer & float) diganti dengan nilai **Median**.

### 3. 📊 Visualisasi Data (Sebelum Normalisasi)
Pada tahap ini, dilakukan eksplorasi data untuk memahami struktur dan pola awal.

* **Histogram Sugars**
    * Menampilkan distribusi kadar gula.
    * *Insight:* Sebagian besar sereal memiliki gula yang cukup tinggi.
* **Histogram Fiber**
    * Menampilkan distribusi kandungan serat.
    * *Insight:* Kebanyakan sereal memiliki serat rendah, hanya sedikit produk dengan serat tinggi.
* **Bar Chart (Rating)**
    * Sumbu X = Rating, Y = Average.
    * Memudahkan melihat kecenderungan produk rating tinggi/rendah.
* **Box Plot (Sugar & Fiber)**
    * Menentukan persebaran nilai serta mendeteksi *outlier*.
    * *Insight:* Sugar memiliki rentang yang jauh lebih lebar daripada fiber.
* **Scatter Plot (Rating vs Sugars)**
    * Sumbu Y = Rating, Sumbu X = Sugars.
    * *Interpretasi:* Terlihat jelas bahwa **semakin tinggi gula → rating semakin rendah.**

### 4. 🔧 Normalisasi Data
* **Node:** `Normalizer`
    * Metode: **Min–Max Normalization** (Rentang 0–1).
    * Tujuan: Agar fitur nutrisi dapat dibandingkan secara adil.
* **Node:** `Statistics`
    * Menghasilkan nilai statistik seperti mean, median, min, max.
    * **Threshold yang ditemukan (Normalized Mean):**
        * Rata-rata sugars: **0.468**
        * Rata-rata fiber: **0.154**
    * *Catatan:* Kedua nilai ini digunakan sebagai dasar aturan klasifikasi di tahap berikutnya.

### 5. 🧭 Klasifikasi & Aturan Logika (Rule Engine)

#### A. Rule Engine 1: Klasifikasi Kesehatan (health_class)
Tujuan: Mengelompokkan sereal menjadi "Sehat" atau "Tidak Sehat".

**Aturan Logika (Code):**
```text
$fiber$ >= 0.154 AND $sugars$ <= 0.468 => "Healthy"
TRUE => "Not Healthy"
```
## 📊 Hasil Visualisasi & Klasifikasi

Berikut adalah hasil dari penerapan aturan logika (*Rule Engine*) dan visualisasi distribusi data.

### A. Klasifikasi Kesehatan (Health Class)
Visualisasi ini menunjukkan proporsi sereal yang memenuhi kriteria "Sehat" (Serat tinggi, Gula rendah).

* **Pie Chart Distribution:**
    * 🟢 **Healthy:** 25.97%
    * 🔴 **Not Healthy:** 74.03%
* **Scatter Plot (Color by Health Class):**
    * Plot antara *fiber* dan *sugar* dengan pewarnaan kategori menunjukkan pemisahan data yang tegas antara kelompok sehat dan tidak.

### B. Rule Engine 2: Klasifikasi Sugar Level (sugar_level)
**Tujuan:** Mengategorikan level gula sereal.

**Aturan Logika (Code):**
```text
$sugars$ <= 0.468 => "Low Sugar"
TRUE => "High Sugar"
```

### Hasil Visualisasi (Pie Chart)
* **Low Sugar:** 54.55%
* **High Sugar:** 45.45%

---

## 💡 Hasil Analisis (Key Insights)

1.  **Hubungan Gula & Rating**
    Scatter plot menunjukkan pola negatif yang kuat. **Semakin tinggi gula, semakin rendah rating** sereal tersebut.
2.  **Sebaran Serat & Gula**
    *Fiber* cenderung rendah pada mayoritas produk. *Sugar* lebih bervariasi, tetapi banyak yang berada di level tinggi.
3.  **Klasifikasi Healthy**
    Hanya **25.97%** produk yang memenuhi kriteria sehat (kombinasi serat tinggi + gula rendah). Ini selaras dengan visualisasi awal yang menunjukkan kelangkaan sereal berserat tinggi.
4.  **Sugar Level vs Health**
    Meskipun **54.55%** sereal berada dalam kategori *Low Sugar*, banyak dari mereka tetap masuk kategori *Not Healthy* karena kandungan seratnya yang sangat rendah.

---

## 🎯 Tujuan Klasifikasi & Visualisasi

### ✨ Tujuan Visualisasi
* **Memahami Pola:** Melihat distribusi kandungan nutrisi secara visual.
* **Analisis Korelasi:** Melihat hubungan antara nutrisi & rating.
* **Dasar Aturan:** Menemukan alasan kuat untuk membuat aturan klasifikasi.
* **Penentuan Threshold:** Menentukan ambang batas (*threshold*) untuk *sugar* & *fiber* berdasarkan nilai statistik dan grafik.

### ✨ Tujuan Klasifikasi
* **Segmentasi Produk:** Mengelompokkan produk berdasarkan kesehatan (*Healthy vs Not Healthy*).
* **Simplifikasi:** Menyederhanakan interpretasi data dengan memberikan label kategori yang mudah dipahami.
* **Rekomendasi:** Membantu menjawab pertanyaan: *"Sereal mana yang paling layak dikategorikan sehat?"*
* **Proporsi Data:** Menganalisis proporsi gula rendah vs gula tinggi pada dataset.
* **Pengambilan Keputusan:** Memberikan dasar yang kuat untuk rekomendasi produk sehat.

---

## 📝 Kesimpulan

Analisis ini menunjukkan bahwa kandungan **gula** dan **serat** adalah faktor paling berpengaruh terhadap persepsi konsumen (*rating*) dan kesehatan sereal secara keseluruhan.

**Poin Penting:**
* Rata-rata sereal memiliki gula tinggi dan serat rendah.
* Hanya sebagian kecil produk yang memenuhi kriteria sehat.
* Kandungan gula memiliki korelasi negatif yang signifikan terhadap rating sereal.

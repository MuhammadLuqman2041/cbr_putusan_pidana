# Sistem Case-Based Reasoning (CBR) – Analisis Putusan Pengadilan Pidana

> **Mata Kuliah:** Penalaran Komputer  
> **Semester:** Genap Tahun Akademik 2025/2026  
> **Fakultas Teknik Informatika – Universitas Muhammadiyah Malang**

---

## Deskripsi Proyek

Proyek ini mengimplementasikan sistem **Case-Based Reasoning (CBR)** berbasis Python untuk mendukung analisis putusan pengadilan pidana. Dataset yang digunakan berasal dari **Direktori Putusan Mahkamah Agung Republik Indonesia**, dengan domain perkara **Pidana Umum – Pencurian (Pid.B)**, PN Malang.

Sistem ini mengikuti siklus CBR lengkap:
1. **Membangun Case Base** – Scraping & preprocessing 40 dokumen PDF putusan
2. **Case Representation** – Ekstraksi metadata dan feature engineering
3. **Case Retrieval** – TF-IDF + SVM / Naive Bayes dengan cosine similarity
4. **Case Solution Reuse** – Prediksi solusi (amar putusan) dengan weighted similarity & majority vote
5. **Model Evaluation** – Accuracy, Precision, Recall, F1-score, Cross-Validation, Error Analysis

---

## Struktur Repository

```
cbr-putusan-pengadilan/
├──  data/
│   ├──  raw/                    # Teks putusan hasil preprocessing (.txt)
│   │   ├── case_001.txt
│   │   ├── case_002.txt
│   │   └── ...
│   ├──  processed/              # Data terstruktur hasil representasi
│   │   ├── cases.csv
│   │   └── cases.json
│   ├──  eval/                   # Data evaluasi dan metrik
│   │   ├── queries.json
│   │   ├── retrieval_metrics.csv
│   │   └── prediction_metrics.csv
│   └──  results/                # Hasil prediksi solusi
│       └── predictions.csv
├──  logs/
│   └── cleaning.log               # Log proses pembersihan dokumen
├──  notebooks/
│   └── cbr_putusan_pidana.ipynb   # Notebook utama pipeline CBR
├──  output/
│   ├── confusion_reuse_weighted.png
│   ├── model_performance.png
│   └── label_distribution.png
├── README.md
└── requirements.txt
```

---

##  Instalasi

### Prasyarat
- Python 3.8 atau lebih baru
- Google Colab (direkomendasikan) **atau** Jupyter Notebook lokal

### Install Dependencies

```bash
pip install -r requirements.txt
```

Atau jika menggunakan Google Colab (sudah terintegrasi di Cell 0 notebook):

```python
import subprocess, sys
subprocess.run([sys.executable, '-m', 'pip', 'install', '-q', 'pdfplumber'])
subprocess.run([sys.executable, '-m', 'pip', 'install', '-q', 'pdfminer.six'])
subprocess.run([sys.executable, '-m', 'pip', 'install', '-q', 'nltk'])
subprocess.run([sys.executable, '-m', 'pip', 'install', '-q', 'scikit-learn'])
subprocess.run([sys.executable, '-m', 'pip', 'install', '-q', 'matplotlib'])
subprocess.run([sys.executable, '-m', 'pip', 'install', '-q', 'seaborn'])
```

---

##  Cara Menjalankan Pipeline

### Menggunakan Google Colab (Direkomendasikan)

1. **Upload notebook** ke Google Colab:  
   Buka [colab.research.google.com](https://colab.research.google.com) → `File → Upload notebook` → pilih `CBR_Putusan_Pidana.ipynb`

2. **Siapkan data** di Google Drive:  
   Upload folder PDF putusan ke Google Drive dengan path:
   ```
   /MyDrive/Kasus_Penalaran/DATA 40 DOKUMEN PUTUSAN/
   ```

3. **Jalankan semua cell secara berurutan** (Cell 0 → Cell 6):
   - `Runtime → Run all` atau jalankan satu per satu dengan `Shift+Enter`

4. **Output** akan tersimpan otomatis di:
   ```
   /MyDrive/Kasus_Penalaran/
   ```

### Menggunakan Jupyter Notebook Lokal

1. Clone repository:
   ```bash
   git clone https://github.com/MuhammadLuqman2041/cbr_putusan_pidana.git
   cd <repo-name>
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Ubah konfigurasi path di **Cell 1** pada notebook:
   ```python
   # Ganti path GDrive dengan path lokal
   BASE_PATH = "./Kasus_Penalaran"         # sebelumnya: /content/drive/MyDrive/Kasus_Penalaran
   DATA_PATH = f"{BASE_PATH}/data/pdf"     # folder berisi 40 PDF putusan
   ```

4. Jalankan Jupyter:
   ```bash
   jupyter notebook notebooks/cbr_putusan_pidana.ipynb
   ```

---

##  Dataset

| Atribut | Detail |
|---|---|
| Sumber | [Direktori Putusan Mahkamah Agung RI](https://putusan3.mahkamahagung.go.id) |
| Domain Perkara | Pidana Umum – Pencurian (Pid.B) – Pengadilan Negeri Malang |
| Jumlah Dokumen | 40 putusan |
| Tahun Putusan | 2025 |
| Format Asli | PDF |
| Label Klasifikasi | Pasal 362, 363, 374, 25 KUHP |

---

##  Metode yang Digunakan

| Tahap CBR | Metode |
|---|---|
| Preprocessing | pdfplumber, regex cleaning, normalisasi teks |
| Representasi Vektor | TF-IDF (15.000 fitur, n-gram 1–2, sublinear TF) |
| Model Retrieval | Support Vector Machine (SVM linear) + Naive Bayes |
| Similarity | Cosine Similarity |
| Solution Reuse | Weighted Similarity & Majority Vote |
| Evaluasi | Accuracy, Precision, Recall, F1 (macro), Stratified K-Fold CV |

---

##  Hasil Evaluasi

| Model | Accuracy | Precision | Recall | F1-Score |
|---|---|---|---|---|
| Retrieval SVM | 1.0000 | 1.0000 | 1.0000 | 1.0000 |
| Retrieval NB | 0.7500 | 0.3750 | 0.5000 | 0.4286 |
| Reuse Weighted | 1.0000 | 1.0000 | 1.0000 | 1.0000 |
| Reuse Majority | 1.0000 | 1.0000 | 1.0000 | 1.0000 |
| SVM Klasifikasi | 0.6250 | 0.3125 | 0.5000 | 0.3846 |
| NB Klasifikasi | 0.6250 | 0.3125 | 0.5000 | 0.3846 |

> **Catatan:** Nilai retrieval 100% diperoleh karena query evaluasi diambil dari ringkasan fakta dokumen dalam dataset (test split). Performa pada kasus benar-benar baru dapat berbeda. Akurasi klasifikasi SVM/NB yang lebih rendah (~62.5%) mencerminkan performa yang lebih realistis akibat ketidakseimbangan kelas dan kemiripan tekstual antar pasal.

---

##  Rekomendasi Perbaikan

1. Gunakan **embedding semantik** (IndoBERT / Sentence-BERT) untuk representasi dokumen yang lebih kaya makna
2. Lakukan **augmentasi data** atau SMOTE untuk menangani kelas minoritas (Pasal 25 & 374)
3. Tambahkan **fitur khusus** untuk membedakan Pasal 362 vs 363 (misalnya keberadaan unsur pemberatan)
4. Terapkan **confidence threshold** – jika similarity maksimum < 0.1, sistem abstain (rejection)
5. Evaluasi dengan **MRR (Mean Reciprocal Rank)** dan **Precision@k** untuk analisis retrieval yang lebih mendalam

---

##  Tim Pengembang

| Nama | NIM |
|---|---|
| (Bukhary Kelian) | (202310370311014) |
| Moch. Luqman Hakim | (202310370311014) |


---

##  Lisensi

Proyek ini dibuat untuk keperluan akademik – Tugas Mata Kuliah Penalaran Komputer, Fakultas Teknik Informatika, Universitas Muhammadiyah Malang.

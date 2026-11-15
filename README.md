
# Proyek Akhir Pembelajaran Mesin — Kelompok 4

**Mata Kuliah:** Pembelajaran Mesin (INF62325)    
**Program Studi:** Informatika  
**Kelas:** 5B  
**Paket Dataset:** 5B-4 — IBM HR Employee Attrition  
**Metode:** K-Means Clustering

## 1. Deskripsi Proyek

Repository ini digunakan untuk mendokumentasikan pengerjaan proyek akhir mata kuliah Pembelajaran Mesin menggunakan algoritma **K-Means Clustering**.

Proyek bertujuan melakukan segmentasi karyawan berdasarkan karakteristik individu dan pekerjaan, kemudian menganalisis profil setiap kelompok yang terbentuk.

Pengerjaan mengikuti delapan tahap yang ditetapkan pada PPT Pertemuan 11, mulai dari penyusunan proposal hingga pelaporan hasil akhir.

## 2. Anggota Kelompok dan Pembagian Varian

| Anggota          | Varian                      | Tugas                                                                              |
| ---------------- | --------------------------- | ---------------------------------------------------------------------------------- |
| Noval Adrian     | Varian 1 — Penentuan K     | Menggunakan Elbow Method untuk K = 2–10 dan menentukan titik siku                 |
| Faqih Huddin SM. | Varian 2 — Validasi K      | Menghitung Silhouette Score pada setiap K dan membandingkan hasilnya dengan Elbow  |
| Irfan Mubarok    | Varian 3 — Pemilihan Fitur | Membandingkan hasil clustering dari dua himpunan fitur dan menyusun profil klaster |

Tahap proposal, eksplorasi, praproses, evaluasi, penyimpanan model, pengembangan aplikasi, dan pelaporan dikerjakan bersama. Tahap eksperimen tiga varian menjadi tanggung jawab masing-masing anggota.

## 3. Dataset

Dataset yang digunakan adalah **IBM HR Analytics Employee Attrition & Performance**, yaitu dataset sintetis yang memuat karakteristik karyawan.

**Sumber:** [IBM HR Analytics Employee Attrition — Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)

Informasi dataset:

- Jumlah observasi: 1.470 baris
- Jumlah atribut awal: 35 kolom
- Unit analisis: satu baris mewakili satu karyawan
- Metode analisis: *Unsupervised Learning* (K-Means Clustering)

Variabel `Attrition` tidak digunakan sebagai fitur pembentuk klaster, tetapi disimpan sebagai bahan interpretasi setelah proses clustering.

## 4. Alur Pengerjaan Proyek

### Tahap 0 — Proposal

Menyusun proposal yang menjelaskan dataset, permasalahan, tujuan analisis, rancangan fitur, pembagian eksperimen, dan rencana aplikasi web sesuai template dosen.

### Tahap 1 — Eksplorasi Data

Eksplorasi data dilakukan untuk memahami karakteristik dan kualitas dataset sebagai dasar pengambilan keputusan praproses.

Pemeriksaan mencakup struktur data, nilai hilang, duplikat, nilai tidak valid, pencilan, distribusi variabel, korelasi antarfitur, dan pemilihan kandidat fitur segmentasi.

**Hasil EDA awal:**

- Dataset memiliki 1.470 baris dan 35 kolom.
- Tidak ditemukan nilai hilang dan duplikat identik.
- Tiga kolom konstan diidentifikasi: `EmployeeCount`, `StandardHours`, dan `Over18`.
- Data bersih memiliki 1.470 baris dan 33 kolom, termasuk satu fitur biner turunan.
- Dua belas fitur ditetapkan sebagai kandidat awal segmentasi.
- Penskalaan diperlukan karena adanya perbedaan skala antarfitur.

Proses dan dasar pengambilan keputusan didokumentasikan dalam `notebook/01_eksplorasi.ipynb`.

### Tahap 2 — Praproses Data

Menyiapkan data untuk proses clustering berdasarkan hasil eksplorasi.

Praproses mencakup pemilihan fitur, transformasi data, dan penskalaan menggunakan metode yang sesuai. Seluruh anggota kelompok menggunakan dataset bersih yang sama sebagai dasar eksperimen.

### Tahap 3 — Eksperimen Tiga Varian

Setiap anggota menjalankan eksperimen K-Means sesuai pembagian tugas:

- **Varian 1:** penentuan jumlah klaster menggunakan Elbow Method.
- **Varian 2:** validasi jumlah klaster menggunakan Silhouette Score.
- **Varian 3:** perbandingan dua himpunan fitur dan analisis profil klaster.

Hasil dari ketiga eksperimen digunakan sebagai bahan evaluasi dan perbandingan model.

### Tahap 4 — Evaluasi Model

Membandingkan hasil dari ketiga varian menggunakan metrik dan interpretasi yang sesuai dengan metode K-Means.

Evaluasi mencakup:

- Perbandingan hasil Elbow Method dan Silhouette Score.
- Analisis perbedaan hasil antarvarian.
- Penyusunan tabel perbandingan eksperimen.
- Pemilihan konfigurasi model berdasarkan hasil evaluasi dan interpretasi.
- Penamaan setiap klaster secara bermakna.
- Penyusunan satu rekomendasi tindakan untuk setiap segmen.

### Tahap 5 — Penyimpanan Model

Menyimpan model K-Means terpilih beserta komponen praproses dan informasi fitur yang dibutuhkan menggunakan berkas `.pkl`.

Model tersimpan akan digunakan kembali pada aplikasi tanpa melakukan pelatihan ulang.

### Tahap 6 — Pengembangan dan Publikasi Aplikasi Web

Mengembangkan aplikasi web yang menerima masukan pengguna, menerapkan praproses yang sesuai, dan menghasilkan informasi klaster berdasarkan model tersimpan.

Aplikasi akan dipublikasikan sehingga dapat diakses melalui URL aktif.

**Tautan aplikasi:** Belum tersedia.

### Tahap 7 — Pelaporan dan Presentasi

Menyusun laporan akhir, dokumentasi README, serta slide presentasi yang menjelaskan proses pengerjaan, hasil eksperimen, evaluasi, dan aplikasi yang dikembangkan.

## 5. Struktur Repository

Struktur repository yang dituju sesuai ketentuan dosen:

```text
Kelompok-4_K-Means_IBM-HR-Employee-Attrition/
├── 00_proposal.pdf
├── notebook/
│   ├── 01_eksplorasi.ipynb
│   ├── 02_praproses.ipynb
│   ├── 03_varian_1.ipynb
│   ├── 03_varian_2.ipynb
│   └── 03_varian_3.ipynb
├── data/
│   ├── data_bersih.csv
│   └── kamus_data.csv
├── model/
├── app.py
├── requirements.txt
└── README.md
```

Struktur tersebut merupakan target akhir. Berkas yang belum dibuat akan ditambahkan sesuai perkembangan pengerjaan proyek.

Dataset mentah tidak disertakan dalam repository dan dapat diperoleh melalui tautan sumber pada bagian Dataset.

## 6. Jadwal Pengerjaan

| Pertemuan | Kegiatan                                                                        |
| --------- | ------------------------------------------------------------------------------- |
| 11        | Pemilihan paket, pembagian varian, pemeriksaan dataset, dan penyusunan proposal |
| 12        | Progres 1: penyiapan data bersih dan mulai eksperimen                           |
| 13        | Penyelesaian tiga varian serta penyusunan tabel perbandingan                    |
| 14        | Progres 2: evaluasi eksperimen, penyimpanan model, dan pengembangan aplikasi    |
| 15        | Progres 3: aplikasi, laporan, dokumentasi, dan persiapan presentasi             |
| 16        | Presentasi akhir proyek (UAS)                                                   |

## 7. Menjalankan Notebook EDA

Notebook eksplorasi dapat dijalankan menggunakan Visual Studio Code dengan ekstensi Python dan Jupyter.

Pustaka yang digunakan antara lain `pandas`, `numpy`, `matplotlib`, dan `seaborn`.

Dataset mentah perlu diunduh terlebih dahulu melalui sumber Kaggle dan disimpan secara lokal pada folder `data/` menggunakan nama:

`IBM_HR_Employee_Attrition.csv`

Setelah itu, buka `notebook/01_eksplorasi.ipynb`, pilih kernel Python yang sesuai, kemudian jalankan seluruh sel secara berurutan.

Petunjuk menjalankan aplikasi web akan ditambahkan setelah tahap pengembangan aplikasi selesai.

## 8. Catatan

Dataset yang digunakan bersifat sintetis dan ditujukan untuk pembelajaran. Hasil segmentasi tidak dapat langsung digeneralisasikan sebagai kondisi aktual karyawan pada suatu perusahaan.

Dokumentasi akan diperbarui sesuai perkembangan pengerjaan proyek dan hasil evaluasi dari setiap tahap.

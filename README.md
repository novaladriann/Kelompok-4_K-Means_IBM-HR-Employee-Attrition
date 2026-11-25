# Proyek Akhir Pembelajaran Mesin — Kelompok 4

**Mata Kuliah:** Pembelajaran Mesin (INF62325)  
**Program Studi:** S1 Informatika, FITK, UIN Siber Syekh Nurjati Cirebon  
**Kelas:** 5B  
**Paket Dataset:** 5B-4 — IBM HR Employee Attrition  
**Metode:** Klastering (K-Means)  
**Tautan Aplikasi Web:** *(Belum tersedia — dijadwalkan pada Pertemuan 14–15)*

---

## 1. Deskripsi Proyek

Repositori ini mendokumentasikan pengerjaan proyek akhir mata kuliah Pembelajaran Mesin (Cabang C — Klastering K-Means) untuk melakukan **segmentasi profil karyawan** berdasarkan karakteristik individu, pengalaman kerja, kompensasi, kepuasan kerja, dan status lembur pada dataset *IBM HR Analytics Employee Attrition & Performance*.

**Rumusan pertanyaan analitik:** *Berapa kelompok alami karyawan yang dapat dibentuk berdasarkan karakteristik pekerjaan dan pengalaman kerja, serta bagaimana profil dan rekomendasi tindakan untuk masing-masing kelompok?*

---

## 2. Anggota Kelompok dan Pembagian Varian

Seluruh anggota mengerjakan tahap penyiapan data (Pertemuan 12), perbandingan kriteria & pemilihan fitur (Bagian 5), penyimpanan model, pengembangan aplikasi web, serta pelaporan secara bersama. Tahap eksperimen tiga varian dikerjakan masing-masing anggota secara mandiri di atas `data/data_bersih.csv`, penskalaan (`StandardScaler`), dan nilai acak (`SEED = 42`) yang sama.

| Anggota | NIM | Varian | Kriteria & Fokus Pengerjaan | Notebook |
| :--- | :--- | :--- | :--- | :--- |
| Noval Adrian | 2488010018 | Varian 1 — Penentuan k | **Metode Elbow (Inertia)** untuk k = 2 sampai 10 dan penentuan titik siku (jarak tegak lurus terjauh) | `notebook/03_varian_1.ipynb` |
| Faqih Huddin SM. | 2488010061 | Varian 2 — Validasi k | **Silhouette Score** untuk k = 2 sampai 10 beserta diagram silhouette per sampel | `notebook/03_varian_2.ipynb` |
| Irfan Mubarok | 2488010075 | Varian 3 — Validasi k | **Davies-Bouldin Index** untuk k = 2 sampai 10 dengan pemeriksa silang Calinski-Harabasz | `notebook/03_varian_3.ipynb` |
| Kelompok 4 | - | Bagian 5 — Evaluasi & Fitur | Perbandingan tiga kriteria penentuan k dan pemilihan himpunan fitur final | `notebook/04_perbandingan.ipynb` |

---

## 3. Dataset

Dataset yang digunakan adalah **IBM HR Analytics Employee Attrition & Performance**, yaitu dataset sintetis sumber terbuka mengenai atribut demografi dan pekerjaan karyawan.

* **Sumber Kaggle:** [IBM HR Analytics Employee Attrition](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)
* **Slug `kagglehub`:** `pavansubhasht/ibm-hr-analytics-attrition-dataset`
* **Ukuran data mentah (`data/raw.csv`):** 1.470 baris dan 35 kolom
* **Unit analisis:** Satu baris mewakili **satu orang karyawan unik** (terverifikasi dari 1.470 nilai unik pada `EmployeeNumber` dan 0 baris duplikat).
* **Ukuran data bersih (`data/data_bersih.csv`):** 1.470 baris dan 13 kolom (1 kolom identitas teks `EmployeeNumber` berformat `EMP_*` + 12 fitur numerik/ordinal/biner kandidat klastering).

**12 Fitur Kandidat Segmentasi:**
`Age`, `MonthlyIncome`, `TotalWorkingYears`, `YearsAtCompany`, `YearsSinceLastPromotion`, `NumCompaniesWorked`, `DistanceFromHome`, `JobSatisfaction`, `EnvironmentSatisfaction`, `WorkLifeBalance`, `JobInvolvement`, dan `OverTime_Yes`.

> **Catatan:** Variabel `Attrition` tidak disertakan sebagai fitur pembentuk klaster karena merupakan label status keluar/bertahannya karyawan; atribut tersebut bersama kolom kategori lain (`Department`, `JobRole`, `JobLevel`, dll.) hanya dipakai sebagai konteks deskriptif setelah segmen terbentuk.

---

## 4. Pernyataan Urutan Kerja (Progres 1 — Penyiapan Data)

> Pada cabang klastering tidak ada pemisahan data latih dan data uji, karena tidak ada label target yang diuji. Pembersihan dilakukan satu kali atas seluruh 1.470 baris data: pemeriksaan duplikat (0 baris dibuang), pemisahan kolom identitas `EmployeeNumber` (diformat sebagai teks `EMP_*` agar tidak terbaca sebagai fitur numerik), pembuangan tiga kolom konstan (`EmployeeCount`, `StandardHours`, `Over18`), konversi biner `OverTime` menjadi `OverTime_Yes` (0/1), pemilihan 12 fitur kandidat sesuai proposal (dengan menyisihkan label `Attrition`, atribut kategori interpretasi, tiga kolom tarif tak terverifikasi, serta `JobLevel` yang berkorelasi 0,950 dengan `MonthlyIncome`), pemeriksaan nilai hilang (0 nilai diimputasi), lalu pembatasan pencilan (*clipping*) dengan IQR 1,5 pada 440 nilai ekstrem di 5 fitur numerik tanpa menghapus baris observasi. Hasilnya disimpan sebagai `data/data_bersih.csv` (1.470 baris, 12 fitur numerik + 1 kolom identitas teks) dan dipakai ketiga varian apa adanya. Penskalaan tidak dilakukan pada tahap pembersihan, melainkan di dalam tiap varian dan sekali lagi saat model final dilatih: `StandardScaler` di-*fit* satu kali pada data bersih, lalu disimpan sebagai `scaler.pkl` dan dipakai aplikasi web dengan `transform`, bukan `fit_transform`.

**Mengapa urutan kerja berbeda dari cabang klasifikasi?**  
Pada klasifikasi (k-NN, Naive Bayes), urutan wajibnya adalah bersihkan data, pisahkan data latih dan uji, lalu skalakan agar statistik data uji tidak bocor ke pelatihan. Pada klastering tanpa label tidak ada pemisahan latih-uji; pembersihan dilakukan sekali atas seluruh data, dan yang dijaga adalah **kesamaan perlakuan serta skala antara notebook dan aplikasi web**.

### Ringkasan Hasil Eksplorasi & Pembersihan (`01_penyiapan_data.ipynb`)

* **Nilai hilang & duplikat:** 0 sel kosong (0 nilai diimputasi) dan 0 baris duplikat identik (0 pengulangan `EmployeeNumber`).
* **Perbedaan skala antarfitur:** Terjauh antara `MonthlyIncome` (min 1.009, max 19.999, std 4.707,96) dan `OverTime_Yes` (min 0, max 1, std 0,45), dengan rasio nilai maksimum **19.999 kali** (rasio standar deviasi **10.448 kali**) sehingga penskalaan wajib diterapkan saat pemodelan.
* **Korelasi tinggi (di atas 0,90):** Ditemukan 1 pasangan pada data mentah, yaitu `JobLevel` dan `MonthlyIncome` (korelasi 0,950); `JobLevel` telah disisihkan ke bahan interpretasi sehingga tidak ada fitur kembar pada 12 fitur kandidat.
* **Pembatasan pencilan (IQR 1,5 — total 440 nilai dibatasi tanpa menghapus baris):**
  * `MonthlyIncome`: 114 nilai (7,76%) — batas atas 16.581,0
  * `YearsSinceLastPromotion`: 107 nilai (7,28%) — batas atas 7,5
  * `YearsAtCompany`: 104 nilai (7,07%) — batas atas 18,0
  * `TotalWorkingYears`: 63 nilai (4,29%) — batas atas 28,5
  * `NumCompaniesWorked`: 52 nilai (3,54%) — batas atas 8,5
* **Bukti visualisasi EDA:** Tersimpan di `gambar/eda_distribusi.png` dan `gambar/eda_korelasi.png`.

---

## 5. Perbandingan Tiga Kriteria & Pemilihan Model

*(Akan diisi pada Pertemuan 13 / Progres 2 dengan menempelkan `data/tabel_perbandingan.md` dan paragraf kesimpulan dari sel 5.4 `notebook/04_perbandingan.ipynb`.)*

---

## 6. Struktur Repositori

~~~text
Kelompok-4_K-Means_IBM-HR-Employee-Attrition/
├── README.md
├── laporan/
│   └── proposal.pdf                     # Proposal proyek akhir
├── data/
│   ├── raw.csv                          # [P12] Salinan mentah dari Kaggle (tidak pernah diubah)
│   ├── data_bersih.csv                  # [P12] Data bersih bersama (1.470 x 13)
│   ├── kamus_data.csv                   # Dokumentasi arti, satuan, dan peran 33 kolom
│   ├── hasil_varian/                    # [P13] varian1.json, varian2.json, varian3.json
│   ├── keputusan_k.json                 # [P13] Keputusan k final kelompok
│   ├── tabel_perbandingan.md            # [P13] Tabel perbandingan 3 kriteria
│   ├── tabel_perbandingan.csv           # [P13] Versi CSV tabel perbandingan
│   └── pemilihan_fitur.json             # [P13] Keputusan himpunan fitur final
├── gambar/
│   ├── eda_distribusi.png               # [P12] Histogram 12 fitur kandidat
│   ├── eda_korelasi.png                 # [P12] Heatmap korelasi 12 fitur kandidat
│   ├── varian1_elbow.png                # [P13] Kurva Elbow (Anggota 1)
│   ├── varian2_silhouette.png           # [P13] Grafik & diagram Silhouette (Anggota 2)
│   ├── varian3_davies_bouldin.png       # [P13] Grafik Davies-Bouldin & CH (Anggota 3)
│   └── pemilihan_fitur.png              # [P13] Perbandingan fitur & PCA klaster
├── notebook/
│   ├── 01_penyiapan_data.ipynb          # [P12] Notebook penyiapan data & EDA (Colab)
│   ├── 03_varian_1.ipynb                # [P13] Varian 1 — Elbow (Noval)
│   ├── 03_varian_2.ipynb                # [P13] Varian 2 — Silhouette (Faqih)
│   ├── 03_varian_3.ipynb                # [P13] Varian 3 — Davies-Bouldin (Irfan)
│   └── 04_perbandingan.ipynb            # [P13] Perbandingan kriteria & fitur (Kelompok)
├── model/                               # [P14] model.pkl, scaler.pkl, fitur.pkl, segmen.json
├── app.py                               # [P14] Aplikasi web segmentasi karyawan
└── requirements.txt                     # [P14] Daftar pustaka (scikit-learn==1.6.1)
~~~

*(Berkas bertanda `[P12]` telah selesai pada Progres 1; berkas `[P13]` dan `[P14]` ditambahkan pada pertemuan berikutnya.)*

---

## 7. Cara Menjalankan

### Menjalankan Notebook Penyiapan Data (Google Colab)
1. Buka `notebook/01_penyiapan_data.ipynb` di Google Colaboratory, lalu pilih **File > Save a copy in Drive**.
2. Jalankan **Bagian 0 (Persiapan)** untuk menghubungkan Google Drive (`MyDrive/ProyekML_KlasteringKMeans`), memuat pustaka, dan menulis modul bersama `alat.py`.
3. Jalankan **Bagian 1 (Sel 1.1–1.8)** secara berurutan. Dataset diunduh otomatis melalui `kagglehub` (`pavansubhasht/ibm-hr-analytics-attrition-dataset`), dibersihkan, lalu disimpan ke folder Drive bersama sebagai `data/raw.csv`, `data/data_bersih.csv`, `gambar/eda_distribusi.png`, dan `gambar/eda_korelasi.png`.
4. Versi pustaka utama yang dicatat saat eksekusi di Colab: `scikit-learn == 1.6.1`.

### Menjalankan Aplikasi Web
*(Petunjuk instalasi dan perintah menjalankan `app.py` akan ditambahkan pada Pertemuan 14 setelah model final disimpan.)*

---

## 8. Batasan dan Catatan

* Dataset bersifat **sintetis** untuk keperluan pembelajaran; hasil segmentasi bersifat deskriptif dan tidak merepresentasikan kondisi aktual perusahaan tertentu.
* Beberapa atribut kepuasan (`JobSatisfaction`, `EnvironmentSatisfaction`, `WorkLifeBalance`, `JobInvolvement`) berskala ordinal 1–4 yang diperlakukan numerik dalam perhitungan jarak K-Means.
* Mata uang `MonthlyIncome` dan satuan jarak `DistanceFromHome` mengikuti nilai asli dataset tanpa asumsi konversi satuan.

# Proyek Akhir Pembelajaran Mesin — Kelompok 4

**Kelas 5B | Paket 5B-4: IBM HR Employee Attrition | Metode K-Means**

Repository ini berisi proses eksplorasi data (EDA), penyiapan data, dan selanjutnya eksperimen K-Means serta aplikasi web. Dataset **sintetis** bersumber dari [IBM HR Analytics Employee Attrition — Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset).

## Berkas saat ini

- `notebook/01_eksplorasi.ipynb` — notebook Tahap 1 EDA, termasuk pemeriksaan kualitas, visualisasi, pilihan fitur, keputusan praproses, dan penjelasan yang dapat dipelajari.
- `data/data_bersih.csv` — 1.470 baris × 33 kolom hasil pembersihan. **Bukan** matriks fitur yang boleh langsung digunakan sebagai masukan K-Means.
- `data/kamus_data.csv` — arti, satuan, tipe, peran, dan catatan mutu kolom.

## Menjalankan notebook di VS Code

1. Unduh dataset mentah melalui tautan sumber dan simpan **secara lokal** sebagai `data/IBM_HR_Employee_Attrition.csv` (dataset mentah tidak perlu dikomit).
2. Pilih Python environment / Jupyter kernel yang sesuai. Instal paket bila diperlukan: `python -m pip install pandas numpy matplotlib seaborn scikit-learn ipykernel`.
3. Buka `notebook/01_eksplorasi.ipynb`, pilih **Restart Kernel → Run All**. Sel pemuatan mencari file dari folder `notebook/` maupun root repository.
4. Pastikan tidak ada error. Simpan notebook dengan output aktual terbaru dan periksa hasil ekspor di folder `data/`.

## Keputusan EDA

- Unit analisis: satu baris adalah satu karyawan.
- Kolom konstan `EmployeeCount`, `Over18`, dan `StandardHours` dikeluarkan.
- `EmployeeNumber` dipertahankan sebagai identifier untuk penelusuran, **bukan fitur model**.
- `Attrition` dipertahankan untuk bahan penafsiran klaster, **bukan target atau fitur K-Means**.
- Fitur kandidat diperiksa distribusi, pencilan, korelasi, dan skalanya. Penskalaan final dilakukan pada tahap eksperimen.

## Rencana pembagian varian

1. **Varian 1:** Elbow Method untuk K=2–10.
2. **Varian 2:** Silhouette Score untuk validasi pilihan K.
3. **Varian 3:** perbandingan himpunan fitur, profil, nama, dan rekomendasi klaster.

**Sebelum menggunakan hasil untuk laporan:** selalu cek angka dan grafik dari eksekusi ulang notebook. Kesimpulan mengenai data sintetis bersifat deskriptif, bukan penilaian tentang karyawan nyata.

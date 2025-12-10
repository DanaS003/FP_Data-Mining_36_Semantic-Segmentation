# Skenario 3: Fine-Tuning Model dari RescueNet ke Dataset Lokal

## 1. Deskripsi Skenario
**Tujuan:** Mengevaluasi efektivitas teknik *Transfer Learning* (Fine-Tuning).
**Metodologi:** Model yang telah dilatih pada dataset *RescueNet* (Badai Michael) dilatih ulang menggunakan *Dataset Lokal* (Banjir Indonesia).
[cite_start]**Hipotesis:** Transfer pengetahuan dari dataset besar (RescueNet) diharapkan dapat meningkatkan akurasi pada dataset lokal yang memiliki jumlah sampel terbatas [cite: 366-368].

---

## 2. Hasil Eksperimen (Tabel Data)

Berikut adalah rekapitulasi performa model (IoU per kelas dan mIoU rata-rata) pada Skenario 3:

| Model | BG | BF (Building Flooded) | BNF (Building Non-Flooded) | RF (Road Flooded) | RNF (Road Non-Flooded) | Water | Tree | Vehicle | Grass | **mIoU** |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **SegFormer (B1)** | 0.2000 | 0.7435 | 0.5686 | 0.3120 | 0.2445 | 0.1569 | 0.5465 | 0.0669 | 0.6640 | **38.92%** |
| **PSPNet** | 0.0045 | 0.6287 | 0.5053 | 0.2594 | 0.2607 | 0.0013 | 0.4288 | 0.1002 | 0.0000 | **24.32%** |
| **CSDNet** | 0.2266 | 0.8048 | 0.6953 | 0.3176 | 0.2647 | 0.2393 | 0.5716 | 0.0989 | 0.6796 | **43.31%** |
| **Mask2Former** | 0.2936 | **0.8513** | **0.7857** | **0.3896** | 0.1698 | **0.3227** | **0.7109** | **0.1860** | **0.7683** | **49.75%** |

*(Sumber Data: Hasil Eksperimen Skenario 3)*

---

## 3. Analisis Performa Model

### A. Mask2Former (Performa Terbaik)
* **Dominasi Total:** Menjadi model dengan **mIoU tertinggi (49.75%)**, mengungguli model lainnya secara signifikan.
* **Peningkatan Kritis pada Objek Kecil:** Mencatat skor **Vehicle sebesar 18.6%**, angka tertinggi dibandingkan seluruh model di semua skenario (termasuk Skenario 2 yang hanya 12.9%). Ini membuktikan *transfer learning* sangat membantu pengenalan objek kecil.
* **Akurasi Bangunan:** Sangat superior dalam mendeteksi *Building Flooded* (**85.13%**) dan *Building Non-Flooded* (**78.57%**), menunjukkan pemahaman struktur bangunan yang kuat dari pre-training.

### B. CSDNet (Stabilitas & Robustness)
* **Satu-satunya yang Meningkat:** CSDNet adalah satu-satunya model yang mengalami **kenaikan performa** (naik ke **43.31%**) dibandingkan jika dilatih dari nol (Skenario 2: 42.80%).
* **Ketahanan Domain:** Hal ini membuktikan bahwa arsitektur *Context-Aware* pada CSDNet lebih stabil dalam menghadapi *domain shift* (perubahan karakteristik data dari badai ke banjir) dibandingkan model Transformer standar.

### C. SegFormer & PSPNet (Negative Transfer)
* **Penurunan Performa (SegFormer):** Mengalami penurunan drastis menjadi **38.92%** (dibandingkan 46.56% di Skenario 2). Model mengalami kesulitan adaptasi (*negative transfer*) terutama pada kelas *Water* (turun ke 15.69%) dan *Vehicle* (6.69%).
* **Kegagalan PSPNet:** Tetap menjadi model terendah (**24.32%**) dengan kegagalan fatal pada kelas *Background* dan *Water* (mendekati 0%).

---

## 4. Temuan Utama: Fenomena *Negative Transfer* vs *Object Detection*

Skenario 3 mengungkapkan dua fenomena bertolak belakang:

1.  **Penurunan Akurasi Global (Negative Transfer):**
    Secara umum, rata-rata mIoU pada Mask2Former dan SegFormer **lebih rendah** dibandingkan pelatihan *from scratch* (Skenario 2).
    * *Penyebab:* Perbedaan karakteristik visual yang tajam antara *RescueNet* (puing badai, air laut biru/hijau) dengan *Dataset Lokal* (air banjir keruh coklat). Model bingung membedakan *background* air banjir.

2.  **Peningkatan Deteksi Objek Spesifik:**
    Meskipun mIoU total turun, strategi ini sukses menangani kelemahan terbesar dataset lokal, yaitu deteksi kendaraan.
    * Mask2Former berhasil meningkatkan deteksi **Vehicle** secara signifikan (**18.6%**), memberikan solusi parsial untuk masalah deteksi objek mikro yang gagal total di Skenario 2.

---

## 5. Kesimpulan Skenario 3

Penerapan *Transfer Learning* memberikan hasil yang bercampur (*mixed results*). Meskipun **Mask2Former** tetap menjadi arsitektur terbaik, strategi ini memicu *negative transfer* pada kelas lingkungan (air/background). Namun, **CSDNet** terbukti paling *robust* (stabil), dan metode ini terbukti krusial untuk **meningkatkan deteksi objek kecil (kendaraan)** yang tidak bisa dicapai dengan pelatihan biasa.

**Rekomendasi:** Menggunakan Mask2Former dengan strategi *fine-tuning* parsial atau pengaturan *learning rate* yang lebih adaptif untuk menyeimbangkan akurasi lingkungan dan deteksi objek.
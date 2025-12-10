# Skenario 1: Pengujian Baseline pada Dataset RescueNet

## 1. Deskripsi Skenario
**Tujuan:** Menetapkan tolok ukur kinerja (*baseline performance*) untuk seluruh model yang diusulkan.
**Metodologi:** Seluruh model (PSPNet, CSDNet, SegFormer, Mask2Former) dilatih dan diuji menggunakan dataset standar internasional **RescueNet** (Pasca Badai Michael).
[cite_start]**Signifikansi:** Skenario ini bertujuan memvalidasi kemampuan arsitektur dalam menangani citra bencana resolusi tinggi dengan kelas yang kompleks (seperti tingkat kerusakan bangunan) sebelum diuji pada data lokal [cite: 324-327].

---

## 2. Hasil Eksperimen (Tabel Data)

Berikut adalah rekapitulasi performa model berdasarkan nilai *Intersection over Union* (IoU) per kelas dan rata-rata (*Mean IoU*) pada Skenario 1:

| Model | BG | Water | BND | BMND | BMJD | BTD | BR | CR | Vehicle | Tree | Pool | **mIoU** |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **SegFormer (B1)** | 80.13 | 79.38 | 57.81 | 48.00 | 48.39 | 52.53 | 52.62 | 69.59 | 69.59 | 34.57 | 78.21 | **59.21%** |
| **PSPNet** | 76.39 | 73.41 | 48.80 | 32.04 | 45.18 | 47.00 | 25.89 | 61.57 | 61.57 | 12.96 | 75.77 | **48.82%** |
| **CSDNet** | 84.03 | 82.80 | 67.79 | 58.10 | 56.30 | 59.80 | 61.00 | 76.02 | 76.02 | **44.60** | 82.58 | **66.97%** |
| **Mask2Former** | **85.49** | **85.89** | **69.91** | **57.90** | **56.12** | **63.28** | **63.94** | **76.77** | **76.77** | 42.77 | **83.55** | **69.43%** |

*(Keterangan: BND=No Damage, BMND=Minor Damage, BMJD=Major Damage, BTD=Total Destruction, BR=Blocked Road, CR=Clear Road)*
*(Sumber Data: Hasil Eksperimen Skenario 1)*

---

## 3. Analisis Performa Model

### A. Mask2Former (Performa Terbaik)
* **Dominasi Transformer Modern:** Meraih **mIoU tertinggi (69.43%)**. Model ini unggul di hampir semua kelas, terutama pada objek buatan manusia yang memiliki batas tegas seperti **Pool (83.55%)** dan **Vehicle (76.77%)**.
* **Deteksi Kerusakan:** Sangat efektif membedakan tingkat kerusakan bangunan (BND s.d. BTD), dengan skor *Building Total Destruction* (BTD) mencapai **63.28%**, tertinggi di antara semua model.

### B. CSDNet (Kompetitor Kuat Berbasis CNN)
* **Keunggulan Spesifik:** Meskipun secara total berada di posisi kedua (**66.97%**), CSDNet berhasil **mengalahkan Mask2Former** pada kelas tersulit, yaitu **Tree (Vegetasi)** dengan skor **44.60%** (vs 42.77%).
* **Efektivitas Desain:** Hal ini membuktikan bahwa modul *Context-Aware* dan *Detection-Guided* pada CSDNet sangat ampuh menangani tekstur alam yang tidak beraturan, yang sering menjadi kelemahan model segmentasi standar.

### C. PSPNet (Keterbatasan Model Lama)
* **Performa Terendah:** Tertinggal jauh dengan **mIoU 48.82%**.
* **Kegagalan pada Vegetasi:** Kelemahan paling fatal terlihat pada kelas **Tree**, di mana PSPNet hanya mencapai **12.96%**. *Pyramid Pooling Module* gagal menangkap detail vegetasi yang rumit pada resolusi tinggi, menyebabkan banyak kesalahan klasifikasi.

---

## 4. Temuan Utama: Hirarki Kemampuan Model

Skenario 1 menghasilkan pemetaan kemampuan model yang jelas:

1.  **Vision Transformer (Mask2Former) > Specialized CNN (CSDNet) > Generic Transformer (SegFormer) > Generic CNN (PSPNet).**
2.  **Kekuatan Transformer:** Unggul dalam menangkap konteks global dan memisahkan objek diskrit (kendaraan, kolam).
3.  **Kekuatan CNN Khusus (CSDNet):** Lebih tangguh dalam menangani tekstur *noise* atau objek alam (pohon/semak) dibandingkan Transformer.

---

## 5. Kesimpulan Skenario 1

Hasil baseline pada dataset RescueNet menunjukkan bahwa **Mask2Former** adalah model paling *powerful* untuk tugas segmentasi kerusakan umum. Namun, **CSDNet** menunjukkan potensi besar sebagai alternatif yang kompetitif, khususnya dalam menangani elemen lingkungan alami. Sebaliknya, arsitektur lama seperti **PSPNet** terbukti tidak memadai untuk kompleksitas citra UAV pascabencana modern.

**Rekomendasi:** Mask2Former dan CSDNet layak dilanjutkan sebagai kandidat utama untuk pengujian pada dataset lokal (Skenario 2).
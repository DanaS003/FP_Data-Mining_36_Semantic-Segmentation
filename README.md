# Analisis Komparatif Segmentasi Semantik Kerusakan Pasca Bencana  
Menggunakan CNN dan Vision Transformer

---

## 👥 Anggota Kelompok
- Dana Saputra – NRP 5025221003 – GitHub: @abc  
- Muhammad Mirza Ralfie Prasetyo – NRP 5025221220 – GitHub: @def  
- Muh Khairul Amtsal – NRP 5025231172  

Program Studi Teknik Informatika  
Institut Teknologi Sepuluh Nopember (ITS)  
Tahun 2025  

---

## 📌 Deskripsi Proyek
Proyek ini membahas perbandingan performa model segmentasi semantik berbasis CNN dan Vision Transformer untuk mendeteksi kerusakan pasca bencana berbasis citra UAV.

Pengujian dilakukan melalui 3 skenario utama:
1. Pengujian langsung pada dataset RescueNet  
2. Pengujian pada dataset lokal Indonesia  
3. Fine-tuning dari RescueNet ke dataset lokal  

---

## 🤖 Model yang Digunakan

| Model | Tipe | Referensi |
|-------|------|------------|
| PSPNet | CNN | [Paper PSPNet](https://arxiv.org/abs/1612.01105) |
| CSDNet | CNN | [Paper CSDNet](https://www.mdpi.com/2072-4292/17/14/2337) |
| SegFormer (B1) | Vision Transformer | [Paper SegFormer](https://arxiv.org/abs/2105.15203) |
| Mask2Former | Vision Transformer | [Paper Mask2Former](https://arxiv.org/abs/2112.01527) |

---

## 📂 Dataset yang Digunakan

### 1. RescueNet (Dataset Internasional)
Dataset citra UAV pasca bencana Hurricane Michael dengan 11 kelas segmentasi.  
[Dataset RescueNet – Kaggle](https://www.kaggle.com/datasets/yaroslavchyrko/rescuenet)

---

### 2. Dataset Lokal Indonesia (Banjir)
Dataset UAV hasil anotasi manual dari video kejadian banjir di Indonesia.  
[Dataset Lokal Indonesia – Google Drive](https://drive.google.com/drive/folders/1-LblfKT3KLUpA82w1FurelLee0RaiY2Z?usp=drive_link)

Jumlah data: 144 frame  
Pembagian: 116 train, 14 val, 14 test  

---

## 🔄 Skenario Pengujian

### ✅ Skenario 1
Training dan evaluasi langsung di dataset RescueNet  

### ✅ Skenario 2
Training dari nol menggunakan dataset lokal Indonesia  

### ✅ Skenario 3
Fine-tuning model dari RescueNet → Dataset Lokal Indonesia  

<tempel gambar flowchart>

---

# 📊 HASIL EKSPERIMEN

========================
SKENARIO 1 — RescueNet
========================

Model | BG | Water | BND | BMND | BMJD | BTD | BR | Vehicle | Tree | Pool | mIoU  
SegFormer | 80.13 | 79.38 | 57.81 | 48.00 | 48.39 | 52.53 | 52.62 | 69.59 | 34.57 | 78.21 | 59.21  
PSPNet | 76.39 | 73.41 | 48.80 | 32.04 | 45.18 | 47.00 | 25.89 | 61.57 | 12.96 | 75.77 | 48.82  
CSDNet | 84.03 | 82.80 | 67.79 | 58.10 | 56.30 | 59.80 | 61.00 | 76.02 | 44.60 | 82.58 | 66.97  
Mask2Former | 85.49 | 85.89 | 69.91 | 57.90 | 56.12 | 63.28 | 63.94 | 76.77 | 42.77 | 83.55 | 69.43  

BEST MODEL: Mask2Former (69.43%)

---

========================
SKENARIO 2 — Dataset Lokal
========================

Model | BG | BF | BNF | RF | RNF | Water | Tree | Vehicle | Grass | mIoU  
SegFormer | 32.31 | 79.20 | 70.32 | 39.59 | 31.86 | 27.20 | 60.38 | 10.73 | 67.52 | 46.57  
PSPNet | 4.20 | 59.76 | 42.16 | 30.62 | 22.77 | 22.77 | 41.10 | 9.23 | 15.92 | 27.61  
CSDNet | 25.48 | 78.21 | 68.26 | 31.10 | 26.82 | 21.46 | 56.34 | 9.10 | 68.43 | 42.80  
Mask2Former | 38.00 | 81.81 | 70.07 | 48.22 | 36.37 | 46.53 | 64.70 | 12.93 | 75.11 | 52.64  

BEST MODEL: Mask2Former (52.64%)

---

========================
SKENARIO 3 — Fine-Tuning
========================

Model | BG | BF | BNF | RF | RNF | Water | Tree | Vehicle | Grass | mIoU  
SegFormer | 20.00 | 74.35 | 56.86 | 31.20 | 24.45 | 15.69 | 54.65 | 6.69 | 66.40 | 38.92  
PSPNet | 0.45 | 62.87 | 50.53 | 25.94 | 26.07 | 0.13 | 42.88 | 10.02 | 0.00 | 24.32  
CSDNet | 22.66 | 80.48 | 69.53 | 31.76 | 26.47 | 23.93 | 57.16 | 9.89 | 67.96 | 43.32  
Mask2Former | 29.36 | 85.13 | 78.57 | 38.96 | 16.98 | 32.27 | 71.09 | 18.60 | 76.83 | 49.75  

BEST MODEL: Mask2Former (49.75%)

---

<tempel gambar hasil visualisasi skenario 1>

---

## 📦 File Model & Notebook
Notebook training dan file model (.pth) dapat diakses di:

[Model & Notebook – Google Drive](https://drive.google.com/drive/folders/1J3GwxnUpHrXIYvOdqGwkuoLnigQ574wC?usp=sharing)

Setiap skenario dan setiap model memiliki notebook terpisah.

---

## 📄 Lisensi
Proyek ini digunakan untuk keperluan akademik dan penelitian.

# Analisis Komparatif Segmentasi Semantik Kerusakan Pasca Bencana Menggunakan CNN dan Vision Transformer

---

## Anggota Kelompok

| Nama                                | NRP         | GitHub                                      |
|------------------------------------|-------------|----------------------------------------------|
| Dana Saputra                       | 5025221003  | [@DanaS003](https://github.com/DanaS003)     |
| Muhammad Mirza Ralfie Prasetyo     | 5025221220  | [@muhammadmirzaralfie](https://github.com/muhammadmirzaralfie) |
| Muh Khairul Amtsal                 | 5025231172  | [@amtsal99](https://github.com/amtsal99)     |

Program Studi Teknik Informatika  
Institut Teknologi Sepuluh Nopember (ITS)  
Tahun 2025  

---

## Deskripsi Proyek
Proyek ini membahas perbandingan performa model segmentasi semantik berbasis CNN dan Vision Transformer untuk mendeteksi kerusakan pasca bencana berbasis citra UAV.

Pengujian dilakukan melalui 3 skenario utama:
1. Pengujian langsung pada dataset RescueNet  
2. Pengujian pada dataset lokal Indonesia  
3. Fine-tuning dari RescueNet ke dataset lokal  

---

## Model yang Digunakan

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

Jumlah data: 4494 gambar  
Pembagian: 3595 train, 449 val, 450 test

#### Definisi Kelas Rescue Net

| Singkatan | Definisi                     |
|-----------|------------------------------|
| BG        | Background                   |
| Water     | Water                        |
| BND       | Building No Damage           |
| BMND      | Building Minor Damage        |
| BMJD      | Building Major Damage        |
| BTD       | Building Total Destruction   |
| CR        | Road-Clear (Clear Road)      |
| BR        | Road-Blocked (Blocked Road)  |
| Vehicle   | Vehicle                      |
| Tree      | Tree                         |
| Pool      | Pool                         |

---

### 2. Dataset Lokal Indonesia (Banjir)
Dataset UAV hasil anotasi manual dari video kejadian banjir di Indonesia.  
[Dataset Lokal Indonesia – Google Drive](https://drive.google.com/drive/folders/1-LblfKT3KLUpA82w1FurelLee0RaiY2Z?usp=drive_link)

Jumlah data: 144 frame  
Pembagian: 116 train, 14 val, 14 test  

#### Definisi Kelas Data Lokal

| Singkatan | Definisi               |
|-----------|------------------------|
| BG        | Background             |
| BF        | Building Flooded       |
| BNF       | Building Non Flooded   |
| RF        | Road Flooded           |
| RNF       | Road Non Flooded       |
| Water     | Water                  |
| Tree      | Tree                   |
| Vehicle   | Vehicle                |
| Grass     | Grass                  |

---

## Skenario Pengujian

<img width="867" height="244" alt="image" src="https://github.com/user-attachments/assets/940d60d7-de95-47b3-9f20-d26ac5ee885b" />

### 1. Skenario 1
Training dan evaluasi langsung di dataset RescueNet  

### 2. Skenario 2
Training dari nol menggunakan dataset lokal Indonesia  

### 3. Skenario 3
Fine-tuning model dari RescueNet → Dataset Lokal Indonesia  

---

# HASIL EKSPERIMEN

SKENARIO 1 (RescueNet)
========================

<img width="7169" height="4405" alt="visualisasi_skenario_1" src="https://github.com/user-attachments/assets/20cdad44-584e-40ba-a5a0-6fb1262f39b6" />

<br>

### Tabel Hasil Skenario 1

| Model          |    BG | Water |   BND |  BMND |  BMJD |   BTD |    BR |    CR | Vehicle |  Tree |  Pool |  mIoU |
|----------------|------:|------:|------:|------:|------:|------:|------:|------:|--------:|------:|------:|------:|
| SegFormer (B1) | 80.13 | 79.38 | 57.81 | 48.00 | 48.39 | 52.53 | 69.59 | 52.62 |   34.57 | 78.21 | 50.13 | 59.21 |
| PSPNet         | 76.39 | 73.41 | 48.80 | 32.04 | 45.18 | 47.00 | 61.57 | 25.89 |   12.96 | 75.77 | 37.97 | 48.82 |
| CSDNet         | 84.03 | 82.80 | 67.79 | 58.10 | 56.30 | 59.80 | 76.02 | 61.00 |   44.60 | 82.58 | 63.70 | 66.97 |
| Mask2Former    | 85.49 | 85.89 | 69.91 | 57.90 | 56.12 | 63.28 | 76.77 | 63.94 |   42.77 | 83.55 | 78.15 | 69.43 |

**BEST MODEL:** Mask2Former (69.43%)

---

SKENARIO 2 (Dataset Lokal)
========================

<img width="7169" height="2915" alt="visualisasi_skenario_2" src="https://github.com/user-attachments/assets/adaaec94-9dbd-43f9-a465-17685b1265ef" />

<br>

### Tabel Hasil Skenario 2

| Model       |    BG |    BF |   BNF |    RF |   RNF | Water |  Tree | Vehicle | Grass |  mIoU |
|-------------|------:|------:|------:|------:|------:|------:|------:|--------:|------:|------:|
| SegFormer   | 32.31 | 79.20 | 70.32 | 39.59 | 31.86 | 27.20 | 60.38 |   10.73 | 67.52 | 46.57 |
| PSPNet      |  4.20 | 59.76 | 42.16 | 30.62 | 22.77 | 22.77 | 41.10 |    9.23 | 15.92 | 27.61 |
| CSDNet      | 25.48 | 78.21 | 68.26 | 31.10 | 26.82 | 21.46 | 56.34 |    9.10 | 68.43 | 42.80 |
| Mask2Former | 38.00 | 81.81 | 70.07 | 48.22 | 36.37 | 46.53 | 64.70 |   12.93 | 75.11 | 52.64 |
  

BEST MODEL: Mask2Former (52.64%)

---

SKENARIO 3 (Fine-Tuning)
========================

<img width="7169" height="2915" alt="visualisasi_skenario_3" src="https://github.com/user-attachments/assets/ca58cfd0-e46d-4220-a880-78a98c2fe862" />

<br>

### Tabel Hasil Skenario 3

| Model       |    BG |    BF |   BNF |    RF |   RNF | Water |  Tree | Vehicle | Grass |  mIoU |
|-------------|------:|------:|------:|------:|------:|------:|------:|--------:|------:|------:|
| SegFormer   | 20.00 | 74.35 | 56.86 | 31.20 | 24.45 | 15.69 | 54.65 |    6.69 | 66.40 | 38.92 |
| PSPNet      |  0.45 | 62.87 | 50.53 | 25.94 | 26.07 |  0.13 | 42.88 |   10.02 |  0.00 | 24.32 |
| CSDNet      | 22.66 | 80.48 | 69.53 | 31.76 | 26.47 | 23.93 | 57.16 |    9.89 | 67.96 | 43.32 |
| Mask2Former | 29.36 | 85.13 | 78.57 | 38.96 | 16.98 | 32.27 | 71.09 |   18.60 | 76.83 | 49.75 |

BEST MODEL: Mask2Former (49.75%)

---

---

## File Model & Notebook
Notebook training dan file model (.pth) dapat diakses di:

[Model & Notebook – Google Drive](https://drive.google.com/drive/folders/1J3GwxnUpHrXIYvOdqGwkuoLnigQ574wC?usp=sharing)

Setiap skenario dan setiap model memiliki notebook terpisah.

---

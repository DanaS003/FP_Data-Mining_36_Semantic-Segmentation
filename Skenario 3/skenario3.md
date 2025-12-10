***

# Laporan Eksperimen Segmentasi Semantik Banjir

## 1. Deskripsi Dataset
Eksperimen ini menggunakan dataset lokal yang dikurasi khusus untuk studi kasus banjir di Indonesia.
* **Jumlah Data:** 144 citra (images).
* **Konteks:** Foto udara dan *street view* dari kejadian banjir nyata di Indonesia.
* **Anotasi:** Dilakukan secara manual (manual annotation) untuk memisahkan objek-objek penting dalam konteks bencana.

Berikut adalah visualisasi distribusi kelas dalam dataset beserta contoh citra yang digunakan:
![Distribusi Kelas](./dist_per_kelas.png)

## 2. Experimental Setup
Sebelumnya, model-model yang digunakan dalam skenario ini sudah dilatih pada dataset RescueNet pada skenario 1 yang mana _weights_ yang digunakan pada skenario 3 ini adalah _pretrained weights_ dari skenario 1. Seluruh model dilatih dengan konfigurasi parameter umum sebagai berikut untuk memastikan perbandingan yang adil:

* **Image Input Size:** $512 \times 512$ pixel.
* **Batch Size:** 16.
    * SegFormer, PSPNet: 16.
    * CSDNet: 4.
    * Mask2Former: 2.
* **Epochs:** 25.
* **Optimizer:**
    * *PSPNet & SegFormer:* AdamW.
    * *Mask2Former:* AdamW (Learning Rate `1e-5`).
    * *CSDNet:* SGD (Momentum `0.9`).
* **Loss Function:** Menggunakan kombinasi Loss (seperti CrossEntropy + Dice Loss atau Focal Loss + Jaccard Loss) untuk menangani ketidakseimbangan kelas.

## 3. Model yang Diuji
Kami menguji empat arsitektur model berbeda untuk melihat efektivitasnya pada dataset kecil ini:
1.  **PSPNet:** Menggunakan *Pyramid Pooling Module* dengan backbone EfficientNet-B3.
2.  **SegFormer:** Model berbasis Transformer (*mit_b1*) yang ringan.
3.  **Mask2Former:** Arsitektur *Masked-attention Mask Transformer* dengan backbone Swin-Large (SOTA).
4.  **CSDNet:** Arsitektur kustom (*Context-Sensitive De-raining/Detection*) dengan modul *Targeted Enhancement* dan *Multi-Scale Fusion*.

## 4. Hasil Performa
Berikut adalah tabel perbandingan performa pada skenario 3 ini.

| Model Name | Background | Bld. Flooded | Bld. Non-Flood | Road Flooded | Road Non-Flood | Water | Tree | Vehicle | Grass | **mIoU** |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **SegFormer (B1)** | 0.2000 | 0.7435 | 0.5686 | 0.3120 | 0.2445 | 0.1569 | 0.5465 | 0.0669 | 0.6640 | **0.3892** |
| **PSPNet** | 0.0045 | 0.6287 | 0.5053 | 0.2594 | 0.2607 | 0.0013 | 0.4288 | 0.1002 | 0.0000 | **0.2432** |
| **CSDNet** | 0.2266 | 0.8048 | 0.6953 | 0.3176 | 0.2647 | 0.2393 | 0.5716 | 0.0989 | 0.6796 | **0.4332** |
| **Mask2Former** | 0.2936 | 0.8513 | 0.7857 | 0.3896 | 0.1698 | 0.3227 | 0.7109 | 0.1860 | 0.7683 | **0.4975** |

Untuk memberikan gambaran kualitatif, berikut adalah visualisasi hasil prediksi dari model-model tersebut pada skenario ini:
![Hasil Prediksi Skenario 3](./visualisasi_skenario_3.png)
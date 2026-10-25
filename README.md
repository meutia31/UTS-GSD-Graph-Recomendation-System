# UTS-GSD-Graph-Recomendation-System
Proyek ini dibuat untuk memenuhi tugas **UTS Mata Kuliah Graf Sains Data (GSD)**. Proyek ini mengimplementasikan sistem rekomendasi produk *e-commerce* menggunakan pendekatan **Item-based Collaborative Filtering** berbasis **Vector Space Modeling (VSM)** dan **Graph Database Neo4j**.

---
Sistem rekomendasi ini memanfaatkan struktur data graf *bipartite* untuk memodelkan interaksi pengguna (`Visitor`) dan produk (`Item`). Selanjutnya, matriks sparse dihitung menggunakan *Cosine Similarity* untuk membangun proyeksi jaringan homogen produk (`:SIMILAR`), yang kemudian diproses menggunakan algoritma K-Nearest Neighbors (KNN) untuk memprediksi rekomendasi barang pada periode uji (*testing*).

### ALur Kerja
1. **Pembersihan & Partisi Data Temporal**:
   - **Periode P1 (Training)**: 3 Mei 2015 – 17 Mei 2015 (Pembentukan Graf & VSM)
   - **Periode P2 (Testing)**: 1 Agustus 2015 – 15 Agustus 2015 (Evaluasi Rekomendasi)
2. **Pemodelan Graf di Neo4j**:
   - Simpul: `Visitor` dan `Item`
   - Sisi: Relasi `VIEWED`, `ADDED_TO_CART`, dan `BOUGHT`
3. **Vector Space Modeling (VSM)**:
   - Penggunaan `scipy.sparse.csr_matrix` untuk mengatasi kerapatan data yang sangat tinggi (*high sparsity*).
   - Perhitungan *Cosine Similarity* antar-item pada Top 100 produk terpopuler.
4. **Proyeksi Jaringan Homogen Item**:
   - Penulisan relasi horizontal `:SIMILAR` beserta bobot kemiripannya (`score`) kembali ke database Neo4j.
5. **Evaluasi Performa Sistem Rekomendasi**:
   - Mengukur efektivitas rekomendasi Top-5 (k=5) menggunakan metrik **Hit@5** dan **NDCG@5**.

---

## Hasil Analisis & Komposisi Data
| Parameter / Metrik | Subgraph P1 (Training) | Subgraph P2 (Testing) |
| :--- | :--- | :--- |
| **Simpul Visitor (User)** | 155,916 | 153,836 |
| **Simpul Item (Produk)** | 78,171 | 76,078 |
| **Total Interaksi (Edge)** | 296,319 | 265,509 |
| **Dimensi Matriks VSM Item** | 78,171 \times 155,916 | — |
| **Tingkat Kerapatan (Density)** | approx 0.0019% (*Highly Sparse*) | — |

> **Kesimpulan Proyeksi**: Pendekatan **VSM Item** terbukti jauh lebih efisien dan stabil dibandingkan VSM User karena katalog produk lebih terstruktur, tidak mudah melonjak drastis secara kuantitas, serta mengatasi masalah *user cold-start* pada platform *e-commerce*.

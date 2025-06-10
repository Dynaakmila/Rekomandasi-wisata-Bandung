# Rekomandasi-wisata-Bandung
# Laporan Proyek Machine Learning - Dyna Akmila
---
# Project Overview
Pariwisata merupakan sektor penting yang mendorong pertumbuhan ekonomi daerah, termasuk Kota Bandung yang dikenal luas sebagai salah satu destinasi wisata favorit di Indonesia. Kota ini menawarkan berbagai jenis objek wisata seperti wisata alam, kuliner, edukasi, hingga budaya, yang menarik bagi wisatawan domestik maupun mancanegara.

Namun, banyaknya pilihan destinasi justru sering membuat wisatawan mengalami kesulitan dalam memilih tempat yang sesuai dengan preferensi, waktu, dan anggaran mereka. Masalah ini diperparah dengan kurangnya media yang menyajikan informasi rekomendasi wisata secara terstruktur, terpersonalisasi, dan mudah diakses.

Proyek ini bertujuan untuk menyusun rekomendasi wisata Bandung yang informatif, terkurasi, dan relevan, guna membantu wisatawan merencanakan perjalanan dengan lebih efisien dan menyenangkan. Rekomendasi disusun berdasarkan kategori wisata (alam, kuliner, budaya, dan edukasi), tingkat popularitas, ulasan pengunjung, serta kemudahan akses lokasi. Solusi ini diharapkan dapat menjadi panduan praktis bagi wisatawan dalam menentukan tujuan wisata mereka.

Berdasarkan data dari Badan Pusat Statistik (BPS) Provinsi Jawa Barat, jumlah wisatawan yang berkunjung ke Bandung pada tahun 2023 tercatat mencapai lebih dari 6 juta orang, yang sebagian besar merupakan wisatawan domestik [1]. Angka ini menunjukkan bahwa Bandung memiliki potensi besar di sektor pariwisata, dan kebutuhan akan informasi wisata yang akurat serta mudah diakses menjadi semakin penting untuk menunjang pengalaman wisata yang optimal.

---
# Bussines Understanding
Problem Statements
- Berdasarkan data mengenai pengguna, bagaimana membuat suatu sistem rekomendasi wisata yang dipersonalisasi dengan teknik Content-Based Filtering?
- Dengan dataset rating yang tersedia, bagaimana cara mendapatkan rekomendasi wisata yang mungkin disukai pengguna serta belum pernah dikunjungi oleh pengguna tersebut?

Goals
- Menghasilkan sejumlah rekomendasi wisata yang dipersonalisasi untuk setiap pengguna berdasarkan data preferensi individu.
- Menghasilkan sejumlah rekomendasi wisata baru berdasarkan kesamaan preferensi, bahkan jika pengguna belum pernah mengunjungi tempat tersebut sebelumnya.

Solution statements
- Agar menghasilkan rekomendasi wisata yang dipersonalisasi untuk pengguna dengan menggunakan teknik content-based filtering.
- Teknik kedua terdapat colaborative filtering agar menghasilkan rekomendasi wisata berdasarkan preferensi pengguna serta pengguna belum pernah mengunjungi wisata tersebut.
---
# Data understanding
Proyek sistem rekomendasi wisata ini menggunakan dataset yang tersedia secara publik melalui Kaggle – https://www.kaggle.com/datasets/athreal/destinasi-wisata-dataset. Dataset ini terdiri dari informasi terkait destinasi wisata dari lima kota besar di Indonesia, termasuk Bandung, serta data pengguna dan rating wisata. Dataset ini dipilih karena menyediakan informasi yang cukup lengkap untuk membangun sistem rekomendasi berbasis Content-Based Filtering maupun Collaborative Filtering.
Jumlah data yang digunakan dalam proyek ini meliputi:

![image](https://github.com/user-attachments/assets/97409025-e3e6-4e08-a3d7-b1900fa0425f)

Variabel-variabel pada Dataset yang digunakan adalah sebagai berikut:
package_tourism : merupakan file yang berisi paket untuk liburan.
tourism_with_id : merupakan file yang berisi tentang berbagai macam wisata dari lima kota terbesar yaitu Bandung, Jakarta, Yogyakarta, Semarang dan Surabaya.
user : merupakan file yang berisi tentang pengguna.
tourism_rating : merupakan file yang berisi tentang kumpulan rating dari berbagai macam wisata yang terdapat di file tourism_with_id.

Berdasarkan hasil eksplorasi awal menggunakan .info() dan .isna().sum(), terdapat beberapa nilai kosong (missing values) yang teridentifikasi:

![image](https://github.com/user-attachments/assets/9568c2ce-1388-4ebd-8d69-4869b39571a5)

Time_Minutes: Memiliki nilai kosong sebanyak 265 data
Untuk baris Unnamed: 11 dan Unnamed: 12: Seluruh kolom ini berisi nilai kosong (NaN) sebanyak 437 data
Karena kedua kolom tersebut sepenuhnya kosong dan tidak memberikan kontribusi informasi, maka akan dihapus pada tahap data preparation.

Pada file tourism_with_id terdapat 437 data dari macam-macam wisata yang tersebar di lima kota besar yang sudah dijelaskan, berikut merupakan visualisasi sebaran data tersebut dari masing-masing kota:

![image](https://github.com/user-attachments/assets/46729e29-5d5a-46df-ab3b-bdfbeede35de)

Pada file tersebut juga terdapat fiel kategory, berikut merupakan visualisasi sebaran data berdasarkan kategory dan data tersebut sudah hanya data wisata yang berada di kota Bandung:

![image](https://github.com/user-attachments/assets/8595e532-23bf-47a5-96b3-6f88f73a460f)

---

# Data Preparation
Tahap selanjutnya yaitu data preparation atau persiapan data. Berikut merupakan tahap-tahap yang dilakukan pada bagian ini:

Menampilkan Hanya Data Wisata di Bandung
Seperti yang diketahui, pada dataset tourism_with_id.csv terdapat data wisata dari lima kota besar. Karena tujuan dari proyek ini adalah untuk membuat sistem rekomendasi wisata yang fokus pada kota Bandung, maka langkah pertama dalam data preparation adalah memfilter data agar hanya menyisakan data wisata yang berada di Bandung.

Langkah ini penting agar sistem rekomendasi benar-benar sesuai dengan cakupan wilayah yang ditentukan, yaitu kota Bandung. Data yang telah difilter kemudian disimpan ke dalam variabel terpisah agar dapat digunakan secara khusus dalam proses modeling dan evaluasi berikutnya.

Berikut merupakan tabel data wisata yang berada di kota Bandung:
| Place_Id | Place_Name                      | Description                                                               | Category      | City    | Price   | Rating | Time_Minutes | Coordinate                                 | Lat        | Long        |
|----------|----------------------------------|---------------------------------------------------------------------------|---------------|---------|---------|--------|---------------|---------------------------------------------|------------|-------------|
| 211      | Gunung Tangkuban Perahu          | Gunung Tangkuban Parahu adalah salah satu gunung...                      | Cagar Alam    | Bandung | 30000   | 4.5    | NaN           | {'lat': -6.7596377, 'lng': 107.6097807}     | -6.759638  | 107.609781  |
| 212      | Jalan Braga                      | Jalan Braga adalah nama sebuah jalan utama di...                         | Budaya        | Bandung | 0       | 4.7    | NaN           | {'lat': -6.9150534, 'lng': 107.6089842}     | -6.915053  | 107.608984  |
| 213      | Gedung Sate                      | Gedung Sate, dengan ciri khasnya berupa orname...                        | Budaya        | Bandung | 5000    | 4.6    | NaN           | {'lat': -6.9024812, 'lng': 107.61881}       | -6.902481  | 107.618810  |
| 214      | Trans Studio Bandung             | Trans Studio Bandung adalah kawasan wisata ter...                        | Taman Hiburan | Bandung | 280000  | 4.5    | 240.0         | {'lat': -6.9250943, 'lng': 107.6364944}     | -6.925094  | 107.636494  |
| 215      | Taman Hutan Raya Ir. H. Djuanda  | Taman Hutan Raya Ir. H. Djuanda (Tahura Djuanda)...                      | Cagar Alam    | Bandung | 15000   | 4.5    | 90.0          | {'lat': -6.8565791, 'lng': 107.6323734}     | -6.856579  | 107.632373  |
| 330      | Bandros City Tour                | Bandros atau Bus Wisata Bandung adalah bus wis...                        | Budaya        | Bandung | 40000   | 4.4    | NaN           | {'lat': -6.9220816, 'lng': 107.6073182}     | -6.922082  | 107.607318  |
| 331      | Kyotoku Floating Market          | Kyotoku Floating Market Bandung, sebuah wisata...                        | Budaya        | Bandung | 175000  | 4.5    | 150.0         | {'lat': -6.819875, 'lng': 107.618707}       | -6.819875  | 107.618707  |
| 332      | Rainbow Garden                   | Rainbow Garden Harapan Indah salah satu taman...                         | Cagar Alam    | Bandung | 20000   | 4.6    | 90.0          | {'lat': -6.8179514, 'lng': 107.618914}      | -6.817951  | 107.618914  |
| 333      | Kota Mini                        | Destinasi yang sangat menarik bernuansa eropa...                         | Taman Hiburan | Bandung | 20000   | 4.4    | NaN           | {'lat': -6.8186888, 'lng': 107.6169403}     | -6.818689  | 107.616940  |
| 334      | Chingu Cafe Little Seoul         | Selain populer karena memiliki pemandangan yan...                        | Taman Hiburan | Bandung | 50000   | 4.5    | NaN           | {'lat': -6.9012241, 'lng': 107.6099853}     | -6.901224  | 107.609980  |

Menggabungkan Data
Setelah data difilter agar hanya berisi wisata yang berlokasi di Bandung, langkah selanjutnya adalah menggabungkan data rating dengan nama wisata dan kategori wisata. Tujuan penggabungan ini adalah untuk melihat nilai rating dari masing-masing tempat wisata di Bandung serta mengetahui kategori wisata tersebut.

Data hasil penggabungan inilah yang nantinya akan digunakan sebagai dasar dalam sistem rekomendasi wisata. Berikut merupakan tabel data setelah digabung:
| Index | User_Id | Place_Id | Place_Ratings | Place_Name               | Category       |
|-------|---------|----------|----------------|---------------------------|----------------|
| 12    | 135     | 211      | 1              | GunungTangkuban perahu    | Cagar Alam     |
| 13    | 160     | 211      | 5              | GunungTangkuban perahu    | Cagar Alam     |
| 10    | 124     | 211      | 2              | GunungTangkuban perahu    | Cagar Alam     |
| 11    | 129     | 211      | 4              | GunungTangkuban perahu    | Cagar Alam     |
| 16    | 220     | 211      | 4              | GunungTangkuban perahu    | Cagar Alam     |
| ...   | ...     | ...      | ...            | ...                       | ...            |
| 2834  | 186     | 334      | 4              | Chingu Cafe Little Seoul  | Taman Hiburan  |
| 2833  | 185     | 334      | 4              | Chingu Cafe Little Seoul  | Taman Hiburan  |
| 2831  | 171     | 334      | 3              | Chingu Cafe Little Seoul  | Taman Hiburan  |
| 2832  | 177     | 334      | 3              | Chingu Cafe Little Seoul  | Taman Hiburan  |
| 2841  | 293     | 334      | 2              | Chingu Cafe Little Seoul  | Taman Hiburan  |

Mencari Data Missing Value
Setelah dataset digabung, maka sebelum digunakan untuk proses pemodelan, langkah selanjutnya adalah memastikan bahwa tidak ada data yang hilang atau kosong (missing value). Dalam proyek ini, pencarian data yang kosong dilakukan menggunakan fungsi isnull() untuk mendeteksi apakah terdapat nilai yang kosong, serta sum() untuk menghitung jumlah total dari nilai kosong tersebut di setiap kolom.

Pada dataset gabungan yang hanya memuat data wisata di Bandung, ternyata tidak ditemukan adanya missing value. Artinya, seluruh kolom pada dataset telah terisi dengan baik tanpa ada nilai yang kosong.

Berikut merupakan hasil pengecekan yang menunjukkan bahwa tidak terdapat data missing value pada dataset wisata Bandung yang telah digabung:
|                  | Missing Values |
|------------------|----------------|
| User_Id          | 0              |
| Place_Id         | 0              |
| Place_Ratings    | 0              |
| Place_Name       | 0              |
| Category         | 0              |

Langkah selanjutnya setelah dipastikan dataset bersih atau tidak terdapat missing value yaitu langkah menghapus data yang duplikat. Tujuan dari tahap ini yaitu agar dataset yang akan digunakan untuk pemodelan hanya berupa data yang uni saja. Sebelum melakukan penghapusan data yang duplikat, jumlah dataset tersebut terdapat 2842 data, dan setelah dilakukan penghapusan terdapat hanya 124 data. berikut merupakan Tabel data setalah melakukan tahapan penghapusan data yang duplikat:
| Index | User_Id | Place_Id | Place_Ratings | Place_Name                      | Category       |
|-------|---------|----------|----------------|----------------------------------|----------------|
| 0     | 9       | 211      | 3              | GunungTangkuban perahu           | Cagar Alam     |
| 28    | 19      | 212      | 3              | Jalan Braga                      | Budaya         |
| 44    | 7       | 213      | 3              | Gedung Sate                      | Budaya         |
| 68    | 13      | 214      | 5              | Trans Studio Bandung             | Taman Hiburan  |
| 86    | 28      | 215      | 5              | Taman Hutan Raya Ir. H. Djuanda  | Cagar Alam     |
| ...   | ...     | ...      | ...            | ...                              | ...            |
| 2725  | 17      | 330      | 3              | Bandros City Tour                | Budaya         |
| 2745  | 9       | 331      | 4              | Kyotoku Floating Market          | Budaya         |
| 2777  | 18      | 332      | 2              | Rainbow Garden                   | Cagar Alam     |
| 2800  | 19      | 333      | 4              | Kota Mini                        | Taman Hiburan  |
| 2824  | 6       | 334      | 2              | Chingu Cafe Little Seoul         | Taman Hiburan  |

Membagi Dataset
Langkah ini hanya digunakan untuk pendekatan colaborative filtering karena pendekatan tersebut menggunakan model untuk dilatih. Pembagian dataset untuk proyek ini yaitu membagi data trai dan validasi dengan komposisi 80:20.

---

# Modeling
Setelah melalui berbagai tahapan seperti Business Understanding, Data Understanding, dan Data Preparation, tahap selanjutnya dalam proyek ini adalah melakukan pemodelan. Pada proyek sistem rekomendasi wisata ini, digunakan dua pendekatan utama, yaitu content-based filtering dan collaborative filtering. Berikut penjelasan dari masing-masing pendekatan:

Content-Based Filtering
Content-Based Filtering merupakan pendekatan dalam sistem rekomendasi yang bekerja dengan mencocokkan item yang disukai oleh pengguna dengan item lain yang memiliki kemiripan berdasarkan atribut-atributnya.
Sebelum mengembangkan sistem rekomendasi dengan pendekatan ini, langkah awal yang dilakukan adalah mengidentifikasi representasi fitur penting dari setiap kategori wisata yang ada di Bandung. Untuk menemukan fitur-fitur penting tersebut, proyek ini menggunakan metode CountVectorizer dari library scikit-learn, kemudian dilakukan proses fit dan transform terhadap data sehingga diperoleh bentuk matriks. Berikut ini adalah hasil matriks dari proses CountVectorizer:

![image](https://github.com/user-attachments/assets/64e55488-3ec0-490b-a52f-6a833732e256)

Selanjutnya memvisualisasikan data wisata dan kategori berdasarkan matriks diatas :
| nama_wisata                    | tempat | perbelanjaan | hiburan | pusat | cagar | ibadah |
|-------------------------------|--------|--------------|---------|-------|--------|--------|
| Masjid Daarut Tauhiid Bandung| 1      | 0            | 0       | 1     | 0      | 1      |
| Mountain View Golf Club      | 0      | 0            | 0       | 0     | 1      | 0      |
| Jalan Braga                  | 0      | 0            | 0       | 0     | 0      | 0      |
| Sanghyang Heuleut            | 0      | 0            | 0       | 0     | 1      | 0      |

## Data Wisata dan Kategori Berdasarkan Matriks
Tabel di atas menunjukkan bahwa Masjid Daarut Tauhiid Bandung termasuk dalam kategori wisata ibadah, yang ditunjukkan oleh nilai 1 pada kolom ibadah. Setelah fitur penting dari setiap wisata berhasil direpresentasikan dalam bentuk matriks (seperti menggunakan CountVectorizer), langkah selanjutnya adalah mengidentifikasi korelasi antara wisata dan kategorinya.

Untuk mengukur tingkat kemiripan antar wisata berdasarkan kategorinya, digunakan teknik cosine similarity dari library scikit-learn. Teknik ini berguna untuk mengetahui sejauh mana dua tempat wisata memiliki kesamaan berdasarkan fitur kategori yang dimilikinya. Setelah itu, hasil dari perhitungan cosine similarity ini dapat divisualisasikan dalam bentuk matriks kesamaan antar wisata. Visualisasi ini membantu dalam mengembangkan sistem rekomendasi yang lebih akurat, karena wisata yang memiliki tingkat kemiripan tinggi dapat direkomendasikan kepada pengguna yang menyukai salah satu dari tempat tersebut.

![image](https://github.com/user-attachments/assets/5caf5f2f-ab0d-4ba3-9c0b-39316d434766)

## Matriks Kesamaan Setiap Wisata
Pada gambar di atas, nilai matriks 1.0 menunjukkan adanya kesamaan wisata antara sumbu x dan sumbu y, dalam hal ini Masjid Daarut Tauhiid Bandung memiliki nilai kesamaan tertinggi dengan dirinya sendiri. Ini mengindikasikan bahwa wisata tersebut berada dalam kategori yang sama atau memiliki fitur yang sangat mirip berdasarkan hasil ekstraksi fitur sebelumnya.

Langkah terakhir dari pendekatan content-based filtering adalah membuat sistem rekomendasi yang dapat menghasilkan Top-N Recommendation — yaitu daftar wisata yang paling relevan untuk direkomendasikan berdasarkan tempat yang disukai pengguna. Untuk menguji apakah fungsi ini berjalan dengan akurat, dilakukan penghapusan sementara terhadap data wisata Masjid Daarut Tauhiid Bandung dari daftar dan dilakukan pencarian rekomendasi wisata terdekat berdasarkan kemiripan fitur.

Hasil dari pendekatan content-based filtering menunjukkan bahwa sistem mampu memberikan rekomendasi wisata lain yang juga termasuk dalam kategori serupa, seperti wisata religi atau tempat ibadah lainnya di Bandung. Dengan demikian, sistem rekomendasi berbasis konten ini dapat menjadi acuan awal yang efektif dalam membantu pengguna menemukan tempat wisata sesuai preferensinya.
| nama\_wisata                      | kategori\_wisata |
| --------------------------------- | ---------------- |
| Masjid Pusdai                     | Tempat Ibadah    |
| Masjid Agung Trans Studio Bandung | Tempat Ibadah    |
| Masjid Raya Bandung               | Tempat Ibadah    |
| Gereja Tiberias Indonesia Bandung | Tempat Ibadah    |
| Masjid Salman ITB                 | Tempat Ibadah    |

## Hasil Rekomendasi dengan Pendekatan Content-Based Filtering
Pada pengujian kali ini, digunakan data Masjid Daarut Tauhiid Bandung yang memiliki kategori Tempat Ibadah, maka sistem rekomendasi seharusnya menyarankan tempat-tempat wisata lain yang memiliki kemiripan kategori, yaitu sama-sama berkategori Tempat Ibadah.

Hasilnya pun sesuai harapan, di mana sistem memberikan Top-N rekomendasi wisata yang semuanya berada dalam kategori Tempat Ibadah, seperti Masjid Raya Bandung, Masjid Pusdai, Masjid Agung Trans Studio Bandung, dan lainnya. Ini membuktikan bahwa sistem berhasil mengidentifikasi dan merekomendasikan tempat dengan kategori yang mirip atau sama.

# Colaborative Filtering
Selain pendekatan content-based filtering, pada proyek ini juga menggunakan pendekatan colaborative filtering dengan tujuan menghasilkan rekomendasi wisata berdasarkan preferensi pengguna dan rating yang telah diberikan sebelumnya. Dari rating tersebut akan digunakan untuk merekomendasikan wisata yang mirip dan belum pernah dikunjung oleh pengguna. Setelah pada tahap Data Preparation pada proses pembagian dataset menjadi data training dan data validasi maka langkah selanjutnya yaitu melakukan mapping data user dan wisata menjadi satu value terlebih dahulu. Berikut merupakan hasil mapping dari data user dan wisata bisa dilihat di notebook.

## Hasil Mapping
mapping tersebut digunakan untuk menyatukan dari berbagai data serta menjembatani perbedaan dari data tersebut. Setelah melakukan mapping maka selanjutnya yaitu membuat kelas RecommenderNet yang digunakan untuk menghitung nilai kecocokan antara user dan wisata. skor kecocokan ditetapkan dalam skala 0 hingga 1 dengan fungsi aktivasi sigmoid. Setelah membuat class tersebut maka model akan dicompile terlebih dahulu sebelum melakukan proses training. Model ini menggunakan Binarry Crossentropy untuk menghitung los function, optimizer menggunakan Adam (Adaptive Moment Estimation), dan RMSE sebagai metriks evaluation, setelah melakukan compile pada model maka model akan ditraining atau dilataih, berikut merupakan hasil erorr setelah model dilatih:

![image](https://github.com/user-attachments/assets/a376d103-4840-4b99-9393-a16dce89fc11)

## Plot Error Model
Langkah terakhir yaitu mengetest model apakah memiliki prediksi rekomendasi yang akurat. Dengan membat variable wisata not visited dengan tujuan atau goal yang sudah dijelaskan agar rekomendasi pengguna yaitu wisata yang belum pernah dikunjungi tetapi mempunyai kemiripan. berikut merupakan output dari pendekatan colaborative filtering :
| No | Nama Destinasi                          | Kategori         |
|----|------------------------------------------|------------------|
| 1  | Selasar Sunaryo Art Space               | Taman Hiburan    |
| 2  | Teras Cikapundung BBWS                  | Taman Hiburan    |
| 3  | Museum Pos Indonesia                    | Budaya           |
| 4  | Masjid Agung Trans Studio Bandung       | Tempat Ibadah    |
| 5  | Museum Nike Ardilla                     | Budaya           |
| 6  | Situ Patenggang                         | Cagar Alam       |
| 7  | Glamping Lakeside Rancabali             | Taman Hiburan    |
| 8  | Bukit Jamur                             | Cagar Alam       |
| 9  | Rainbow Garden                          | Cagar Alam       |
| 10 | Kota Mini                               | Taman Hiburan    |
---
# Evaluation
Proyek machine learning **Sistem Rekomendasi Wisata Bandung** ini menggunakan matriks evaluasi **RMSE** atau singkatan dari *Root Mean Squared Error*. RMSE merupakan metode pengukuran perbedaan antara nilai prediksi dari sebuah model dengan nilai aktual yang diamati. RMSE adalah hasil dari akar kuadrat terhadap **MSE** (*Mean Squared Error*).

Untuk mengetahui tingkat keakuratan dari model, dapat dilihat dari nilai RMSE—semakin kecil nilai RMSE, maka model semakin akurat. Sebaliknya, semakin besar nilai RMSE, maka model tersebut semakin jauh dari kata akurat.

Perhitungan RMSE dilakukan dengan cara:

1. Mengurangkan nilai aktual dan nilai hasil prediksi (*forecasting*),
2. Kemudian kuadratkan selisih tersebut,
3. Jumlahkan seluruh hasil kuadrat tersebut,
4. Bagi dengan jumlah data (*n*),
5. Lalu ambil akar kuadrat dari hasil tersebut.

Berikut adalah gambaran umum rumus dari RMSE:

$$
\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (\hat{y}_i - y_i)^2}
$$

Keterangan:

* $\hat{y}_i$: nilai prediksi
* $y_i$: nilai aktual
* $n$: jumlah data

Pada Modeling terlihat nilai RMSE 0.2850 untuk train dan 0.3768 untuk error validasi. angka tersebut tergolong bagus untuk rekomendasi karena error yang dihasilkan nilainya cukup kecil.

## Evaluasi Menggunakan Precision pada Content-Based Filtering

Selain menggunakan **RMSE (Root Mean Squared Error)** sebagai matriks evaluasi, proyek sistem rekomendasi wisata Bandung ini juga menggunakan **Precision** untuk mengevaluasi performa model dengan pendekatan **Content-Based Filtering**.

**Precision** merupakan metrik yang mengukur seberapa banyak hasil rekomendasi yang relevan dari seluruh hasil yang direkomendasikan. Artinya, precision digunakan untuk mengetahui **proporsi rekomendasi yang sesuai dengan preferensi pengguna**, dalam hal ini berdasarkan **kategori wisata** yang sama.

Sebagai contoh, pada pengujian dengan menggunakan data wisata **Masjid Daarut Tauhiid Bandung** yang berkategori **Tempat Ibadah**, sistem merekomendasikan beberapa wisata lainnya. Jika dilihat dari tabel berikut:

| nama\_wisata                      | kategori\_wisata |
| --------------------------------- | ---------------- |
| Masjid Pusdai                     | Tempat Ibadah    |
| Masjid Agung Trans Studio Bandung | Tempat Ibadah    |
| Masjid Raya Bandung               | Tempat Ibadah    |
| Gereja Tiberias Indonesia Bandung | Tempat Ibadah    |
| Masjid Salman ITB                 | Tempat Ibadah    |

Semua hasil rekomendasi yang diberikan memiliki **kategori yang sama** dengan wisata awal, yaitu **Tempat Ibadah**.

Dengan demikian, jika dari **5 wisata yang direkomendasikan**, semuanya benar-benar relevan (sama kategorinya), maka:

$$
\textbf{Precision} = \frac{\text{Jumlah Rekomendasi Relevan}}{\text{Jumlah Total Rekomendasi}} = \frac{5}{5} = 1.0
$$

Nilai **precision sebesar 1.0** menunjukkan bahwa model **berhasil memberikan rekomendasi yang sangat relevan**, yang menunjukkan bahwa pendekatan **Content-Based Filtering** pada kategori ini bekerja **dengan sangat baik**.



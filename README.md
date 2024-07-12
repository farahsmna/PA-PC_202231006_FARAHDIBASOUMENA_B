# PA-PC_202231006_FARAHDIBASOUMENA_B

Nama  : Farahdiba Soumena <br>
Nim   : 202231006<br>
Kelas : B<br>
## UAS_PCD
# PENGERTIAN FILTERING CITRA 
   Filtering citra adalah proses penting dalam pengolahan citra yang bertujuan untuk meningkatkan kualitas visual atau mempersiapkan gambar untuk analisis lebih lanjut.Teknik-teknik filtering memungkinkan penghilangan noise, peningkatan ketajaman, dan penonjolan fitur-fitur yang penting dalam gambar. Teknik yang tepat dipilih tergantung pada sifat dari noise yang ada dalam gambar dan tujuan akhir dari analisis citra.
- Jenis-Jenis Filtering Citra
1. Linear Filtering:
* Mean Filter: Filter ini menggantikan nilai pixel dengan rata-rata nilai pixel di sekitarnya. Ini efektif untuk menghaluskan gambar dan mengurangi noise sederhana.
* Gaussian Filter: Filter ini menggunakan kernel Gaussian untuk menghitung nilai rata-rata ponderasi pixel di sekitarnya. Ini berguna untuk menghaluskan gambar sambil mempertahankan detail tepi.
* Median Filter: Filter ini menggantikan nilai pixel dengan nilai median dari nilai pixel di sekitarnya. Ini sangat efektif untuk menghilangkan noise salt-and-pepper pada gambar.
2. Non-linear Filtering:
* Bilateral Filter: Filter ini mempertimbangkan kedua jarak spasial dan perbedaan intensitas warna antara pixel di sekitarnya dan pixel yang sedang diproses. Ini menjaga detail tepi gambar sambil mengurangi noise dengan baik.
* Adaptive Filter: Filter ini menyesuaikan kernel dengan kondisi lokal di sekitar setiap pixel, memungkinkan pengolahan yang lebih adaptif terhadap variasi lokal dalam gambar.
- Teknik filtering citra yang umum digunakan:
1. Mean Filter
   Mean filter adalah salah satu teknik filtering sederhana yang digunakan untuk menghaluskan citra dengan cara mengambil rata-rata nilai pixel di sekitar setiap pixel dalam gambar. Ini berguna untuk mengurangi noise yang tersebar di seluruh gambar.
   * Proses: Sebuah kernel berukuran mxn
m×n diterapkan pada setiap pixel dalam gambar. Setiap nilai pixel digantikan oleh rata-rata nilai di dalam kernel.
   * Kelebihan: Mudah diimplementasikan dan efektif untuk mengurangi noise jenis salt-and-pepper.
   * Kekurangan: Tidak efektif untuk menghilangkan noise yang bersifat spasial atau menghaluskan tepi gambar.
2. Median Filter
   Median filter adalah metode filtering yang mengganti nilai pixel dengan nilai median di sekitarnya. Ini merupakan salah satu teknik yang paling umum digunakan untuk menghilangkan noise salt-and-pepper yang acak.
   * Proses: Seperti mean filter, tetapi nilai pixel digantikan oleh nilai median dari nilai pixel dalam kernel.
   * Kelebihan: Efektif menghilangkan noise salt-and-pepper tanpa mengaburkan tepi gambar.
   * Kekurangan: Kurang efektif dalam mengurangi noise Gaussian atau noise berfrekuensi tinggi.
3. Gaussian Filter
   Gaussian filter adalah jenis filtering yang memanfaatkan distribusi Gaussian untuk menghaluskan citra. Kernel dari filter ini diterapkan pada setiap pixel dalam gambar dengan bobot yang lebih tinggi diberikan pada pixel yang lebih dekat dengan pixel pusat.
   * Proses: Setiap nilai pixel digantikan oleh nilai rata-rata ponderasi dari nilai pixel dalam kernel Gaussian.
   * Kelebihan: Efektif dalam mengurangi noise Gaussian dan dapat diatur untuk mempertahankan tepi gambar.
   * Kekurangan: Lebih kompleks dalam implementasinya dibandingkan dengan mean atau median filter.
4. Bilateral Filter
   Bilateral filter adalah jenis filtering non-linear yang mempertahankan tepi gambar sambil mengurangi noise. Filter ini menghitung nilai pixel baru dengan mempertimbangkan kedua jarak spasial dan perbedaan intensitas warna antara pixel tetangga dan pixel pusat dalam kernel.
   * Proses: Filter ini mengkombinasikan filter spasial (seperti Gaussian) dengan filter intensitas (menurut perbedaan intensitas warna).
* Kelebihan: Efektif dalam menjaga detail tepi gambar sambil mengurangi noise.
   * Kekurangan: Lebih lambat dalam komputasi dibandingkan dengan teknik filtering linear seperti mean atau median filter.

#  PENJELASAN PROGRAMA
- import cv2
- import numpy as np
- import matplotlib.pyplot as plt
   * cv2: Library OpenCV untuk pemrosesan citra.
   * numpy: Library untuk operasi numerik.
   * matplotlib.pyplot: Library untuk visualisasi.
- original_image = cv2.imread('Farah.jpg')
   * cv2.imread('Farah.jpg'): Membaca gambar dari file Farah.jpg.
- original_image_rgb = cv2.cvtColor(original_image, cv2.COLOR_BGR2RGB)
   * cv2.cvtColor: Mengkonversi gambar dari format BGR (yang digunakan oleh OpenCV) ke RGB (yang digunakan oleh Matplotlib untuk visualisasi).
- gray_image = cv2.cvtColor(original_image, cv2.COLOR_BGR2GRAY)
   * cv2.cvtColor: Mengkonversi gambar dari format BGR ke grayscale.
- median_filtered_image = np.zeros_like(original_image_rgb)
for i in range(3):
    median_filtered_image[:, :, i] = cv2.medianBlur(original_image_rgb[:, :, i], 3)
   * np.zeros_like: Membuat array kosong dengan bentuk yang sama seperti original_image_rgb.
   * cv2.medianBlur: Menerapkan median filter dengan ukuran kernel 3x3 pada setiap channel RGB (R, G, dan B) secara terpisah.
- def mean_filter_manual(image, kernel_size=3):
    pad_size = kernel_size // 2
    padded_image = np.pad(image, pad_size, mode='constant', constant_values=0)
    mean_filtered_image = np.zeros_like(image)
    
    for i in range(image.shape[0]):
        for j in range(image.shape[1]):
            neighborhood = padded_image[i:i+kernel_size, j:j+kernel_size]
            mean_value = np.mean(neighborhood)
            mean_filtered_image[i, j] = mean_value
    
    return mean_filtered_image
   * mean_filter_manual: Fungsi untuk menerapkan mean filter secara manual.
   * kernel_size: Ukuran kernel (default 3).
   * pad_size: Ukuran padding (setengah dari ukuran kernel).
   * np.pad: Menambahkan padding pada gambar dengan nilai nol (padding konstan).
   * np.zeros_like: Membuat array kosong dengan bentuk yang sama seperti image.
   * np.mean: Menghitung nilai rata-rata dari neighborhood sekitar setiap piksel.
- mean_filtered_image_manual = mean_filter_manual(gray_image)
   * Menerapkan fungsi mean_filter_manual pada gambar grayscale untuk menghasilkan gambar yang difilter secara manual dengan mean filter.
- fig, axes = plt.subplots(1, 2, figsize=(12, 6))
ax = axes.ravel()

ax[0].imshow(original_image_rgb)
ax[0].set_title('Citra Asli')
ax[0].axis('on')

ax[1].imshow(median_filtered_image)
ax[1].set_title('After  Filter Median ')
ax[1].axis('on')

plt.show()
   * plt.subplots: Membuat subplots untuk menampilkan beberapa gambar dalam satu figure.
   * imshow: Menampilkan gambar.
   * set_title: Memberi judul pada subplot.
   * axis: Menampilkan sumbu gambar.
   * plt.show(): Menampilkan figure.
- fig, axes = plt.subplots(1, 2, figsize=(10, 10))
ax = axes.ravel()

ax[0].imshow(gray_image, cmap='gray')
ax[0].set_title('Citra Asli (Grayscale)')
ax[0].axis('on')

ax[1].imshow(mean_filtered_image_manual, cmap='gray')
ax[1].set_title('Citra Setelah Mean Filter Manual')
ax[1].axis('on')

plt.show()
-  Menampilkan gambar grayscale asli dan hasil mean filter manual dalam satu figure.
- fig, axs = plt.subplots(2, 2, figsize=(10, 10))

axs[0, 0].imshow(original_image_rgb)
axs[0, 0].set_title("Citra Asli")
axs[0, 0].axis('on')

axs[0, 1].imshow(median_filtered_image)
axs[0, 1].set_title("Setelah Median Filter")
axs[0, 1].axis('on')

axs[1, 0].imshow(gray_image, cmap='gray')
axs[1, 0].set_title("Citra Asli (Grayscale)")
axs[1, 0].axis('on')

axs[1, 1].imshow(mean_filtered_image_manual, cmap='gray')
axs[1, 1].set_title("Setelah Mean Filter Manual")
axs[1, 1].axis('on')

plt.show()
   * Menampilkan empat gambar dalam satu figure untuk membandingkan gambar asli, setelah median filter, grayscale asli, dan setelah mean filter manual.
# KESIMPULAN PROGRAM 
Program di atas merupakan pengolahan gambar menggunakan teknik median filtering dan mean filtering secara manual. Pertama, gambar dibaca dan dikonversi ke format yang sesuai untuk pengolahan (RGB dan grayscale). Selanjutnya, dilakukan median filtering pada setiap channel RGB untuk mengurangi noise gambar. Selain itu, dilakukan mean filtering secara manual pada gambar grayscale untuk memperhalus gambar dengan menghitung rata-rata nilai piksel di sekitar setiap titik. Hasil dari proses ini ditampilkan dalam berbagai subplot untuk membandingkan gambar asli, gambar setelah median filtering, gambar grayscale, dan gambar setelah mean filtering manual. Pendekatan ini memungkinkan analisis visual yang komprehensif terhadap efek filtering terhadap citra, baik secara keseluruhan maupun dalam komponen warna dan kecerahan.

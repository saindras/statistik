# Analysis of Covariance (Ancova) - Bahasa Indonesia

## Sekilas tentang Ancova

Ancova adalah singkatan dari "Analysis of Covariance" yang merupakan metode statistik yang menggabungkan elemen-elemen dari dua teknik statistik yang berbeda: Analisis Varians (Anova) dan Analisis Regresi. Tujuan utama Ancova adalah untuk membandingkan rata-rata antara dua atau lebih kelompok dengan mempertimbangkan pengaruh variabel bebas yang disebut sebagai covariate. Ini digunakan untuk memeriksa apakah ada perbedaan signifikan antara kelompok-kelompok tersebut setelah mengontrol atau memperhitungkan perbedaan dalam nilai rata-rata covariate. Covariate adalah variabel yang mempengaruhi variabel terikat, dan dengan mengontrol covariate ini, Ancova membantu mengurangi variabilitas yang dapat menyebabkan kesalahan dalam penentuan perbedaan antara kelompok-kelompok tersebut.

Dalam konteks Ancova, ada beberapa istilah yang perlu dipahami:
1. Variabel Terikat: Ini adalah variabel yang ingin Anda bandingkan antara kelompok-kelompok. Misalnya, dalam penelitian pendidikan, variabel terikat mungkin adalah hasil tes peserta didik.
2. Variabel Bebas: Ini adalah variabel yang Anda gunakan untuk membagi kelompok-kelompok. Misalnya, dalam penelitian pendidikan, variabel bebas mungkin adalah metode pengajaran, dengan beberapa kelompok diajar dengan metode A dan yang lainnya dengan metode B.
3. Covariate: Ini adalah variabel yang dapat mempengaruhi variabel terikat dan perlu dikontrol. Ini adalah bagian penting dari Ancova dan membantu mengurangi kesalahan dalam penentuan perbedaan antara kelompok-kelompok. Misalnya, dalam penelitian pendidikan, covariate mungkin adalah tingkat intelegensi peserta didik.

Ancova digunakan ketika Anda ingin membandingkan rata-rata antara kelompok-kelompok yang berbeda, sambil mempertimbangkan pengaruh dari covariate. Ini dapat membantu Anda memahami apakah perbedaan antara kelompok-kelompok tetap signifikan setelah mengontrol faktor-faktor tertentu yang dapat memengaruhi hasil Anda.

## Contoh kasus Ancova satu jalur

Diketahui variabel bebas VLEs terdiri dari 3 kelompok yakni VLE1, VLE2, dan VLE3. VLE1 treatment menggunakan video, VLE2 treatment menggunakan Cisco-PT, dan VLE3 treatment menggunakan Vilanets. Covariate adalah hasil pretest peserta didik. Variabel terikat adalah hasil posttest peserta didik. Data untuk variabel bebas bertipe kategorikal dan data untuk variabel terikat dan covariate adalah tipe numerikal.

Dalam hal ini, peneliti ingin menemukan pengaruh dari Virtual Learning Environments (VLEs) terhadap prestasi belajar peserta didik (PB). Apabila ada pengaruh, peneliti juga ingin menemukan apakah covariate merupakan sebuah variabel penting dalam menentukan pengaruh. Selain itu, peneliti juga dapat menemukan perbedaan antara ketiga jenis variabel bebas (VLE1, VLE2, dan VLE3) menggunakan perhitungan EMM dan Bonferroni post-hoc test.

Adapun uji asumsi yang terlebih dahulu dilakukan sebelum ke proses Ancova adalah uji asumsi ouliers, linearity, homogeneity of regression slopes, normality of residuals, dan homogeneity of variances.

## Hasil perhitungan menggunakan Python, R, dan SPSS

1. Hasil perhitungan menggunakan bahasa Python: https://nbviewer.org/github/saindras/Ancova/blob/main/ANCOVA.ipynb
2. Hasil perhitungan menggunakan bahasa R: https://rpubs.com/gsaindras/Ancova-satu-jalur
3. Hasil perhitungan menggunakan SPSS: Anda dapat menghubungi author melalui email di alamat gsaindras@undiksha.ac.id

## Referensi

1. Analysis of covariance using Python: https://www.youtube.com/watch?v=FhZB1oGVrYc
2. Ancova in R: https://www.datanovia.com/en/lessons/Ancova-in-r/
3. Ancova using R and Python: https://www.reneshbedre.com/blog/Ancova.html

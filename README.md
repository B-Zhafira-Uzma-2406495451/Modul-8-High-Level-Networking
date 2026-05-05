## Reflection Module 8

1. Dalam gRPC, perbedaan utama antara metode unary, server streaming, dan bi-directional streaming terletak pada jumlah
pesan yang dikirimkan antara client dan server. Unary gRPC adalah protokol komunikasi sederhana di mana client
mengirimkan satu permintaan dan menerima satu respons tunggal, sangat cocok untuk tugas seperti autentikasi atau
pengambilan satu item basis data. Sementara itu, server streaming memungkinkan server mengirimkan aliran data
berkelanjutan setelah menerima satu permintaan dari client, yang ideal untuk pembaruan data real-time seperti harga
pasar saham atau cuaca. Terakhir, bi-directional streaming memungkinkan kedua belah pihak mengirimkan banyak pesan
secara bersamaan dalam satu koneksi, menjadikannya pilihan utama untuk aplikasi interaktif seperti chat atau analitik
waktu nyata.

2. Walaupun tutorial modul 8 ini berfokus pada fungsionalitas dasar, implementasi layanan gRPC di dunia nyata memerlukan
pertimbangan keamanan yang ketat. Hal ini mencakup penerapan enkripsi data melalui TLS untuk melindungi informasi selama
transmisi, serta mekanisme autentikasi untuk memverifikasi identitas pengguna. Selain itu, diperlukan sistem otorisasi
guna memastikan bahwa pengguna hanya dapat mengakses metode RPC yang sesuai dengan hak akses mereka untuk mencegah
kebocoran data pada layanan terdistribusi.

3. Menangani aliran dua arah dalam Rust gRPC, menghadirkan tantangan teknis seperti pengelolaan siklus hidup koneksi
yang kompleks dan penanganan kesalahan pada tugas asinkron. Pengembang harus memastikan bahwa jika terjadi kegagalan
dalam penerimaan pesan, layanan tidak akan berhenti total, melainkan berhenti secara gracefully. Selain itu, penentuan
ukuran buffer pada saluran komunikasi sangat penting untuk menjaga performa tetap optimal di bawah tekanan lalu lintas
data yang tinggi.

4. Penggunaan tokio_stream::wrappers::ReceiverStream dalam layanan gRPC Rust memberikan keuntungan besar karena
mempermudah konversi penerima saluran (channel receiver) menjadi aliran data yang dapat diolah oleh framework Tonic.
Namun, kekurangannya adalah kompleksitas tambahan dalam manajemen memori dan risiko kehilangan pesan jika saluran
tersebut tidak dikelola dengan benar, terutama saat kapasitas buffer penuh atau terjadi keterlambatan pemrosesan.

5. Untuk meningkatkan maintainabilitas dan ekstensi kode, struktur gRPC di Rust dapat diatur secara modular dengan
memisahkan definisi layanan ke dalam file yang berbeda dan menggunakan modul publik. Penggunaan makro
tonic::include_proto! sangat membantu karena secara otomatis menghasilkan kode boilerplate dari definisi .proto
sehingga pengembang dapat fokus pada logika bisnis layanan tanpa terganggu oleh detail teknis komunikasi jaringan.

6. Dalam implementasi MyPaymentService pada tutorial modul 8 ini, sistem hanya mengembalikan hasil sukses secara instan
untuk tujuan tutorial. Untuk menangani logika pembayaran yang lebih kompleks, diperlukan langkah-langkah tambahan
seperti integrasi dengan basis data untuk memverifikasi saldo, komunikasi dengan gerbang pembayaran atau payment
gateway) eksternal, serta penanganan berbagai status kesalahan seperti kegagalan transaksi atau dana tidak mencukupi.

7. Adopsi gRPC sebagai protokol komunikasi secara signifikan meningkatkan efisiensi dan interoperabilitas sistem
terdistribusi melalui penggunaan tipe data yang kuat dan kontrak layanan yang jelas. Meskipun memberikan keunggulan
dalam performa, hal ini juga memerlukan keseragaman infrastruktur karena setiap komponen dalam arsitektur harus
mendukung protokol HTTP/2 dan mampu memproses format Protocol Buffers agar dapat berkomunikasi dengan lancar antar
platform.

8. gRPC memanfaatkan HTTP/2 sebagai protokol dasarnya, yang menawarkan keunggulan berupa multiplexing atau banyaknya
permintaan dalam satu koneksi dan kompresi header yang jauh lebih efisien dibandingkan HTTP/1.1 yang digunakan pada REST
API tradisional. Walaupun REST dengan WebSocket dapat menangani komunikasi dua arah, HTTP/2 pada gRPC memberikan
efisiensi biner yang lebih tinggi, meski memiliki kekurangan dalam hal kemudahan proses debugging karena format datanya
yang tidak dapat dibaca langsung oleh manusia.

9. Kontras antara model permintaan-respons REST API dengan kemampuan bi-directional streaming gRPC terlihat pada aspek
responsivitas. REST bersifat sinkron dan kaku, di mana client harus menunggu respons sebelum memulai tindakan
berikutnya. Sebaliknya, gRPC memungkinkan komunikasi interaktif dua arah secara real-time, yang memungkinkan data
dikirimkan segera setelah tersedia tanpa perlu menunggu permintaan eksplisit dari sisi lain.

10. Pendekatan berbasis skema menggunakan Protocol Buffers dalam gRPC menjamin bahwa data yang dikirimkan selalu valid
dan memiliki ukuran payload yang sangat ringkas karena format biner. Hal ini berbeda dengan JSON yang bersifat
schema-less dan fleksibel, namun cenderung memakan lebih banyak bandwidth dan sumber daya CPU karena harus melakukan
serialisasi dan deserialisasi teks yang berulang-ulang pada setiap transmisi data.
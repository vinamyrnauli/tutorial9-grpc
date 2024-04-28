# Tutorial 9 (gRPC)
Nama: Vina Myrnauli Abigail Siallagan<br>
NPM: 2206825776<br>
Kelas: Pemrograman Lanjut - A<br>

---
## REFLEKSI 1

###### 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
* RPC Unary:
    - Jenis RPC paling sederhana di mana klien mengirim satu permintaan ke server dan menerima satu respons sebagai balasan.
    - Digunakan dalam situasi permintaan dan respons sederhana seperti autentikasi atau pengiriman formulir.
* RPC Streaming Server:
    - Klien mengirim satu permintaan ke server dan menerima respons dalam bentuk stream yang terdiri dari beberapa pesan.
    - Klien membaca pesan dari stream tersebut hingga tidak ada lagi pesan yang dikirimkan oleh server.
    - Cocok digunakan ketika server perlu mengirimkan sejumlah besar data, misalnya daftar besar atau file, kepada klien.
* RPC Bi-directional Streaming:
    - Klien dan server dapat mengirim serangkaian pesan menggunakan stream yang dapat dibaca dan ditulis (read-write).
    - Kedua stream tersebut beroperasi secara independen, memungkinkan klien dan server untuk membaca dan menulis data dalam urutan yang berbeda.
    - Ideal untuk aplikasi yang memerlukan pertukaran data besar antara klien dan server secara independen, seperti aplikasi obrolan.

###### 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
* Autentikasi:
    - Perlunya autentikasi untuk setiap bagian data dalam gRPC untuk memastikan keamanan.
    - Mekanisme autentikasi seperti sertifikat klien SSL/TLS atau sistem token perlu diintegrasikan dan divalidasi.
    - Pentingnya validasi yang kuat untuk mencegah serangan.
* Otorisasi:
    - Diperlukan logika otorisasi yang efektif untuk menegakkan kebijakan kontrol akses.
    - Otorisasi perlu diterapkan pada setiap akses data dalam gRPC.
* Enkripsi Data:
    - Perlunya enkripsi data pada setiap potongan data yang dikirim oleh server dan klien untuk menjaga privasi data.
    - Penggunaan SSL/TLS untuk melindungi saluran komunikasi dan mencegah akses tidak sah.
    - Pentingnya validasi input yang kuat untuk mencegah kerentanan umum.

###### 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
* Concurrency Challenges:
    - Mengelola streaming dua arah dalam gRPC Rust, terutama dalam skenario seperti aplikasi obrolan, memunculkan tantangan dalam hal konkurensi.
    - Perlu sinkronisasi yang cermat dan penanganan kesalahan untuk memastikan konsistensi dan keandalan data.
* Penanganan Kesalahan:
    - Pentingnya implementasi mekanisme penanganan kesalahan yang kuat untuk mengatasi situasi yang tidak terduga dan menjaga keandalan aplikasi.
* Manajemen Koneksi:
    - Tantangan dalam mengelola koneksi antara klien dan server untuk memastikan komunikasi yang lancar dan efisien.
* Kontrol Aliran:
    - Perlunya strategi pengendalian aliran untuk mengelola aliran pesan dengan efektif, termasuk penanganan tekanan balik.
* Uji dan Debugging:
    - Pentingnya pengujian dan debugging yang menyeluruh untuk mengidentifikasi dan mengatasi masalah potensial, sehingga menjaga stabilitas dan kinerja aplikasi gRPC Rust.

###### 4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?
* Keunggulan Penggunaan ReceiverStream dalam Layanan Rust gRPC:
    - Integrasi yang mulus dengan runtime asinkron Tokio, cocok untuk aplikasi yang sudah menggunakan Tokio untuk I/O asinkron.
    - Fleksibilitas dalam menangani berbagai jenis stream.
    - Memungkinkan streaming asinkron data, sehingga layanan Rust gRPC dapat menangani respons streaming tanpa memblokir event loop.
* Keterbatasan dan Tantangan:
    - Bergantung pada Tokio sebagai dependensi, sehingga tidak sesuai untuk proyek yang menggunakan runtime asinkron lain.
    - ReceiverStream cocok untuk skenario streaming dasar, tetapi mungkin kurang memiliki fitur lanjutan dan opsi kustomisasi yang tersedia di perpustakaan streaming lain.
    - Mungkin ada kurva belajar bagi pengembang yang baru mempelajari pemrograman asinkron di Rust atau Tokio, terutama jika mereka belum familiar dengan API dan konsep Tokio.

###### 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
* Peningkatan Kode Reusable dan Modular:
    - Struktur kode harus memiliki pemisahan yang jelas antara definisi layanan dan implementasinya.
    - Penggunaan pola dependency injection dan objek trait memungkinkan keterkaitan yang longgar antara komponen-komponen, serta meningkatkan testabilitas dan fleksibilitas.
* Penggunaan Komponen dan Utilitas yang Dapat Digunakan Kembali:
    - Komponen-komponen yang dapat digunakan kembali harus dienkapsulasi ke dalam modul atau kerangka kerja terpisah.
    - Hal ini memungkinkan akses dan penggunaan yang mudah di berbagai bagian kode.
* Parameterisasi Perilaku Layanan dan Penanganan Kesalahan yang Konsisten:
    - Parameterisasi perilaku layanan dan penerapan mekanisme penanganan kesalahan yang konsisten memastikan adaptabilitas terhadap perubahan kebutuhan dan lingkungan.
    - Ini juga membantu menjaga ketahanan terhadap kegagalan.
* Dampak Terhadap Pemeliharaan dan Perluasan:
    - Pendekatan ini mempromosikan kodebase yang mudah dipelihara dan diperluas, memungkinkan pengembangan dan evolusi sistem Rust gRPC yang kompleks secara efisien seiring waktu.

###### 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
* Peningkatan Implementasi MyPaymentService:
    - Untuk menangani logika pemrosesan pembayaran yang lebih kompleks, perlu dilakukan perubahan pada implementasi MyPaymentService.
    - Metode process_payment dapat diubah menjadi server streaming untuk mengirimkan respons secara bertahap kepada klien.
    - Ini memungkinkan pengiriman data yang lebih kompleks dengan lebih efisien, karena respons dapat dikirimkan secara bertahap selama proses pemrosesan pembayaran.

###### 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
* Dampak Adopsi gRPC pada Arsitektur dan Desain Sistem Terdistribusi:
    - gRPC memengaruhi secara signifikan arsitektur dan desain sistem terdistribusi.
    - Tidak perlu lagi memikirkan cara akses modul melalui metode HTTP karena gRPC menangani koneksi secara otomatis.
    - Kesepakatan antara klien dan server melalui file proto memungkinkan klien memanggil fungsi server seolah-olah mereka berinteraksi secara langsung.
    - Ini menyederhanakan konektivitas dan operasi antar teknologi, platform, dan sistem terdistribusi.
    - Meningkatkan interoperabilitas antara berbagai teknologi dan platform yang digunakan dalam arsitektur sistem yang kompleks.

###### 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
* Keunggulan Penggunaan HTTP/2 untuk gRPC:
    - Multiplexing, kompresi header, server push, dan prioritas stream adalah keunggulan utama HTTP/2 dibandingkan HTTP/1.1 atau HTTP/1.1 dengan WebSocket.
    - Keuntungan ini secara kolektif mengarah pada penurunan latency, peningkatan efisiensi, dan penggunaan sumber daya jaringan yang lebih baik.
* Tantangan Penggunaan HTTP/2:
    - Kompleksitas yang meningkat dalam implementasi dan debugging dibandingkan dengan HTTP/1.1.
    - Potensial untuk menghadapi masalah kompatibilitas dengan sistem yang lebih lama.
    - Kemungkinan konsumsi sumber daya server yang lebih tinggi dibandingkan HTTP/1.1.
* Perbandingan dengan WebSocket:
    - Meskipun HTTP/2 mendukung komunikasi dua arah, WebSocket lebih cocok untuk skenario komunikasi real-time dan full-duplex.
* Faktor-Faktor Penentu Pilihan Protokol:
    - Keputusan antara HTTP/2, HTTP/1.1, atau WebSocket tergantung pada persyaratan kinerja, kendala kompatibilitas, dan kasus penggunaan API yang spesifik.
* Pertimbangan Trade-Offs:
    - Pengembang perlu mempertimbangkan trade-offs secara hati-hati sebelum membuat keputusan.

###### 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
* Karakteristik REST API:
    - Dikenal dengan kesederhanaannya, menggunakan metode HTTP standar dan statelessness untuk kemudahan penggunaan.
    - Tidak mendukung komunikasi real-time, sering menggunakan polling atau long-polling yang dapat meningkatkan kompleksitas dan latensi.
* Karakteristik gRPC:
    - Menawarkan streaming dua arah yang memungkinkan klien dan server untuk mengirim data secara independen.
    - Cocok untuk skenario waktu nyata dan berpotensi meningkatkan responsivitas aplikasi.
* Perbandingan Antara REST API dan gRPC:
    - Meskipun REST API mudah digunakan, tidak memiliki dukungan untuk komunikasi real-time dan sering mengandalkan teknik polling atau long-polling.
    - gRPC, meskipun lebih kompleks dalam implementasinya, menawarkan kemampuan streaming dua arah yang dapat meningkatkan kinerja dan manfaat komunikasi real-time dalam kasus penggunaan tertentu.

###### 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
* gRPC dengan Protocol Buffer:
    - Menawarkan kompatibilitas yang baik, kinerja yang lebih tinggi, dan alat yang lebih baik karena menggunakan pendekatan berbasis skema.
    - Cocok untuk skenario yang membutuhkan strong typing dan kinerja yang kuat.
* JSON dalam REST API:
    - Memberikan fleksibilitas dan keterbacaan di berbagai bahasa dan platform.
    - Meskipun demikian, bisa mengakibatkan biaya tambahan dalam beberapa kasus.
* Pilihan Antara keduanya:
    - Keputusan antara gRPC dengan Protocol Buffer dan JSON dalam REST API tergantung pada persyaratan spesifik.
    - gRPC lebih diuntungkan untuk skenario yang memprioritaskan strong typing dan kinerja yang kuat, sementara JSON dalam REST API sesuai untuk situasi yang mengutamakan fleksibilitas dan keterbacaan.



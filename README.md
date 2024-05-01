## Reflection
1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable? <br>
Jawab: 
- Unary merupakan metode paling dasar pada RPC dimana metode ini biasa digunakan saat suatu query
  membutuhkan interaksi sederhana tanpa perlu aliran data yang berkelanjutan atau multiple responses, seperti pada
  operasi CRUD (Create, Read, Update, Delete) di database, mengambil informasi pengguna, atau permintaan pengaturan sederhana dalam aplikasi
- Server streaming merupakan metode pada RPC dimana client akan terus menerima data sampai server menandakan bahwa stream telah selesai.
  Metode ini biasa digunakan saat data yang dihasilkan atau diakses oleh server bersifat berkelanjutan atau berjumlah besar, misalnya saat hendak mengirimkan log event
  secara real-time
- Bi-directional streaming merupakan metode RPC memungkinkan kedua client dan server mengirim dan menerima series of messages menggunakan satu koneksi yang sama. Pada metode ini,
  stream dari kedua arah beroperasi secara independen sehingga client dan server dapat berkomunikasi secara asinkron. Metode ini biasanya digunakan saat aplikasi membutuhkan interaksi
  dua arah dengan komunikasi yang intensif. Metode ini biasanya diimplementasikan dalam pembuatan aplikasi chat dimana pesan perlu dikirim dan diterima secara bersamaan

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption? <br>
Jawab: Dengan mengimplementasikan gRPC di Rust, diperlukan autoriasi serta autentikasi berkali-kali untuk 1 stream request. Pengelolaan otorisasi ini diperlukan untuk
mengendalikan akses berdasarkan peran atau aturan yang ditetapkan. Sementara itu, untuk enkripsi data tiap potongan data yang dikirimkan baik oleh server maupun client
perlu di enkripsi secara terpisah untuk menjamin privasi data yang dikirimkan.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications? <br>
Jawab: Dalam implementasi bidirectional streaming pada gRPC di Rust, terdapat beberapa potensi tantangan seperti proses sinkronisasi data atau pesan. Dalam konteks aplikasi chat
seperti pada tutorial ini, mengelola banyak koneksi secara bersamaan dan menjaga agar koneksi tetap aktif tanpa memberatkan server tentunya dapat menjadi suatu tantangan tersendiri.
Dalam streaming dua arah, pesan sebaiknya dapat diterima dan dikirim pada waktu yang bersamaan. Hal ini menuntut sistem untuk dapat menangani operasi asinkron dengan baik serta
memastikan bahwa tidak ada deadlock atau race condition yang terjadi saat mengakses suatu sumber secara bersamaan

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services? <br>
Jawab: Dalam implementasi gRPC pada Rust, penggunaan `tokio_stream::wrappers::ReceiverStream` dimanfaatkan untuk mengelola pengolahan data secara asinkronus dengan cepat serta
mempermudah integrasi dengan modul Tokio lainnya. Namun, terdapat pula beberapa kerugian dari implementasi ini seperti adanya kemungkinan terjadi kompleksitas error handling
dari pengolahan data asinkronus hingga diperlukannya autentikasi dan autorisasi untuk setiap data

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time? <br>
Jawab: Untuk meningkatkan penggunaan ulang dan modularitas kode gRPC dalam Rust, pembuatan suatu interface yang diimplementasikan dengan penggunaan proto dapat membantu menjaga
maintainability dan extensibility kode yang dimiliki. Selain itu, membagi kode menjadi modul atau crate berdasarkan fungsionalitas juga dapat dilakukan untuk
meningkatkan penggunaan ulang dan modularitas kode

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic? <br>
Jawab: Untuk menangani logika pemrosesan pembayaran yang lebih kompleks pada implementasi MyPaymentService, diperlukan beberapa peningkatan yang dapat dilakukan
seperti mengubah implementasi unary menjadi server streaming sehingga pengiriman data-data yang kompleks dapat dilakukan dengan lebih cepat serta dapat mengurangi overhead
pembuatan koneksi antara client dan server

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms? <br>
Jawab: Pengaruh yang dapat terjadi ketika kita melakukan adopsi gRPC sebagai protokol komunikasi yakni memungkinkannya terjadi interaksi yang lebih efisien antarmodul.
Dengan implementasi ini, kita tidak perlu memikirkan bagaimana suatu modul dapat diakses melalui http method karena gRPC akan melakukan pemanggilan method yang diinginkan secara otomatis
melalui file proto dimana client seolah dapat memanggil function dari server. Penggunaan adopsi ini tentunya dapat mempermudah konektivitas dan interoperabilitas antar teknologi serta platform terkait

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs? <br>
Jawab: Jika dibandingkan dengan HTTP/1.1 atau HTTP/1.1 dengan WebSocket untuk REST APIs, HTTP/2 memiliki beberapa keuntungan seperti memberikan opsi dimana banyak
request dan response dapat dilakukan dalam sebuah koneksi tanpa perlu mematikan suatu koneksi dengan konsep multiplexing. Sementara itu untuk kekurangannya, penggunaan
HTTP/2 memerlukan biaya overhead yang lebih tinggi baik dari segi performance maupun memory sehingga diperlukan penyesuaian pada server dan client

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness? <br>
Jawab: Perbedaan model permintaan-respons REST API dengan kemampuan streaming dua arah gRPC dalam hal komunikasi dan daya tanggap real-time terletak dari seberapa lama
waktu yang diperlukan untuk melakukan real-time communication. Pada umumnya, REST APIs akan memerlukan waktu lebih lama dibanding gRPC karena setiap pengiriman request
memerlukan pembuatan koneksi baru dari client ke server sementara gRPC hanya memerlukan 1 koneksi hingga seluruh request selesai. Jadi, dalam hal komunikasi dan daya tanggap real-time,
penggunaan gRPC akan lebih efektif dibandingkan penggunaan REST APIs

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads? <br>
Jawab: Pendekatan gRPC berbasis skema menggunakan protocol buffer dapat menghasilkan pengiriman data menjadi lebih efisien dan konsisten. Sehingga implikasi dari pendekatan ini yakni
tiap data yang masuk dan keluar bisa dibilang terjamin dapat digunakan serta tidak salah tipe. Hal sebaliknya jika kita membandingkan dengan JSON di REST APIs,
sangat mungkin terjadi kasus pengiriman data yang berisi informasi yang tidak bisa dipakai oleh salah satu pihak serta sulit melakukan pengecekan data karena strukturnya
yang mungkin saja berubah sewaktu-waktu.

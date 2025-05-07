**Nama: Ghina Nabila Gunawan**

**Kelas: AdvProg - A**

**Kode Asdos: HAN**

---

### **1️⃣ Differences between Unary, Server Streaming, and Bi-Directional streaming RPC**

- **Unary RPC**: Komunikasi di mana client mengirimkan satu permintaan dan menerima satu respons. Ini adalah model paling sederhana, di mana server hanya return satu respons untuk setiap request. 
    Cocok untuk operasi yang membutuhkan hasil tunggal, seperti memproses pembayaran, mengambil data pengguna, atau mengautentikasi pengguna.


- **Server Streaming RPC**: Client mengirimkan satu request, tetapi server mengirimkan banyak respons. 
    Ini cocok untuk mengambil data dalam jumlah besar atau yang terus berubah, seperti history transaksi atau log aplikasi secara real-time.


- **Bi-Directional Streaming RPC**: Baik client ataupun server bisa mengirim dan menerima banyak pesan secara simultan. 
    Sangat cocok untuk aplikasi real-time seperti aplikasi chat, video call, atau game multiplayer di mana kedua pihak (client dan server) perlu terus bertukar data.



### **2️⃣ Security considerations involved in implementing a gRPC in Rust**

- **Authentication and Authorization**: 
    Salah satu pendekatan yang umum digunakan adalah menggunakan **JWT (JSON Web Tokens)** atau **OAuth** untuk memastikan bahwa hanya pengguna yang sah yang bisa mengakses layanan tertentu.
    Selalu pastikan bahwa token atau kredensial yang diterima valid dan memiliki hak akses yang sesuai sebelum memproses request.


- **Data Encryption**:
    Encryption sangat penting dalam komunikasi gRPC. gRPC secara default mendukung **TLS (Transport Layer Security)** untuk mengenkripsi data yang dikirim antara client dan server. Pastikan TLS diaktifkan di setiap komunikasi untuk melindungi data dari akses tidak sah dan pencurian data.

Pastikan untuk selalu memvalidasi setiap pesan yang dikirim dalam streaming dan menggunakan teknik untuk mencegah **denial-of-service (DoS)** atau serangan buffer overflow.



### **3️⃣ Challenges that may arise when handling Bi-Directional streaming in Rust gRPC**


- **Session Management**:
    Menjaga sesi koneksi open antara client dan server bisa menjadi tantangan. Server perlu memastikan bahwa setiap pesan yang masuk dikirim ke client yang benar, dan sebaliknya.

- **Concurrency**: Dalam chat atau aplikasi real-time lainnya, banyak pesan yang dikirim dan diterima secara bersamaan. Mengelola aliran data ini dalam gRPC memerlukan penggunaan concurrency untuk memastikan aplikasi tetap responsif.

- **Timeout and Connection Recovery**: Untuk aplikasi seperti chat, kalau koneksi terputus, klien harus bisa menghubungkan kembali ke server tanpa kehilangan history chat atau status. 



### **4️⃣ Advantages and Disadvantages using `ReceiverStream` in Rust gRPC services**

- **Advantages**:
  - `ReceiverStream` memungkinkan kita untuk membungkus stream yang dikendalikan oleh `mpsc` (multiple producer, single consumer) channel dan mengirimkannya dengan mudah ke client. 
  - Bisa memanfaatkan **async/await** dari Rust, memungkinkan komunikasi yang sangat efisien dan non-blocking.


- **Disadvantages**:
  - Menggunakan `ReceiverStream` membutuhkan buffer untuk menyimpan pesan yang akan dikirim, yang bisa menjadi masalah ketika volume pesan meningkat secara drastis.
  - Meskipun asynchronous, kita tetap harus hati-hati dalam mengelola concurrency, terutama ketika banyak pesan dikirim dalam waktu bersamaan.



### **5️⃣ In what ways could the Rust gRPC code be structured for Modularity and Reusability**

Untuk mendukung modularity dan reusability dalam kode, kita bisa memisahkan setiap layanan ke dalam file terpisah (misalnya `grpc_client.rs` dab `grpc_service.rs`). 

- **Modular:** Dengan menggunakan trait di Rust, kita bisa mendefinisikan service contract yang kemudian diimplementasikan dalam modul-modul terpisah. Dengan cara ini, setiap komponen layanan bisa di-test, diperbarui, dan dikembangkan secara independen, yang pastinya meningkatkan modularitas app.


- **Reusable:** Penggunaan generics dan helper functions di dalam modul memungkinkan kode yang lebih clean dan bisa digunakan kembali di berbagai bagian app. Ini meningkatkan reusabilitas kode sehingga mempermudah pengembangan selanjutnya.



### **6️⃣ Additional Steps to Handle more Complex Payment Processing Logic**

- Menyimpan status pembayaran, riwayat transaksi, dan data pengguna di **database**.

- Menambahkan proses **validasi** untuk memastikan bahwa pembayaran bisa diproses, misalnya, memeriksa apakah saldo cukup.

- Menambahkan **logic** untuk menangani kesalahan jika terjadi masalah selama pemrosesan pembayaran (misalnya, saldo tidak cukup atau masalah jaringan).



### **7️⃣ Impact adoption of gRPC on Architecture and Design of Distributed Systems**

- gRPC bisa bekerja dengan berbagai bahasa pemrograman, memungkinkan **interoperabilitas** yang mudah antara layanan yang ditulis dalam bahasa yang berbeda (misalnya, Rust, Go, Python, Java).

- gRPC lebih cepat daripada REST karena menggunakan **Protocol Buffers** untuk data serialization, yang lebih efisien dibandingkan JSON dalam hal data size dan kecepatan.

- gRPC memfasilitasi komunikasi antar layanan dalam arsitektur **microservices** karena dukungannya untuk streaming, multiplexing, dan efisien dalam komunikasi antar-layanan.



### **8️⃣ Advantages dan Disadvantages using HTTP/2 for gRPC compared to HTTP/1.1 or WebSocket for REST API**

* **Advantages HTTP/2**:
  * **Multiplexing**: Beberapa request bisa dikirim dalam satu koneksi, mengurangi latensi dan meningkatkan efisiensi.
  * **Pengurangan Overhead**: HTTP/2 lebih efisien dalam hal overhead dibandingkan HTTP/1.1, terutama untuk komunikasi berbasis request-response.

- **Disadvantages HTTP/2**:
  - HTTP/2 memerlukan pengaturan yang lebih rumit pada server dan client dibandingkan HTTP/1.1.

**WebSocket** untuk REST API bisa lebih cocok untuk aplikasi yang membutuhkan open connection secara terus-menerus, seperti aplikasi chat atau streaming real-time, tetapi membutuhkan pengelolaan koneksi yang lebih kompleks.



### **9️⃣ Comparison of Model Request-Response REST with capabilities of Bi-Directional Streaming gRPC**

* **REST API (Request-Response)**: Lebih cocok untuk aplikasi yang punya pola komunikasi sederhana (client mengirim request, server memberikan respons). 
* **gRPC (Bidirectional Streaming)**: Lebih cocok untuk aplikasi yang membutuhkan komunikasi dua arah secara terus-menerus dan real-time, seperti aplikasi chat, live streaming, atau collaboration app.



### **1️⃣0️⃣ Implications of the schema-based approach of gRPC (Protocol Buffers) compared to JSON in REST API**

* **gRPC with Protocol Buffers**: Lebih efisien dalam hal ukuran data dan kecepatan parsing, karena menggunakan format biner yang lebih compact dibandingkan JSON.
* **REST API with JSON**: Lebih fleksibel karena JSON tidak memerlukan skema dan mudah dibaca manusia, tetapi lebih berat dibandingkan Protocol Buffers dalam hal size dan kecepatan.


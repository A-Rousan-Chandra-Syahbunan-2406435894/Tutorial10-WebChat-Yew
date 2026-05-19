# Tutorial 10: WebChat using Yew

## Experiment 3.1: Original Code
Berhasil menjalankan client YewChat dan SimpleWebsocketServer. Aplikasi dapat mengirim pesan antar tab menggunakan WebSocket.
![alt text](image.png)
![alt text](image-1.png)

Dalam tahap awal ini, saya melakukan implementasi sistem WebChat menggunakan bahasa pemrograman **Rust** dengan *framework* **Yew** sebagai *frontend* (*webclient*). Arsitektur ini memungkinkan pembuatan aplikasi web berperforma tinggi menggunakan **WebAssembly (WASM)**. 

Komunikasi data dilakukan secara *asynchronous* melalui protokol **WebSocket**, sehingga setiap pengguna dapat mengirim dan menerima pesan secara *real-time* tanpa perlu melakukan *refresh* pada browser. Implementasi ini melibatkan dua komponen utama:
1. **YewChat**: Bertugas sebagai *client-side interface* yang menangani *routing* dan interaksi pengguna.
2. **SimpleWebsocketServer**: Folder server berbasis Node.js yang bertugas sebagai *relay* pesan antar *client*.


**Menjalankan WebSocket Server:**
Masuk ke direktori server dan jalankan perintah berikut untuk mengaktifkan *backend*:
```bash
npm install
npm start
```

## Experiment 3.2: Be Creative!

![alt text](image-3.png)
![alt text](image-2.png)

Pada eksperimen ini, saya memberikan sentuhan kreativitas pada tampilan webclient YewChat dengan mengusung tema **"Nusantara Cyber-Terminal"**. Saya merombak total skema warna original menjadi dominan hitam dengan aksen hijau neon (`#22c55e`) untuk menciptakan suasana terminal keamanan tingkat tinggi yang futuristik dan ikonik.

Selain itu, saya memodifikasi seluruh teks dalam aplikasi menjadi bahasa teknis terminal yang lebih ekspresif. Judul aplikasi diubah menjadi `// TERMINAL: ROUSAN-NET` dan tombol login diubah menjadi `INITIATE LINK [DIR:CHAT]`. Saya juga mengganti sistem avatar menggunakan API Dicebear **Bottts** sehingga setiap pengguna direpresentasikan oleh ikon robot unik yang selaras dengan tema teknologi Rust.

Perubahan ini juga mencakup penambahan elemen dekoratif seperti indikator status `● encryption_active` dan `signal: stable` yang memiliki animasi *pulse*. Melalui eksperimen ini, saya memahami bahwa kreativitas dalam pengembangan perangkat lunak tidak hanya soal estetika, tetapi juga tentang bagaimana membangun *branding* dan atmosfer aplikasi melalui visual, teks, dan interaksi yang konsisten tanpa merubah mekanisme utama WebSocket di belakangnya.

## Bonus: Rust Websocket Server Integration

### 1. Implementasi (How I did it)
Untuk mengintegrasikan *webchat* dari Tutorial 3 dengan server WebSocket berbasis Rust dari Tutorial 2, saya melakukan langkah-langkah berikut:
1. **Sinkronisasi Port:** Memastikan server Rust (Tutorial 2) berjalan pada *port* yang sama dengan yang diharapkan oleh *client* Yew (Tutorial 3), atau mengubah *endpoint* pada *client* Yew agar mengarah ke alamat server Rust (misalnya `ws://127.0.0.1:8080`).
2. **Broadcasting Logic:** Memastikan logika server Rust tetap menggunakan pola *broadcast*, di mana setiap pesan yang diterima dari satu *client* akan dikirimkan kembali ke seluruh *client* yang terhubung.
3. **Execution:** Mematikan server Javascript (Node.js) dan menjalankan server Rust menggunakan perintah `cargo run`, kemudian menjalankan *client* Yew menggunakan `trunk serve`.

### 2. Mengapa Perubahan Ini Berhasil? (Why it is a successful change)
Integrasi ini berhasil karena prinsip dasar pengiriman data pada protokol WebSocket:
* **Serialisasi String:** Meskipun *client* Yew mengirimkan data terstruktur dalam format **JSON** (yang berisi *username*, *timestamp*, dan isi pesan), data tersebut sebenarnya dikonversi (diserialisasi) menjadi sebuah **String** (teks biasa) sebelum dikirim melalui *socket*.
* **Agnostik Terhadap Konten:** Server Rust yang dibuat pada Tutorial 2 memiliki sifat *content-agnostic*. Artinya, server tersebut tidak peduli apakah teks yang ia terima adalah kalimat biasa ("Halo") atau sebuah string JSON (`{"type": "message", "data": "..."}`). Tugas server hanyalah sebagai "kurir" yang menerima *packet* teks dan membroadcast-nya ke koneksi lain.
* **Client-Side Deserialization:** Selama semua *client* yang terhubung menggunakan logika yang sama (Tutorial 3) untuk membongkar kembali (*deserialization*) string JSON tersebut menjadi objek, maka aplikasi akan tetap berjalan normal tanpa perlu mengubah kode pada sisi server.

### 3. Opini: Javascript vs Rust Server
Setelah mencoba kedua versi tersebut, berikut adalah opini saya:

**Saya lebih memilih versi Rust.**
* **Alasan:** Rust memberikan kontrol yang jauh lebih ketat terhadap memori dan keamanan data melalui *Ownership model*-nya. Dalam aplikasi *real-time* seperti WebSocket yang menangani banyak koneksi simultan, performa *multi-threading* pada Rust jauh lebih stabil dibandingkan dengan *event-loop* tunggal pada Javascript. Selain itu, menggunakan Rust di seluruh *stack* (Frontend dengan Yew dan Backend dengan Toko/Tungstenite) memudahkan pemeliharaan kode karena kesamaan sintaks dan sistem tipe data yang kuat.

**Kapan saya memilih Javascript?**
* Jika tujuannya hanyalah pembuatan prototipe cepat (*Rapid Prototyping*) atau aplikasi skala kecil yang tidak memerlukan efisiensi tinggi, Node.js tetap menjadi pilihan karena ekosistemnya yang sangat besar dan kecepatan dalam penulisan kodenya.

# Tutorial 10: WebChat using Yew

## 3.1. Original Code
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


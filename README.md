# API Wisata Sulawesi Tengah

Layanan RESTful API dan GraphQL untuk pengelolaan data destinasi wisata di Sulawesi Tengah yang mendukung versioning API (`/v1`, `/v2`, serta header `x-api-version`).

## Prasyarat
- **Node.js** versi 20.6 atau lebih baru (diperlukan untuk membaca file `.env` menggunakan flag `--env-file=.env`).

## Cara Menjalankan Project Setelah Clone
1. Masuk ke folder project dan pasang dependency yang diperlukan (`graphql`):
   ```bash
   npm install
   ```
2. Buat tiga file konfigurasi dan data rahasia berikut secara manual karena tidak disertakan di dalam repository:
   - **`.env`** (di folder utama, sejajar dengan `server.js`):
```env
TOKEN_SECRET=isi-dengan-kalimat-rahasia-milik-anda
```
   - **`data/users.json`** (berisi daftar akun pengguna seperti `admin` dan `editor`, dapat dibuat dengan menjalankan `node buat-user.js`).
   - **`data/api-keys.json`** (berisi daftar API key client yang diizinkan mengakses server).
3. Jalankan server dengan perintah:
   ```bash
   node --watch --env-file=.env server.js
   ```

## Daftar Versi API yang Tersedia
1. **URI Versioning (`/v1` dan `/v2`)**
   - `GET /v1/destinations` & `GET /v1/destinations/:id`: Format lama yang menampilkan nama daerah pada field `region`.
   - `GET /v2/destinations` & `GET /v2/destinations/:id`: Format terbaru yang menggunakan `regionId` serta mendukung filter query `?regionId=`.
   - `GET /v2/regions` & `GET /v2/regions/:id`: Endpoint data wilayah (hanya tersedia pada `v2`).
2. **Header Versioning (`x-api-version`)**
   - Endpoint tanpa awalan versi (seperti `GET /destinations/2`) dilayani berdasarkan header `x-api-version` bernilai `1` atau `2`.
3. **GraphQL (`/graphql`)**
   - Menggunakan satu endpoint `POST /graphql` tanpa nomor versi (evolusi schema menggunakan direktif `@deprecated`).

## Jadwal Berakhirnya Layanan v1 (Deprecation & Sunset)
- **API v1** berstatus *deprecated* dan akan dimatikan secara permanen pada **`2027-06-30T00:00:00Z`** (`Wed, 30 Jun 2027 00:00:00 GMT`), sebagaimana tercantum pada header HTTP `Sunset` di setiap response `v1`. Client diimbau segera bermigrasi ke **API v2**.
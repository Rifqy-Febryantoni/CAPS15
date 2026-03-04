## Dependencies
- Docker
- Docker Compose
- Git

## Instalasi Lokal

1. Copy file environment dan sesuaikan dengan konfigurasi lokal masing-masing:
   ```bash
   cp .env.example .env
   ```
2. Set Up Docker Compose:
   ```bash
   docker-compose up -d
   ```
3. Cek apakah container berjalan:
   ```bash
   docker-compose ps
   ```

4. **Akses Internal Team**:
   - Frontend Mock: `http://<IP-SERVER>:8080`
   - Backend Mock: `http://<IP-SERVER>:8080/api/`

## Workflow Git & Standar Operasional

### Secrets
Dilarang memasukkan password database, API Key, atau token apapun ke dalam Git!
Semua *secret* murni hanya boleh disimpan di file `.env` di masing-masing laptop developer (file ini sudah di-ignore secara global melalui `.gitignore`).

### Branching Git
- `main`: Lingkungan Production. Branch ini protected dan tidak boleh direct push.
- `development`: Branch utama untuk proses development tim.
- `feature/<nama-fitur>` atau `fix/<nama-bug>`: Branch team dev bekerja.

### Alur Kerja Developer

Karena branch `main` dan `development` terproteksi, semua anggota tim Wajib mengikuti flow ini:

1. **Pastikan Selalu Update**:
   Sebelum mulai ngoding fitur baru, pastikan berada di branch `development` dan pull kode terbaru dari server:
   ```bash
   git checkout development
   git pull origin development
   ```
2. **Buat Branch Khusus sesuai task**:
   Buat cabang baru berdasarkan nama fitur:
   ```bash
   git checkout -b feature/nama-fitur-anda
   # atau untuk perbaikan bug:
   git checkout -b fix/nama-bug-anda
   ```
3. **Ngoding & Commit**:
   Lakukan perubahan pada file *Frontend* atau *Backend*. Jika sudah selesai lakukan (*commit*):
   ```bash
   git add .
   git commit -m "feat: Niat Baik"
   ```
4. **Push ke GitHub**:
   push *branch* ke server GitHub:
   ```bash
   git push origin feature/nama-fitur
   ```
5. **Buat Pull Request (PR)**:
   - Buka website Github Repositori CAPS15.
   - Klik tombol hijau **Compare & pull request** yang muncul.
   - Arahkan target merge: `feature/nama-fitur-anda` ➔ ke ➔ `development`.
   - Minta setidaknya satu anggota tim lain (reviewer) untuk memeriksa kode (*Approve*).
   - Setelah di-ACC, klik **Merge Pull Request**.

6. **Hapus Branch Lama (Opsional)**:
   Agar rapi, hapus branch lokal yang sudah selesai:
   ```bash
   git checkout development
   git branch -d feature/nama-fitur
   ```
   Lalu ulangi lagi pekerjaan dari Langkah 1 untuk mengambil fitur yang sudah merged.

# Info

## Arsitektur Testing Server (Docker & Nginx)

1. **Docker Compose (`docker-compose.yml`)**:
   *Docker Compose* digunakan untuk kontenerisasi kode Frontend dan Backend ke dalam "kontainer" yang masing-masing jalan di jaringan `gis_network`. 
   Dengan Docker, tim Frontend gak perlu ribet instal backend stack di PC mereka untuk mengetes Endpoint, begitupun sebaliknya, cukup jalankan `docker-compose up -d`.

2. **Nginx Reverse Proxy (`nginx/nginx.conf`)**:
   Karena Frontend dan Backend jalan di `gis_network`, kita memerlukan **Gateway** agar _browser_ bisa mengakses mereka berdua dengan satu IP dan satu Port saja.
   Inilah tugas container Nginx. Nginx menyala pada port **8080** dan bertugas mengatur (*traffic router*):
   - Jika ada akses ke URL utama `/` (contoh: `http://<IP-SERVER>:8080`), Nginx secara otomatis meneruskannya ke dalam kontainer **Frontend**.
   - Jika ada akses ke endpoint `/api/`, Nginx meneruskannya ke dalam kontainer **Backend**.
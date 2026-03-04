# Cesium Logistic Platform

Misi: Membangun platform Peta Digital 3D (berbasis Cesium) untuk memvisualisasikan rantai pasok kelapa di Indonesia guna menekan biaya logistik.

## Persyaratan
- Docker
- Docker Compose
- Git

## Instalasi Lokal

1. Copy file environment dan sesuaikan dengan konfigurasi mesin lokal masing-masing:
   ```bash
   cp .env.example .env
   ```
2. Nyalakan layanan melalui Docker Compose:
   ```bash
   docker-compose up -d
   ```
   *Catatan: Saat ini container backend & frontend hanya berupa placeholder shell yang terus menyala untuk menjaga jaringan tetap stabil sembari menunggu kepastian tech stack dari masing-masing tim.*

3. Cek apakah container berjalan:
   ```bash
   docker-compose ps
   ```

## Workflow Git & Standar Operasional

### Manajemen Rahasia (Secrets)
**DILARANG KERAS** memasukkan password database, API Key, atau token apapun ke dalam Git!
Semua *secret* murni hanya boleh disimpan di file `.env` di masing-masing laptop developer (file ini sudah di-ignore secara global melalui `.gitignore`).

### Branching Git
- `main`: Lingkungan Production. Branch ini sifatnya suci (protected) dan tidak boleh ada direct push.
- `development`: Cabang utama untuk proses development tim.
- `feature/<nama-fitur>` atau `fix/<nama-bug>`: Cabang tempat masing-masing developer bekerja.

### Pull Request (PR)
Ikuti prosedur Pull Request (PR) ke branch `development` sebelum kode dapat digabungkan. Wajib melewati tahapan Code Review.

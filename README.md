## Dependencies
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

### Pull Request (PR)
Ikuti prosedur Pull Request (PR) ke branch `development` sebelum kode dapat digabungkan. Wajib melewati tahapan Code Review.

# Eventku

Eventku adalah aplikasi Laravel 13 untuk pengelolaan event, tiket gratis digital, QR check-in, dan laporan penyelenggara.

## Prasyarat

- PHP 8.3+ (teruji pada PHP 8.4.25)
- Composer 2+
- Node.js 20+ dan npm
- MySQL 8+/MariaDB melalui XAMPP (development lokal juga dapat memakai SQLite)

## Instalasi XAMPP

```powershell
cd C:\xampp\htdocs\Eventku
copy .env.example .env
php artisan key:generate
```

Buat database `eventku` melalui phpMyAdmin, lalu isi `DB_DATABASE`, `DB_USERNAME`, dan `DB_PASSWORD` pada `.env`. Untuk database development SQLite, gunakan `DB_CONNECTION=sqlite` dan buat file `database/database.sqlite`.

```powershell
composer install
npm install
php artisan migrate --seed
php artisan storage:link
npm run dev
php artisan serve
```

Buka `http://127.0.0.1:8000`. Jika memakai Apache XAMPP, arahkan DocumentRoot/virtual host ke folder `public`, bukan root repository.

## Akun demo development

Seeder membuat akun berikut hanya untuk development lokal:

- Admin: `admin@eventku.test` / `Admin12345!`
- Organizer: `organizer@eventku.test` / `Organizer12345!`
- Participant: `participant@eventku.test` / `Participant12345!`

Ganti atau hapus akun demo sebelum deployment.

## Alur fitur

Organizer membuat draft event → mengajukan review → Admin menyetujui → event muncul di halaman publik → Participant mendaftar tiket gratis → tiket digital dan QR acak diterbitkan → Organizer melakukan check-in dengan kode manual/QR → tiket berubah menjadi `used`.

Halaman tiket memiliki tombol unduh PDF nyata (TCPDF) yang memuat QR dari kode tiket database. Halaman check-in menyediakan kamera `BarcodeDetector` dengan fallback manual; kamera production harus dijalankan melalui HTTPS.

## Testing

```powershell
php artisan test
```

Test memakai SQLite in-memory dan tidak mereset database development. Build production frontend:

```powershell
npm run build
```

## Route penting

| Area | Route | Role |
|---|---|---|
| Discovery | `/`, `/events`, `/events/{slug}` | Publik |
| Auth | `/login`, `/register` | Guest |
| Participant | `/participant/*` | Participant |
| Organizer | `/organizer/*` | Organizer |
| Admin | `/admin/*` | Admin |
| Profile | `/profile` | Semua user login |

Detail schema ada di [docs/ERD.md](docs/ERD.md), aturan bisnis di [docs/BUSINESS_RULES.md](docs/BUSINESS_RULES.md), dan arsitektur di [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

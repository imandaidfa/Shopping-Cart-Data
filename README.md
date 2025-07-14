# Shopping Cart Data

Proyek ini adalah aplikasi web sederhana yang menampilkan data keranjang belanja menggunakan **Tailwind CSS** dan **JavaScript**. Data keranjang diambil dari API publik [dummyjson.com/carts](https://dummyjson.com/carts) dan ditampilkan dalam antarmuka modern dan responsif.

## Fitur

- Menampilkan 10 keranjang belanja teratas.
- Informasi yang ditampilkan meliputi:
  - ID keranjang
  - User ID
  - Jumlah produk
  - Total harga keranjang (format Rupiah)
  - Daftar produk beserta gambar, nama, jumlah, dan total harga per produk
- Loading spinner saat data dimuat.
- Pesan error jika terjadi masalah saat mengambil data.

## Teknologi

- [Tailwind CSS](https://tailwindcss.com/) (melalui CDN)
- [Inter Font](https://fonts.google.com/specimen/Inter)
- Pure HTML & JavaScript (tanpa framework tambahan)
- Fetch API untuk mengambil data

## Cara Menjalankan

1. **Clone repository ini:**

   ```bash
   git clone https://github.com/imandaidfa/Shopping-Cart-Data.git
   ```

2. **Buka file `index.html` di browser favoritmu.**
   - Tidak diperlukan server lokal karena aplikasi hanya mengambil data dari API publik.

## Struktur File

- `index.html`  
  Halaman utama aplikasi, seluruh logika dan tampilan ada di file ini.

## Contoh Tampilan

![Contoh Tampilan](https://dummyjson.com/image/i/products/1/thumbnail.jpg)
*Gambar produk diambil langsung dari data API.*

## Catatan

- Pastikan koneksi internet aktif karena data diambil dari API eksternal dan Tailwind CSS via CDN.
- Jika API tidak bisa diakses, aplikasi akan menampilkan pesan error.

## Lisensi

Proyek ini menggunakan lisensi [MIT](LICENSE).


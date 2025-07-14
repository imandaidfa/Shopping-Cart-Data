## Dokumentasi: Mengambil Data Menggunakan Fetch API

**Fetch API** menyediakan antarmuka modern untuk mengambil sumber daya (data) secara asinkron melalui jaringan. Ini adalah pengganti yang lebih fleksibel dan kuat daripada `XMLHttpRequest` yang lebih lama, bekerja dengan konsep **Promises**.

### Kode yang Relevan dari Contoh Sebelumnya:

Berikut adalah bagian kunci dari kode JavaScript yang Anda miliki, yang bertanggung jawab untuk mengambil data dari `https://dummyjson.com/carts`:

```javascript
            /**
             * Fetches cart data from the specified URL and displays it.
             */
            async function fetchAndDisplayCarts() {
                try {
                    // 1. Tampilkan indikator loading dan sembunyikan pesan error sebelumnya
                    loadingIndicator.classList.remove('hidden');
                    errorMessage.classList.add('hidden');

                    // 2. Lakukan permintaan ke API menggunakan fetch()
                    const response = await fetch('https://dummyjson.com/carts');

                    // 3. Periksa status respons HTTP
                    if (!response.ok) {
                        // Jika status bukan 2xx, berarti ada error HTTP
                        throw new Error(`HTTP error! Status: ${response.status}`);
                    }

                    // 4. Ubah respons menjadi objek JSON
                    const data = await response.json();

                    // 5. Validasi struktur data yang diterima
                    if (!data || !Array.isArray(data.carts)) {
                        throw new Error('Format data tidak valid: properti "carts" tidak ditemukan atau bukan array.');
                    }

                    // 6. Ambil 10 item pertama (atau sesuai kebutuhan aplikasi)
                    const top10Carts = data.carts.slice(0, 10);

                    // ... (lanjutkan dengan kode untuk menampilkan 'top10Carts' di UI)

                } catch (error) {
                    // Tangani error yang terjadi selama proses fetch atau parsing
                    console.error('Error fetching or displaying carts:', error);
                    errorText.textContent = `Terjadi kesalahan saat memuat data: ${error.message}.`;
                    errorMessage.classList.remove('hidden'); // Tampilkan pesan error di UI
                    cartsContainer.innerHTML = ''; // Bersihkan konten parsial
                } finally {
                    // Selalu sembunyikan indikator loading, terlepas dari keberhasilan atau kegagalan
                    loadingIndicator.classList.add('hidden');
                }
            }
```

-----

### Penjelasan Mendalam tentang Kode:

1.  **`async function fetchAndDisplayCarts()`**

      * **`async`**: Kata kunci `async` menandai sebuah fungsi sebagai fungsi asinkron. Ini berarti fungsi tersebut akan selalu mengembalikan sebuah **Promise**. Manfaat utamanya adalah Anda bisa menggunakan kata kunci `await` di dalamnya.
      * Ini adalah praktik terbaik untuk membungkus logika pengambilan data dalam fungsi agar kode lebih terstruktur dan mudah dipanggil.

2.  **`try...catch...finally` Block**

      * **`try`**: Blok ini berisi kode yang "berisiko" dapat menyebabkan kesalahan, seperti permintaan jaringan yang bisa gagal atau parsing data yang bisa bermasalah. Jika ada kesalahan di dalam blok `try`, eksekusi akan segera berhenti di titik kesalahan dan melompat ke blok `catch`.
      * **`catch (error)`**: Jika terjadi kesalahan di blok `try`, blok `catch` akan dieksekusi. Variabel `error` akan menampung objek kesalahan, yang bisa berisi informasi detail tentang apa yang salah (misalnya, `TypeError` untuk masalah jaringan, atau `SyntaxError` untuk JSON yang tidak valid). Di sini, kami mencatat error ke konsol browser dan menampilkannya kepada pengguna.
      * **`finally`**: Blok ini **selalu** dieksekusi, tidak peduli apakah kode di blok `try` berjalan sukses atau dilemparkan kesalahan dan ditangani oleh `catch`. Ini ideal untuk membersihkan atau mereset status UI, seperti menyembunyikan indikator loading.

3.  **`loadingIndicator.classList.remove('hidden');` & `errorMessage.classList.add('hidden');`**

      * Ini adalah langkah-langkah UI untuk memberikan **umpan balik visual** kepada pengguna. Sebelum memulai pengambilan data, indikator loading ditampilkan, dan pesan error sebelumnya (jika ada) disembunyikan.

4.  **`const response = await fetch('https://dummyjson.com/carts');`**

      * **`fetch(url)`**: Ini adalah fungsi global JavaScript yang memulai permintaan HTTP untuk mengambil sumber daya. Fungsi ini mengembalikan sebuah **Promise**.
      * `'https://dummyjson.com/carts'`: Ini adalah **URL endpoint API** yang ingin kita ambil datanya.
      * **`await`**: Kata kunci `await` hanya dapat digunakan di dalam fungsi `async`. Ia "menunggu" sebuah Promise untuk diselesaikan (resolved) dan kemudian mengembalikan nilai yang telah diselesaikan tersebut. Dalam kasus ini, ia menunggu respons dari server dan kemudian menetapkan objek `Response` ke variabel `response`. Kode eksekusi JavaScript akan "berhenti" di baris ini sampai Promise dari `fetch()` selesai.

5.  **`if (!response.ok)`**

      * Objek `response` yang dikembalikan oleh `fetch()` memiliki properti `ok` (boolean).
      * `response.ok` adalah `true` jika status respons HTTP berada dalam rentang `200-299` (misalnya, 200 OK, 201 Created).
      * Jika `response.ok` adalah `false` (misalnya, 404 Not Found, 500 Internal Server Error), berarti ada masalah di sisi server atau resource tidak ditemukan. Kode ini akan melemparkan (`throw`) sebuah `Error` yang akan ditangkap oleh blok `catch`.

6.  **`const data = await response.json();`**

      * Objek `Response` tidak secara langsung memberikan data JSON. Kita harus mem-parsing body responsnya.
      * `response.json()`: Ini adalah metode dari objek `Response` yang membaca aliran respons hingga selesai. Ia mengembalikan sebuah Promise yang diselesaikan dengan hasil parsing body teks sebagai JSON.
      * **`await`**: Menunggu Promise dari `response.json()` selesai, lalu menetapkan objek JavaScript yang dihasilkan (dari parsing JSON) ke variabel `data`.

7.  **`if (!data || !Array.isArray(data.carts))`**

      * Ini adalah **validasi data** penting. Setelah berhasil mem-parsing JSON, kita perlu memastikan bahwa struktur data yang kita terima sesuai dengan harapan.
      * `!data`: Memeriksa apakah `data` tidak kosong (misalnya, `null` atau `undefined`).
      * `!Array.isArray(data.carts)`: Memeriksa apakah `data` memiliki properti bernama `carts` dan apakah properti tersebut merupakan sebuah array. Ini spesifik untuk API `dummyjson.com/carts` yang mengembalikan data dalam objek dengan properti `carts`. Jika validasi ini gagal, sebuah `Error` dilemparkan.

8.  **`const top10Carts = data.carts.slice(0, 10);`**

      * Setelah data berhasil diambil dan divalidasi, baris ini mengambil hanya 10 elemen pertama dari array `carts`. Ini adalah contoh bagaimana Anda bisa memproses data setelah diambil, sesuai kebutuhan aplikasi Anda.

-----

### Kesimpulan:

`Fetch API` yang dikombinasikan dengan `async/await` dan `try...catch...finally` menyediakan cara yang bersih dan efisien untuk melakukan permintaan jaringan dan menangani responsnya, termasuk penanganan error dan pembaruan UI. Ini adalah pendekatan standar dalam pengembangan web modern untuk interaksi dengan API.

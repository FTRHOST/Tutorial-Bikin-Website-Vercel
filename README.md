-----

# Tutorial: Dari Poster ke Website (AI Studio + Vite + Vercel)

Tutorial ini akan memandu kamu membuat website React berbasis poster menggunakan AI, melakukan konfigurasi khusus pada `index.html` dan `vite.config.js`, lalu mengunggahnya ke Vercel.

### Prasyarat

  * Akun Google (untuk AI Studio).
  * Akun GitHub dan Vercel.

-----

## Langkah 1: Generate Kode di Google AI Studio

1.  Buka **[aistudio.google.com/apps](https://aistudio.google.com/apps)**.
2.  **Upload Poster** desain website kamu ke dalam chat.
3.  Masukkan prompt berikut:
    > "Tolong buatkan landing page yang persis seperti desain pada poster ini. Buat komponennya modular."
4. Jika masih kurang perbaiki di promptnya.

## Langkah 2: Konfigurasi index.html

Sesuai permintaanmu, kita akan memodifikasi file `index.html` yang berada di root folder project.

1.  Buka file `index.html`.
2.  Cari bagian `<body>`.
3.  Tambahkan atau ubah script tag menjadi seperti di bawah ini (pastikan path-nya mengarah ke `/index.tsx`):

<!-- end list -->

```html
<!doctype html>
<html lang="en">
  <head>
    </head>
  <body>
    <div id="root"></div>
    <script src="/index.tsx" type="module"></script>
  </body>
</html>
```
copy ini:
```html
    <script src="/index.tsx" type="module"></script>
```

*Catatan: Karena script menunjuk ke `/index.tsx`, pastikan file entry point kamu di folder `src` (atau root, tergantung struktur kamu) bernama `index.tsx`. Jika default Vite adalah `main.tsx`, silakan rename filenya menjadi `index.tsx` dan pindahkan ke lokasi yang sesuai jika perlu.*

-----

## Langkah 2: Membuat File Konfigurasi Vite

Ini adalah langkah krusial untuk memasukkan konfigurasi env dan path yang kamu berikan.

1.  Di root folder project, buka (atau buat) file bernama **`vite.config.js`** (atau `vite.config.ts`).
2.  Hapus isinya dan tempelkan kode berikut:

<!-- end list -->

```javascript
import path from 'path';
import { defineConfig, loadEnv } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig(({ mode }) => {
    const env = loadEnv(mode, '.', '');
    return {
      server: {
        host: true,
      },
      plugins: [react()],
      define: {
        'process.env.API_KEY': JSON.stringify(env.GEMINI_API_KEY),
        'process.env.GEMINI_API_KEY': JSON.stringify(env.GEMINI_API_KEY)
      },
      resolve: {
        alias: {
          '@': path.resolve(__dirname, '.'),
        }
      }
    };
});
```

**Penting:** Karena konfigurasi ini membaca `env.GEMINI_API_KEY`, pastikan kamu membuat file `.env` di root folder (jangan di-commit ke Git) dan isi dengan:

```env
GEMINI_API_KEY=isi_api_key_kamu_disini
```

-----

## Langkah 3: Upload dan Deploy ke Vercel

Untuk "mengupload file konfigurasi" agar berjalan di Vercel, cara terbaik adalah melalui integrasi Git.

### A. Push ke Github
1. klik icon github disebelah kanan atas.
2. silahkan login dulu.
3. lalu buatlah project.
4. masukan nama dan deskripsi project.
5. buat repository menjadi public.
6. terakhir, klik commit.
### B. Deploy di Vercel

1.  Buka dashboard **[Vercel](https://vercel.com)**.
2. Jangan lupa sigup mengguankan github.
2.  Klik **"Add New..."** -\> **"Project"**.
3.  Import repository GitHub yang baru saja kamu push.
4.  Pada bagian **Environment Variables**, ini langkah yang sangat penting agar konfigurasi `vite.config.js` kamu berjalan:
      * **Key:** `GEMINI_API_KEY`
      * **Value:** (Masukkan API Key Google Gemini kamu)
5.  Klik **Deploy**.

Vercel akan membaca `vite.config.js` kamu, mengenali variabel lingkungan yang baru saja dimasukkan, dan membangun website tersebut.

-----

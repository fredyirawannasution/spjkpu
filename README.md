# SPJ KPU Labuhanbatu Utara - PWA GitHub / Cloudflare

Paket ini membuat PWA wrapper untuk aplikasi Google Apps Script. `code.gs` dan `index.html` tetap dipasang di Apps Script. GitHub/Cloudflare hanya menyajikan shell PWA, manifest, service worker, dan iframe ke URL Web App Apps Script.

## Struktur

```text
appscript/
  code.gs
  index.html

github-cloudflare/
  index.html
  config.js
  manifest.webmanifest
  sw.js
  _headers
  assets/icons/icon-192.png
  assets/icons/icon-512.png
```

## 1. Pasang Apps Script

1. Buka https://script.google.com/.
2. Buat project baru.
3. Buat file `code.gs`, salin isi dari folder `appscript/code.gs`.
4. Buat file HTML bernama `index`, salin isi dari folder `appscript/index.html`.
5. Simpan project.
6. Jalankan fungsi `setupApp()` dari editor Apps Script.
7. Berikan izin akses Spreadsheet, Drive, Mail, dan layanan terkait.
8. Deploy > New deployment > Web app.
9. Execute as: `Me`.
10. Who has access: pilih sesuai kebutuhan. Untuk PWA umum biasanya `Anyone with the link`, tetapi login aplikasi tetap dikendalikan oleh sistem user internal.
11. Salin URL Web App yang berakhiran `/exec`.

## 2. Atur PWA

Edit `github-cloudflare/config.js`:

```js
window.SPJ_APP_CONFIG = {
  APPS_SCRIPT_URL: 'https://script.google.com/macros/s/AKfycbxxxxxxxxxxxxxxxxxxxxxxxx/exec',
  APP_NAME: 'SPJ KPU Labuhanbatu Utara'
};
```

## 3. Upload ke GitHub

```bash
cd github-cloudflare
git init
git add .
git commit -m "Initial SPJ PWA"
git branch -M main
git remote add origin https://github.com/USERNAME/NAMA-REPO.git
git push -u origin main
```

## 4. Deploy ke Cloudflare Pages dari GitHub

1. Buka Cloudflare Dashboard.
2. Workers & Pages > Create application > Pages > Connect to Git.
3. Pilih repository GitHub.
4. Framework preset: `None`.
5. Build command: kosongkan.
6. Build output directory: `/`.
7. Deploy.

Jika Cloudflare meminta output folder, gunakan root repository karena ini static site murni.

## 5. Alternatif upload langsung ke Cloudflare Pages

1. ZIP isi folder `github-cloudflare`, bukan folder induknya.
2. Cloudflare Pages > Create application > Upload assets.
3. Upload ZIP tersebut.

## 6. Catatan penting

- PWA cache hanya menyimpan shell PWA, bukan data SPJ dari Apps Script.
- Apps Script harus memakai `setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)` agar bisa tampil di iframe. File `code.gs` sudah memakainya.
- Jika halaman hanya menampilkan pesan URL belum diatur, cek kembali `config.js`.
- Jika iframe kosong, cek deployment Apps Script sudah Web App `/exec`, bukan `/dev`.
- Setelah update Apps Script, lakukan Deploy > Manage deployments > Edit > New version.

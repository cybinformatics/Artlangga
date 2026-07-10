# Artlangga

**Karya teman, untukmu.** — Art market online khusus civitas Universitas Airlangga (UNAIR). Workshop UI Project individu, dibuat from scratch pakai HTML + CSS + JavaScript vanilla (no framework, no build step).

## Web vs Mobile App — dua pola navigasi, satu konsep

Project ini punya **dua folder terpisah** yang berbagi desain & data yang sama persis (brand token, produk, artikel, dst di `html/js/data.js`), tapi beda pola navigasi sesuai konvensi platform:

| | `html/` (Web) | `mobile-app/` (App) |
|---|---|---|
| Navigasi | Navbar atas + footer, hamburger di mobile-web | **Bottom tab bar** (Beranda · Katalog · Keranjang · Akun) + top app-bar dengan tombol back |
| Cakupan halaman | Semua 13 halaman brief (buyer + seller + admin) | Alur utama buyer: Splash → Onboarding → Login/Register → Beranda/Katalog → Detail Produk → Keranjang → Checkout |
| Breakpoint | Desktop-first, responsive turun ke 320px | Mobile-only, 1 lebar frame (~480px), dipreview sebagai kartu di layar besar |
| Dashboard/Kelola * (seller & admin) | ✅ ada | Belum — direncanakan menyusul, sementara diarahkan balik ke versi web dari tab Akun |

`mobile-app/*.html` me-reference CSS & JS dari `../html/css/` dan `../html/js/` langsung (bukan file duplikat) — jadi kalau data produk/artikel di `html/js/data.js` diubah, otomatis konsisten di kedua versi.

```
artlangga/
├── html/                  (Web — lihat struktur lengkap di bawah)
└── mobile-app/
    ├── splash.html, onboarding.html, login.html, register.html
    ├── home.html           (tab: Beranda)
    ├── katalog.html        (tab: Katalog)
    ├── cart.html           (tab: Keranjang)
    ├── account.html        (tab: Akun)
    ├── product-detail.html (?id=P001 — no tab bar, back button + sticky CTA)
    ├── checkout.html       (no tab bar — back button + sticky "Bayar Sekarang")
    ├── css/app-shell.css   (top app-bar, bottom tab bar — khusus app)
    └── assets/img/logo-arte.png
```

Entry point mobile app: `mobile-app/splash.html`.

## Cara menjalankan

Nggak butuh instalasi apa-apa. Buka salah satu entry point langsung di browser, atau serve foldernya pakai live server (disarankan biar path assetnya konsisten):

```bash
cd artlangga
npx serve .
# atau: python3 -m http.server 5500
```

- Web: `http://localhost:5500/html/splash.html`
- Mobile app: `http://localhost:5500/mobile-app/splash.html` (buka di DevTools mobile view atau HP asli biar pas)

### Deploy ke GitHub Pages
1. Push folder ini ke repo GitHub.
2. Settings → Pages → Source: pilih branch `main`, folder root (`/`).
3. GitHub Pages serve dari root, jadi:
   - Web: `https://<username>.github.io/<repo>/html/splash.html`
   - Mobile app: `https://<username>.github.io/<repo>/mobile-app/splash.html`
   - (Opsional) bikin `index.html` kecil di root yang redirect/link ke keduanya biar gampang diakses dosen.

## Alur halaman

```
splash.html → onboarding.html → register.html / login.html
                                        │
                                        ▼
index.html (Landing) ⇄ arsip-artikel.html ⇄ detail-artikel.html
        │
        ▼
katalog-produk.html → detail-produk.html → keranjang.html → pembayaran.html
        │                                                        │
        ▼                                                        ▼
history-transaksi.html                              dashboard.html (shared seller/admin)
                                                              │
                                     kelola-artikel / kelola-produk / kelola-pengguna* / kelola-transaksi
```
`*` Kelola Pengguna khusus admin (link disembunyikan otomatis kalau role = seller).

## Struktur folder

```
artlangga/
└── html/
    ├── splash.html, onboarding.html, register.html, login.html
    ├── index.html                     (Landing Page)
    ├── arsip-artikel.html             (list + filter + search)
    ├── detail-artikel.html            (?id=A001)
    ├── katalog-produk.html            (grid + filter + sort)
    ├── detail-produk.html             (?id=P001 — galeri, tab ulasan, add to cart)
    ├── keranjang.html                 (qty & total dinamis, localStorage)
    ├── pembayaran.html                (checkout, QRIS/e-wallet/VA — digital only)
    ├── history-transaksi.html         (filter status)
    ├── dashboard.html                 (shared: seller ⇄ admin, role switcher)
    ├── kelola-artikel.html            (CRUD)
    ├── kelola-produk.html             (CRUD + upload foto)
    ├── kelola-pengguna.html           (verifikasi KTM, suspend — admin)
    ├── kelola-transaksi.html          (update status)
    ├── css/
    │   ├── style.css                  (variables, reset, typography)
    │   ├── components.css             (navbar, card, button, table, modal, sidebar dashboard)
    │   ├── onboarding.css             (splash/onboarding/auth screens)
    │   └── responsive.css             (breakpoint 768px & 480px)
    ├── js/
    │   ├── data.js                    (dummy data: produk, artikel, user, transaksi)
    │   ├── auth.js                    (mock role: buyer/seller/admin, localStorage)
    │   ├── cart.js                    (keranjang, localStorage)
    │   └── main.js                    (navbar hamburger, active link, toast, splash redirect)
    └── assets/img/logo-arte.png
```

## Cara demo Dashboard shared page (seller vs admin)

Nggak ada backend beneran, jadi role di-mock lewat localStorage:
- Dari `login.html`, klik tombol **"Seller demo"** atau **"Admin demo"** → langsung masuk ke `dashboard.html` dengan role sesuai.
- Atau di `dashboard.html`, pakai dropdown **"Lihat sebagai"** di kanan atas buat switch on-the-fly.
- Link **Kelola Pengguna** di sidebar cuma muncul kalau role = admin.

## Data & state

- Semua dummy data (produk, artikel, user, transaksi, review) ada di `js/data.js` — realistis, bukan lorem ipsum.
- Keranjang, role aktif, dan hasil CRUD di halaman Kelola * disimpan di `localStorage` browser (client-side only, sesuai scope workshop UI — belum ada backend/database).
- Checkout di `pembayaran.html` bikin order baru yang otomatis muncul di `history-transaksi.html` dan `kelola-transaksi.html`.

## Brand tokens

| Token | Value |
|---|---|
| Primary (biru) | `#4a86e8` |
| Primary dark | `#2f4f9e` |
| Accent (kuning) | `#ffd23f` |
| Background | `#f7f6f1` |
| Font heading | Baloo 2 |
| Font body | DM Sans |
| Radius card / button | 16px / pill (999px) |

## Belum termasuk (di luar scope UI workshop)
- Backend/API & database beneran (semua CRUD & auth masih mock/localStorage)
- Integrasi payment gateway Midtrans yang sesungguhnya
- Upload foto produk permanen ke server (preview lokal only, per sesi browser)

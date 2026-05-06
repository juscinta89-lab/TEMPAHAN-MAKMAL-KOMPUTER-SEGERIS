# 🚀 Panduan Setup – Sistem Tempahan Makmal Komputer SK Kuala Geris

## 📁 Struktur Fail

```
pwa-makmal/
├── index.html              ← Aplikasi utama
├── manifest.json           ← Konfigurasi PWA
├── sw.js                   ← Service Worker (offline support)
├── firestore.rules         ← Peraturan keselamatan Firebase
├── generate-icons.html     ← Jana ikon PWA
├── icons/                  ← Ikon aplikasi (perlu dijana)
│   ├── icon-72.png
│   ├── icon-96.png
│   ├── icon-128.png
│   ├── icon-144.png
│   ├── icon-152.png
│   ├── icon-192.png
│   ├── icon-384.png
│   └── icon-512.png
├── .github/
│   └── workflows/
│       └── deploy.yml      ← Auto-deploy ke GitHub Pages
└── SETUP.md                ← Panduan ini
```

---

## 🔥 LANGKAH 1: Setup Firebase

### 1.1 – Buat Projek Firebase

1. Pergi ke **https://console.firebase.google.com**
2. Klik **"Add project"** atau **"Tambah projek"**
3. Nama projek: `makmal-skkg` (atau nama lain)
4. **Disable Google Analytics** (tidak diperlukan)
5. Klik **"Create project"**

---

### 1.2 – Aktifkan Firestore Database

1. Dalam Firebase Console, klik **"Firestore Database"** (menu kiri)
2. Klik **"Create database"**
3. Pilih **"Start in test mode"** → Klik Next
4. Pilih region: **`asia-southeast1` (Singapore)** → Klik Enable
5. Tunggu beberapa saat untuk database dibuat

---

### 1.3 – Dapatkan Firebase Config

1. Klik ⚙️ **"Project Settings"** (ikon gear di atas menu kiri)
2. Scroll ke bawah ke **"Your apps"**
3. Klik ikon **`</>`** (Web app)
4. Register nama app: `Makmal SK Kuala Geris`
5. **JANGAN** tick "Firebase Hosting"
6. Klik **"Register app"**
7. Anda akan dapat config seperti ini:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "makmal-skkg.firebaseapp.com",
  projectId: "makmal-skkg",
  storageBucket: "makmal-skkg.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef1234567890"
};
```

8. **Salin config ini** – akan digunakan dalam langkah seterusnya

---

### 1.4 – Kemaskini Firebase Config dalam index.html

Buka `index.html` dan cari bahagian ini (baris ~20):

```javascript
const firebaseConfig = {
  apiKey: "GANTI_DENGAN_API_KEY_ANDA",
  authDomain: "GANTI_DENGAN_AUTH_DOMAIN_ANDA",
  projectId: "GANTI_DENGAN_PROJECT_ID_ANDA",
  storageBucket: "GANTI_DENGAN_STORAGE_BUCKET_ANDA",
  messagingSenderId: "GANTI_DENGAN_MESSAGING_SENDER_ID_ANDA",
  appId: "GANTI_DENGAN_APP_ID_ANDA"
};
```

**Gantikan** dengan config yang anda dapat dari Firebase Console.

---

### 1.5 – Setup Firestore Security Rules

1. Dalam Firebase Console → **Firestore Database** → **Rules**
2. Salin kandungan fail `firestore.rules` dan tampal di sana
3. Klik **"Publish"**

Atau untuk test mudah (kurang selamat):
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

---

## 📁 LANGKAH 2: Jana Ikon PWA

1. Buka `generate-icons.html` dalam browser
2. Klik butang **"Jana Semua Ikon"**
3. Download setiap ikon
4. Buat folder `icons/` dalam projek anda
5. Simpan semua ikon ke dalam folder `icons/`

---

## 🐙 LANGKAH 3: Upload ke GitHub

### 3.1 – Buat Repository GitHub

1. Pergi ke **https://github.com**
2. Klik **"New"** untuk buat repository baru
3. Nama: `makmal-komputer-skkg`
4. Pastikan **Public** (untuk GitHub Pages percuma)
5. Klik **"Create repository"**

---

### 3.2 – Upload Fail

**Cara mudah (drag & drop):**
1. Dalam repository, klik **"uploading an existing file"**
2. Drag semua fail ke sana:
   - `index.html`
   - `manifest.json`
   - `sw.js`
   - Folder `icons/`
3. Klik **"Commit changes"**

**Cara terminal:**
```bash
cd pwa-makmal
git init
git add .
git commit -m "Initial commit - Sistem Tempahan Makmal"
git branch -M main
git remote add origin https://github.com/USERNAME/makmal-komputer-skkg.git
git push -u origin main
```

---

## 🌐 LANGKAH 4: Aktifkan GitHub Pages

1. Dalam repository GitHub → **Settings** → **Pages**
2. Source: **"GitHub Actions"**
3. Pergi ke tab **"Actions"**
4. Tunggu workflow **"Deploy PWA ke GitHub Pages"** selesai (1-2 minit)
5. URL aplikasi anda: `https://USERNAME.github.io/makmal-komputer-skkg`

---

## ✅ LANGKAH 5: Test Aplikasi

1. Buka URL GitHub Pages anda
2. Cuba daftar akaun guru baru
3. Log masuk
4. Cuba buat tempahan slot
5. Test Mode Admin: username `admin`, password `admin123`

### Test PWA Install:
- **Chrome/Edge**: Klik ikon install di address bar
- **Android**: Klik banner "Tambah ke skrin utama"
- **iOS Safari**: Share → "Add to Home Screen"

---

## 🔧 LANGKAH 6: Tukar Password Admin

Dalam `index.html`, cari:
```javascript
if (u === 'admin' && p === 'admin123') {
```

Tukar `admin123` kepada password yang lebih selamat.

---

## 📱 Ciri-ciri PWA

- ✅ **Boleh dipasang** seperti app native
- ✅ **Real-time sync** – semua guru nampak perubahan segera
- ✅ **Offline support** – boleh guna tanpa internet (data di-cache)
- ✅ **Auto-update** – update automatik apabila push ke GitHub
- ✅ **Cross-platform** – Android, iOS, Windows, Mac

---

## ❓ Soalan Lazim

**Q: Data hilang bila tutup browser?**
A: Tidak! Data disimpan dalam Firebase Firestore (cloud).

**Q: Boleh guna di telefon?**
A: Ya, ia PWA – boleh pasang di Android dan iPhone.

**Q: Bila update kod, pengguna perlu reload?**
A: Service Worker akan update secara automatik dalam masa beberapa minit.

**Q: Bagaimana nak tambah kelas baru?**
A: Cari `<option>1 ALPHA</option>` dalam `index.html` dan tambah kelas baru.

**Q: Bagaimana nak tambah slot waktu baru?**
A: Cari `const SLOTS = [` dalam script dan tambah slot baru.

---

## 🆘 Hubungi Sokongan

Jika ada masalah, semak:
1. Firebase Console → Firestore → Data (pastikan data masuk)
2. Browser Console (F12) → Errors
3. GitHub Actions → Workflow logs (untuk deploy errors)

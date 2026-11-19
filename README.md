# PROJECT KOKURIKULER 3D – PEMILAHAN SAMPAH

Mini game edukasi 3D (HTML + CSS + JavaScript + Three.js).
Pengguna masuk ke lingkungan sekolah 3D, mengambil sampah, lalu memasukkannya ke salah satu dari 5 tempat sampah.

> 5 JENIS SAMPAH, 5 TEMPAT, 1 LINGKUNGAN YANG LEBIH BERSIH.

## Cara menjalankan

Three.js dimuat dari CDN (jsDelivr), jadi **perangkat perlu terhubung internet saat pertama kali dibuka**.

**Opsi A – paling mudah (klik dua kali):**
Buka `index-standalone.html` di Chrome / Edge / Firefox. Semua kode sudah digabung dalam satu file.

**Opsi B – lewat server lokal (untuk `index.html` + file terpisah):**
Browser memblokir file JavaScript modul jika dibuka langsung dari `file://`, jadi gunakan server lokal:

```bash
# di dalam folder project
python -m http.server 8000
# lalu buka http://localhost:8000
```

Alternatif: ekstensi *Live Server* di VS Code, atau unggah ke GitHub Pages / Netlify / Vercel.

## Kontrol

| Aksi | Desktop | Mobile |
|---|---|---|
| Putar kamera | Klik + drag di area kosong | Geser 1 jari |
| Zoom | Scroll | Cubit 2 jari |
| Bergerak | W A S D / tombol panah | Tombol ▲ ◀ ▼ ▶ |
| Geser kamera | Klik kanan + drag | Geser 2 jari |
| Ambil & pindahkan sampah | Tekan, tahan, seret | Sentuh, tahan, seret |
| Reset kamera | Tombol **RESET CAMERA** | sama |

## Fitur

- 5 tempat sampah 3D (body, tutup berlubang, flap berengsel, label melengkung, bayangan) dengan animasi masuk yang berbeda tiap kategori.
- 25 sampah 3D low-poly (5 per kategori), posisi diacak setiap permainan.
- Lingkungan sekolah: gedung, jalan setapak, rumput bergoyang, pohon, tanaman, bangku, lampu taman, bendera, awan, papan informasi 3D.
- Hover (outline + membesar + cursor pointer), pickup, drag, drop dengan lintasan melengkung.
- Feedback benar (partikel, centang 3D, +10 poin) dan salah (shake, tanda X, sampah kembali).
- HUD skor / benar / salah / sisa, petunjuk, reset, **Mode Presentasi** (kamera sinematik 5 kategori), layar selesai dengan akurasi.
- Efek suara dibuat dengan WebAudio (tanpa file audio). Tombol 🔊 untuk mute.

## Struktur file

```
index.html
style.css
script.js
index-standalone.html   (versi satu file, siap klik dua kali)
assets/
  models/    (opsional: model .glb)
  textures/
  icons/
  sounds/
```

Semua model dibuat secara procedural di `script.js` sehingga project langsung jalan tanpa file aset dan tetap ringan.

## Mengganti model dengan GLB (opsional)

Di `script.js`, isi `GLB_OVERRIDES` (butuh server, Opsi B):

```js
const GLB_OVERRIDES = {
  apel: 'assets/models/apel.glb',
  kaleng: 'assets/models/kaleng.glb',
};
```

Kunci yang tersedia: `kulit_pisang, apel, daun, sayuran, sisa_makanan, botol_plastik, kaleng, kemasan_plastik, botol_minuman, wadah_plastik, koran, kardus, majalah, kertas, buku, baterai, lampu_neon, obat, bahan_kimia, limbah_medis, tisu, popok, pembalut, puntung, sampah_sisa`.
Model otomatis diskalakan dan diletakkan di lantai. Jika file tidak ditemukan, model bawaan dipakai.

## Menambah / mengubah sampah

Tambahkan satu baris di `ITEM_DEFS` dan satu fungsi pembuat model di objek `BUILD`.

## Optimasi

Pixel ratio dibatasi, shadow map 2048 (1024 di perangkat sentuh), rumput memakai `InstancedMesh`, jendela instancing, partikel satu `Points`, satu lampu efek bersama, dan tanpa tekstur besar (semua tekstur kecil dibuat dengan canvas).

# Tracing the Past — WebGIS

Peta interaktif Leaflet + OpenStreetMap untuk memetakan bangunan/situs bersejarah
di Jawa (dan Bali) yang hilang, roboh, atau tinggal reruntuhan.

## Cara deploy ke GitHub Pages
1. Buat repo baru di GitHub, misal `tracing-the-past-webgis`.
2. Upload SEMUA file di folder ini (`index.html` dan `data.js`) ke root repo tsb.
3. Buka Settings -> Pages pada repo.
4. Pada bagian Source, pilih branch `main` dan folder `/ (root)`, lalu Save.
5. Tunggu 1-2 menit, link situs akan muncul: `https://<username>.github.io/tracing-the-past-webgis/`

## Menambahkan foto dulu vs sekarang (slider before-after)
Setiap objek di `data.js` sekarang punya 2 field kosong: `foto_lama` dan `foto_sekarang`.
Isi dengan URL gambar langsung (harus berakhiran .jpg/.png, bukan link halaman biasa):

```js
{ ..., "foto_lama": "https://upload.wikimedia.org/.../gedung_lama.jpg",
       "foto_sekarang": "https://upload.wikimedia.org/.../kondisi_sekarang.jpg" }
```

- Isi keduanya -> muncul slider geser bandingkan dulu/sekarang di popup.
- Isi salah satu saja -> muncul 1 foto tanpa slider.
- Kosongkan keduanya -> muncul placeholder "foto belum ditambahkan".

**Cara dapat URL gambar langsung**, klik kanan gambar di browser -> "Copy image address"
(bukan "Copy link"/URL halaman).

**Rekomendasi sumber foto yang aman hak cipta:**
- Foto/lukisan/peta lama: [Wikimedia Commons](https://commons.wikimedia.org) (cari nama
  bangunan + tahun), banyak koleksi Tropenmuseum/KITLV Leiden/Nationaal Archief yang berstatus
  domain publik atau CC-BY-SA -- wajib cantumkan sumber di kolom `sumber` bila memakainya.
- Foto kondisi sekarang: hasil jepretan sendiri (unggah ke folder `img/` repo ini lalu
  isi field dengan path relatif, misal `"foto_sekarang": "img/kartasura_2024.jpg"`), atau
  akun resmi BPCB/Dinas Kebudayaan setempat (minta izin/beri kredit).

Karena verifikasi keaslian foto per situs butuh pengecekan manual, isian ini sengaja
dikosongkan dulu -- silakan lengkapi bertahap, atau minta bantuan Claude cari kandidat
foto untuk situs tertentu yang kamu sebutkan namanya.

## Cara update data
Edit isi array di `data.js` (atau generate ulang dari CSV kamu), lalu commit & push
perubahan -- GitHub Pages otomatis re-deploy.

## Menambah layer batas administrasi (opsional, dari SHP)
1. Buka SHP di QGIS, pastikan CRS = EPSG:4326 (reproject bila perlu).
2. Layer > Export > Save Features As... > format GeoJSON > simpan sebagai `batas.geojson`
   di folder ini.
3. Tambahkan di index.html sebelum baris `DATA.forEach`:
   fetch('batas.geojson').then(r=>r.json()).then(geo=>{
     L.geoJSON(geo, {style:{color:'#555', weight:1, fillOpacity:0.05}}).addTo(map);
   });

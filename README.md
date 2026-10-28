## DylomAdmin v2.3.0

### Command baru
**Karakter (Mod)**
- `;size <pemain> <ukuran>` / `;unsize` — ubah ukuran karakter (0.2 sampai 10)
- `;bighead <pemain> [ukuran]` / `;unbighead` — perbesar kepala (bawaan 2x)
- `;fire`, `;light`, `;smoke`, `;neon` `[pemain] [warna]` (+ `;unfire`, `;unlight`, `;unsmoke`, `;unneon`)
- `;ice [pemain]` / `;unice` — badan jadi es, masih bisa bergerak
- `;flatten [pemain] [tebal]` / `;unflatten` — badan gepeng seperti kertas (R15)
- `;terminal` — jendela untuk menjalankan command & melihat hasilnya

**Server (Admin)**
- `;disco` / `;undisco` — lampu satu server berganti warna
- `;music <id> [speed]` / `;stopmusic` — musik satu server
- `;vote <pemain> <pilihan,pilihan> <pertanyaan>` — voting 30 detik dengan grafik hasil
- `;viewguis <pemain>` / `;unviewguis` — lihat GUI di layar pemain

### Perbaikan
- Petunjuk & autocomplete kini menampilkan nama argumen yang jelas, misalnya `;music <id> [speed]` dan `;speed <pemain> <kecepatan>`, bukan `<angka>`
- Addon bisa memberi nama argumen sendiri lewat `labels` (lihat `_ContohAddon`)

### Cara update
Ganti folder **`Loader`** di dalam DylomAdmin dengan yang baru. Folder **`Config`** (pengaturanmu) tidak perlu diubah.

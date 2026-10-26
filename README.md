# DylomAdmin

**Admin commands gratis untuk Roblox.** Kotak command dengan autocomplete, panel admin, efek hukuman lengkap dengan sound, ban untuk pemain offline, dan semuanya bisa diatur tanpa menyentuh script.

> DylomAdmin **gratis untuk semua** dan akan tetap gratis.

**Versi terbaru:** v2.1.1 · 52 command · PC & HP

---

## Daftar isi
- [Fitur](#fitur)
- [Cara pasang](#cara-pasang)
- [Struktur folder](#struktur-folder)
- [Pengaturan](#pengaturan)
- [Rank & admin](#rank--admin)
- [Cara memakai](#cara-memakai)
- [Daftar command](#daftar-command)
- [Panel admin](#panel-admin)
- [Membuat command sendiri (Addons)](#membuat-command-sendiri-addons)
- [Cara update](#cara-update)
- [Keamanan](#keamanan)
- [Masalah umum](#masalah-umum)
- [Kredit](#kredit)

---

## Fitur

- **Kotak command + autocomplete.** Ketik `fre` lalu muncul saran `freeze <pemain>`. Tekan **Tab** di PC atau ketuk saran di HP. Nama pemain cukup ditulis sebagian.
- **Panel admin (`;panel`)** dengan tab **Akses** (beri/cabut rank), **Banned** (daftar & unban, ban pemain offline), **Role**, dan **Assets**.
- **Efek hukuman dengan sound:** freeze (balok es), jail (penjara jatuh dari langit), smite (petir), nuke, rocket, dan rope (tali, termasuk tergantung saat terbang).
- **Ban lengkap:** permanen, sementara (menit), atau hanya di server ini. Bisa untuk pemain **offline**. Memakai **Ban API resmi Roblox** dan ikut mem-ban akun alt.
- **Restart server** (`;restart`): semua pemain otomatis pindah ke server baru, bersama-sama.
- **Fly & noclip** untuk PC dan HP.
- **Pesan pribadi (`;pm`)** yang tetap tampil sampai pemain menutupnya.
- **Panel FPS** (Ping, FPS, Render) yang bisa digeser.
- **Pemain biasa juga bisa** memakai `;fps`, `;rejoin`, `;help`, `;cmds`, dan `;admins`.
- **Pengaturan lewat Properties.** Tidak ada script yang perlu diubah, jadi tidak ada risiko salah ketik yang membuat admin error.
- **Addons:** tambahkan command buatanmu sendiri tanpa mengubah mesin.
- **Notifikasi versi baru** untuk Owner.

---

## Cara pasang

1. Unduh **`DylomAdmin.rbxm`** dari halaman [Releases](../../releases).
2. Buka game-mu di Roblox Studio, lalu **drag** file tersebut ke jendela Studio.
3. Pindahkan folder **`DylomAdmin`** ke **ServerScriptService**.
4. **Publish** game-mu.

Selesai. Pembuat game otomatis menjadi **Owner**.

> **Agar ban & rank permanen bisa dites di Studio:**
> Home → **Game Settings** → **Security** → aktifkan **Enable Studio Access to API Services**.

Beberapa fitur hanya berjalan di game yang sudah dipublish (tidak di Studio): `;rejoin`, `;restart`, dan Ban API Roblox.

---

## Struktur folder

```
DylomAdmin
├─ Config  [Configuration]   ← SEMUA yang boleh kamu ubah ada di sini
│   ├─ (Attributes)          prefix, tombol, ban, batas speed/jump, radius nuke, dll
│   ├─ Settings              daftar: urutan rank, admin, grup, tombol khusus
│   ├─ Addons                command buatanmu sendiri (lihat _ContohAddon)
│   ├─ Assets                tool untuk ;give (contoh: Building Tools)
│   └─ Effects               model & sound efek (Freeze, Jail, Rocket, Sounds)
└─ Loader  [Script]          ← mesin DylomAdmin, tidak perlu diubah
    └─ MainModule
        ├─ Server/           Util, Settings, Network, Ranks, Data, Process, Lists, Chat, Players, Update
        ├─ Commands/         Moderasi, Teleport, Karakter, Utilitas, Hukuman
        └─ DylomAdminGui     GUI + script client
```

**Aturannya sederhana:** semua yang ingin kamu ubah ada di **`Config`**. Saat update, cukup ganti folder **`Loader`**.

---

## Pengaturan

### Pengaturan sederhana (Attributes)

Klik **`DylomAdmin` → `Config`**, lalu ubah di panel **Properties → Attributes**.

| Atribut | Bawaan | Keterangan |
|---|---|---|
| `Prefix` | `;` | Awalan command (maks. 3 karakter, tanpa spasi) |
| `CommandBarKey` | `Quote` | Tombol keyboard untuk membuka kotak command (nama `KeyCode`, `Quote` = tombol `'`) |
| `ButtonMobile` | `true` | Tampilkan tombol `;` di HP / tablet |
| `ButtonPC` | `false` | Tampilkan tombol `;` di PC |
| `ButtonMobileSize` | `48` | Ukuran tombol di HP (24–120) |
| `ButtonPCSize` | `40` | Ukuran tombol di PC (24–120) |
| `ButtonMinRank` | `Mod` | Rank minimal untuk melihat tombol `;` |
| `PanelRank` | `Mod` | Rank minimal untuk membuka `;panel` * |
| `UseRobloxBanAPI` | `true` | Pakai Ban API resmi Roblox untuk `;ban` / `;tban` |
| `BanAltAccounts` | `true` | Ikut mem-ban akun alt yang terdeteksi Roblox |
| `DefaultFlySpeed` | `60` | Kecepatan `;fly` kalau angka tidak diisi (5–500) |
| `MaxWalkSpeed` | `500` | Batas `;speed` |
| `MaxJumpPower` | `500` | Batas `;jump` |
| `NukeRadius` | `60` | Radius `;nuke` dalam stud (5–500) |
| `MaxLogs` | `300` | Jumlah log command yang disimpan per server |
| `CheckUpdates` | `true` | Beri tahu Owner kalau ada versi baru |
| `BanStoreName` | `DylomAdmin_Bans_v1` | Nama DataStore ban * |
| `RankStoreName` | `DylomAdmin_Ranks_v1` | Nama DataStore rank permanen * |

Perubahan saat game berjalan **langsung berlaku**, kecuali yang bertanda * (perlu restart server).
Isian yang tidak valid ditolak dengan peringatan di Output, dan admin tetap memakai nilai lama.

> ⚠️ Jangan mengganti `BanStoreName` / `RankStoreName` setelah game ramai. Data ban & rank lama tidak akan terbaca.

### Pengaturan daftar (`Config/Settings`)

Buka script **`Config/Settings`** untuk mengatur admin, grup, dan tombol khusus.

```lua
-- Rank tetap per pemain. Kunci: UserId (angka) atau Username.
Users = {
	[12345678] = "Admin",
	["NamaPemain"] = "Mod",
},

-- Rank otomatis dari grup Roblox. Id = 0 berarti tidak dipakai.
Group = {
	Id = 1234567,
	Ranks = {
		[255] = "Owner",
		[200] = "Admin",
		[100] = "Mod",
	},
},

-- Tombol ";" di layar, diatur manual per pemain
CommandButton = {
	ShowButton = { 12345678, "username" }, -- selalu tampilkan (walau tidak punya akses)
	HideButton = { "username" },           -- selalu sembunyikan (walau admin)
},
```

---

## Rank & admin

| Rank | Level | Keterangan |
|---|---|---|
| Player | 0 | Semua pemain |
| Mod | 2 | Moderasi & efek dasar |
| Admin | 3 | Ban, nuke, give, pengumuman |
| SuperAdmin | 4 | Beri rank, restart & shutdown server |
| Developer | 6 | Setara Owner (untuk developer map) |
| Owner | 6 | Pembuat game (otomatis) |

- Angka makin besar berarti rank makin tinggi. Kamu **tidak bisa** memakai command ke pemain yang rank-nya **sama atau lebih tinggi**.
- Rank bisa berasal dari: **pembuat game**, **`Config/Settings` → Users**, **grup Roblox**, **`;prank`** (permanen), atau **`;rank`** (sementara, hanya di server ini).
- **Developer** sengaja dibuat setara Owner, tapi tetap tampil dengan namanya sendiri.
- Jangan mengubah angka rank yang sudah dipakai, karena rank permanen disimpan sebagai angka.

---

## Cara memakai

### Membuka kotak command
- **PC:** tekan tombol **`'`** (bisa diubah lewat `CommandBarKey`).
- **HP:** ketuk tombol **`;`** di layar (tampil untuk rank `ButtonMinRank` ke atas).
- **Chat:** ketik langsung, misalnya `;fly me`. Pesan command tidak terlihat oleh pemain lain.

### Autocomplete
- Ketik sebagian nama command, lalu tekan **Tab** (PC) atau **ketuk saran** (HP).
- Argumen berikutnya muncul samar sebagai petunjuk, contoh: `freeze <pemain>`.
- **Panah atas/bawah** untuk memilih saran lain.

### Menulis target pemain
| Tulisan | Artinya |
|---|---|
| `me` | Dirimu sendiri |
| `all` atau `*` | Semua pemain |
| `others` | Semua kecuali kamu |
| `random` | Satu pemain acak |
| `admins` / `nonadmins` | Semua admin / semua non-admin |
| `andi` | Pemain yang username / display name-nya diawali "andi" |
| `@andi123` | Username persis |
| `andi,budi` | Beberapa pemain sekaligus |

Kalau nama sebagian cocok dengan **lebih dari satu** pemain, command ditolak dan kamu diminta mengetik lebih lengkap. Ini mencegah salah sasaran.

---

## Daftar command

`<...>` wajib diisi, `[...]` boleh dikosongkan. Rank yang lebih tinggi bisa memakai semua command di bawahnya.

### Player (semua pemain)
| Command | Keterangan |
|---|---|
| `;cmds` | Daftar command yang bisa kamu pakai (alias `commands`) |
| `;admins` | Admin yang sedang online (alias `staff`) |
| `;fps` | Panel Ping, FPS & Render, bisa digeser (alias `ping`, `stats`) |
| `;help [pesan]` | Panggil admin; admin mendapat notif dengan tombol **TELEPORT** (alias `tolong`) |
| `;rejoin` | Keluar lalu masuk lagi ke server yang sama (alias `rj`) |

### Mod
| Command | Keterangan |
|---|---|
| `;fly [pemain] [kecepatan]` / `;unfly` | Terbang. PC: **E** terbang/berhenti, **Space** naik, **Ctrl/Q** turun. HP: lompat 2x |
| `;noclip [pemain]` / `;clip` | Tembus dinding / normal lagi |
| `;speed <pemain> <angka>` | Kecepatan jalan (normal 16) (alias `ws`, `walkspeed`) |
| `;jump <pemain> <angka>` | Kekuatan lompat (normal 50) (alias `jp`, `jumppower`) |
| `;god [pemain]` / `;ungod` | Kebal / normal lagi |
| `;heal [pemain]` | Pulihkan darah penuh |
| `;kill <pemain>` | Bunuh karakter |
| `;invisible [pemain]` / `;visible` | Tidak terlihat / terlihat lagi (alias `invis`, `vis`) |
| `;refresh [pemain]` | Muncul ulang di posisi yang sama (alias `ref`) |
| `;respawn [pemain]` | Muncul ulang di titik spawn (alias `re`) |
| `;hat <pemain> <asset id>` / `;unhat` | Pakaikan aksesoris katalog, contoh `;hat me 48474313` (alias `acc`) |
| `;to <pemain>` | Pindah ke posisi pemain (alias `goto`) |
| `;bring <pemain>` | Bawa pemain ke posisimu |
| `;tp <pemain> <pemain>` | Pindahkan pemain ke pemain lain, contoh `;tp all me` |
| `;freeze <pemain>` / `;unfreeze` | Bekukan dalam balok es / pecahkan es |
| `;jail <pemain>` / `;unjail` | Penjara. Kabur atau respawn otomatis dikembalikan ke dalam |
| `;smite <pemain>` | Sambar petir (suaranya terdengar satu server) |
| `;rocket <pemain>` | Roket di punggung, meluncur, lalu meledak |
| `;rope <pemain> [pemegang]` / `;unrope` | Ikat dengan tali. `;rope starsB969 dylom` = Budi yang memegang. Saat pemegang terbang, target tergantung di bawahnya |
| `;kick <pemain> [alasan]` | Keluarkan dari server |
| `;sban <pemain> [alasan]` / `;unsban` | Ban hanya di server ini (alias `serverban`) |
| `;mute <pemain>` / `;unmute` | Larang / izinkan chat |
| `;pm <pemain> <pesan>` | Pesan pribadi, tampil sampai ditutup (alias `dm`) |
| `;panel` | Panel admin (alias `admin`, `dashboard`) |
| `;logs` | Log command di server ini |
| `;version` | Versi DylomAdmin & info versi baru (alias `ver`) |

### Admin
| Command | Keterangan |
|---|---|
| `;ban <username/UserId> [alasan]` | Ban permanen, **bisa untuk pemain offline** (alias `pban`) |
| `;tban <username/UserId> <menit> [alasan]` | Ban sementara, bisa offline (alias `timeban`) |
| `;unban <username/UserId>` | Cabut ban permanen / sementara |
| `;nuke <pemain>` | Peringatan 2,5 detik, lalu ledakan. Pemain dalam radius dengan rank lebih rendah ikut mati. Map tidak rusak |
| `;give <pemain> <item>` | Beri tool dari `Config/Assets`, contoh `;give me Building Tools` |
| `;announce <pesan>` | Pengumuman ke semua pemain (alias `m`, `ann`) |

### SuperAdmin
| Command | Keterangan |
|---|---|
| `;rank <pemain> <role>` | Rank sementara (server ini) (alias `setrank`) |
| `;prank <pemain> <role>` | Rank permanen (tersimpan) (alias `permrank`) |
| `;unrank <pemain>` | Cabut rank sementara & permanen (rank dari Settings/grup tetap) |
| `;restart [alasan]` | Restart server, semua pemain otomatis pindah ke server baru (alias `softshutdown`) |
| `;shutdown [alasan]` | Tutup server |

---

## Panel admin

Buka dengan **`;panel`**. Panel bisa digeser, punya kotak cari dan tombol **Muat**, dan di HP tab-nya otomatis pindah ke atas.

| Tab | Isi |
|---|---|
| **Akses** | Pemain online & rank-nya. Tombol per role dan **Cabut**, dengan pilihan **Sementara / Permanen** (butuh SuperAdmin) |
| **Banned** | Semua ban (permanen, sementara, server) beserta alasan, pelaku & tanggal. Tombol **Unban**, dan **Ban** pemain offline lewat username / UserId |
| **Role** | Urutan role & jumlah command-nya. **Ketuk role** untuk melihat anggotanya |
| **Assets** | Isi `Config/Assets`. Tombol **Ambil** untuk mengambil tool |

Semua tombol di panel menjalankan command biasa, jadi cek rank tetap dilakukan di server.

---

## Membuat command sendiri (Addons)

1. Buka **`Config/Addons`** dan duplikat **`_ContohAddon`**.
2. Ganti namanya **tanpa `_` di depan**, misalnya `Hadiah`. Modul yang diawali `_` tidak dimuat.
3. Ubah isinya:

```lua
return function(api)
	return {
		{
			name = "halo", aliases = { "hello" }, rank = "Mod", args = { "players" },
			desc = "Kirim sapaan ke pemain.",
			run = function(by, targets)
				for _, p in ipairs(targets) do
					api.notify(p, "Halo!", by.DisplayName .. " menyapamu.", "info")
				end
				return #targets .. " pemain disapa."
			end,
		},
	}
end
```

**Jenis argumen:** `players` (banyak pemain), `player` (satu pemain), `number`, `word` (satu kata), `text` (sisa kalimat). Tambahkan `?` untuk argumen opsional, contoh `"number?"`.
**`run`** mengembalikan pesan sukses (teks), atau `return false, "pesan gagal"`.
Nama command tidak boleh sama dengan command bawaan; kalau sama, addon dilewati dengan peringatan.

---

## Cara update

1. Unduh **`DylomAdmin.rbxm`** versi terbaru dari [Releases](../../releases).
2. Di Studio, **hapus folder `DylomAdmin/Loader`** yang lama.
3. Dari file baru, ambil **hanya folder `Loader`** dan taruh di dalam `DylomAdmin`.
4. **Jangan ganti folder `Config`.** Pengaturan, admin, addon, dan asetmu tetap aman.
5. Publish game.

Data ban & rank permanen tetap terbaca di versi baru.
Ketik **`;version`** di game untuk mengecek versi yang sedang dipakai.

---

## Keamanan

- Semua pengecekan rank dilakukan **di server**. Client yang dimodifikasi tidak bisa menjalankan command yang bukan haknya.
- Semua input dari client diperiksa tipe & panjangnya; angka `nan` / `inf` ditolak.
- Remote memiliki **anti-spam**.
- Teks dari pemain (alasan, pesan, pengumuman, PM) **difilter sesuai aturan Roblox**.
- Ban memakai **Ban API resmi Roblox** dengan DataStore sebagai cadangan.
- Tidak memakai `require(assetId)`, `loadstring`, `getfenv`/`setfenv`, atau HttpService di kode DylomAdmin.
- Data remote yang tidak valid dicatat sebagai `[KEAMANAN]` di `;logs`.

---

## Masalah umum

**Admin tidak menyala / tidak ada command.**
Pastikan folder `DylomAdmin` ada di **ServerScriptService**, lalu cek **Output** untuk tulisan `[DylomAdmin] v... aktif`.

**Ban / rank permanen tidak tersimpan di Studio.**
Aktifkan **Game Settings → Security → Enable Studio Access to API Services**, dan pastikan game sudah dipublish.

**`;rejoin` / `;restart` tidak jalan.**
Kedua command ini hanya berjalan di game yang sudah dipublish, bukan di Studio. `;restart` juga tidak bisa dipakai di private server (VIP).

**Tombol `;` tidak muncul di HP.**
Tombol hanya tampil untuk rank `ButtonMinRank` (bawaan: Mod) ke atas, dan bisa diatur per pemain lewat `ShowButton` / `HideButton`.

**Peringatan "Config.X tidak valid".**
Ada atribut di `Config` dengan isi yang salah, misalnya teks di kolom angka. Perbaiki isinya; sementara itu admin tetap memakai nilai lama.

---

## Kredit

- **DylomAdmin** dibuat oleh **starsB969**.
- **Building Tools (F3X)** di folder `Config/Assets` dibuat oleh tim F3X dan tetap menjadi milik pembuatnya.
- Sebagian **sound efek** berasal dari Creator Store Roblox (ProSoundEffects & Roblox) dan dipakai lewat asset id.

Kalau DylomAdmin membantu game-mu, beri ⭐ pada repo ini!

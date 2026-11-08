# website

Situs unduhan aplikasi NUMA (konversi not angka ke MIDI). Satu berkas: `index.html`.

## Tempat yang harus diisi pemilik

- Kedua installer, `NUMA-Setup.exe` (117 MB) dan `NUMA-Setup.msi` (122 MB), tidak disimpan di repo. GitHub menolak berkas di atas 100 MB, jadi keduanya menempel sebagai aset rilis dan sudah masuk `.gitignore`. Unggah ulang keduanya setiap kali membuat rilis baru.
- `index.html`: tombol unduh memakai `https://github.com/xrgsss/web-numa/releases/latest/download/<nama-berkas>`, jadi rilis baru tidak perlu mengubah HTML. Nama aset peka huruf besar-kecil, jadi nama berkas yang diunggah ke rilis harus persis `NUMA-Setup.exe` dan `NUMA-Setup.msi`. Kalau nama salah, tautannya 404.
- Unggah aset jangan lewat halaman rilis di browser: untuk berkas di atas 100 MB halaman itu gagal dengan pesan "Something went really wrong". `gh release create` juga putus di tengah dengan `http2: client connection force closed`. Yang berhasil: buat rilis draf dulu, unggah tiap berkas dengan `curl --http1.1 -T <berkas>` ke `https://uploads.github.com/repos/xrgsss/web-numa/releases/<id>/assets?name=<nama>`, lalu terbitkan drafnya. Rilis `v1.0.0` (id 438090890) dibuat dengan cara ini.
- `index.html`: versi yang tampil sekarang `1.0.0`, di hero dan di seksi Unduh. Samakan dengan nomor versi rilis sebelum situs dipublikasikan.
- `index.html`: `og:image` memakai `icon.png` secara relatif. Ganti ke URL absolut setelah situs punya alamat.
- `icon.png` harus tetap di folder yang sama dengan `index.html`. Berkas ini dipakai sebagai favicon, apple-touch-icon, tanda navigasi, dan og:image.
- Contoh notasi di halaman memakai contoh yang sama dengan tombol Muat Contoh di aplikasi (3 birama). Piano roll di hero adalah ilustrasi pemetaan pitch, bukan tangkapan layar aplikasi.

## Catatan teknis

- Ukuran berkas di halaman (117 MB dan 122 MB) dihitung dari berkas asli dalam satuan yang dipakai Windows Explorer. Ukur ulang kalau installer diganti.
- `icon.png` dipakai sebagai favicon, apple-touch-icon, tanda di navigasi, dan og:image. Berkas itu harus tetap berada di folder yang sama dengan `index.html`.
- Warna aksen diturunkan dari ikon: `#589dfb` ke `#273289`. Jangan mengganti aksen tanpa mengukur ulang kontras di kedua tema.
- Tema gelap dan terang memakai atribut `data-tema` di `<html>`, tersimpan di localStorage dengan kunci `numa-tema`.
- Navigasi ponsel memakai kelas `js` di `<html>`: tanpa JavaScript, daftar menu tetap tampil dan tombol menu disembunyikan.
- Piano roll adalah elemen HTML/CSS, bukan gambar. Posisi blok diatur lewat `grid-row` dan `grid-column`.
- Semua permukaan memakai kelas `.kaca` (navigasi, panel piano roll, panel tahap, blok dokumen, panel langkah, kartu unduh, menu ponsel). Bahannya diatur variabel `--kaca-*` di `:root` dan `html[data-tema="terang"]`.
- `.latar` adalah lapisan `position:fixed` berisi tiga bidang radial (`.pudar-1/2/3`). Kaca membutuhkan lapisan ini untuk dibiaskan, jadi jangan dihapus atau dipindah ke dalam `main`.
- Bidang radial itu bergerak lewat `@keyframes gerak-1/2/3`, hanya pada `transform`. Jangan menambahkan `opacity` ke keyframes: tema terang menimpa opasitasnya dan animasi akan menang. `prefers-reduced-motion` mematikan gerakan ini lewat `animation:none !important`.
- Di layar potret (maksimal 820px), ukuran dan posisi `.pudar-*` ditimpa di dalam `@media(max-width:820px) and (orientation:portrait)` memakai `vh`, karena `vw` di ponsel potret terlalu kecil untuk halaman setinggi 800px lebih. Jangan mengubah posisi itu tanpa mengukur ulang kontras teks redup di latar langsung.
- Kontrol kecil di navigasi memakai `--kaca-garis-kontrol`, bukan `--kaca-tepi-aksi`. `--kaca-tepi-aksi` disimpan untuk batas tombol yang berdiri sendiri, yang diukur 3:1 terhadap isinya sendiri.
- Panel menu ponsel (`.nav-menu` di dalam `@media(max-width:820px)`) sengaja tidak memakai `backdrop-filter`. Ia berada di dalam `.nav` yang juga ber-`backdrop-filter`, dan elemen ber-`backdrop-filter` menjadi backdrop root untuk anaknya, jadi blur di panel tidak akan membiaskan halaman di belakangnya. Alasnya memakai `--menu-isi` yang pekat. Jangan mengembalikannya ke `--kaca-isi-kuat` tanpa menguji ulang: teks halaman akan tembus.
- Tiga variabel baru khusus panel dan kontrol ponsel: `--kontrol-isi`/`--kontrol-isi-hover`/`--kontrol-garis`/`--kontrol-bayang` untuk tombol, `--menu-isi`/`--menu-garis`/`--menu-hover` untuk panel. Jangan memakai `--kaca-isi-kuat` di dalam panel: isi kaca tidak terlihat di atas alas yang sudah pekat.
- Tombol Menu dan tombol tema di ponsel memakai `--kontrol-isi` yang pekat, bukan `background:transparent`. Aturan `:hover` dan `:active` di dalam kueri ponsel wajib ada; tanpa itu aturan dasar (isi kaca) menang dan tombol berkedip transparan saat ditekan.
- `.menu-toggle` memakai `inline-flex` dengan `align-items`/`justify-content:center` dan `flex:none`. Tanpa `align-items`, kotak teks melar setinggi tombol dan label menempel di atas. Tanpa `flex:none`, tombol menyusut di batang flex sampai bentuk pilnya gepeng. `line-height` harus ditulis setelah `font:inherit`, karena shorthand `font` meresetnya.
- Ada blok `@supports not ((backdrop-filter:blur(1px)) or (-webkit-backdrop-filter:blur(1px)))` yang membuat kaca hampir pekat di peramban lama. Jangan hapus tanpa pengganti.
- Teks aksen di atas kaca memakai `--aksen-teks-kaca` (`#a8cbff` gelap, `#1f47b8` terang), bukan `--aksen`. Warna `--aksen` hanya untuk blok nada, kepala playback, dan garis birama.
- Kaca di semua permukaan adalah override R-10 dan R-13 atas permintaan pemilik. Jangan mengembalikannya ke permukaan solid tanpa bertanya.

<!-- antislop:start -->
## antislop
For UI, copy, people, mobile layout, or code comments work, read `skills/antislop/SKILL.md` (core) and then the skill for the task:
- UI / visual: `skills/antislop-ui/SKILL.md`
- Copy & text: `skills/antislop-copywriting/SKILL.md`
- People: `skills/antislop-human/SKILL.md`
- Mobile / responsive: `skills/antislop-layoutmobile/SKILL.md`
- Code comments: `skills/antislop-code/SKILL.md`
Before starting, follow the core's "Two Usage Modes" section in strict order: explicit session instruction first, then global preference, then ask. A session instruction always wins. For a resolved mode, say `antislop active: <mode> (session override).` or `antislop active: <mode> (global preference).` once before presenting findings or making edits, using the actual mode and source. Acknowledging the user's request without naming the source does not replace this notice.
Only an explicit choice of antislop during or after selects a session mode. A request to review, audit, or avoid file edits does not select a mode; read the global preference in that case. Another skill's mode does not select antislop's mode.
If the mode is unresolved, ask during/after and end the response; wait for the answer before any UI review, planning, or concept. For read-only tasks, put the active-mode notice only at the start of the final answer, never in progress messages. For editing tasks, announce before the first edit and omit it from the final answer.
To update antislop later: download `antislop.md` again, or run `npx antislop-ai --update` if it was installed as skill folders.
<!-- antislop:end -->

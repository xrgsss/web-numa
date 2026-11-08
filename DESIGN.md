# DESIGN.md - NUMA (situs unduhan aplikasi)

Arah ini ditetapkan pemilik lewat pilihan "studio audio gelap", semua bagian dirombak, dan jendela tiruan aplikasi diganti. Berkas ini mencatat arah itu supaya sesi berikutnya tidak menebak.

## Identitas
- Produk: NUMA, aplikasi desktop untuk mengubah not angka menjadi berkas MIDI (.mid).
- Audiens: penulis not angka, guru, dan pemain musik yang bekerja dengan notasi angka.
- Nada: tenang, teknis, jujur. Menampilkan isi panel aplikasi apa adanya, tanpa angka pengguna atau klaim yang belum terbukti.

## Palet
Aksen diambil dari ikon aplikasi (`icon.png`): gradien `#589dfb` di ujung terang ke `#273289` di ujung gelap. Ikon itu juga dipakai sebagai favicon dan tanda di navigasi, jadi warna situs dan ikon aplikasi sama.

Dua tema, keduanya diukur dengan pemeriksa kontras WCAG pada kondisi terburuk: kaca yang menumpuk bidang warna paling terang, termasuk kilap di tepi atas.

Gelap (bawaan, alasan: aplikasi desktop musik lazim dibuka di ruang kerja gelap, dan pemilik memilih arah studio):
- Dasar #070910. Tiga bidang radial di belakang kaca memakai #2b53c0, #589dfb, dan #1f47b8 dengan opasitas 0.55, 0.2, dan 0.45.
- Latar terburuk hasil komposit: rgb(27, 50, 113).
- Teks #eef1f5 (7.88:1 di tepi kaca), teks redup #bcc3ce (5.03:1).
- Aksen grafis #589dfb dipakai untuk blok nada dan kepala playback. Aksen sebagai teks memakai #a8cbff (5.37:1), karena #589dfb hanya 3.24:1 di atas kaca; warna itu dipakai untuk angka tahap, label format, dan tanda `|` di dokumen contoh.
- Teks di atas aksen #08122b (6.74:1).
- Tombol utama: isi #2b53c0, teks putih 6.77:1. Hover #3b6fe0 dengan teks putih 4.63:1.

Terang:
- Dasar #eef1f6. Bidang radial yang sama dengan opasitas 0.22, 0.16, dan 0.18.
- Latar terburuk hasil komposit: rgb(195, 206, 234).
- Teks #0f131a (17.36:1), teks redup #4e5663 (6.91:1).
- Aksen digelapkan ke #1f47b8 (7.39:1 di atas kaca), karena biru terang ikon hanya 2.46:1 di latar terang.
- Teks di atas aksen putih (7.92:1). Tombol utama #1f47b8 dengan teks putih, hover #273289 (11.03:1).

Aksen hanya dipakai untuk blok nada, kepala playback, angka tahap, label format, dan tanda birama di dokumen contoh. Tombol unduh memakai turunan gelap dari biru yang sama supaya teks putihnya tetap lolos AA. Sisanya netral.

## Material kaca
Semua permukaan memakai satu kelas, `.kaca`: isi putih transparan, kilap linear sampai 46% tinggi, rim terang 1px di tepi atas, garis bawah gelap tipis, bayang lembut, dan `backdrop-filter: blur(26px) saturate(165%)` (terang: `saturate(150%)`).

- Gelap: isi `rgba(255,255,255,.055)`, isi kuat `.09`, kilap `.05`, tepi `.14`, tepi aksi `.5`, garis kontrol `.22`, rim `.3`, bayang `0 24px 60px rgba(0,0,0,.45)`.
- Terang: isi `.55`, isi kuat `.72`, kilap `.62`, tepi `.9`, tepi aksi `rgba(16,24,40,.5)`, garis kontrol `rgba(16,24,40,.22)`, rim `.95`, bayang `0 20px 44px rgba(23,35,66,.14)`.
- Tiga tingkat garis, dipakai menurut tugasnya: `--kaca-tepi` untuk tepi panel, `--kaca-tepi-aksi` untuk batas tombol yang berdiri sendiri (diukur 3:1 terhadap isinya sendiri), dan `--kaca-garis-kontrol` untuk kontrol kecil di navigasi yang sudah punya label atau ikon. `--garis-tautan` khusus garis bawah tautan teks, diukur 4.97:1 (gelap) dan 4.21:1 (terang) terhadap latar halaman, dan tetap di atas 3:1 di atas bidang warna paling pekat.
- Kontrol di navigasi ponsel dan alas panel menu memakai isi pekat, bukan kaca: `--kontrol-isi` (#1b2130 gelap, #e7ebf5 terang), `--kontrol-isi-hover`, `--kontrol-garis`, `--kontrol-bayang`, `--menu-isi` (#12161f gelap, #fbfcfe terang), `--menu-garis`, dan `--menu-hover`. Teks di dalamnya 14.18:1 dan 15.60:1; teks tautan panel 15.98:1 dan 18.13:1; teks redup 10.20:1 dan 7.22:1. Garis kontrol diukur 3.49:1 (gelap) dan 3.23:1 (terang) terhadap isinya, jadi batas tombol tetap terbaca tanpa mengandalkan ikon saja.
- Garis panel (`--menu-garis`) sengaja lebih tipis dari garis kontrol. Ring kontrol disetel 3:1 terhadap isi tombol kecil, dan kalau nilai itu dipakai di sekeliling panel besar hasilnya terbaca sebagai kartu berat.
- Kaca perlu sesuatu untuk dibiaskan, jadi `.latar` tetap di belakang semua konten: lapisan `position:fixed` berisi tiga bidang radial dari keluarga biru ikon.
- Batas tombol kedua memakai `--kaca-tepi-aksi`. Nilainya diukur terhadap kaca di kedua sisinya: gelap 3.66:1, terang 3.08:1, di atas ambang 3:1 untuk batas komponen non-teks.
- Tanpa dukungan `backdrop-filter`, blok `@supports not` membuat kaca hampir pekat supaya teks tetap terbaca.
- Override: R-10 (dose cap kaca 1-2 elemen) dan R-13 (glow) dilanggar atas permintaan pemilik, yang memilih kaca di semua permukaan. Hierarki dijaga lewat ketebalan isi dan rim, bukan lewat warna. R-01 dilonggarkan hanya untuk bidang radial latar, karena kaca butuh bahan untuk dibiaskan.
- Satu pengecualian pada kaca: panel menu ponsel dan kontrol di navigasi memakai isi pekat. Panel itu menumpuk teks halaman, dan kaca tembus membuat menunya tidak terbaca; blur di sana juga tidak berfungsi karena berada di dalam elemen ber-`backdrop-filter`. Ini keputusan keterbacaan, bukan kelalaian.

### Latar bergerak
Tiga bidang radial di `.latar` melayang sangat lambat: 40s, 32s, dan 48s di desktop, 56s, 46s, dan 64s di ponsel. Arahnya `alternate`, jadi tidak pernah ada lompatan saat siklus berbalik, dan durasinya sengaja berbeda supaya ketiganya tidak pernah sinkron. Geser 6 sampai 10 persen ditambah skala 1.12 sampai 1.18 memberi gerak yang terlihat dalam hitungan detik tanpa terasa seperti animasi.

- Hanya `transform` (geser dan skala) yang dianimasikan. Jarak geser memakai persen dari ukuran bidang itu sendiri, bukan `vw` atau `vh`, jadi gerakannya seimbang di layar sempit maupun lebar tanpa nilai terpisah.
- Di layar potret (lebar maksimal 820px dan orientasi tegak), ukuran bidang memakai `vh` dan posisinya disebar: satu di kepala halaman, satu di tengah, satu di sepertiga bawah. Alasannya: pada layar 390x844, bidang berukuran `vw` hanya selebar 290px sementara halamannya 844px, jadi warna menumpuk di pojok atas dan separuh halaman kehilangan bahan yang dibiaskan kaca. Nilai ini tidak berlaku di layar mendatar, jadi desktop tidak berubah.
- Posisi itu dipilih supaya bidang hanya bersinggungan di ujung gradasinya, bukan bertumpuk penuh. Diukur dengan penyampel kisi pada 390x844 dan 1440x900, termasuk keadaan akhir animasi (skala 1.18): latar terburuk tetap sama dengan satu bidang penuh, yaitu rgb(27, 50, 113) di tema gelap dan rgb(195, 206, 234) di tema terang. Kontras teks redup tidak berubah: 6.83:1 di latar gelap, 4.72:1 di latar terang, 5.07:1 dan 6.89:1 di atas kaca.
- Opasitas tidak boleh dianimasikan. Tema terang menimpa opasitas tiap bidang lewat `html[data-tema="terang"] .pudar-*`, dan animasi akan mengalahkan aturan itu sehingga tema terang kehilangan pengurangan intensitasnya.
- `.latar` memakai `overflow:hidden`, jadi bidang yang membesar dan bergeser tidak pernah menimbulkan gulir mendatar.
- `prefers-reduced-motion: reduce` mematikan semua animasi lewat `animation:none !important`, jadi latar kembali diam.
- Gerakan diperlambat di bawah 820px karena kaca di atasnya harus menghitung ulang blur setiap bingkai. Kalau perangkat lama terasa berat, tuas berikutnya adalah menurunkan `--kaca-blur` pada kueri yang sama.

Gradien ikon sengaja tidak dipakai sebagai isi tombol. Gradien membuat satu bagian tombol jatuh di bawah 4.5:1 (putih di #589dfb hanya 2.75:1), jadi tombol memakai satu warna datar dari keluarga yang sama.

## Tipografi
- Judul dan teks: Archivo. Alasan: grotesk lebar dengan ujung tegas, terasa seperti label peralatan, bukan font antarmuka bawaan.
- Data dan notasi: IBM Plex Mono. Alasan: dipakai untuk tempo, birama, nama berkas, status panel, dan dokumen not angka, jadi teks teknis punya suara sendiri.

## Dial
ENERGY 2 (kokoh, tidak riuh) / RHYTHM 2 (hero dua kolom, daftar tahap bergaris, pasangan dokumen dan langkah, blok unduh berbentuk transport) / MOTION 2 (transisi hover dan fokus, ditambah satu gerakan latar yang berjalan terus). MOTION naik dari 1 ke 2 atas permintaan pemilik: latar harus terasa hidup. Gerakan itu satu-satunya animasi di halaman, sengaja sangat lambat supaya tidak menyaingi isi, dan tidak ada elemen antarmuka yang ikut bergerak.

## Motif identitas
Ikon aplikasi dan piano roll. Tanda di navigasi dan favicon memakai `icon.png`; gradien birunya menjadi keluarga warna aksen. Kotak nada, garis kisi, dan kepala playback diambil dari cara kerja produk: angka dipetakan ke pitch dan durasi. Garis kisi piano roll bukan tekstur hiasan, melainkan sumbu pitch dan ketuk yang sama dengan isi aplikasi. Angka tahap 01 sampai 05 mengikuti penomoran panel di aplikasi.

## Navigasi
Satu batang kaca berbentuk pil yang menempel di atas saat digulir. Isinya: tanda ikon dan nama di kiri, tautan di tengah-kanan, tombol Unduh terisi warna, lalu tombol tema hantu (batasnya hanya muncul saat hover) di ujung kanan. Alasannya: batang pil menyatukan navigasi sebagai satu objek, jadi tombol tema tidak lagi terlihat sebagai kotak kedua yang bersaing dengan tombol Unduh. Tautan dan tombol tema 38px di desktop, 44px di ponsel.

Di ponsel, tautan dan tombol Unduh turun ke panel terpisah di bawah batang. Batangnya hanya menyisakan logo, tombol Menu, dan tombol tema, dan keduanya memakai isi serta garis yang sama (`--kontrol-isi`, `--kontrol-garis`) supaya tidak ada yang terlihat lebih penting. Sebelumnya Menu bergaris tegas sementara ikon tema tanpa garis, sehingga Menu terbaca sebagai aksi utama padahal aksi utamanya ada di dalam panel, dan tombol tema terbaca sebagai lingkaran kosong.

Panel menu memakai alas pekat (`--menu-isi`), bukan kaca tembus. Ada dua alasan. Pertama, panel ini menumpuk teks halaman; kaca yang tembus membuat menunya tidak terbaca. Kedua, `backdrop-filter` di panel tidak akan bekerja sama sekali: `.nav` induknya sudah memakai `backdrop-filter`, dan elemen ber-`backdrop-filter` menjadi *backdrop root* untuk anaknya, sehingga anaknya hanya bisa membiaskan isi di dalam induk itu, bukan halaman di belakangnya. Jadi blur di panel adalah biaya GPU tanpa hasil, dan sudah dihapus. Kilap, rim, dan bayang tetap ada supaya panel masih terbaca sebagai material yang sama dengan batang di atasnya.

## Ponsel
- Aksi utama di hero melebar penuh di bawah 520px supaya sasaran ketuknya jelas. Tautan .msi tetap selebar teksnya, karena kalau ikut melebar garis bawahnya memanjang jadi seperti pemisah.
- Semua kontrol minimal 44px. Tautan menu 44px, tombol tema 44px, tombol unduh di panel 44px.
- Kolom label piano roll 52px. Di bawah 520px huruf solfège di dalamnya disembunyikan dan angka di dalam blok nada dinolkan, supaya blok selebar 17px tidak pecah.
- Dokumen not angka memakai `overflow-x:auto`; baris `1 2 3 4 | 5 - 5/ 6/ | 7' 0 1'' - |` tidak boleh memaksa halaman melebar.
- Latar potret memakai ukuran `vh` dan posisi yang disebar, lihat bagian Latar bergerak.

Piano roll di hero adalah ilustrasi pemetaan pitch, bukan tangkapan hasil konversi. Isi panel di seksi fitur adalah teks keadaan kosong dari aplikasi, bukan data karangan.

## Batasan yang dijaga
- Kaca dipakai di semua permukaan atas persetujuan pemilik (override R-10 dan R-13). Gradasi yang tersisa hanya bidang radial latar dan kilap kaca, keduanya memberi bahan yang bisa dibiaskan, bukan dekorasi tambahan. Satu-satunya animasi adalah gerakan latar yang lambat; tidak ada animasi masuk dan tidak ada glow berdenyut.
- Tidak ada angka versi karangan. Versi yang tampil sekarang 1.0.0 dan perlu dikonfirmasi pemilik. Ukuran berkas dihitung dari berkas installer asli di folder situs (117 MB dan 122 MB).
- Seksi unduh menyajikan dua format sebagai daftar dua baris, bukan dua kartu kembar: format, kegunaan, nama berkas, ukuran, lalu tombol. Tombol .exe berisi warna, tombol .msi bergaris, karena .exe adalah jalur yang dipakai mayoritas orang.
- Tema terang dan gelap sama-sama lengkap; pilihan tersimpan di localStorage dan mengikuti preferensi sistem pada kunjungan pertama.

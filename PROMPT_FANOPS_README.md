# PROMPT: GitHub Profile README "FanOps" (retro OS + network analyst, animasi SVG)

Salin semua isi file ini ke AI coding kamu. Ganti nilai di bagian `0. VARIABEL` kalau perlu, sisanya biarkan.

---

## 0. VARIABEL (isi / ganti)

```text
HANDLE_DISPLAY   = FanOps
GITHUB_USERNAME  = USERNAME            # ganti dengan username asli
EMAIL            = you@example.com
ROLE             = Network Analyst & Sysadmin
BAHASA_COPY      = Indonesia santai, istilah teknis tetap Inggris
SKILL_UNLOCKED   = Linux(4), Bash(4), Python(3), TCP/IP(4), Cisco IOS(3), Wireshark(3), Monitoring(2), Grafana(1)   # angka = level 1-5
SKILL_TRAINING   = Ansible, BGP, Docker
SKILL_LOCKED     = Kubernetes
NEXT_UNLOCK      = Ansible
```

## 1. TUJUAN

Bangun satu paket **GitHub Profile README** untuk "FanOps": README.md + SVG animasi buatan sendiri + workflow GitHub Actions untuk snake kontribusi. Hasilnya harus langsung bisa di-upload ke repo `USERNAME/USERNAME` dan jalan di GitHub tanpa build step.

Referensi visual: tampilan GitHub profile bertema **OS jadul**: window dengan title bar dan tombol minimize/maximize/close, font piksel, scanline CRT, panel "TRAFFIC_ANALYST.EXE", "SNAKE_GAME.EXE", dan terminal. Semuanya bernuansa **network / sysadmin** (server rack, kabel, packet, ping, topologi).

## 2. ATURAN KERAS ("JANGAN AI SLOP")

Wajib dipatuhi. Kalau ragu, pilih yang lebih spesifik dan lebih jujur.

- **Jangan pakai** layanan/kartu pasaran: capsule-render, readme-typing-svg, github-readme-stats, streak-stats, skillicons, shields/badge berderet, trophy, visitor counter, GIF wave "Hi there 👋".
- **Jangan** emoji dekoratif di heading atau bullet. Jangan kalimat motivasi generik ("passionate about...", "always learning", "love to code").
- **Jangan** klaim palsu yang terlihat seperti data nyata (jumlah repo, uptime 99.99%, sertifikasi). Nilai dekoratif harus jelas bersifat dekoratif atau bisa diedit user.
- Setiap animasi harus **nyambung dengan tema network/sysadmin** dan punya alasan: LED berjalan, packet di kabel, ping counter, grafik traffic, kursor terminal. Dilarang animasi random (partikel acak, pelangi, bounce tanpa makna).
- Copy README: singkat, punya suara orang sungguhan, sedikit humor kering khas sysadmin. Bukan brosur.
- Semua visual dibuat **tangan sebagai SVG** (hand-written), bukan gambar rasterisasi.

## 3. TOKEN DESAIN

Palet (pakai persis):

| token | hex | dipakai untuk |
| --- | --- | --- |
| bg-deep | `#0b0b1f` | latar utama |
| panel | `#0f0f2b` / `#12122f` | isi window |
| titlebar | `#3a3466` | title bar |
| frame-light | `#cdb4d8` | border luar window |
| frame-mid | `#6f5f9a` | border dalam |
| btn | `#8f80b8` | tombol min/max |
| btn-close | `#c26b8a` | tombol close |
| cream | `#ffe7a3` | judul, teks utama |
| cyan | `#5de2e7` | automation, garis traffic |
| green | `#54d66a` | networking, status ONLINE, kursor |
| orange | `#ff9a62` | ops & observability, warning LED |
| violet | `#b78cff` / `#9b6fc7` | grafik bandwidth, node topologi |
| text | `#cfc9ee` | teks biasa |
| dim | `#8e89b8` / `#6a668f` | teks redup, locked |
| cat-fur | `#fff3ea` | kucing piksel |

Font: **hanya system monospace stack** `ui-monospace,SFMono-Regular,Menlo,Consolas,"Liberation Mono",monospace` (GitHub tidak memuat font eksternal di SVG). Efek "piksel" untuk judul besar dibuat dengan **path glyph 5x7** (lihat 5.1), bukan font.

Efek CRT di setiap window: overlay `<pattern>` garis 1px tiap 4px (opacity ~.25) di atas konten.

Struktur window yang dipakai ulang: border luar `#cdb4d8` 2px, title bar `#3a3466`, judul cream, tiga tombol di kanan (min = garis, max = kotak, close = X).

## 4. BATASAN TEKNIS GITHUB (kritis)

SVG di README dimuat lewat `<img>` (camo proxy). Artinya:

- **Boleh:** `<style>` dengan CSS `@keyframes`, SMIL (`animateMotion`, `animate`), `<pattern>`, `<mask>`, `<clipPath>`, gradient, `<use>`.
- **Dilarang:** JavaScript, `<script>`, `<foreignObject>`, link/gambar/font eksternal, `<image href=http...>`.
- Pakai `xlink:href` (+ `xmlns:xlink`) di `<use>` supaya aman di semua browser.
- Untuk CSS transform di elemen SVG yang perlu pusat sendiri: `transform-box: fill-box; transform-origin: ...`.
- Tiap SVG punya `viewBox` + `width`/`height`, `role="img"`, `<title>` dan `<desc>`.
- Wajib ada `@media (prefers-reduced-motion: reduce){ *{animation:none!important} }` dan pastikan kondisi diam tetap tampil lengkap (teks output tidak hilang).
- Ukuran tiap SVG kecil (target < 15 KB). Gunakan path relatif berulang (`l14 -12 ...`) bukan ribuan elemen.
- Semua SVG harus **valid XML** (escape `&` jadi `&amp;`).

## 5. DAFTAR FILE DAN SPESIFIKASI

```text
README.md
assets/banner.svg        viewBox 1000x380
assets/terminal.svg      viewBox 900x280
assets/skills.svg        viewBox 960x500
assets/traffic.svg       viewBox 480x300
assets/snake-bar.svg     viewBox 960x36
assets/footer.svg        viewBox 960x120
.github/workflows/snake.yml
```

### 5.1 `banner.svg` (hero)

Window "README.md" penuh. Komposisi kiri ke kanan:

1. **3 server rack** (x=44/146/248, lebar 88, y=80..340, 10 unit tinggi 26). Isi pola alur (groove) lewat `<pattern>`. LED per unit digambar sebagai **path dashed** (`stroke-dasharray:5 6`, stroke-width 5) sebagai lapisan redup, lalu dua lapisan aktif (cyan `5 17`, hijau `5 28`) dengan animasi `stroke-dashoffset` bertahap (`steps(2)` / `steps(3)`) supaya LED terlihat "berlari". Tiap rack beda durasi dan delay. Tambah 2 LED warning oranye/merah yang berkedip.
2. **Kabel** dari rack ke satu garis **uplink** horizontal (y=262) ke kanan. Kabel berwarna (cyan/oranye/violet) dengan dash yang mengalir (`stroke-dashoffset` linear). Di uplink ada 3 **packet** (rect 12x6) bergerak via `<animateMotion>` dengan `begin` negatif berbeda; ujung kanan berupa titik hijau berdenyut.
3. **Kucing piksel** (karakter ORIGINAL, bukan karakter berhak cipta) mengintip dari tepi bawah window. Grid 1 unit = 8px: kepala 16x8 unit, telinga terpisah, mata hijau, hidung pink, kumis, dua kaki depan. Outline gelap dibuat dengan menggambar path yang sama dua kali (stroke tebal gelap di bawah, fill bulu di atas). Animasi: kedip mata (scaleY), telinga kiri/kanan twitch bergantian dengan periode berbeda (7s dan 9s).
4. **Judul piksel "FANOPS"** (x=580, y=92, 1 unit = 10px). Glyph 5x7 sebagai path (dipakai di `<g id="ttl">`, lalu di-`<use>` 4 kali: bayangan ungu offset (6,6), lapisan magenta, lapisan cyan, lapisan cream). Lapisan cyan/magenta bergeser sesekali (glitch singkat di ~90-97% siklus 6 detik, `steps(1)`).
   Path glyph (viewBox unit, transform `scale(10)` + `translate(x 0)` kelipatan 60):
   - F: `M0 0h5v1H0z M0 1h1v6H0z M1 3h3v1H1z`
   - A: `M1 0h3v1H1z M0 1h1v6H0z M4 1h1v6h-1z M1 3h3v1H1z`
   - N: `M0 0h1v7H0z M4 0h1v7h-1z M1 1h1v2H1z M2 3h1v1H2z M3 4h1v2h-1z`
   - O: `M1 0h3v1H1z M1 6h3v1H1z M0 1h1v5H0z M4 1h1v5h-1z`
   - P: `M0 0h4v1H0z M0 1h1v6H0z M4 1h1v2h-1z M1 3h3v1H1z`
   - S: `M1 0h4v1H1z M0 1h1v2H0z M1 3h3v1H1z M4 4h1v2h-1z M0 6h4v1H0z`
5. Di bawah judul: "NETWORK ANALYST" (cream, bold, `textLength` dipaksa agar lebar tetap) + kursor hijau berkedip; baris kecil cyan `sysadmin / linux / packets / uptime`; baris status `STATUS: ONLINE | eth0 1000Mb/s | loss 0%` dengan titik hijau berdenyut.
6. Efek: efek boot (judul berkedip sekali saat load, `animation: ... 1 both`), band cahaya yang menyapu vertikal tiap 7 detik, overlay scanline.

### 5.2 `terminal.svg`

Terminal yang **mengetik sendiri**, loop 16 detik:

```text
fanops@net:~$ whoami            -> fanops  --  network analyst & sysadmin
fanops@net:~$ uptime            -> up since forever, 1 user, load average: 0.42, 0.37, 0.31
fanops@net:~$ cat motto.txt     -> if it's not DNS, it's the firewall. if it's not the firewall, it's DNS.
fanops@net:~$ █   (kursor berkedip)
```

Teknik (jangan pakai JS): teks sudah ada penuh, ditutup oleh **rect "cover"** berwarna sama dengan background yang bergeser ke kanan dengan `transform: translateX(...)` memakai `steps(N)` (N = jumlah karakter). Kursor hijau menempel di tepi kiri cover dan hanya tampil saat baris itu diketik. Output baris muncul lewat animasi opacity `steps(1)` setelah ngetik selesai. Seluruh grup fade out di 93-97% lalu mengulang. Lebar karakter diasumsikan 10.8px untuk font 18px. Fallback reduced-motion: semua teks tampil.

### 5.3 `skills.svg` ("unlock skills" ala game)

Skill tree digambar sebagai **topologi jaringan**, bukan bar persentase.

- Node akar (lingkaran, "FAN/OPS") di kiri, berdenyut (ring membesar + memudar). Tiga cabang melengkung ke tiga baris.
- Tiga cabang (warna berbeda): `// AUTOMATION` (cyan): Linux, Bash, Python, Ansible. `// NETWORKING` (hijau): TCP/IP, Cisco IOS, Wireshark, BGP. `// OPS & OBSERVABILITY` (oranye): Monitoring, Grafana, Docker, Kubernetes. Sesuaikan dengan variabel `SKILL_*`.
- Node **unlocked**: kotak 70x70 dengan sudut kuning (corner ticks), ikon teks (`Lx`, `>_`, `Py`, `IP`, `IOS`, `pkt`, `mon`, `Grf`), label, dan **pips level** (garis dashed `6 3`, panjang = level*6 + (level-1)*3 di atas track redup). Garis antar node unlocked **mengalir** (dash bergerak).
- Node **training**: border putus-putus + ikon gembok + label redup + teks `TRAINING` oranye, semua berdenyut (opacity). Node **locked**: lebih gelap, tanpa denyut, teks `LOCKED`.
- Garis menuju node terkunci: dotted statis (tidak mengalir).
- Panel kanan "PLAYER": nama, `LVL` = jumlah unlocked, bar XP yang naik turun pelan, `NEXT UNLOCK` + kursor, legenda unlocked/training/locked, keterangan "pips = skill level".
- Title bar: `SKILL_TREE.EXE`, teks `8 / 12 unlocked` (hitung dari variabel).
- Frame node dibuat sekali di `<defs>` lalu di-`<use>` dengan `style="color:#..."` dan `stroke="currentColor"`.

### 5.4 `traffic.svg` ("TRAFFIC_ANALYST.EXE")

- Grafik area **scroll tanpa putus**: path periodik (akhir = awal) diulang 4x, di dalam `<clipPath>`, digeser `translateX(-satu_periode)` linear infinite (periode 110px / 3.3s untuk grafik atas, 90px / 2.1s untuk bandwidth). Garis cyan + isi gradient ungu.
- Topologi mini: 1 hub + 5 node, tepi ungu, **titik packet** berwarna bergerak di tepi via `animateMotion`, ring denyut di hub.
- Teks yang "hidup": diagnostik berputar (`// scanning...`, `// tracing route...`, `// checking ports...`, `// all clear`) dan counter (`ping -c`, `packets sent`, `throughput`) berganti nilai. Teknik: beberapa `<text>` bertumpuk, tiap elemen terlihat 25% siklus dengan `animation-delay` negatif.
- Bar `LINK UTIL` hijau naik-turun (scaleX, origin kiri). Titik hijau `PACKETS: ACTIVE` berdenyut.
- Label chart bawah pendek agar tidak bertabrakan (`BANDWIDTH` dan `PORT: UP`).
- Reduced-motion: tampilkan nilai pertama tiap grup.

### 5.5 `snake-bar.svg`

Title bar saja: `CONTRIBUTIONS / SNAKE_GAME.EXE` + titik hijau berdenyut + `live, regenerates every 12h` + tiga tombol window. Dipasang tepat di atas gambar snake.

### 5.6 `footer.svg`

Garis detak **ECG hijau** (periode 240px, diulang 5x, geser -240px/4s linear) di atas grid redup, dengan mask gradient horizontal agar ujung kiri-kanan memudar. Teks tengah: `--- fanops ping statistics ---  1 packet transmitted, 1 received, 0% packet loss` + kursor berkedip.

### 5.7 `.github/workflows/snake.yml`

Gunakan `Platane/snk/svg-only@v3` dengan `github_user_name: ${{ github.repository_owner }}` dan output `dist/snake.svg` dengan warna: ular `#54d66a`, kotak `#1b1b3d,#244a3a,#2f7a4c,#54d66a,#b8f5b8`. Lanjut push ke branch `output` dengan `crazy-max/ghaction-github-pages@v3.1.0` (`target_branch: output`, `build_dir: dist`, `GITHUB_TOKEN`). Trigger: `schedule` tiap 12 jam, `workflow_dispatch`, dan `push` ke `main`. `permissions: contents: write`.

### 5.8 `README.md`

Urutan dan isi:

1. Banner (center, `width="100%"`) + navigasi teks kecil: about / skill tree / traffic / snake / contact (anchor link, bukan badge).
2. `## about`: `terminal.svg`, lalu satu paragraf pendek:
   "Network analyst & sysadmin. Kerjaan utamanya: bikin jaringan tetap hidup, cari tahu kenapa dia mati, lalu bikin script biar nggak mati dengan cara yang sama dua kali."
   Disusul code block `text`: role, daily, habit (`baca packet capture sebelum nyalahin siapa pun`), status.
3. `## skill tree`: 1-2 kalimat penjelasan cara baca (menyala = unlocked, berkedip = training, gelap = belum), `skills.svg`, dan `<details>` berisi **tabel teks** yang setara (aksesibilitas / fallback gambar tidak muncul).
4. `## traffic`: `traffic.svg` (lebar sekitar 62%) + code block `$ fanops --now` dengan baris learning / building / reading (isi jujur, ada komentar `# next unlock`).
5. `## snake`: `snake-bar.svg` lalu `https://raw.githubusercontent.com/USERNAME/USERNAME/output/snake.svg`.
6. `## contact`: code block `text` (github, email) + `footer.svg` di akhir.

Heading huruf kecil, rapi, tanpa emoji. Semua gambar punya `alt` yang deskriptif.

## 6. CARA KERJA YANG DIMINTA

1. Kerjakan **semua file sekaligus**, jangan tanya balik kecuali variabel di bagian 0 kosong dan benar-benar dibutuhkan.
2. Tulis setiap SVG dengan tangan. Hindari men-generate ratusan elemen; pakai pola berulang (`<pattern>`, `<use>`, path relatif, dasharray).
3. Setelah selesai, **verifikasi sendiri**:
   - parse semua SVG sebagai XML (harus valid),
   - render tiap SVG dengan Chrome headless, ukuran sama dengan viewBox. Lakukan **dua kali**: normal dan dengan `--force-prefers-reduced-motion` (supaya kondisi diam terlihat lengkap, termasuk judul banner dan output terminal),
   - cek tidak ada teks bertabrakan atau terpotong, terutama di `traffic.svg` dan panel kanan `skills.svg`.
4. Perbaiki sendiri semua masalah visual yang ketemu sebelum melapor.
5. Laporan akhir singkat: file apa saja yang dibuat, apa yang harus diganti user (`USERNAME`, email, level skill), dan langkah menjalankan workflow snake pertama kali (tab Actions, "Generate snake", Run workflow).

## 7. CHECKLIST PENERIMAAN

- [ ] 7 file persis seperti daftar di bagian 5, struktur folder benar.
- [ ] Semua SVG valid XML, tanpa script / foreignObject / resource eksternal.
- [ ] Tidak ada layanan kartu pasaran, badge, emoji dekoratif, atau kalimat generik.
- [ ] Setiap animasi punya makna network/sysadmin.
- [ ] Judul banner terbaca "FANOPS" dalam piksel dan tetap terlihat saat reduced-motion.
- [ ] Terminal tetap terbaca lengkap saat reduced-motion.
- [ ] Skill tree punya fallback tabel teks di README.
- [ ] `USERNAME` dan email jelas ditandai sebagai placeholder.
- [ ] Kucing adalah karakter orisinal.

## 8. IDE ITERASI (setelah versi pertama jadi)

Hanya kerjakan kalau diminta: tambah avatar user sebagai node di skill tree; mode terang khusus lewat `<picture>`; panel "now playing" sysadmin (job cron, backup OK) dengan data statis yang jelas berlabel contoh; versi pendek banner untuk tampilan mobile.

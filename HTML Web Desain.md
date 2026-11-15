<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Alya Afiyah · Junior Network Administrator</title>
<link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;600;700&family=Fira+Code:wght@400;500&display=swap" rel="stylesheet">
<style>
:root{--bg:#f4ecdf;--bg2:#e6d9c3;--ink:#2d2118;--mut:#6d5a49;--card:#fffaf0;--line:#d8c6a8;--acc:#d9622b;--acc2:#1f8f6b;--sky1:#f9d9a8;--sky2:#c9dfd3;--hill1:#9fb88f;--hill2:#6f9a6c;--hill3:#3f6b4a;--stone:#8b8579;
box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#12161d;--bg2:#1a2130;--ink:#f1e8d8;--mut:#a99d8a;--card:#1b2230;--line:#2f3a4f;--acc:#ff8a4c;--acc2:#5ef0c0;--sky1:#3b2a55;--sky2:#0e1a2b;--hill1:#2c4a47;--hill2:#1f3a3a;--hill3:#122a2a;--stone:#4b5566}}
:root[data-theme="dark"]{--bg:#12161d;--bg2:#1a2130;--ink:#f1e8d8;--mut:#a99d8a;--card:#1b2230;--line:#2f3a4f;--acc:#ff8a4c;--acc2:#5ef0c0;--sky1:#3b2a55;--sky2:#0e1a2b;--hill1:#2c4a47;--hill2:#1f3a3a;--hill3:#122a2a;--stone:#4b5566}
*{box-sizing:border-box;margin:0}
html{scroll-behavior:smooth;scroll-padding-top:calc(70px + env(safe-area-inset-top,0px));height:100%}
body{font-family:'Fredoka',system-ui,sans-serif;background:var(--bg);color:var(--ink);min-height:100%;overflow-x:hidden}
body::before{content:"";position:fixed;inset:0 0 auto;height:env(safe-area-inset-top,0px);z-index:60;background:var(--bg)}
a{color:inherit;text-decoration:none}
nav{position:sticky;top:env(safe-area-inset-top,0px);z-index:50;display:flex;align-items:center;gap:8px;padding:10px 5vw;background:color-mix(in srgb,var(--bg) 82%,transparent);backdrop-filter:blur(10px);border-bottom:2px dashed var(--line)}
.logo{font-weight:700;font-size:1.2rem;margin-right:auto}.logo b{color:var(--acc)}
nav ul{display:flex;gap:4px;list-style:none;padding:0}
nav ul a{padding:8px 14px;border-radius:99px;font-weight:600;font-size:.92rem;transition:.25s}
nav ul a:hover,nav ul a.on{background:var(--acc);color:#fff;transform:translateY(-2px) rotate(-2deg)}
.ico{border:2px solid var(--line);background:var(--card);color:var(--ink);border-radius:50%;width:38px;height:38px;font-size:1rem;cursor:pointer}
#burger{display:none}
@media(max-width:820px){#burger{display:block}nav ul{display:none;position:absolute;top:100%;left:0;right:0;flex-direction:column;background:var(--bg);padding:10px 5vw;border-bottom:2px dashed var(--line)}nav ul.open{display:flex}}
#home{position:relative;min-height:92vh;overflow:hidden;background:linear-gradient(var(--sky2),var(--sky1))}
#scene{position:absolute;inset:0;width:100%;height:100%}
#fx{position:absolute;inset:0;width:100%;height:100%;pointer-events:none}
.px{transition:transform .2s ease-out}
.hero{position:relative;z-index:3;display:grid;grid-template-columns:1.1fr .9fr;align-items:center;min-height:92vh;padding:60px 6vw 40px;gap:20px}
.hero h1{font-size:clamp(2.2rem,6vw,4.4rem);line-height:1.05;color:#fff;text-shadow:0 4px 0 rgba(0,0,0,.25)}
.hero h1 span{color:#ffd166}
.tag{display:inline-block;background:rgba(0,0,0,.35);color:#fff;padding:6px 14px;border-radius:99px;font-size:.9rem;margin-bottom:14px;backdrop-filter:blur(6px)}
.type{font-family:'Fira Code',monospace;color:#fff;background:rgba(0,0,0,.4);display:inline-block;padding:8px 14px;border-radius:12px;margin:18px 0;min-height:42px}
.type::after{content:"▍";animation:bl 1s steps(1) infinite;color:#5ef0c0}
@keyframes bl{50%{opacity:0}}
.btn{display:inline-block;padding:12px 24px;border-radius:99px;background:var(--acc);color:#fff;font-weight:700;box-shadow:0 6px 0 rgba(0,0,0,.25);transition:.2s;margin-right:8px}
.btn:hover{transform:translateY(3px);box-shadow:0 3px 0 rgba(0,0,0,.25)}
.btn.g{background:rgba(255,255,255,.2);backdrop-filter:blur(6px)}
.main-kid{width:min(100%,380px);justify-self:center;filter:drop-shadow(0 18px 14px rgba(0,0,0,.35));animation:fl 4s ease-in-out infinite}
@media(max-width:820px){.hero{grid-template-columns:1fr;text-align:center}.main-kid{width:240px}}
@keyframes fl{50%{transform:translateY(-14px) rotate(2deg)}}
.kid{overflow:visible}
.eyes{transform-box:fill-box;transform-origin:center;animation:blink 4.5s infinite}
@keyframes blink{0%,94%,100%{transform:scaleY(1)}97%{transform:scaleY(.1)}}
.wave{transform-box:fill-box;transform-origin:50% 100%;animation:wv 1.1s ease-in-out infinite}
@keyframes wv{50%{transform:rotate(-22deg)}}
.scr{animation:glow 2s ease-in-out infinite}
@keyframes glow{50%{opacity:.55}}
section{padding:80px 6vw;position:relative}
h2{font-size:clamp(1.8rem,4vw,2.8rem);margin-bottom:8px}
h2::after{content:"";display:block;width:90px;height:8px;border-radius:9px;margin-top:6px;background:repeating-linear-gradient(90deg,var(--acc) 0 14px,var(--acc2) 14px 28px)}
.sub{color:var(--mut);margin-bottom:34px;max-width:60ch}
.rv{opacity:0;transform:translateY(34px) scale(.97);transition:.7s cubic-bezier(.2,.8,.2,1)}
.rv.in{opacity:1;transform:none}
.grid{display:grid;gap:18px;grid-template-columns:repeat(auto-fit,minmax(230px,1fr))}
.card{background:var(--card);border:2px solid var(--line);border-radius:22px;padding:22px;box-shadow:0 8px 0 var(--line);transition:transform .15s,box-shadow .3s;transform-style:preserve-3d}
.card:hover{box-shadow:0 14px 0 var(--acc)}
.card .e{font-size:2rem;display:block;margin-bottom:8px}
.card h3{margin-bottom:6px}.card p{color:var(--mut);font-size:.94rem;line-height:1.5}
#about{background:var(--bg2)}
.about{display:grid;grid-template-columns:.8fr 1.2fr;gap:30px;align-items:center}
@media(max-width:820px){.about{grid-template-columns:1fr}}
.id{background:var(--card);border:3px solid var(--ink);border-radius:26px;padding:24px;position:relative;transform:rotate(-1.5deg)}
.id::before{content:"📎";position:absolute;top:-16px;left:50%;font-size:1.8rem}
.id dl{display:grid;grid-template-columns:auto 1fr;gap:6px 14px;margin-top:8px}.id dt{color:var(--mut)}.id dd{font-weight:600}
.squad{display:grid;gap:14px;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));margin-top:30px}
.squad figure{background:var(--card);border-radius:24px;border:2px solid var(--line);padding:14px 8px 10px;text-align:center;transition:.3s}
.squad figure:hover{transform:translateY(-10px) rotate(2deg)}
.squad svg{width:120px;height:auto;animation:fl 3.5s ease-in-out infinite}
.squad figure:nth-child(2n) svg{animation-delay:-1.2s}.squad figure:nth-child(3n) svg{animation-delay:-2.4s}
.squad figcaption{font-weight:600;font-size:.9rem;margin-top:4px}
.tl{display:grid;gap:18px}
.step{display:grid;grid-template-columns:52px 1fr;gap:16px;background:var(--card);border:2px solid var(--line);border-radius:22px;padding:20px}
.num{width:52px;height:52px;border-radius:50%;background:var(--acc);color:#fff;display:grid;place-items:center;font-weight:700;font-size:1.3rem;box-shadow:0 0 0 6px color-mix(in srgb,var(--acc) 25%,transparent)}
.step p{color:var(--mut);font-size:.94rem;line-height:1.5;margin:4px 0 10px}
.bar{height:14px;background:var(--bg2);border-radius:99px;overflow:hidden;border:2px solid var(--line)}
.bar i{display:block;height:100%;width:0;border-radius:99px;background:repeating-linear-gradient(45deg,var(--acc2) 0 10px,color-mix(in srgb,var(--acc2) 70%,#fff) 10px 20px);transition:width 1.6s cubic-bezier(.2,.8,.2,1);background-size:28px 28px;animation:mv 1s linear infinite}
@keyframes mv{to{background-position:28px 0}}
.pct{font-family:'Fira Code',monospace;font-size:.8rem;color:var(--mut);float:right}
.chip{display:inline-block;font-size:.75rem;background:var(--bg2);border-radius:99px;padding:3px 10px;margin:2px 4px 0 0;font-family:'Fira Code',monospace}
.two{display:grid;grid-template-columns:1fr 1fr;gap:20px;margin-top:30px}
@media(max-width:820px){.two{grid-template-columns:1fr}}
.term{background:#10151c;color:#d6ffe9;border-radius:18px;padding:16px;font-family:'Fira Code',monospace;font-size:.78rem;line-height:1.7;overflow-x:auto;border:2px solid #263041;white-space:pre}
.term .d{color:#ff9d6b}.term .c{color:#7d8aa0}
.topo{background:var(--card);border:2px solid var(--line);border-radius:18px;padding:10px;overflow-x:auto}
.topo svg{min-width:420px;width:100%;height:auto}
.topo text{fill:var(--ink);font-family:'Fredoka';font-size:12px}
.dash{stroke-dasharray:6 6;animation:dsh 1s linear infinite}@keyframes dsh{to{stroke-dashoffset:-12}}
.ct a.card{display:block}.ct a.card:hover{transform:translateY(-6px) rotate(-1deg)}
footer{text-align:center;padding:30px;color:var(--mut);border-top:2px dashed var(--line)}
#glow{position:fixed;width:300px;height:300px;margin:-150px 0 0 -150px;border-radius:50%;pointer-events:none;z-index:1;background:radial-gradient(circle,rgba(255,170,90,.22),transparent 65%);mix-blend-mode:screen}
@media(prefers-reduced-motion:reduce){*{animation:none!important;transition:none!important}.rv{opacity:1;transform:none}}
</style>
</head>
<body>
<div id="glow"></div>

<!-- Karakter boneka (kartun orisinal). Warna diatur lewat --hair, --cloth, --scarf -->
<svg width="0" height="0" style="position:absolute" aria-hidden="true">
<defs>
<symbol id="kid" viewBox="0 0 120 150">
<path d="M26 150Q22 100 60 96Q98 100 94 150Z" fill="var(--cloth)"/>
<path d="M36 100Q60 116 84 100L80 92Q60 104 40 92Z" fill="var(--scarf)"/>
<path d="M44 104l6 8M56 108l5 9M68 108l5 8" stroke="#fff" stroke-opacity=".45" stroke-width="2.5" stroke-linecap="round"/>
<ellipse cx="60" cy="58" rx="36" ry="34" fill="#f7d9c4"/>
<path d="M22 58Q18 14 60 16Q102 14 98 58Q94 38 80 34Q70 46 56 38Q40 42 30 62Z" fill="var(--hair)"/>
<path d="M92 28l3 7 7 1-5 5 1 7-6-4-6 4 1-7-5-5 7-1z" fill="#ffd166"/>
<g class="eyes"><ellipse cx="45" cy="62" rx="7.5" ry="8.5" fill="#2a1d1d"/><ellipse cx="75" cy="62" rx="7.5" ry="8.5" fill="#2a1d1d"/>
<circle cx="47.5" cy="59" r="2.6" fill="#fff"/><circle cx="77.5" cy="59" r="2.6" fill="#fff"/>
<path d="M36 61Q45 50 54 61Z" fill="#f7d9c4"/><path d="M66 61Q75 50 84 61Z" fill="#f7d9c4"/></g>
<ellipse cx="35" cy="76" rx="6" ry="3.5" fill="#ff8fa0" opacity=".65"/><ellipse cx="85" cy="76" rx="6" ry="3.5" fill="#ff8fa0" opacity=".65"/>
<path d="M55 79Q60 83 65 79" stroke="#8a4b3c" stroke-width="2.2" fill="none" stroke-linecap="round"/>
</symbol>
<symbol id="lap" viewBox="0 0 120 150">
<rect x="26" y="102" width="68" height="40" rx="5" fill="#2b3340"/>
<rect x="30" y="106" width="60" height="32" rx="3" fill="#0e2a33"/>
<text class="scr" x="60" y="127" text-anchor="middle" font-size="14" font-family="Fira Code,monospace" fill="#5ef0c0">&lt;/&gt;</text>
<path d="M18 142h84l-4 7H22z" fill="#a4adbb"/>
<circle cx="28" cy="126" r="6.5" fill="#f7d9c4"/><circle cx="92" cy="126" r="6.5" fill="#f7d9c4"/>
</symbol>
</defs>
</svg>

<nav>
  <a href="#home" class="logo">Alya<b>.</b>net 🦊</a>
  <ul id="menu">
    <li><a href="#home" class="on">Home</a></li>
    <li><a href="#about">About Me</a></li>
    <li><a href="#belajar">Belajar TKJ</a></li>
    <li><a href="#ukom">Progress UKK</a></li>
    <li><a href="#kontak">Kontak</a></li>
  </ul>
  <button class="ico" id="theme" title="Ganti tema">🌙</button>
  <button class="ico" id="burger" aria-label="Menu">☰</button>
</nav>

<!-- HOME -->
<header id="home">
  <svg id="scene" viewBox="0 0 1440 700" preserveAspectRatio="xMidYMax slice" aria-hidden="true">
    <circle cx="1130" cy="140" r="64" fill="#fff6dc" opacity=".9"/>
    <g class="px" data-d="6"><path d="M0 430L170 250 280 360 430 200 600 380 760 270 930 400 1100 240 1280 380 1440 300V700H0Z" fill="var(--hill1)" opacity=".75"/></g>
    <g class="px" data-d="14"><path d="M0 500Q200 380 420 470T820 450T1180 470T1440 420V700H0Z" fill="var(--hill2)"/>
      <g fill="var(--stone)"><rect x="640" y="380" width="34" height="86" rx="12"/><rect x="700" y="364" width="40" height="100" rx="14"/><rect x="768" y="384" width="32" height="80" rx="11"/><rect x="826" y="396" width="28" height="64" rx="10"/></g>
      <g fill="#1d4a37"><path d="M140 470l30-110 30 110z"/><path d="M200 480l24-90 24 90z"/><path d="M1180 470l28-100 28 100z"/><path d="M1240 480l22-80 22 80z"/></g></g>
    <g class="px" data-d="26"><path d="M0 590Q260 500 560 570T1100 560T1440 540V700H0Z" fill="var(--hill3)"/>
      <g fill="#ffd166" opacity=".9"><circle cx="300" cy="590" r="4"/><circle cx="1020" cy="580" r="4"/></g>
      <g transform="translate(1250 520)"><circle r="46" fill="#fff" opacity=".9"/><circle r="34" fill="#d9622b"/><circle r="22" fill="#fff"/><circle r="10" fill="#d9622b"/><path d="M-8 30L40 -40" stroke="#4a2f1b" stroke-width="5"/></g></g>
  </svg>
  <canvas id="fx"></canvas>
  <div class="hero">
    <div>
      <span class="tag">🏴 Highland Network Adventure</span>
      <h1>Halo, aku <span>Alya Afiyah</span><br>Calon Junior Network Administrator</h1>
      <div class="type" id="typed"></div><br>
      <a class="btn" href="#ukom">Lihat Progress UKK</a><a class="btn g" href="#about">Kenalan dulu</a>
    </div>
    <svg class="main-kid kid" viewBox="-10 0 150 150" style="--hair:#3a2416;--cloth:#d9622b;--scarf:#1f8f6b">
      <use href="#kid" width="120" height="150"/><use href="#lap" width="120" height="150"/>
      <g class="wave"><path d="M96 104L106 70" stroke="#d9622b" stroke-width="11" stroke-linecap="round"/><circle cx="107" cy="66" r="8" fill="#f7d9c4"/></g>
    </svg>
  </div>
</header>

<!-- ABOUT -->
<section id="about">
  <div class="about">
    <div class="id rv">
      <h3>🪪 Kartu Siswa</h3>
      <dl><dt>Nama</dt><dd>Alya Afiyah</dd><dt>Kelas</dt><dd>12 TKJ A</dd><dt>Sekolah</dt><dd>SMKN 1 Kota Bekasi</dd><dt>Jurusan</dt><dd>Teknik Komputer &amp; Jaringan</dd><dt>Sedang</dt><dd>UKK Junior Network Administrator</dd></dl>
    </div>
    <div class="rv">
      <h2>About Me</h2>
      <p class="sub">Hai! Aku Alya, siswa kelas 12 TKJ A di SMKN 1 Kota Bekasi. Sekarang aku lagi menjalani Uji Kompetensi Kejuruan (UKK) dengan skema Junior Network Administrator: membangun jaringan, server, database, sampai website dari nol. Dunia jaringan itu seru, apalagi kalau semuanya akhirnya connected! ✨</p>
    </div>
  </div>
  <div class="squad">
    <figure class="rv"><svg class="kid" viewBox="0 0 120 150" style="--hair:#3a2416;--cloth:#d9622b;--scarf:#1f8f6b"><use href="#kid" width="120" height="150"/><use href="#lap" width="120" height="150"/></svg><figcaption>Ngoding santai</figcaption></figure>
    <figure class="rv"><svg class="kid" viewBox="0 0 120 150" style="--hair:#7a2e12;--cloth:#3b6ea8;--scarf:#ffd166"><use href="#kid" width="120" height="150"/><use href="#lap" width="120" height="150"/><g class="wave"><path d="M96 104L106 70" stroke="#3b6ea8" stroke-width="11" stroke-linecap="round"/><circle cx="107" cy="66" r="8" fill="#f7d9c4"/></g></svg><figcaption>Halo, dunia!</figcaption></figure>
    <figure class="rv"><svg class="kid" viewBox="0 0 120 150" style="--hair:#23314a;--cloth:#7a4fa3;--scarf:#ff8fa0"><use href="#kid" width="120" height="150"/><g><rect x="24" y="116" width="72" height="26" rx="8" fill="#2b3340"/><circle cx="40" cy="129" r="3.5" fill="#5ef0c0" class="scr"/><circle cx="52" cy="129" r="3.5" fill="#ffd166" class="scr"/><path d="M78 118V104M64 118V108" stroke="#2b3340" stroke-width="4" stroke-linecap="round"/><path d="M60 100q4-6 8 0M56 96q8-10 16 0" stroke="#5ef0c0" stroke-width="2.5" fill="none" stroke-linecap="round"/><circle cx="26" cy="126" r="6.5" fill="#f7d9c4"/><circle cx="94" cy="126" r="6.5" fill="#f7d9c4"/></g></svg><figcaption>Pasang router</figcaption></figure>
    <figure class="rv"><svg class="kid" viewBox="0 0 120 150" style="--hair:#1f1a17;--cloth:#2e6f4e;--scarf:#d9622b"><use href="#kid" width="120" height="150"/><use href="#lap" width="120" height="150"/><path d="M14 40l4 8 9 1-6 6 2 9-9-5-8 5 2-9-6-6 9-1z" fill="#ffd166"/></svg><figcaption>Lulus UKK ⭐</figcaption></figure>
  </div>
</section>

<!-- BELAJAR TKJ -->
<section id="belajar">
  <h2 class="rv">Apa yang Dipelajari di TKJ?</h2>
  <p class="sub rv">Bukan cuma pasang kabel! TKJ mempelajari cara membangun, mengelola, dan mengamankan jaringan komputer.</p>
  <div class="grid">
    <div class="card rv"><span class="e">🌐</span><h3>Dasar Jaringan</h3><p>Model OSI &amp; TCP/IP, pengalamatan IP, subnetting, dan cara data berjalan antar perangkat.</p></div>
    <div class="card rv"><span class="e">📡</span><h3>Router &amp; Mikrotik</h3><p>Konfigurasi routing, DHCP, NAT, firewall, hotspot, dan manajemen bandwidth.</p></div>
    <div class="card rv"><span class="e">🖥️</span><h3>Administrasi Server</h3><p>Instalasi dan pengelolaan server Linux (Debian/Ubuntu) beserta layanan jaringannya.</p></div>
    <div class="card rv"><span class="e">🗄️</span><h3>Basis Data</h3><p>Membuat dan mengelola database dengan MySQL/MariaDB, termasuk query SQL.</p></div>
    <div class="card rv"><span class="e">🕸️</span><h3>Web Server</h3><p>Menjalankan website di Apache/Nginx dan PHP, lalu menghubungkannya ke database.</p></div>
    <div class="card rv"><span class="e">🔌</span><h3>Kabel &amp; Perangkat</h3><p>Crimping UTP, memasang switch, access point, hingga dasar fiber optic.</p></div>
    <div class="card rv"><span class="e">🛡️</span><h3>Keamanan Jaringan</h3><p>Mengamankan akses dengan firewall, user management, dan password policy.</p></div>
    <div class="card rv"><span class="e">🔧</span><h3>Troubleshooting</h3><p>Mencari penyebab jaringan putus dengan ping, traceroute, dan analisis log.</p></div>
  </div>
</section>

<!-- PROGRESS UKK -->
<section id="ukom" style="background:var(--bg2)">
  <h2 class="rv">Progress UKK</h2>
  <p class="sub rv">Skema Junior Network Administrator: dari konfigurasi router sampai website yang bisa diakses client.</p>
  <div class="tl">
    <div class="step rv"><div class="num">1</div><div><h3>Konfigurasi Router Mikrotik <span class="pct">0%</span></h3><p>Router jadi gerbang jaringan: hotspot untuk client wireless dan koneksi LAN untuk client kabel.</p><div class="bar"><i data-p="85"></i></div><span class="chip">hotspot</span><span class="chip">client kabel</span><span class="chip">DHCP</span><span class="chip">NAT</span></div></div>
    <div class="step rv"><div class="num">2</div><div><h3>Instalasi Server Ubuntu <span class="pct">0%</span></h3><p>Memasang Ubuntu Server, mengatur IP address, SSH, dan update paket.</p><div class="bar"><i data-p="70"></i></div><span class="chip">Ubuntu Server</span><span class="chip">SSH</span><span class="chip">IP statis</span></div></div>
    <div class="step rv"><div class="num">3</div><div><h3>Database <span class="pct">0%</span></h3><p>Instalasi database server, membuat database, user, dan tabel untuk website.</p><div class="bar"><i data-p="50"></i></div><span class="chip">MariaDB / MySQL</span><span class="chip">SQL</span></div></div>
    <div class="step rv"><div class="num">4</div><div><h3>Website <span class="pct">0%</span></h3><p>Web server berjalan, halaman web tersambung ke database dan bisa dibuka dari client.</p><div class="bar"><i data-p="30"></i></div><span class="chip">Apache / Nginx</span><span class="chip">PHP</span></div></div>
  </div>
  <div class="two">
    <div class="topo rv">
      <svg viewBox="0 0 440 250"><g fill="none" stroke="var(--acc2)" stroke-width="3"><path class="dash" d="M60 125H150M230 125H300M230 125L300 60M230 125L300 190M300 190H370"/></g>
        <g font-size="26" text-anchor="middle"><text x="40" y="133">☁️</text><text x="190" y="133">📡</text><text x="330" y="66">📶</text><text x="330" y="196">🔀</text><text x="400" y="196">💻</text><text x="400" y="66">📱</text></g>
        <text x="40" y="160" text-anchor="middle">Internet</text><text x="190" y="160" text-anchor="middle">Mikrotik</text><text x="330" y="92" text-anchor="middle">Hotspot</text><text x="330" y="222" text-anchor="middle">Switch</text><text x="400" y="222" text-anchor="middle">Client kabel</text>
        <g><rect x="150" y="180" width="70" height="34" rx="8" fill="var(--bg2)" stroke="var(--line)"/><text x="185" y="202" text-anchor="middle">🖥️ Server</text><path class="dash" d="M190 135V180" stroke="var(--acc)" stroke-width="3"/></g></svg>
    </div>
    <div class="term rv" id="term"></div>
  </div>
</section>

<!-- KONTAK -->
<section id="kontak">
  <h2 class="rv">Kontak Person</h2>
  <p class="sub rv">Mau diskusi soal jaringan atau sekadar menyapa? Hubungi aku di sini.</p>
  <div class="grid ct">
    <a class="card rv" href="https://wa.me/6285813556689"><span class="e">💬</span><h3>WhatsApp</h3><p>0858-1355-6689</p></a>
    <a class="card rv" href="mailto:alyaaafiyah77@gmail.com"><span class="e">✉️</span><h3>Email</h3><p>alyaaafiyah77@gmail.com</p></a>
    <a class="card rv" href="https://instagram.com/allyayayaaa"><span class="e">📸</span><h3>Instagram</h3><p>@allyayayaaa</p></a>
    <a class="card rv" href="https://github.com/alyaafiyah"><span class="e">🐙</span><h3>GitHub</h3><p>github.com/alyaafiyah</p></a>
  </div>
</section>
<footer>Dibuat dengan ☕ dan jaringan stabil oleh Alya Afiyah · 12 TKJ A · SMKN 1 Kota Bekasi</footer>

<script>
(function(){
var $=function(s,c){return(c||document).querySelector(s)},$$=function(s,c){return[].slice.call((c||document).querySelectorAll(s))};
var root=document.documentElement;
/* tema */
var tb=$('#theme');
try{var t=localStorage.getItem('th');if(t)root.setAttribute('data-theme',t)}catch(e){}
tb.onclick=function(){var d=root.getAttribute('data-theme')==='dark'||(!root.getAttribute('data-theme')&&matchMedia('(prefers-color-scheme:dark)').matches);var n=d?'light':'dark';root.setAttribute('data-theme',n);try{localStorage.setItem('th',n)}catch(e){}};
/* menu */
$('#burger').onclick=function(){$('#menu').classList.toggle('open')};
$$('#menu a').forEach(function(a){a.onclick=function(){$('#menu').classList.remove('open')}});
/* reveal + progress */
var io=new IntersectionObserver(function(es){es.forEach(function(e){if(e.isIntersecting){e.target.classList.add('in');var b=$('.bar i',e.target);if(b){b.style.width=b.dataset.p+'%';var p=$('.pct',e.target),n=0,v=+b.dataset.p,iv=setInterval(function(){n+=2;if(n>=v){n=v;clearInterval(iv)}p.textContent=n+'%'},25)}io.unobserve(e.target)}})},{threshold:.2});
$$('.rv').forEach(function(el){io.observe(el)});
/* nav aktif */
var secs=$$('header,section'),links=$$('#menu a');
addEventListener('scroll',function(){var y=scrollY+120,c=0;secs.forEach(function(s,i){if(s.offsetTop<=y)c=i});links.forEach(function(a,i){a.classList.toggle('on',i===c)})},{passive:true});
/* ketik */
var words=['Mikrotik hotspot ✔','Ubuntu Server ✔','Database MariaDB ✔','Website siap online 🚀','Network is my playground'],wi=0,ci=0,del=false,ty=$('#typed');
(function tick(){var w=words[wi];ty.textContent=w.slice(0,ci);if(!del&&ci===w.length){del=true;return setTimeout(tick,1400)}if(del&&ci===0){del=false;wi=(wi+1)%words.length}ci+=del?-1:1;setTimeout(tick,del?35:70)})();
/* terminal */
var lines=['<span class="c"># Mikrotik: hotspot untuk client wifi</span>','<span class="d">/ip hotspot setup</span>','<span class="c"># DHCP untuk client kabel</span>','<span class="d">/ip dhcp-server setup</span>','<span class="c"># Ubuntu Server: web + database</span>','<span class="d">$</span> sudo apt install apache2 mariadb-server php','<span class="d">$</span> sudo systemctl enable --now apache2'],tm=$('#term'),li=0;
var tio=new IntersectionObserver(function(e){if(e[0].isIntersecting){tio.disconnect();(function nx(){if(li<lines.length){tm.innerHTML+=lines[li++]+'\n';setTimeout(nx,550)}})()}});tio.observe(tm);
/* parallax + glow */
var px=$$('.px'),g=$('#glow');
addEventListener('mousemove',function(e){g.style.transform='translate('+e.clientX+'px,'+e.clientY+'px)';var x=e.clientX/innerWidth-.5,y=e.clientY/innerHeight-.5;px.forEach(function(p){var d=+p.dataset.d;p.style.transform='translate('+(-x*d)+'px,'+(-y*d/2)+'px)'});mx=e.clientX;my=e.clientY});
/* tilt */
$$('.card').forEach(function(c){c.addEventListener('mousemove',function(e){var r=c.getBoundingClientRect(),x=(e.clientX-r.left)/r.width-.5,y=(e.clientY-r.top)/r.height-.5;c.style.transform='perspective(600px) rotateY('+x*14+'deg) rotateX('+(-y*14)+'deg) translateY(-4px)'});c.addEventListener('mouseleave',function(){c.style.transform=''})});
/* kunang-kunang / will-o-wisps */
var cv=$('#fx'),cx=cv.getContext('2d'),W,H,ps=[],mx=-999,my=-999;
function rs(){W=cv.width=cv.offsetWidth;H=cv.height=cv.offsetHeight}rs();addEventListener('resize',rs);
for(var i=0;i<55;i++)ps.push({x:Math.random()*2000,y:Math.random()*900,r:Math.random()*2.5+1,s:Math.random()*.6+.2,a:Math.random()*6.28,c:Math.random()>.5?'94,240,192':'255,209,102'});
(function an(){cx.clearRect(0,0,W,H);ps.forEach(function(p){p.a+=.02;p.x+=Math.sin(p.a)*.5+p.s*.3;p.y-=p.s*.35;var d=Math.hypot(p.x-mx,p.y-my);if(d<110){p.x+=(p.x-mx)/d*2;p.y+=(p.y-my)/d*2}if(p.y<-10){p.y=H+10;p.x=Math.random()*W}if(p.x>W+10)p.x=-10;if(p.x<-10)p.x=W;var o=.5+Math.sin(p.a*2)*.4;cx.beginPath();cx.fillStyle='rgba('+p.c+','+o+')';cx.shadowColor='rgb('+p.c+')';cx.shadowBlur=14;cx.arc(p.x,p.y,p.r,0,6.28);cx.fill()});requestAnimationFrame(an)})();
})();
</script>
</body>
</html>

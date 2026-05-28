<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>GUICHET 9 — Jeu de société</title>
  <meta name="description" content="GUICHET 9, un jeu semi-coopératif d’administration absurde, de négociation et de dossiers secrets." />
  <style>
    :root{
      --paper:#f3eadb;
      --paper-2:#efe2cf;
      --ink:#171514;
      --muted:#6e5f50;
      --red:#a92820;
      --red-dark:#671813;
      --line:#26201b;
      --blue:#1f3549;
      --black:#111;
      --shadow:0 18px 45px rgba(40,25,12,.18);
    }
    *{box-sizing:border-box}
    html{scroll-behavior:smooth}
    body{
      margin:0;
      color:var(--ink);
      background:
        radial-gradient(circle at 15% 10%, rgba(255,255,255,.65), transparent 32%),
        linear-gradient(135deg, var(--paper), var(--paper-2));
      font-family: Impact, Haettenschweiler, "Arial Narrow Bold", system-ui, sans-serif;
      letter-spacing:.02em;
    }
    body:before{
      content:"";
      position:fixed;
      inset:0;
      pointer-events:none;
      opacity:.13;
      background-image:
        linear-gradient(rgba(60,35,20,.18) 1px, transparent 1px),
        linear-gradient(90deg, rgba(60,35,20,.12) 1px, transparent 1px);
      background-size:42px 42px,42px 42px;
      mix-blend-mode:multiply;
    }
    a{color:inherit;text-decoration:none}
    .topbar{
      position:sticky;
      top:0;
      z-index:20;
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:24px;
      padding:18px clamp(18px,4vw,64px);
      background:rgba(246,237,222,.91);
      border-bottom:3px solid var(--ink);
      backdrop-filter:blur(8px);
    }
    .logo{
      display:inline-flex;
      align-items:center;
      border:3px solid var(--ink);
      padding:5px 10px 3px;
      font-size:clamp(28px,4vw,52px);
      line-height:.9;
      letter-spacing:.03em;
      background:#f8f0e4;
      box-shadow:inset 0 0 0 2px rgba(0,0,0,.12);
    }
    .logo span{
      display:inline-flex;
      margin-left:7px;
      padding:4px 9px;
      color:#fff;
      background:var(--red);
      border:2px solid var(--red-dark);
      box-shadow:inset 0 0 0 2px rgba(255,255,255,.15);
    }
    .nav{
      display:flex;
      align-items:center;
      gap:clamp(16px,3vw,44px);
      font-size:18px;
      text-transform:uppercase;
    }
    .nav a{padding:8px 0;border-bottom:2px solid transparent}
    .nav a:hover{border-bottom-color:var(--red)}
    .stamp-small{
      display:block;
      padding:10px 18px;
      color:var(--red-dark);
      border:2px solid var(--red-dark);
      text-align:center;
      transform:rotate(.4deg);
      font-family:"Courier New", monospace;
      font-weight:800;
      background:rgba(255,255,255,.28);
    }
    .stamp-small strong{display:block;font-size:20px;letter-spacing:.08em}
    .stamp-small em{display:block;font-size:11px;font-style:normal}
    .hero{
      min-height:760px;
      display:grid;
      grid-template-columns: .92fr 1.08fr;
      gap:38px;
      padding:58px clamp(18px,4vw,70px) 34px;
      border-bottom:3px solid var(--ink);
      position:relative;
      overflow:hidden;
    }
    .hero-copy{padding-top:20px;max-width:780px}
    .eyebrow{
      color:var(--red-dark);
      font-family:"Courier New", monospace;
      font-weight:900;
      text-align:center;
      margin:0 0 16px;
      position:relative;
    }
    .eyebrow:before,.eyebrow:after{
      content:"";
      display:inline-block;
      width:150px;
      border-top:2px solid var(--red-dark);
      margin:0 18px 5px;
      opacity:.7;
    }
    h1{
      margin:0;
      font-size:clamp(72px,11vw,160px);
      line-height:.82;
      letter-spacing:.015em;
      text-transform:uppercase;
      text-shadow:2px 2px 0 rgba(0,0,0,.08);
    }
    h1 .nine{
      display:inline-block;
      color:#fff;
      background:var(--red);
      padding:.04em .13em .07em;
      border-radius:6px;
      box-shadow:inset 0 0 0 4px rgba(255,255,255,.08);
    }
    .tagline{
      margin:28px 0 24px;
      font-size:clamp(24px,3vw,38px);
      line-height:1.15;
      max-width:850px;
      text-transform:uppercase;
    }
    .claim{
      display:inline-block;
      margin:8px 0 26px;
      padding:16px 22px 14px;
      border:3px solid var(--red-dark);
      color:var(--red-dark);
      background:rgba(255,255,255,.22);
      box-shadow:inset 0 0 0 2px rgba(255,255,255,.3);
      font-family:"Courier New", monospace;
      font-weight:900;
      font-size:clamp(18px,2vw,29px);
      text-transform:uppercase;
    }
    .claim strong{display:block;font-family:Impact, Haettenschweiler, "Arial Narrow Bold", sans-serif;font-size:1.35em;letter-spacing:.03em}
    .facts{display:flex;flex-wrap:wrap;gap:18px;margin:0 0 28px}
    .fact{
      display:flex;
      align-items:center;
      gap:12px;
      min-width:150px;
      padding:13px 20px 10px;
      border:3px solid var(--ink);
      background:rgba(255,255,255,.28);
      box-shadow:0 4px 0 rgba(0,0,0,.08);
    }
    .fact b{font-size:31px;line-height:.9}
    .fact small{font-size:14px;line-height:1;display:block;text-transform:uppercase}
    .ico{font-size:30px;line-height:1;filter:grayscale(1) contrast(1.3)}
    .actions{display:flex;flex-wrap:wrap;gap:18px}
    .btn{
      display:inline-flex;
      align-items:center;
      justify-content:center;
      min-height:62px;
      padding:0 30px;
      border:3px solid var(--ink);
      background:#f4eadb;
      color:var(--ink);
      font-size:22px;
      text-transform:uppercase;
      box-shadow:0 5px 0 rgba(0,0,0,.12);
    }
    .btn.primary{background:var(--red);border-color:var(--red-dark);color:#fff}
    .btn:hover{transform:translateY(1px);box-shadow:0 3px 0 rgba(0,0,0,.16)}
    .table-scene{
      position:relative;
      min-height:620px;
      perspective:1200px;
    }
    .board{
      position:absolute;
      left:16%; top:74px;
      width:min(720px,78%);
      aspect-ratio:1.32;
      background:#eadbc6;
      border:4px solid #332820;
      box-shadow:var(--shadow);
      transform:rotate(5deg) skewX(-2deg);
      padding:24px;
    }
    .saturation{
      display:grid;
      grid-template-columns:110px repeat(10,1fr);
      align-items:center;
      border:2px solid #342820;
      margin-bottom:24px;
      font-family:"Courier New", monospace;
      font-weight:900;
      font-size:14px;
    }
    .saturation div{border-left:1px solid #342820;padding:7px;text-align:center;min-height:34px}
    .saturation .label{border-left:0;text-align:left;color:var(--red-dark)}
    .peg{position:absolute;right:31%;top:42px;width:25px;height:25px;background:var(--red);border-radius:50%;box-shadow:0 8px 10px rgba(0,0,0,.25)}
    .board-logo{text-align:center;font-size:54px;border:2px solid #342820;width:max-content;margin:0 auto 24px;padding:3px 14px;background:#f4eadb;line-height:.9}.board-logo span{color:#fff;background:var(--red);padding:2px 8px;margin-left:5px}
    .slots{display:grid;grid-template-columns:repeat(3,1fr);gap:16px}
    .slot{min-height:125px;border:2px solid #5a493b;background:rgba(255,255,255,.22);padding:10px;position:relative}.slot b{font-size:18px}.slot em{display:block;color:var(--red);font-style:normal;font-family:"Courier New",monospace;font-weight:900}.slot:after{content:"VALIDÉ";position:absolute;right:11px;bottom:12px;color:var(--red);border:2px solid var(--red);border-radius:50%;padding:9px 5px;opacity:.38;transform:rotate(-15deg);font-family:"Courier New",monospace;font-size:12px;font-weight:900}
    .deck{position:absolute;width:128px;height:178px;border:3px solid #2e251f;box-shadow:0 16px 25px rgba(0,0,0,.22);display:grid;place-items:center;text-align:center;font-family:"Courier New",monospace;font-weight:900;text-transform:uppercase}.deck:before{content:"";position:absolute;inset:-10px 10px 10px -10px;border:2px solid rgba(0,0,0,.22);background:inherit;z-index:-1}.deck.dossier{left:0;top:70px;background:#efe5d6}.deck.systeme{left:4%;top:284px;background:#171717;color:#eee;border-color:#111}.deck.sequence{left:8%;top:493px;background:#efe5d6}.deck.issue{right:0;top:142px;background:#efe5d6}.deck.personnage{right:2%;top:358px;background:var(--blue);color:#eee}.deck strong{font-size:22px}.deck span{display:block;margin-top:12px;color:var(--red);font-size:24px}
    .tokens{position:absolute;left:20%;bottom:54px;display:flex;gap:9px;flex-wrap:wrap;width:170px;transform:rotate(-11deg)}.token{width:42px;height:42px;border-radius:50%;display:grid;place-items:center;background:var(--red);color:#fff;border:3px solid var(--red-dark);font-size:14px;box-shadow:0 6px 8px rgba(0,0,0,.18)}.token.dark{background:#202020;border-color:#111}.token.light{background:#eadbc6;color:#202020;border-color:#47372d}
    .pen{position:absolute;right:8%;bottom:74px;width:290px;height:15px;background:#111;transform:rotate(-19deg);border-radius:12px;box-shadow:0 9px 12px rgba(0,0,0,.23)}.pen:after{content:"";position:absolute;right:-70px;top:1px;border-left:80px solid #111;border-top:7px solid transparent;border-bottom:7px solid transparent}
    .paperclip{position:absolute;right:15%;bottom:42px;width:36px;height:78px;border:5px solid #999;border-radius:18px;transform:rotate(-9deg);opacity:.65}.paperclip:after{content:"";position:absolute;inset:9px;border:4px solid #999;border-radius:14px}
    .features{display:grid;grid-template-columns:repeat(3,1fr);border-bottom:3px solid var(--ink);background:rgba(248,239,226,.64)}
    .feature{display:grid;grid-template-columns:112px 1fr;gap:24px;padding:28px clamp(18px,3vw,52px);border-right:2px solid rgba(0,0,0,.18);align-items:center}.feature:last-child{border-right:0}.feature-icon{width:88px;height:88px;border:2px solid var(--red-dark);display:grid;place-items:center;font-size:44px;background:rgba(255,255,255,.25)}.feature h3{margin:0 0 8px;font-size:27px;text-transform:uppercase}.feature p{margin:0;font-family:Arial, sans-serif;font-size:16px;line-height:1.45;color:#2d2722;letter-spacing:0}
    section{padding:70px clamp(18px,4vw,70px);border-bottom:3px solid var(--ink)}
    .section-title{font-size:52px;margin:0 0 22px;text-transform:uppercase}.lead{font-family:Arial, sans-serif;letter-spacing:0;font-size:20px;line-height:1.6;max-width:900px}.cards-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:20px;margin-top:34px}.info-card{min-height:210px;border:3px solid var(--ink);background:rgba(255,255,255,.24);padding:24px;box-shadow:0 8px 0 rgba(0,0,0,.08)}.info-card h3{font-size:29px;margin:0 0 12px}.info-card p{font-family:Arial,sans-serif;letter-spacing:0;line-height:1.5;margin:0;color:#2d2722}
    .bottom-strip{display:flex;justify-content:center;gap:34px;flex-wrap:wrap;background:#111;color:#eee;padding:28px;font-family:"Courier New",monospace;text-transform:uppercase}.bottom-strip span{color:#d7c6ae}.bottom-strip b{color:#b83228;border:2px solid var(--red-dark);padding:6px 12px;background:#1d1110}
    @media(max-width:1100px){.hero{grid-template-columns:1fr}.table-scene{min-height:600px}.features{grid-template-columns:1fr}.feature{border-right:0;border-bottom:2px solid rgba(0,0,0,.15)}.cards-grid{grid-template-columns:repeat(2,1fr)}.stamp-small{display:none}}
    @media(max-width:720px){.topbar{position:relative;align-items:flex-start}.nav{display:none}.hero{padding-top:34px}.eyebrow:before,.eyebrow:after{width:54px}.tagline{font-size:24px}.claim{font-size:16px}.btn{width:100%}.table-scene{transform:scale(.72);transform-origin:top left;width:138%;min-height:500px}.cards-grid{grid-template-columns:1fr}.feature{grid-template-columns:1fr}.section-title{font-size:38px}.logo{font-size:30px}}
  </style>
</head>
<body>
  <header class="topbar">
    <a class="logo" href="#top" aria-label="GUICHET 9">GUICHET <span>9</span></a>
    <nav class="nav" aria-label="Navigation principale">
      <a href="#jeu">Le jeu</a>
      <a href="#regles">Règles</a>
      <a href="#materiel">Matériel</a>
      <a href="#personnages">Personnages</a>
      <a href="#contact">Contact</a>
    </nav>
    <div class="stamp-small"><strong>FORM. G9-RG/2026</strong><em>Livret de procédure officiel</em></div>
  </header>
  <main id="top">
    <section class="hero" id="jeu">
      <div class="hero-copy">
        <p class="eyebrow">PROMESSE DE JEU</p>
        <h1>GUICHET <span class="nine">9</span></h1>
        <p class="tagline">Un jeu semi-coopératif d’administration absurde, de négociation et de dossiers secrets.</p>
        <div class="claim">Tout le monde coopère pour sortir.<strong>Mais un seul dossier sera reconnu.</strong></div>
        <div class="facts">
          <div class="fact"><span class="ico">👥</span><div><b>4-6</b><small>joueurs</small></div></div>
          <div class="fact"><span class="ico">◷</span><div><b>45-60</b><small>minutes</small></div></div>
          <div class="fact"><span class="ico">!</span><div><b>14+</b><small>ans</small></div></div>
        </div>
        <div class="actions">
          <a class="btn primary" href="#regles">Découvrir le jeu →</a>
          <a class="btn" href="#materiel">Lire les règles ▤</a>
        </div>
      </div>
      <div class="table-scene" aria-label="Vue stylisée du matériel de GUICHET 9">
        <div class="deck dossier"><strong>Dossier</strong></div>
        <div class="deck systeme"><strong>Système</strong><span>S04</span></div>
        <div class="deck sequence"><strong>Séquence</strong><span>01</span><small>30 secondes maximum</small></div>
        <div class="deck issue"><strong>Issue<br>personnelle</strong></div>
        <div class="deck personnage"><strong>Personnage</strong><span>Ω</span></div>
        <div class="board">
          <div class="peg"></div>
          <div class="saturation"><div class="label">SATURATION</div><div>0</div><div>1</div><div>2</div><div>3</div><div>4</div><div>5</div><div>6</div><div>7</div><div>8</div><div>9</div></div>
          <div class="board-logo">GUICHET <span>9</span></div>
          <div class="slots">
            <div class="slot"><b>GUICHET 1</b><em>À VALIDER</em></div>
            <div class="slot"><b>GUICHET 2</b><em>À VALIDER</em></div>
            <div class="slot"><b>GUICHET 3</b><em>À VALIDER</em></div>
            <div class="slot"><b>GUICHET 4</b><em>À VALIDER</em></div>
            <div class="slot"><b>GUICHET 5</b><em>À VALIDER</em></div>
            <div class="slot"><b>GUICHET 6</b><em>À VALIDER</em></div>
          </div>
        </div>
        <div class="tokens">
          <div class="token">S1</div><div class="token">S2</div><div class="token">S3</div>
          <div class="token dark">PREUVE</div><div class="token dark">PREUVE</div><div class="token light">📁</div><div class="token light">📁</div><div class="token light">📁</div>
        </div>
        <div class="pen"></div>
        <div class="paperclip"></div>
      </div>
    </section>
    <div class="features">
      <article class="feature"><div class="feature-icon">👥</div><div><h3>Coopérez sous pression</h3><p>Complétez les guichets, gérez la Saturation et gardez le contrôle avant la fermeture du Système.</p></div></article>
      <article class="feature"><div class="feature-icon">▣</div><div><h3>Négociez, promettez, bluffez</h3><p>Parlez, échangez, promettez… ou trahissez vos promesses. Tout est une question d’intérêt.</p></div></article>
      <article class="feature"><div class="feature-icon">🔒</div><div><h3>Évitez la fermeture du Système</h3><p>Un seul dossier sera reconnu. Utilisez la Sortie collective pour garder le contrôle ensemble.</p></div></article>
    </div>
    <section id="regles">
      <h2 class="section-title">Le principe</h2>
      <p class="lead">Dans une administration absurde, les joueurs doivent officiellement coopérer pour compléter et valider six guichets. En réalité, chaque joueur possède une Issue personnelle secrète. Pour gagner seul, il faut accomplir son objectif, accumuler des Contributions, obtenir des Preuves et tenter son dossier avant la fermeture du Système.</p>
    </section>
    <section id="materiel">
      <h2 class="section-title">Matériel</h2>
      <div class="cards-grid">
        <article class="info-card"><h3>6 Guichets</h3><p>Objectifs communs à compléter puis valider avec un Tampon.</p></article>
        <article class="info-card"><h3>36 Dossiers</h3><p>Cartes à poser pour remplir les exigences administratives.</p></article>
        <article class="info-card"><h3>24 Système</h3><p>Contraintes, blocages et perturbations qui augmentent la pression.</p></article>
        <article class="info-card"><h3>12 Issues</h3><p>Objectifs secrets qui poussent chaque joueur vers sa propre victoire.</p></article>
      </div>
    </section>
    <section id="personnages">
      <h2 class="section-title">Personnages</h2>
      <p class="lead">Chaque personnage possède un pouvoir utilisable une seule fois par partie. Cotard, Capgras, Fregoli, La Main étrangère, Ganser et Diogène modifient la procédure sans jamais supprimer la pression collective.</p>
    </section>
    <section id="contact">
      <h2 class="section-title">Contact</h2>
      <p class="lead">Pour tester, présenter ou suivre le développement de GUICHET 9, remplacez ce bloc par votre adresse e-mail, votre page Facebook, votre Instagram ou votre formulaire de contact.</p>
    </section>
  </main>

  <footer class="bottom-strip">
    <span>Administration absurde</span> — <span>Objectifs secrets</span> — <span>Négociation</span> — <span>Humour bureaucratique</span> — <span>Suspense</span> <b>Saturation 6→9</b>
  </footer>
</body>
</html>

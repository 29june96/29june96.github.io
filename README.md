<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Invito di Compleanno</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Cormorant+Garamond:ital,wght@0,300;0,400;1,300&family=Montserrat:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --primary: #c8a97e;
    --secondary: #2d1b4e;
    --accent: #e8d5b7;
    --bg: #f9f5ee;
    --text: #1a1a1a;
    --card: rgba(255,255,255,0.85);
    --hero-img: url('https://images.unsplash.com/photo-1530103862676-de8c9debad1d?w=1200&q=80');
    --confetti-color: #c8a97e;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'Cormorant Garamond', Georgia, serif;
    background: var(--bg);
    color: var(--text);
    overflow-x: hidden;
  }

  /* ─── EDIT PANEL ─── */
  #edit-panel {
    position: fixed;
    top: 0; right: 0;
    width: 320px; height: 100vh;
    background: #fff;
    box-shadow: -4px 0 30px rgba(0,0,0,0.12);
    z-index: 1000;
    transform: translateX(100%);
    transition: transform 0.4s cubic-bezier(0.4,0,0.2,1);
    overflow-y: auto;
    font-family: 'Montserrat', sans-serif;
  }
  #edit-panel.open { transform: translateX(0); }

  #edit-toggle {
    position: fixed;
    top: 50%;
    right: 0;
    transform: translateY(-50%);
    z-index: 1001;
    background: var(--secondary);
    color: #fff;
    border: none;
    padding: 14px 10px;
    cursor: pointer;
    font-family: 'Montserrat', sans-serif;
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 2px;
    text-transform: uppercase;
    writing-mode: vertical-rl;
    border-radius: 8px 0 0 8px;
    transition: background 0.3s;
  }
  #edit-toggle:hover { background: var(--primary); }

  .panel-header {
    background: var(--secondary);
    color: #fff;
    padding: 24px 20px 20px;
    position: sticky;
    top: 0;
    z-index: 10;
  }
  .panel-header h2 {
    font-family: 'Playfair Display', serif;
    font-size: 18px;
    font-weight: 400;
    margin-bottom: 4px;
  }
  .panel-header p { font-size: 11px; opacity: 0.7; letter-spacing: 1px; }

  .panel-section {
    padding: 20px;
    border-bottom: 1px solid #f0ede8;
  }
  .panel-section h3 {
    font-size: 10px;
    font-weight: 500;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: #999;
    margin-bottom: 14px;
  }

  .field-group { margin-bottom: 14px; }
  .field-group label {
    display: block;
    font-size: 11px;
    color: #666;
    margin-bottom: 5px;
    letter-spacing: 0.5px;
  }
  .field-group input[type="text"],
  .field-group input[type="date"],
  .field-group input[type="time"],
  .field-group textarea,
  .field-group input[type="url"] {
    width: 100%;
    padding: 9px 12px;
    border: 1.5px solid #e8e4dc;
    border-radius: 6px;
    font-family: 'Montserrat', sans-serif;
    font-size: 12px;
    color: #333;
    background: #faf9f7;
    transition: border-color 0.2s;
    outline: none;
  }
  .field-group input:focus,
  .field-group textarea:focus {
    border-color: var(--primary);
    background: #fff;
  }
  .field-group textarea { resize: vertical; min-height: 60px; }

  .color-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
  }
  .color-field label {
    display: block;
    font-size: 11px;
    color: #666;
    margin-bottom: 5px;
  }
  .color-field input[type="color"] {
    width: 100%;
    height: 38px;
    border: 1.5px solid #e8e4dc;
    border-radius: 6px;
    cursor: pointer;
    padding: 3px;
    background: #faf9f7;
  }

  .preset-colors {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    margin-bottom: 14px;
  }
  .preset-btn {
    width: 32px; height: 32px;
    border-radius: 50%;
    border: 2px solid transparent;
    cursor: pointer;
    transition: transform 0.2s, border-color 0.2s;
  }
  .preset-btn:hover { transform: scale(1.15); }
  .preset-btn.active { border-color: #333; }

  .apply-btn {
    width: 100%;
    padding: 12px;
    background: var(--secondary);
    color: #fff;
    border: none;
    border-radius: 8px;
    font-family: 'Montserrat', sans-serif;
    font-size: 12px;
    font-weight: 500;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    cursor: pointer;
    margin-top: 6px;
    transition: background 0.3s;
  }
  .apply-btn:hover { background: var(--primary); }

  /* ─── MUSIC PLAYER ─── */
  #music-bar {
    position: fixed;
    bottom: 24px;
    left: 24px;
    z-index: 999;
    display: flex;
    align-items: center;
    gap: 10px;
    background: rgba(0,0,0,0.75);
    backdrop-filter: blur(10px);
    border-radius: 50px;
    padding: 10px 18px;
    color: #fff;
    font-family: 'Montserrat', sans-serif;
    font-size: 11px;
    letter-spacing: 1px;
  }
  #music-btn {
    width: 36px; height: 36px;
    border-radius: 50%;
    background: var(--primary);
    border: none;
    color: #fff;
    cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    font-size: 14px;
    transition: transform 0.2s;
  }
  #music-btn:hover { transform: scale(1.1); }
  .music-note { animation: notePulse 2s infinite; }
  @keyframes notePulse {
    0%, 100% { opacity: 0.6; }
    50% { opacity: 1; }
  }

  /* ─── HERO ─── */
  #hero {
    min-height: 100vh;
    background-image: var(--hero-img);
    background-size: cover;
    background-position: center;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
  }
  #hero::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(45,27,78,0.65) 0%, rgba(0,0,0,0.4) 100%);
  }
  .hero-content {
    position: relative;
    z-index: 1;
    padding: 40px;
    max-width: 700px;
  }
  .hero-eyebrow {
    font-family: 'Montserrat', sans-serif;
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 4px;
    text-transform: uppercase;
    color: var(--primary);
    margin-bottom: 24px;
    animation: fadeUp 1s 0.2s both;
  }
  .hero-title {
    font-family: 'Playfair Display', serif;
    font-size: clamp(52px, 9vw, 100px);
    font-weight: 700;
    color: #fff;
    line-height: 1;
    margin-bottom: 8px;
    animation: fadeUp 1s 0.4s both;
  }
  .hero-subtitle {
    font-family: 'Playfair Display', serif;
    font-size: clamp(22px, 4vw, 38px);
    font-weight: 400;
    font-style: italic;
    color: var(--accent);
    margin-bottom: 32px;
    animation: fadeUp 1s 0.6s both;
  }
  .hero-cta {
    display: inline-block;
    padding: 14px 40px;
    border: 1.5px solid var(--primary);
    color: #fff;
    font-family: 'Montserrat', sans-serif;
    font-size: 12px;
    font-weight: 500;
    letter-spacing: 3px;
    text-transform: uppercase;
    text-decoration: none;
    border-radius: 50px;
    transition: background 0.3s, color 0.3s;
    animation: fadeUp 1s 0.8s both;
    background: rgba(200,169,126,0.15);
    cursor: pointer;
    backdrop-filter: blur(4px);
  }
  .hero-cta:hover { background: var(--primary); color: #fff; }

  /* ─── CONFETTI ─── */
  .confetti-container {
    position: absolute;
    inset: 0;
    overflow: hidden;
    pointer-events: none;
    z-index: 0;
  }
  .confetto {
    position: absolute;
    top: -20px;
    width: 8px; height: 8px;
    border-radius: 50%;
    animation: fall linear infinite;
  }
  @keyframes fall {
    to { transform: translateY(110vh) rotate(720deg); opacity: 0; }
  }

  /* ─── DETAILS SECTION ─── */
  #details {
    padding: 100px 20px;
    text-align: center;
    background: var(--bg);
    position: relative;
  }
  .section-label {
    font-family: 'Montserrat', sans-serif;
    font-size: 10px;
    font-weight: 500;
    letter-spacing: 4px;
    text-transform: uppercase;
    color: var(--primary);
    margin-bottom: 20px;
  }
  .section-title {
    font-family: 'Playfair Display', serif;
    font-size: clamp(32px, 5vw, 52px);
    font-weight: 400;
    font-style: italic;
    color: var(--secondary);
    margin-bottom: 60px;
  }

  .details-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 30px;
    max-width: 800px;
    margin: 0 auto 70px;
  }
  .detail-card {
    background: var(--card);
    border: 1px solid rgba(200,169,126,0.25);
    border-radius: 16px;
    padding: 36px 24px;
    backdrop-filter: blur(10px);
    transition: transform 0.3s, box-shadow 0.3s;
  }
  .detail-card:hover {
    transform: translateY(-6px);
    box-shadow: 0 20px 40px rgba(0,0,0,0.08);
  }
  .detail-icon {
    font-size: 32px;
    margin-bottom: 16px;
    display: block;
  }
  .detail-card h3 {
    font-family: 'Montserrat', sans-serif;
    font-size: 10px;
    font-weight: 500;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: #999;
    margin-bottom: 10px;
  }
  .detail-card p {
    font-family: 'Playfair Display', serif;
    font-size: 20px;
    font-weight: 400;
    color: var(--secondary);
    line-height: 1.4;
  }

  /* ─── MESSAGE SECTION ─── */
  #message {
    padding: 100px 20px;
    background: var(--secondary);
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  #message::before {
    content: '✦';
    position: absolute;
    font-size: 300px;
    color: rgba(200,169,126,0.05);
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .message-text {
    font-family: 'Playfair Display', serif;
    font-size: clamp(22px, 4vw, 38px);
    font-weight: 400;
    font-style: italic;
    color: var(--accent);
    max-width: 700px;
    margin: 0 auto 30px;
    line-height: 1.6;
    position: relative;
    z-index: 1;
  }
  .message-sign {
    font-family: 'Cormorant Garamond', serif;
    font-size: 18px;
    color: var(--primary);
    letter-spacing: 2px;
    position: relative;
    z-index: 1;
  }

  /* ─── RSVP SECTION ─── */
  #rsvp {
    padding: 100px 20px;
    background: var(--bg);
    text-align: center;
  }
  .rsvp-box {
    background: var(--secondary);
    border-radius: 24px;
    padding: 60px 40px;
    max-width: 560px;
    margin: 0 auto;
    position: relative;
    overflow: hidden;
  }
  .rsvp-box::after {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(circle at top right, rgba(200,169,126,0.15), transparent 60%);
    pointer-events: none;
  }
  .rsvp-box h2 {
    font-family: 'Playfair Display', serif;
    font-size: 36px;
    font-weight: 400;
    color: #fff;
    margin-bottom: 16px;
  }
  .rsvp-box p {
    font-family: 'Cormorant Garamond', serif;
    font-size: 18px;
    color: rgba(255,255,255,0.7);
    margin-bottom: 36px;
    line-height: 1.6;
  }
  .rsvp-contact {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    background: var(--primary);
    color: #fff;
    padding: 14px 32px;
    border-radius: 50px;
    font-family: 'Montserrat', sans-serif;
    font-size: 13px;
    font-weight: 500;
    letter-spacing: 1px;
    text-decoration: none;
    transition: opacity 0.3s;
  }
  .rsvp-contact:hover { opacity: 0.85; }

  /* ─── FOOTER ─── */
  footer {
    padding: 30px;
    text-align: center;
    font-family: 'Montserrat', sans-serif;
    font-size: 11px;
    color: #bbb;
    letter-spacing: 1px;
    background: var(--secondary);
    border-top: 1px solid rgba(255,255,255,0.05);
    color: rgba(255,255,255,0.3);
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(30px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  /* Responsive */
  @media (max-width: 600px) {
    #edit-panel { width: 100vw; }
    .rsvp-box { padding: 40px 24px; }
    .hero-content { padding: 24px; }
  }

  .divider {
    display: flex; align-items: center; gap: 16px;
    max-width: 300px; margin: 0 auto 60px;
    color: var(--primary); font-size: 18px;
  }
  .divider::before, .divider::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(to right, transparent, var(--primary), transparent);
  }
</style>
</head>
<body>

<!-- MUSIC -->
<audio id="bg-music" loop>
  <source id="music-src" src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
</audio>

<!-- MUSIC BAR -->
<div id="music-bar">
  <button id="music-btn" title="Play/Pausa musica">▶</button>
  <span class="music-note" id="music-label">♪ Musica di sottofondo</span>
</div>

<!-- EDIT TOGGLE -->
<button id="edit-toggle" onclick="togglePanel()">✏ Personalizza</button>

<!-- EDIT PANEL -->
<div id="edit-panel">
  <div class="panel-header">
    <h2>✨ Personalizza</h2>
    <p>MODIFICA I TESTI, COLORI & IMMAGINI</p>
  </div>

  <!-- TESTI -->
  <div class="panel-section">
    <h3>🎂 Testi principali</h3>
    <div class="field-group">
      <label>Soprattitolo (es. "Sei invitato a festeggiare")</label>
      <input type="text" id="f-eyebrow" value="Sei invitato a festeggiare">
    </div>
    <div class="field-group">
      <label>Nome del festeggiato / N° anni</label>
      <input type="text" id="f-title" value="30">
    </div>
    <div class="field-group">
      <label>Sottotitolo</label>
      <input type="text" id="f-subtitle" value="anni meravigliosi">
    </div>
  </div>

  <!-- DETTAGLI EVENTO -->
  <div class="panel-section">
    <h3>📅 Dettagli evento</h3>
    <div class="field-group">
      <label>Titolo sezione dettagli</label>
      <input type="text" id="f-details-title" value="Quando ci troviamo">
    </div>
    <div class="field-group">
      <label>Data</label>
      <input type="text" id="f-date" value="Sabato 21 Giugno 2025">
    </div>
    <div class="field-group">
      <label>Ora</label>
      <input type="text" id="f-time" value="dalle 19:30">
    </div>
    <div class="field-group">
      <label>Luogo</label>
      <input type="text" id="f-place" value="Villa delle Rose, Treviso">
    </div>
    <div class="field-group">
      <label>Dress code (opzionale)</label>
      <input type="text" id="f-dress" value="Elegante chic">
    </div>
  </div>

  <!-- MESSAGGIO -->
  <div class="panel-section">
    <h3>💌 Messaggio personale</h3>
    <div class="field-group">
      <label>Testo del messaggio</label>
      <textarea id="f-message">"La vita va celebrata ogni giorno, ma certi compleanni meritano qualcosa di speciale. Vieni a festeggiare con noi!"</textarea>
    </div>
    <div class="field-group">
      <label>Firma</label>
      <input type="text" id="f-sign" value="Con amore ♡">
    </div>
  </div>

  <!-- RSVP -->
  <div class="panel-section">
    <h3>📬 RSVP</h3>
    <div class="field-group">
      <label>Testo RSVP</label>
      <textarea id="f-rsvp-text">Facci sapere se ci sei entro il 10 Giugno — ti aspettiamo!</textarea>
    </div>
    <div class="field-group">
      <label>Numero WhatsApp / Link</label>
      <input type="text" id="f-rsvp-link" value="https://wa.me/39XXXXXXXXXX">
    </div>
    <div class="field-group">
      <label>Testo bottone RSVP</label>
      <input type="text" id="f-rsvp-btn" value="Conferma su WhatsApp">
    </div>
  </div>

  <!-- COLORI -->
  <div class="panel-section">
    <h3>🎨 Tema colori</h3>
    <p style="font-size:11px;color:#999;margin-bottom:12px;">Preset veloci:</p>
    <div class="preset-colors">
      <div class="preset-btn active" style="background:#c8a97e" title="Oro" onclick="applyPreset('#c8a97e','#2d1b4e','#f9f5ee')" data-id="gold"></div>
      <div class="preset-btn" style="background:#e05a82" title="Rosa" onclick="applyPreset('#e05a82','#3d0f2b','#fff5f8')" data-id="pink"></div>
      <div class="preset-btn" style="background:#4ecdc4" title="Turchese" onclick="applyPreset('#4ecdc4','#1a3a3a','#f0fafa')" data-id="teal"></div>
      <div class="preset-btn" style="background:#6c63ff" title="Viola" onclick="applyPreset('#6c63ff','#1a1040','#f5f3ff')" data-id="purple"></div>
      <div class="preset-btn" style="background:#2ecc71" title="Verde" onclick="applyPreset('#2ecc71','#0d2b1a','#f0faf5')" data-id="green"></div>
      <div class="preset-btn" style="background:#e67e22" title="Arancio" onclick="applyPreset('#e67e22','#2b1500','#fff8f0')" data-id="orange"></div>
    </div>
    <div class="color-grid">
      <div class="color-field">
        <label>Colore accento</label>
        <input type="color" id="c-primary" value="#c8a97e" oninput="updateColor('--primary',this.value)">
      </div>
      <div class="color-field">
        <label>Colore scuro</label>
        <input type="color" id="c-secondary" value="#2d1b4e" oninput="updateColor('--secondary',this.value)">
      </div>
      <div class="color-field">
        <label>Sfondo</label>
        <input type="color" id="c-bg" value="#f9f5ee" oninput="updateColor('--bg',this.value)">
      </div>
      <div class="color-field">
        <label>Testo chiaro</label>
        <input type="color" id="c-accent" value="#e8d5b7" oninput="updateColor('--accent',this.value)">
      </div>
    </div>
  </div>

  <!-- IMMAGINE -->
  <div class="panel-section">
    <h3>🖼 Immagine di sfondo</h3>
    <div class="field-group">
      <label>URL immagine hero</label>
      <input type="url" id="f-hero-img" value="https://images.unsplash.com/photo-1530103862676-de8c9debad1d?w=1200&q=80">
    </div>
    <div style="font-size:11px;color:#999;margin-bottom:12px;">Suggerimenti Unsplash (copia l'URL):</div>
    <div style="display:flex;gap:8px;flex-wrap:wrap;">
      <button onclick="setImg('balloons')" style="font-size:11px;padding:5px 10px;border:1px solid #ddd;border-radius:20px;cursor:pointer;background:#fff;">🎈 Palloncini</button>
      <button onclick="setImg('flowers')" style="font-size:11px;padding:5px 10px;border:1px solid #ddd;border-radius:20px;cursor:pointer;background:#fff;">🌸 Fiori</button>
      <button onclick="setImg('celebration')" style="font-size:11px;padding:5px 10px;border:1px solid #ddd;border-radius:20px;cursor:pointer;background:#fff;">🎉 Festa</button>
      <button onclick="setImg('confetti')" style="font-size:11px;padding:5px 10px;border:1px solid #ddd;border-radius:20px;cursor:pointer;background:#fff;">✨ Confetti</button>
    </div>
  </div>

  <!-- MUSICA -->
  <div class="panel-section">
    <h3>🎵 Musica di sottofondo</h3>
    <div class="field-group">
      <label>URL file audio (.mp3)</label>
      <input type="url" id="f-music" value="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3">
    </div>
    <div style="font-size:11px;color:#999;margin-bottom:10px;">Incolla il link diretto a un file .mp3 (es. da Google Drive, Dropbox, o hosting pubblico)</div>
    <button class="apply-btn" onclick="changeMusic()">Aggiorna musica</button>
  </div>

  <!-- APPLICA -->
  <div class="panel-section">
    <button class="apply-btn" onclick="applyAll()">✅ Applica tutte le modifiche</button>
    <div style="margin-top:10px;text-align:center;font-size:11px;color:#bbb;">Le modifiche sono visibili in tempo reale</div>
  </div>
</div>

<!-- ═══════════════════════════════
     INVITO VERO E PROPRIO
═══════════════════════════════ -->

<!-- HERO -->
<section id="hero">
  <div class="confetti-container" id="confetti"></div>
  <div class="hero-content">
    <p class="hero-eyebrow" id="t-eyebrow">Sei invitato a festeggiare</p>
    <h1 class="hero-title" id="t-title">30</h1>
    <p class="hero-subtitle" id="t-subtitle">anni meravigliosi</p>
    <a class="hero-cta" href="#details">Scopri i dettagli ↓</a>
  </div>
</section>

<!-- DETAILS -->
<section id="details">
  <p class="section-label">Dove & Quando</p>
  <h2 class="section-title" id="t-details-title">Quando ci troviamo</h2>
  <div class="divider">✦</div>
  <div class="details-grid">
    <div class="detail-card">
      <span class="detail-icon">📅</span>
      <h3>Data</h3>
      <p id="t-date">Sabato 21 Giugno 2025</p>
    </div>
    <div class="detail-card">
      <span class="detail-icon">⏰</span>
      <h3>Orario</h3>
      <p id="t-time">dalle 19:30</p>
    </div>
    <div class="detail-card">
      <span class="detail-icon">📍</span>
      <h3>Luogo</h3>
      <p id="t-place">Villa delle Rose, Treviso</p>
    </div>
    <div class="detail-card">
      <span class="detail-icon">👗</span>
      <h3>Dress code</h3>
      <p id="t-dress">Elegante chic</p>
    </div>
  </div>
</section>

<!-- MESSAGE -->
<section id="message">
  <p class="message-text" id="t-message">"La vita va celebrata ogni giorno, ma certi compleanni meritano qualcosa di speciale. Vieni a festeggiare con noi!"</p>
  <p class="message-sign" id="t-sign">Con amore ♡</p>
</section>

<!-- RSVP -->
<section id="rsvp">
  <div class="rsvp-box">
    <h2>RSVP</h2>
    <p id="t-rsvp-text">Facci sapere se ci sei entro il 10 Giugno — ti aspettiamo!</p>
    <a class="rsvp-contact" id="t-rsvp-link" href="https://wa.me/39XXXXXXXXXX" target="_blank">
      💬 <span id="t-rsvp-btn">Conferma su WhatsApp</span>
    </a>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <p>Con tutto l'amore del mondo ✦ Ci vediamo presto</p>
</footer>

<script>
// ─── PANEL ───
function togglePanel() {
  document.getElementById('edit-panel').classList.toggle('open');
}

// ─── MUSIC ───
const audio = document.getElementById('bg-music');
const musicBtn = document.getElementById('music-btn');
let playing = false;

function tryPlay() {
  audio.play().then(() => {
    playing = true;
    musicBtn.textContent = '⏸';
  }).catch(() => {});
}

// Try autoplay
window.addEventListener('load', () => {
  setTimeout(tryPlay, 800);
});

// Fallback: play on first user interaction
document.addEventListener('click', function once() {
  if (!playing) tryPlay();
  document.removeEventListener('click', once);
}, { once: true });

musicBtn.addEventListener('click', (e) => {
  e.stopPropagation();
  if (playing) {
    audio.pause();
    musicBtn.textContent = '▶';
    playing = false;
  } else {
    audio.play();
    musicBtn.textContent = '⏸';
    playing = true;
  }
});

function changeMusic() {
  const url = document.getElementById('f-music').value;
  const wasPlaying = playing;
  audio.pause();
  document.getElementById('music-src').src = url;
  audio.load();
  if (wasPlaying) {
    audio.play().then(() => {
      playing = true;
      musicBtn.textContent = '⏸';
    });
  }
}

// ─── COLORS ───
function updateColor(varName, val) {
  document.documentElement.style.setProperty(varName, val);
}

function applyPreset(primary, secondary, bg) {
  document.getElementById('c-primary').value = primary;
  document.getElementById('c-secondary').value = secondary;
  document.getElementById('c-bg').value = bg;
  updateColor('--primary', primary);
  updateColor('--secondary', secondary);
  updateColor('--bg', bg);
  document.querySelectorAll('.preset-btn').forEach(b => b.classList.remove('active'));
  event.target.classList.add('active');
}

// ─── IMAGES ───
const imgs = {
  balloons: 'https://images.unsplash.com/photo-1527529482837-4698179dc6ce?w=1200&q=80',
  flowers: 'https://images.unsplash.com/photo-1490750967868-88df5691cc5e?w=1200&q=80',
  celebration: 'https://images.unsplash.com/photo-1530103862676-de8c9debad1d?w=1200&q=80',
  confetti: 'https://images.unsplash.com/photo-1516450360452-9312f5e86fc7?w=1200&q=80'
};
function setImg(key) {
  const url = imgs[key];
  document.getElementById('f-hero-img').value = url;
  document.documentElement.style.setProperty('--hero-img', `url('${url}')`);
}

// ─── APPLY ALL ───
function applyAll() {
  const map = {
    'f-eyebrow': 't-eyebrow',
    'f-title': 't-title',
    'f-subtitle': 't-subtitle',
    'f-details-title': 't-details-title',
    'f-date': 't-date',
    'f-time': 't-time',
    'f-place': 't-place',
    'f-dress': 't-dress',
    'f-message': 't-message',
    'f-sign': 't-sign',
    'f-rsvp-text': 't-rsvp-text',
    'f-rsvp-btn': 't-rsvp-btn',
  };
  for (const [field, target] of Object.entries(map)) {
    const el = document.getElementById(target);
    const val = document.getElementById(field).value;
    if (el) el.textContent = val;
  }
  // Hero image
  const imgUrl = document.getElementById('f-hero-img').value;
  document.documentElement.style.setProperty('--hero-img', `url('${imgUrl}')`);
  // RSVP link
  const link = document.getElementById('f-rsvp-link');
  const linkVal = document.getElementById('f-rsvp-link-input') ? document.getElementById('f-rsvp-link-input').value : document.getElementById('f-rsvp-link').href;
  const rsvpHref = document.getElementById('f-rsvp-link').href;
  link.href = document.getElementById('f-rsvp-link').href;
  // Update RSVP link from input
  document.getElementById('t-rsvp-link').href = document.getElementById('f-rsvp-link').value || '#';

  // Flash confirmation
  const btn = document.querySelector('.apply-btn');
  const orig = btn.textContent;
  btn.textContent = '✅ Applicato!';
  btn.style.background = '#2ecc71';
  setTimeout(() => { btn.textContent = orig; btn.style.background = ''; }, 1800);
}

// Live update on input change
document.querySelectorAll('#edit-panel input[type="text"], #edit-panel textarea').forEach(input => {
  const targetId = input.id.replace('f-', 't-');
  const target = document.getElementById(targetId);
  if (target) {
    input.addEventListener('input', () => { target.textContent = input.value; });
  }
});
// Live update image
document.getElementById('f-hero-img').addEventListener('input', function() {
  document.documentElement.style.setProperty('--hero-img', `url('${this.value}')`);
});
// Live RSVP link
document.getElementById('f-rsvp-link').addEventListener = null;
document.getElementById('f-rsvp-link').oninput = function() {
  document.getElementById('t-rsvp-link').href = this.value;
};

// ─── CONFETTI ───
const container = document.getElementById('confetti');
const colors = ['#c8a97e','#e8d5b7','#fff','#f4d03f','#e8b4d0'];
for (let i = 0; i < 24; i++) {
  const el = document.createElement('div');
  el.className = 'confetto';
  el.style.cssText = `
    left: ${Math.random()*100}%;
    width: ${4 + Math.random()*8}px;
    height: ${4 + Math.random()*8}px;
    background: ${colors[Math.floor(Math.random()*colors.length)]};
    border-radius: ${Math.random() > 0.5 ? '50%' : '2px'};
    animation-duration: ${4 + Math.random()*8}s;
    animation-delay: ${Math.random()*6}s;
    opacity: ${0.3 + Math.random()*0.7};
  `;
  container.appendChild(el);
}
</script>
</body>
</html>

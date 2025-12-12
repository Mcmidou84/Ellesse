<script>
  let menuOpen = $state(false);
  
  function toggleMenu() {
    menuOpen = !menuOpen;
  }
  
  function closeMenu() {
    menuOpen = false;
  }
</script>

<div class="page-wrapper">
  <header>
    <div class="left">
      <img src="./logo.png" alt="Logo" class="logo">
      <span class="brand">Ellessé</span>
    </div>
    
    <nav class:open={menuOpen}>
      <a href="/" onclick={closeMenu}>Accueil</a>
      <a href="/galerie" onclick={closeMenu}>Galerie</a>
      <a href="/a-propos" onclick={closeMenu}>À propos</a>
      <a href="/contact" onclick={closeMenu}>Contact</a>
      <a href="/boutique" onclick={closeMenu}>Boutique</a>
    </nav>

    <button class="burger" class:open={menuOpen} onclick={toggleMenu} aria-label="Menu">
      <span></span>
      <span></span>
      <span></span>
    </button>

    <div class="spacer"></div>
  </header>

  {#if menuOpen}
    <div class="overlay" onclick={closeMenu}></div>
  {/if}

  <main>
    <section class="hero">
      <p class="subtitle">Cabinet d'esthétique</p>
      <h1 class="title">L'art de la beauté à la russe</h1>
    </section>
    
    <slot />
  </main>

  <footer>
  <div class="values-container">
    <div class="value-item">
      <svg class="value-icon" viewBox="0 0 80 80" fill="none" stroke="currentColor" stroke-width="1.5">
        <!-- Élégance - Plume stylisée -->
        <path d="M40 10 C30 25, 25 40, 40 70" />
        <path d="M40 10 C50 25, 55 40, 40 70" />
        <path d="M40 20 C35 30, 33 40, 40 55" />
        <path d="M40 20 C45 30, 47 40, 40 55" />
        <path d="M30 35 Q40 32, 50 35" />
        <path d="M32 45 Q40 42, 48 45" />
        <circle cx="40" cy="10" r="3" />
      </svg>
      <h3 class="value-title">Élégance</h3>
      <p class="value-text">La grâce et le raffinement inspirés de l'esthétique impériale russe</p>
    </div>

    <div class="value-item">
      <svg class="value-icon" viewBox="0 0 80 80" fill="none" stroke="currentColor" stroke-width="1.5">
        <!-- Tradition - Matriochka stylisée -->
        <ellipse cx="40" cy="55" rx="18" ry="22" />
        <circle cx="40" cy="28" r="14" />
        <circle cx="40" cy="28" r="9" />
        <ellipse cx="40" cy="50" rx="10" ry="8" />
        <path d="M32 24 Q40 20, 48 24" />
        <circle cx="36" cy="26" r="1.5" />
        <circle cx="44" cy="26" r="1.5" />
        <path d="M37 31 Q40 33, 43 31" />
      </svg>
      <h3 class="value-title">Tradition</h3>
      <p class="value-text">Un savoir-faire ancestral transmis de génération en génération</p>
    </div>

    <div class="value-item">
      <svg class="value-icon" viewBox="0 0 80 80" fill="none" stroke="currentColor" stroke-width="1.5">
        <!-- Excellence - Diamant/Cristal -->
        <path d="M40 8 L55 25 L40 72 L25 25 Z" />
        <path d="M25 25 L40 35 L55 25" />
        <path d="M40 35 L40 72" />
        <path d="M30 16 L40 8 L50 16" />
        <path d="M25 25 L30 16 L40 25 L50 16 L55 25" />
        <path d="M32 25 L40 50" />
        <path d="M48 25 L40 50" />
      </svg>
      <h3 class="value-title">Excellence</h3>
      <p class="value-text">La quête de la perfection dans chaque geste et chaque soin</p>
    </div>

    <div class="value-item">
      <svg class="value-icon" viewBox="0 0 80 80" fill="none" stroke="currentColor" stroke-width="1.5">
        <!-- Pureté - Fleur de bouleau/nature -->
        <circle cx="40" cy="30" r="12" />
        <circle cx="40" cy="30" r="6" />
        <path d="M40 42 L40 70" />
        <path d="M40 50 Q30 45, 25 50" />
        <path d="M40 50 Q50 45, 55 50" />
        <path d="M40 58 Q32 54, 28 58" />
        <path d="M40 58 Q48 54, 52 58" />
        <path d="M28 30 Q20 25, 22 18" />
        <path d="M52 30 Q60 25, 58 18" />
        <path d="M35 20 Q30 12, 35 8" />
        <path d="M45 20 Q50 12, 45 8" />
      </svg>
      <h3 class="value-title">Pureté</h3>
      <p class="value-text">Des soins naturels inspirés des secrets de beauté sibériens</p>
    </div>
  </div>
  
  <div class="footer-bottom">
    <p>© 2025 Ellessé — Cabinet d'esthétique</p>
  </div>
</footer>
</div>

<style>
  @font-face {
    font-family: 'LittleMicroSans';
    src: url('/Ellesse/LittleMicroSansTrial-Li.otf') format('opentype');
    font-weight: 400;
    font-style: normal;
    font-display: swap;
  }

  @font-face {
    font-family: 'Priamos';
    src: url('/Ellesse/Priamos-Regular.ttf') format('truetype');
    font-weight: 400;
    font-style: normal;
    font-display: swap;
  }

  :global(html) {
    height: 100%;
  }

  :global(body) {
    margin: 0;
    min-height: 100%;
    background-image: url('/fond.jpg');
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    background-attachment: fixed;
  }

  .page-wrapper {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
  }

  /* ===== HEADER INVERSÉ ===== */
  header {
    background-color: rgb(38, 25, 17);
    padding: 20px 30px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: relative;
    z-index: 100;
  }

  .left {
    display: flex;
    align-items: center;
    gap: 15px;
    flex: 1;
  }

  .spacer {
    flex: 1;
  }

  .logo {
    height: 50px;
    filter: brightness(0) invert(1);
  }

  .brand {
    font-family: 'LittleMicroSans', sans-serif;
    text-transform: uppercase;
    font-size: 28px;
    font-weight: 300;
    color: rgb(249, 246, 239);
  }

  nav {
    display: flex;
    gap: 30px;
  }

  nav a {
    font-family: 'Priamos', serif;
    text-decoration: none;
    color: rgb(200, 180, 160);
    font-weight: 300;
    font-size: 16px;
    transition: color 0.3s;
  }

  nav a:hover {
    color: rgb(249, 246, 239);
  }

  .burger {
    display: none;
    flex-direction: column;
    justify-content: space-between;
    width: 30px;
    height: 22px;
    background: transparent;
    border: none;
    cursor: pointer;
    padding: 0;
    z-index: 110;
  }

  .burger span {
    display: block;
    width: 100%;
    height: 3px;
    background-color: rgb(249, 246, 239);
    border-radius: 2px;
    transition: all 0.3s ease;
  }

  .burger.open span:nth-child(1) {
    transform: rotate(45deg) translate(6px, 6px);
  }

  .burger.open span:nth-child(2) {
    opacity: 0;
  }

  .burger.open span:nth-child(3) {
    transform: rotate(-45deg) translate(6px, -6px);
  }

  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 90;
  }

  /* ===== MAIN ===== */
  main {
    padding: 20px;
    flex: 1;
    display: flex;
    flex-direction: column;
  }

  .hero {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    flex: 1;
    text-align: center;
  }

  .subtitle {
    font-family: 'Priamos', serif;
    font-size: 18px;
    color: rgb(249, 246, 239);
    margin: 0 0 15px 0;
    letter-spacing: 2px;
  }

  .title {
    font-family: 'LittleMicroSans', sans-serif;
    font-size: 90px;
    font-weight: 100;
    text-transform: uppercase;
    color: rgb(249, 246, 239);
    margin: 0;
    max-width: 1200px;
    line-height: 1.2;
    text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
  }

  /* ===== FOOTER ===== */
  footer {
    background-color: rgb(38, 25, 17);
    padding: 60px 30px 30px;
    margin-top: auto;
  }

  .values-container {
    display: flex;
    justify-content: center;
    gap: 60px;
    max-width: 1200px;
    margin: 0 auto;
    flex-wrap: wrap;
  }

  .value-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    max-width: 200px;
  }

  .value-icon {
    width: 70px;
    height: 70px;
    color: rgb(200, 180, 160);
    margin-bottom: 20px;
    transition: color 0.3s ease;
  }

  .value-item:hover .value-icon {
    color: rgb(249, 246, 239);
  }

  .value-title {
    font-family: 'LittleMicroSans', sans-serif;
    text-transform: uppercase;
    font-size: 16px;
    font-weight: 300;
    color: rgb(249, 246, 239);
    margin: 0 0 10px 0;
    letter-spacing: 2px;
  }

  .value-text {
    font-family: 'Priamos', serif;
    font-size: 14px;
    color: rgb(200, 180, 160);
    margin: 0;
    line-height: 1.6;
  }

  .footer-bottom {
    margin-top: 50px;
    padding-top: 20px;
    border-top: 1px solid rgba(200, 180, 160, 0.3);
    text-align: center;
  }

  .footer-bottom p {
    font-family: 'Priamos', serif;
    font-size: 13px;
    color: rgb(150, 130, 110);
    margin: 0;
  }

  /* ===== RESPONSIVE ===== */
  @media (max-width: 768px) {
    header {
      padding: 15px 20px;
    }

    .spacer {
      display: none;
    }

    .burger {
      display: flex;
    }

    nav {
      position: fixed;
      top: 0;
      right: -100%;
      width: 70%;
      max-width: 300px;
      height: 100vh;
      background-color: rgb(38, 25, 17);
      flex-direction: column;
      padding: 80px 30px 30px;
      gap: 0;
      box-shadow: -5px 0 20px rgba(0, 0, 0, 0.4);
      transition: right 0.3s ease;
      z-index: 105;
    }

    nav.open {
      right: 0;
    }

    nav a {
      padding: 15px 0;
      font-size: 18px;
      border-bottom: 1px solid rgba(200, 180, 160, 0.2);
    }

    nav a:last-child {
      border-bottom: none;
    }

    .title {
      font-size: 40px;
    }

    .subtitle {
      font-size: 14px;
    }

    .brand {
      font-size: 22px;
    }

    .logo {
      height: 40px;
    }

    /* Footer responsive */
    .values-container {
      gap: 40px;
    }

    .value-item {
      max-width: 45%;
    }

    .value-icon {
      width: 55px;
      height: 55px;
    }
  }

  @media (max-width: 480px) {
    .title {
      font-size: 32px;
    }

    .values-container {
      flex-direction: column;
      align-items: center;
      gap: 35px;
    }

    .value-item {
      max-width: 250px;
    }

    .value-icon {
      width: 60px;
      height: 60px;
    }

    footer {
      padding: 40px 20px 25px;
    }
  }
</style>

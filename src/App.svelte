<script>
  let menuOpen = $state(false);
  
  function toggleMenu() {
    menuOpen = !menuOpen;
  }
  
  function closeMenu() {
    menuOpen = false;
  }
</script>

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

<style>
  @font-face {
    font-family: 'LittleMicroSans';
    src: url('public/little-micro-sans-font-family/LittleMicroSansTrial-li.otf') format('opentype');
    font-weight: 400;
    font-style: normal;
    font-display: swap;
  }

  @font-face {
    font-family: 'Priamos';
    src: url('public/priamos-regular/Priamos Regular.ttf') format('truetype');
    font-weight: 400;
    font-style: normal;
    font-display: swap;
  }

  :global(body) {
    margin: 0;
    background-image: url('/fond.jpg');
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    background-attachment: fixed;
    min-height: 100vh;
  }

  header {
    background-color: rgb(249, 246, 239);
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
  }

  .brand {
    font-family: 'LittleMicroSans', sans-serif;
    text-transform: uppercase;
    font-size: 28px;
    font-weight: 300;
    color: rgb(38, 25, 17);
  }

  nav {
    display: flex;
    gap: 30px;
  }

  nav a {
    font-family: 'Priamos', serif;
    text-decoration: none;
    color: rgb(102, 74, 50);
    font-weight: 300;
    font-size: 16px;
    transition: color 0.3s;
  }

  nav a:hover {
    color: rgb(38, 25, 17);
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
    background-color: rgb(38, 25, 17);
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

  main {
    padding: 20px;
  }

  .hero {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 70vh;
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

  @media (max-width: 768px) {
    /* ... reste inchangé ... */
  }
</style>

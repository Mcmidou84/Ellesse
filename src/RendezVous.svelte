<script>
  import { onMount } from 'svelte';
  import { link } from 'svelte-spa-router';
  
  let menuOpen = $state(false);
  let calLoaded = $state(false);

  onMount(() => {
    // Charger le script Cal.com
    const script = document.createElement('script');
    script.src = 'https://cal.com/embed.js';
    script.async = true;
    script.onload = () => {
      initCal();
    };
    document.head.appendChild(script);
    
    return () => {
      // Cleanup si nécessaire
    };
  });

  function initCal() {
    if (typeof Cal !== 'undefined') {
      // @ts-ignore
      Cal("init", { origin: "https://cal.com" });
      // @ts-ignore
      Cal("inline", {
        elementOrSelector: "#cal-embed",
        calLink: "ton-username/consultation", // ⚠️ Remplace par ton lien Cal.com
        config: {
          theme: "light",
          hideEventTypeDetails: false
        }
      });
      calLoaded = true;
    } else {
      setTimeout(initCal, 100);
    }
  }

  function toggleMenu() {
    menuOpen = !menuOpen;
  }

  function closeMenu() {
    menuOpen = false;
  }
</script>

<svelte:head>
  <title>Rendez-vous | Ellessé - Cabinet d'esthétique à la russe</title>
  <meta name="description" content="Prenez rendez-vous en ligne au cabinet d'esthétique Ellessé." />
</svelte:head>

<div class="page-wrapper">
  <header>
    <div class="left">
      <a href="/" use:link aria-label="Accueil Ellessé">
        <img src="./logo.png" alt="Logo Ellessé" class="logo" width="50" height="50" />
      </a>
      <span class="brand">Ellessé</span>
    </div>
    
    <nav class:open={menuOpen}>
      <a href="/" use:link onclick={closeMenu}>Accueil</a>
      <a href="/galerie" use:link onclick={closeMenu}>Galerie</a>
      <a href="/a-propos" use:link onclick={closeMenu}>À propos</a>
      <a href="/contact" use:link onclick={closeMenu}>Contact</a>
      <a href="/rendez-vous" use:link class="active" onclick={closeMenu}>Rendez-vous</a>
      <a href="/boutique" use:link onclick={closeMenu}>Boutique</a>
    </nav>
    
    <button
      class="burger"
      class:open={menuOpen}
      onclick={toggleMenu}
      aria-label={menuOpen ? "Fermer le menu" : "Ouvrir le menu"}
      aria-expanded={menuOpen}
    >
      <span></span>
      <span></span>
      <span></span>
    </button>
    <div class="spacer"></div>
  </header>

  {#if menuOpen}
    <div class="overlay" onclick={closeMenu} role="presentation"></div>
  {/if}

  <main>
    <section class="booking-section">
      <div class="booking-header">
        <p class="subtitle">Réservation en ligne</p>
        <h1 class="title">Prendre rendez-vous</h1>
        <p class="description">
          Choisissez votre soin et le créneau qui vous convient. 
          Vous recevrez une confirmation par email.
        </p>
      </div>
      
      <div class="calendar-container">
        {#if !calLoaded}
          <div class="loading">
            <div class="spinner"></div>
            <p>Chargement du calendrier...</p>
          </div>
        {/if}
        <div id="cal-embed"></div>
      </div>
      
      <div class="booking-info">
        <div class="info-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
            <path d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
          <div>
            <h3>Horaires</h3>
            <p>Lundi - Vendredi : 9h - 19h</p>
          </div>
        </div>
        <div class="info-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
            <path d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z" />
          </svg>
          <div>
            <h3>Contact</h3>
            <p>+33 0 00 00 00 00</p>
          </div>
        </div>
        <div class="info-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
            <path d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" />
            <path d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
          </svg>
          <div>
            <h3>Adresse</h3>
            <p>Votre adresse, Ville</p>
          </div>
        </div>
      </div>
    </section>
  </main>

  <footer>
    <div class="footer-bottom">
      <p>© 2025 Ellessé — Cabinet d'esthétique</p>
    </div>
  </footer>
</div>

<style>
  @font-face {
    font-family: "LittleMicroSans";
    src: url("/Ellesse/LittleMicroSansTrial-Li.otf") format("opentype");
    font-weight: 400;
    font-style: normal;
    font-display: swap;
  }

  @font-face {
    font-family: "Priamos";
    src: url("/Ellesse/Priamos-Regular.ttf") format("truetype");
    font-weight: 400;
    font-style: normal;
    font-display: swap;
  }

  :global(body) {
    margin: 0;
    min-height: 100%;
    background-color: rgb(249, 246, 239);
  }

  .page-wrapper {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
  }

  /* ===== HEADER ===== */
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

  .left a {
    display: flex;
    align-items: center;
  }

  .spacer {
    flex: 1;
  }

  .logo {
    height: 50px;
    width: auto;
    filter: brightness(0) invert(1);
  }

  .brand {
    font-family: "LittleMicroSans", sans-serif;
    text-transform: uppercase;
    font-size: 28px;
    font-weight: 300;
    color: rgb(249, 246, 239);
  }

  nav {
    display: flex;
    gap: 50px;
  }

  nav a {
    font-family: "Priamos", serif;
    text-decoration: none;
    color: rgb(200, 180, 160);
    font-weight: 300;
    font-size: 16px;
    letter-spacing: 1px;
    transition: color 0.3s;
  }

  nav a:hover,
  nav a:focus,
  nav a.active {
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
    flex: 1;
    padding: 40px 20px;
  }

  .booking-section {
    max-width: 1000px;
    margin: 0 auto;
  }

  .booking-header {
    text-align: center;
    margin-bottom: 40px;
  }

  .subtitle {
    font-family: "Priamos", serif;
    font-size: 16px;
    color: rgb(102, 74, 50);
    margin: 0 0 10px 0;
    letter-spacing: 2px;
  }

  .title {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 48px;
    font-weight: 100;
    text-transform: uppercase;
    color: rgb(38, 25, 17);
    margin: 0 0 20px 0;
    letter-spacing: 3px;
  }

  .description {
    font-family: "Priamos", serif;
    font-size: 16px;
    color: rgb(102, 74, 50);
    margin: 0;
    line-height: 1.6;
  }

  /* ===== CALENDAR ===== */
  .calendar-container {
    background: white;
    border-radius: 8px;
    box-shadow: 0 4px 20px rgba(38, 25, 17, 0.1);
    overflow: hidden;
    min-height: 600px;
    position: relative;
  }

  #cal-embed {
    width: 100%;
    min-height: 600px;
  }

  .loading {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
  }

  .spinner {
    width: 40px;
    height: 40px;
    border: 3px solid rgba(102, 74, 50, 0.2);
    border-top-color: rgb(102, 74, 50);
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin: 0 auto 15px;
  }

  @keyframes spin {
    to { transform: rotate(360deg); }
  }

  .loading p {
    font-family: "Priamos", serif;
    color: rgb(102, 74, 50);
    margin: 0;
  }

  /* ===== BOOKING INFO ===== */
  .booking-info {
    display: flex;
    justify-content: center;
    gap: 60px;
    margin-top: 50px;
    flex-wrap: wrap;
  }

  .info-item {
    display: flex;
    align-items: flex-start;
    gap: 15px;
  }

  .info-item svg {
    width: 28px;
    height: 28px;
    color: rgb(102, 74, 50);
    flex-shrink: 0;
    margin-top: 2px;
  }

  .info-item h3 {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 14px;
    font-weight: 400;
    text-transform: uppercase;
    color: rgb(38, 25, 17);
    margin: 0 0 5px 0;
    letter-spacing: 1px;
  }

  .info-item p {
    font-family: "Priamos", serif;
    font-size: 14px;
    color: rgb(102, 74, 50);
    margin: 0;
  }

  /* ===== FOOTER ===== */
  footer {
    background-color: rgb(38, 25, 17);
    padding: 25px 30px;
    margin-top: auto;
  }

  .footer-bottom {
    text-align: center;
  }

  .footer-bottom p {
    font-family: "Priamos", serif;
    font-size: 13px;
    color: rgb(200, 180, 160);
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

    .brand {
      font-size: 22px;
    }

    .logo {
      height: 40px;
    }

    main {
      padding: 30px 15px;
    }

    .title {
      font-size: 32px;
    }

    .booking-info {
      flex-direction: column;
      gap: 25px;
      align-items: center;
    }

    .calendar-container,
    #cal-embed {
      min-height: 500px;
    }
  }

  @media (max-width: 480px) {
    .title {
      font-size: 26px;
    }

    .subtitle,
    .description {
      font-size: 14px;
    }
  }
</style>

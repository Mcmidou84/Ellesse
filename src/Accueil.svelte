<script>
  import { onMount } from "svelte";
  import { link } from "svelte-spa-router";

  let menuOpen = $state(false);
  let currentImage = $state(0);

  const images = [
    {
      src: "/Ellesse/accueil_01.webp",
      alt: "Soin esthétique visage au cabinet Ellessé à Toulouse",
    },
    {
      src: "/Ellesse/accueil_02.webp",
      alt: "Ambiance luxueuse du salon de beauté Ellessé",
    },
    {
      src: "/Ellesse/accueil_03.webp",
      alt: "Soins de beauté inspirés des traditions russes",
    },
  ];

  onMount(() => {
    const interval = setInterval(() => {
      currentImage = (currentImage + 1) % images.length;
    }, 5000);

    return () => clearInterval(interval);
  });

  function toggleMenu() {
    menuOpen = !menuOpen;
  }

  function closeMenu() {
    menuOpen = false;
  }
</script>

<svelte:head>
  <!-- SEO de base -->
  <title
    >Ellessé | Cabinet d'esthétique à la russe à Toulouse - Soins de beauté</title
  >
  <meta
    name="description"
    content="Ellessé, cabinet d'esthétique à Toulouse spécialisé dans l'art de la beauté à la russe. Découvrez nos soins inspirés des traditions impériales et des secrets de beauté sibériens."
  />
  <meta
    name="keywords"
    content="esthétique, beauté russe, soins visage, institut beauté Toulouse, manucure russe, soins naturels, beauté sibérienne, cabinet esthétique 31200"
  />
  <link rel="canonical" href="https://ellesse-beaute.fr/" />

  <!-- Open Graph / Facebook -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://ellesse-beaute.fr/" />
  <meta
    property="og:title"
    content="Ellessé | Cabinet d'esthétique à la russe à Toulouse"
  />
  <meta
    property="og:description"
    content="L'art de la beauté à la russe. Soins inspirés des traditions impériales et des secrets de beauté sibériens."
  />
  <meta
    property="og:image"
    content="https://ellesse-beaute.fr/Ellesse/accueil_01.webp"
  />
  <meta property="og:image:width" content="1200" />
  <meta property="og:image:height" content="630" />
  <meta property="og:locale" content="fr_FR" />
  <meta property="og:site_name" content="Ellessé" />

  <!-- Twitter -->
  <meta name="twitter:card" content="summary_large_image" />
  <meta
    name="twitter:title"
    content="Ellessé | Cabinet d'esthétique à la russe à Toulouse"
  />
  <meta
    name="twitter:description"
    content="L'art de la beauté à la russe. Soins inspirés des traditions impériales."
  />
  <meta
    name="twitter:image"
    content="https://ellesse-beaute.fr/Ellesse/accueil_01.webp"
  />

  <!-- Schema.org page spécifique -->
  {@html `<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "WebPage",
    "@id": "https://ellesse-beaute.fr/#webpage",
    "url": "https://ellesse-beaute.fr/",
    "name": "Ellessé - Cabinet d'esthétique à la russe à Toulouse",
    "description": "Cabinet d'esthétique à Toulouse spécialisé dans l'art de la beauté à la russe. Soins visage, manucure russe, secrets de beauté sibériens au 73 Bd Silvio Trentin, 31200 Toulouse.",
    "isPartOf": {
      "@id": "https://ellesse-beaute.fr/#organization"
    },
    "primaryImageOfPage": {
      "@type": "ImageObject",
      "url": "https://ellesse-beaute.fr/Ellesse/accueil_01.webp"
    },
    "breadcrumb": {
      "@type": "BreadcrumbList",
      "itemListElement": [
        {
          "@type": "ListItem",
          "position": 1,
          "name": "Accueil",
          "item": "https://ellesse-beaute.fr/"
        }
      ]
    }
  }
  </script>`}
</svelte:head>

<div
  class="background-slideshow"
  role="img"
  aria-label="Diaporama présentant le cabinet d'esthétique Ellessé"
>
  {#each images as image, index}
    <div
      class="background-slide"
      class:active={currentImage === index}
      style="background-image: url('{image.src}')"
      aria-hidden={currentImage !== index}
    ></div>
  {/each}
</div>

<div class="page-wrapper">
  <header>
    <div class="left">
      <a href="/" use:link aria-label="Accueil Ellessé">
        <img
          src="./logo.png"
          alt="Logo Ellessé - Cabinet d'esthétique à Toulouse"
          class="logo"
          width="50"
          height="50"
        />
      </a>
      <span class="brand">Ellessé</span>
    </div>

    <nav class:open={menuOpen} aria-label="Navigation principale">
      <a href="/" use:link class="active" onclick={closeMenu}>Accueil</a>
      <a href="/galerie" use:link onclick={closeMenu}>Galerie</a>
      <a href="/prestations" use:link onclick={closeMenu}>Prestations</a>
      <a href="/contact" use:link onclick={closeMenu}>Contact</a>
      <a href="/rendez-vous" use:link onclick={closeMenu}>Rendez-vous</a>
      <a href="/boutique" use:link onclick={closeMenu}>Boutique</a>
    </nav>

    <button
      class="burger"
      class:open={menuOpen}
      onclick={toggleMenu}
      aria-label={menuOpen ? "Fermer le menu" : "Ouvrir le menu"}
      aria-expanded={menuOpen}
      aria-controls="mobile-menu"
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

  <main id="main-content">
    <article class="hero">
      <p class="subtitle">Cabinet d'esthétique</p>
      <h1 class="title">
        <span>L'art de la beauté</span>
        <span>à la russe</span>
      </h1>
      <a
        href="/rendez-vous"
        use:link
        class="cta-button"
        aria-label="Prendre rendez-vous pour un soin esthétique"
        >Prendre rendez-vous</a
      >
    </article>
  </main>

  <footer>
<section class="values-container" aria-label="Nos valeurs">
  <article class="value-item">
    <img src="elegant.png" alt="Élégance" class="value-icon" />
    <h2 class="value-title">Élégance</h2>
    <p class="value-text">
      La grâce et le raffinement inspirés de l'esthétique impériale russe
    </p>
  </article>

  <article class="value-item">
    <img src="tradition.png" alt="Tradition" class="value-icon" />
    <h2 class="value-title">Tradition</h2>
    <p class="value-text">
      Un savoir-faire ancestral transmis de génération en génération
    </p>
  </article>

  <article class="value-item">
    <img src="diamond.png" alt="Excellence" class="value-icon" />
    <h2 class="value-title">Excellence</h2>
    <p class="value-text">
      La quête de la perfection dans chaque geste et chaque soin
    </p>
  </article>

  <article class="value-item">
    <img src="pure.png" alt="Pureté" class="value-icon" />
    <h2 class="value-title">Pureté</h2>
    <p class="value-text">
      Des soins naturels inspirés des secrets de beauté sibériens
    </p>
  </article>
</section>

    <div class="footer-bottom">
      <p>© 2025 Ellessé — Réalisation M.C.</p>
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

  :global(html) {
    height: 100%;
  }

  :global(body) {
    margin: 0;
    min-height: 100%;
  }

  .background-slideshow {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
  }

  .background-slide {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    opacity: 0;
    transition: opacity 1.5s ease-in-out;
  }

  .background-slide.active {
    opacity: 1;
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
  nav a:focus {
    color: rgb(249, 246, 239);
  }

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
    font-family: "Priamos", serif;
    font-size: 18px;
    color: rgb(249, 246, 239);
    margin: 0 0 15px 0;
    letter-spacing: 2px;
  }

  .title {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 90px;
    font-weight: 100;
    text-transform: uppercase;
    color: rgb(249, 246, 239);
    margin: 0;
    max-width: 1200px;
    line-height: 1.2;
    text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
  }

  .title span {
    display: block;
  }

  .cta-button {
    margin-top: 40px;
    padding: 16px 40px;
    font-family: "Priamos", serif;
    font-size: 16px;
    color: rgb(38, 25, 17);
    background-color: rgb(249, 246, 239);
    border: none;
    text-decoration: none;
    letter-spacing: 1px;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .cta-button:hover,
  .cta-button:focus {
    background-color: rgb(38, 25, 17);
    color: rgb(249, 246, 239);
    outline: 2px solid rgb(249, 246, 239);
    outline-offset: 2px;
  }

  /* ===== FOOTER ===== */
  footer {
    background-color: rgb(249, 246, 239);
    padding: 60px 30px 30px;
    margin-top: auto;
  }

  .values-container {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 30px;
    max-width: 1200px;
    margin: 0 auto;
  }

  .value-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
  }

  .value-icon {
    width: 70px;
    height: 70px;
    color: rgb(102, 74, 50);
    margin-bottom: 20px;
    transition: color 0.3s ease;
  }

  .value-item:hover .value-icon {
    color: rgb(38, 25, 17);
  }

  .value-title {
    font-family: "LittleMicroSans", sans-serif;
    text-transform: uppercase;
    font-size: 16px;
    font-weight: 300;
    color: rgb(38, 25, 17);
    margin: 0 0 10px 0;
    letter-spacing: 2px;
  }

  .value-text {
    font-family: "Priamos", serif;
    font-size: 14px;
    color: rgb(102, 74, 50);
    margin: 0;
    line-height: 1.6;
  }

  .footer-bottom {
    margin-top: 50px;
    padding-top: 20px;
    border-top: 1px solid rgba(102, 74, 50, 0.3);
    text-align: center;
  }

  .footer-bottom p {
    font-family: "Priamos", serif;
    font-size: 13px;
    color: rgb(102, 74, 50);
    margin: 0;
  }

  /* ===== RESPONSIVE ===== */
  @media (max-width: 1035px) and (min-width: 769px) {
    .values-container {
      grid-template-columns: repeat(4, 1fr);
      gap: 20px;
    }

    .value-icon {
      width: 50px;
      height: 50px;
      margin-bottom: 12px;
    }

    .value-title {
      font-size: 12px;
    }

    .value-text {
      font-size: 12px;
    }
  }

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

    .cta-button {
      margin-top: 25px;
      padding: 12px 30px;
      font-size: 14px;
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

    footer {
      padding: 25px 15px 15px;
    }

    .values-container {
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
    }

    .value-icon {
      width: 35px;
      height: 35px;
      margin-bottom: 8px;
    }

    .value-title {
      font-size: 9px;
      letter-spacing: 1px;
    }

    .value-text {
      display: none;
    }

    .footer-bottom {
      margin-top: 20px;
      padding-top: 15px;
    }

    .footer-bottom p {
      font-size: 11px;
    }
  }

  @media (max-width: 480px) {
    .title {
      font-size: 32px;
    }

    .cta-button {
      margin-top: 20px;
      padding: 10px 25px;
      font-size: 13px;
    }

    .values-container {
      grid-template-columns: repeat(4, 1fr);
      gap: 8px;
    }

    .value-icon {
      width: 28px;
      height: 28px;
      margin-bottom: 5px;
    }

    .value-title {
      font-size: 7px;
      letter-spacing: 0.5px;
    }

    .value-text {
      display: none;
    }

    footer {
      padding: 20px 10px 12px;
    }

    .footer-bottom {
      margin-top: 15px;
      padding-top: 10px;
    }
  }
</style>

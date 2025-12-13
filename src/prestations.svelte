<script>
  import { onMount } from "svelte";
  import { link } from "svelte-spa-router";

  let menuOpen = $state(false);
  let sliderEl;

  function toggleMenu() {
    menuOpen = !menuOpen;
  }

  function closeMenu() {
    menuOpen = false;
  }

  // ICONES PNG LOCALES
  const prestations = [
    {
      id: "ongles-mains",
      title: "Beauté des Ongles Mains",
      icon: "nail.png",
      image: "/Ellesse/ongles.webp",
      services: [
        { name: "Manucure russe seule", duration: "30min", price: "15€" },
        { name: "Manucure russe + soins", duration: "1h", price: "35€" },
        { name: "Pose Vernis Semi-permanent", duration: "30min", price: "25€" },
        { name: "Vernis semi permanent + Manucure russe", duration: "1h20", price: "40€" },
        { name: "Forfait pose et dépose semi-permanent", duration: "45min", price: "40€" },
      ]
    },
    {
      id: "pieds",
      title: "Beauté des Pieds",
      icon: "nail.png",
      image: "/Ellesse/ongles.webp",
      services: [
        { name: "Vernis Semi-permanent PIEDS + cuticules", duration: "30min", price: "29€" },
        { name: "Vernis Semi-permanent PIEDS", duration: "20min", price: "20€" },
        { name: "Technique russe + dépose & pose VSP", duration: "1h", price: "47€" },
        { name: "Technique russe + pose VSP", duration: "45min", price: "38€" },
        { name: "Supplément French", duration: "15min", price: "5€" },
      ]
    },
    {
      id: "cils",
      title: "Extensions de Cils",
      icon: "eyelashes.png",
      image: "/Ellesse/cils.webp",
      services: [
        { name: "Cil à cil naturel", duration: "1h", price: "60€" },
        { name: "Remplissage cil à cil naturel", duration: "45min", price: "40€" },
        { name: "Cil à cil volumineux", duration: "1h30", price: "70€" },
        { name: "Remplissage cil à cil volumineux", duration: "1h", price: "45€" },
        { name: "Extensions Mixte", duration: "1h30", price: "90€" },
        { name: "Remplissage Mixte", duration: "1h15", price: "55€" },
        { name: "Volume russe", duration: "2h", price: "100€" },
        { name: "Remplissage volume russe", duration: "1h30", price: "80€" },
        { name: "Dépose extensions", duration: "20min", price: "10€" },
      ]
    },
    {
      id: "rehaussement",
      title: "Rehaussement de Cils",
      icon: "eyelashes.png",
      image: "/Ellesse/cils.webp",
      services: [
        { name: "Rehaussement + teinture", duration: "1h", price: "55€" },
        { name: "Rehaussement sans teinture", duration: "45min", price: "50€" },
        { name: "Teinture cils haut", duration: "15min", price: "10€" },
        { name: "Teinture cils haut + bas", duration: "15min", price: "15€" },
      ]
    },
    {
      id: "levres",
      title: "Semi Permanent Lèvres",
      icon: "lips.png",
      image: "/Ellesse/levres.webp",
      services: [
        { name: "Candy Lips (remplissage complet)", duration: "3h", price: "200€" },
        { name: "Contour des lèvres (retouche incluse)", duration: "1h", price: "150€" },
        { name: "Retouche fixatrice", duration: "3h", price: "50€" },
        { name: "Retouche annuelle candy lips", duration: "2h", price: "110€" },
        { name: "Retouche annuelle contour", duration: "1h", price: "80€" },
      ]
    },
    {
      id: "sourcils",
      title: "Semi Permanent Sourcils",
      icon: "eyebrows.png",
      image: "/Ellesse/sourcils.webp",
      services: [
        { name: "Microblading", duration: "2h", price: "190€" },
        { name: "Retouche fixatrice microblading", duration: "1h", price: "50€" },
        { name: "Microshading / MIXT", duration: "3h", price: "200€" },
        { name: "Retouche fixatrice microshading", duration: "2h", price: "50€" },
        { name: "Entretien 3-6 mois", duration: "45min", price: "70€" },
        { name: "Retouche annuelle", duration: "1h30", price: "100€" },
        { name: "Retouche nouveau client", duration: "1h", price: "140€" },
      ]
    },
    {
      id: "yeux",
      title: "Semi Permanent Yeux",
      icon: "eyelashes.png",
      image: "/Ellesse/cils.webp",
      services: [
        { name: "Ras de cils simple (retouche incluse)", duration: "1h", price: "180€" },
        { name: "Retouche annuelle ras de cils simple", duration: "30min", price: "80€" },
        { name: "Ras de cils double (retouche incluse)", duration: "1h30", price: "210€" },
        { name: "Retouche annuelle ras de cils double", duration: "1h", price: "130€" },
        { name: "Eye Liner", duration: "30min", price: "230€" },
      ]
    },
    {
      id: "entretien-sourcils",
      title: "Entretien Sourcils",
      icon: "eyebrows.png",
      image: "/Ellesse/sourcils.webp",
      services: [
        { name: "Épilation simple sourcils", duration: "15min", price: "10€" },
        { name: "Teinture Sourcils", duration: "30min", price: "25€" },
        { name: "Restructuration sourcils", duration: "30min", price: "25€" },
        { name: "Restructuration + teinture", duration: "45min", price: "45€" },
        { name: "Airbrush Sourcils", duration: "45min", price: "40€" },
        { name: "Browlift", duration: "30min", price: "40€" },
        { name: "Browlift + teinture", duration: "45min", price: "45€" },
        { name: "Browlift + teinture + restructuration", duration: "45min", price: "60€" },
      ]
    },
    {
      id: "visage",
      title: "Soins Visage",
      icon: "nail.png",
      image: "/Ellesse/sourcils.webp",
      services: [
        { name: "Soin visage hydratant", duration: "1h", price: "50€" },
        { name: "Gommage et dermaplaning", duration: "30min", price: "65€" },
        { name: "Microneedling", duration: "1h", price: "80€" },
        { name: "Microneedling forfait 4 séances", duration: "45min", price: "290€" },
        { name: "BB Glow", duration: "30min", price: "80€" },
        { name: "BB Glow Forfait 4 séances", duration: "30min", price: "290€" },
        { name: "Microglow", duration: "1h", price: "110€" },
        { name: "Hydrafacial", duration: "1h", price: "90€" },
        { name: "Hydraneedling", duration: "1h20", price: "150€" },
        { name: "Hydraglow", duration: "1h20", price: "150€" },
      ]
    },
    {
      id: "radiofrequence",
      title: "Radiofréquence Visage",
      icon: "nail.png",
      image: "/Ellesse/sourcils.webp",
      services: [
        { name: "Radiofréquence visage - 1 séance", duration: "30min", price: "60€" },
        { name: "Radiofréquence visage - Forfait 6", duration: "30min", price: "250€" },
        { name: "Radiofréquence visage - Forfait 10", duration: "30min", price: "450€" },
      ]
    },
    {
      id: "corps",
      title: "Soins Corps",
      icon: "nail.png",
      image: "/Ellesse/ongles.webp",
      services: [
        { name: "Lipocavitation ventre et taille", duration: "1h30", price: "120€" },
        { name: "Lipocavitation cuisses", duration: "1h", price: "99€" },
        { name: "Drainage lymphatique corps entier", duration: "1h30", price: "100€" },
        { name: "Cure 3 séances drainage", duration: "1h30", price: "240€" },
      ]
    },
    {
      id: "lifting",
      title: "Lifting Colombien",
      icon: "nail.png",
      image: "/Ellesse/ongles.webp",
      services: [
        { name: "Lifting visage", duration: "30min", price: "30€" },
        { name: "Lifting poitrine", duration: "30min", price: "55€" },
        { name: "Rehaussement fessier", duration: "30min", price: "60€" },
        { name: "Forfait fessier 6 séances", duration: "45min", price: "250€" },
        { name: "Forfait fessier 10 séances", duration: "1h", price: "484€" },
      ]
    },
    {
      id: "bronzage",
      title: "Bronzage Sans UV",
      icon: "nail.png",
      image: "/Ellesse/ongles.webp",
      services: [
        { name: "Visage et décolleté", duration: "10min", price: "15€" },
        { name: "Visage + Buste + Bras", duration: "15min", price: "25€" },
        { name: "Jambes", duration: "15min", price: "25€" },
        { name: "Corps entier visage inclus", duration: "30min", price: "40€" },
      ]
    },
    {
      id: "autres",
      title: "Autres Prestations",
      icon: "nail.png",
      image: "/Ellesse/sourcils.webp",
      services: [
        { name: "Blanchiment dentaire", duration: "1h", price: "180€" },
        { name: "Grain de Beauté (retouche incluse)", duration: "15min", price: "50€" },
        { name: "Taches de Rousseur", duration: "1h", price: "120€" },
      ]
    }
  ];
</script>


<svelte:head>
  <title>Nos Prestations | Ellessé - Maquillage permanent, Cils, Ongles à Toulouse</title>
  <meta name="description" content="Découvrez nos prestations : microblading, candy lips, extension de cils volume russe, manucure russe, pose de gel. Cabinet d'esthétique à Toulouse." />
  <meta name="keywords" content="microblading Toulouse, candy lips, extension cils volume russe, manucure russe Toulouse, pose gel, maquillage semi permanent" />
  <link rel="canonical" href="https://ellesse-beaute.fr/prestations" />

  <!-- Open Graph -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://ellesse-beaute.fr/prestations" />
  <meta property="og:title" content="Nos Prestations | Ellessé - Maquillage permanent, Cils, Ongles à Toulouse" />
  <meta property="og:description" content="Découvrez nos prestations : microblading, candy lips, extension de cils, manucure russe." />
  <meta property="og:image" content="https://ellesse-beaute.fr/Ellesse/sourcils.webp" />

  <!-- Schema.org -->
  {@html `<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "WebPage",
    "@id": "https://ellesse-beaute.fr/prestations#webpage",
    "url": "https://ellesse-beaute.fr/prestations",
    "name": "Nos Prestations - Ellessé Cabinet d'esthétique",
    "description": "Découvrez nos prestations esthétiques : maquillage semi permanent sourcils et lèvres, extension de cils, manucure russe.",
    "isPartOf": {
      "@id": "https://ellesse-beaute.fr/#organization"
    },
    "breadcrumb": {
      "@type": "BreadcrumbList",
      "itemListElement": [
        {"@type": "ListItem", "position": 1, "name": "Accueil", "item": "https://ellesse-beaute.fr/"},
        {"@type": "ListItem", "position": 2, "name": "Prestations", "item": "https://ellesse-beaute.fr/prestations"}
      ]
    }
  }
  </script>`}
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
      <a href="/prestations" use:link class="active" onclick={closeMenu}>Prestations</a>
      <a href="/contact" use:link onclick={closeMenu}>Contact</a>
      <a href="/rendez-vous" use:link onclick={closeMenu}>Rendez-vous</a>
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
    <section class="prestations-section">
      <div class="prestations-header">
        <p class="subtitle">Nos soins</p>
        <h1 class="title">Prestations</h1>
        <p class="description">
          Découvrez notre gamme complète de soins esthétiques inspirés des traditions russes
        </p>
      </div>

      <div class="scroll-hint">
        <span>Glissez pour découvrir</span>
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <line x1="5" y1="12" x2="19" y2="12"></line>
          <polyline points="12 5 19 12 12 19"></polyline>
        </svg>
      </div>
      
      <div class="slider-container">
        <div class="slider" bind:this={sliderEl}>
          {#each prestations as prestation}
            <article class="prestation-card">
              <div class="card-image-wrapper">
                <img src={prestation.image} alt={prestation.title} class="card-image" loading="lazy" />
                <div class="card-gradient"></div>
              </div>
              
              <div class="card-body">
                <div class="card-header">
                  <img src={prestation.icon} alt={prestation.title} class="card-icon" />
                  <h2>{prestation.title}</h2>
                </div>
                
                <ul class="services-list">
                  {#each prestation.services as service}
                    <li>
                      <span class="service-name">{service.name}</span>
                      {#if service.price}
                        <span class="service-details">
                          {#if service.duration}<span class="service-duration">{service.duration}</span>{/if}
                          <span class="service-price">{service.price}</span>
                        </span>
                      {/if}
                    </li>
                  {/each}
                </ul>
                
                <a href="/rendez-vous" use:link class="card-cta">Réserver</a>
              </div>
            </article>
          {/each}
        </div>
      </div>

      <div class="cta-section">
        <p>Envie d'un soin sur mesure ?</p>
        <a href="/rendez-vous" use:link class="main-cta">Prendre rendez-vous</a>
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

    
  .card-icon {
    width: 28px;
    height: 28px;
    object-fit: contain;
    filter: brightness(0) saturate(100%);
  }




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

  :global(iconify-icon) {
    color: rgb(102, 74, 50);
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
    padding: 40px 0;
  }

  .prestations-section {
    width: 100%;
  }

  .prestations-header {
    text-align: center;
    margin-bottom: 30px;
    padding: 0 20px;
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

  /* ===== SCROLL HINT ===== */
  .scroll-hint {
    display: none;
    align-items: center;
    justify-content: center;
    gap: 8px;
    font-family: "Priamos", serif;
    font-size: 14px;
    color: rgb(102, 74, 50);
    margin-bottom: 15px;
    padding: 0 20px;
  }

  .scroll-hint svg {
    color: rgb(102, 74, 50);
  }

  /* ===== SLIDER ===== */
  .slider-container {
    width: 100%;
    overflow: hidden;
  }

  .slider {
    display: flex;
    gap: 25px;
    padding: 10px 40px 30px 40px;
    justify-content: center;
    flex-wrap: nowrap;
  }

  /* ===== CARD ===== */
  .prestation-card {
    flex-shrink: 0;
    width: 320px;
    background: white;
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0 4px 20px rgba(38, 25, 17, 0.08);
    display: flex;
    flex-direction: column;
  }

  .prestation-card:hover {
    box-shadow: 0 8px 35px rgba(38, 25, 17, 0.14);
  }

  /* Card Image */
  .card-image-wrapper {
    position: relative;
    height: 180px;
    overflow: hidden;
  }

  .card-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }

  .card-gradient {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 80px;
    background: linear-gradient(
      to top,
      rgba(255, 255, 255, 1) 0%,
      rgba(255, 255, 255, 0.9) 20%,
      rgba(255, 255, 255, 0.6) 45%,
      rgba(255, 255, 255, 0.2) 70%,
      rgba(255, 255, 255, 0) 100%
    );
    pointer-events: none;
  }

  /* Card Body */
  .card-body {
    padding: 20px 22px 22px;
    display: flex;
    flex-direction: column;
    flex: 1;
  }

  .card-header {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 16px;
  }

  .card-header h2 {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 17px;
    font-weight: 300;
    text-transform: uppercase;
    color: rgb(38, 25, 17);
    margin: 0;
    letter-spacing: 1.5px;
  }

  /* Services List */
  .services-list {
    list-style: none;
    padding: 0;
    margin: 0 0 18px 0;
    flex: 1;
  }

  .services-list li {
    font-family: "Priamos", serif;
    font-size: 13px;
    color: rgb(102, 74, 50);
    padding: 8px 0;
    border-bottom: 1px solid rgba(102, 74, 50, 0.08);
    padding-left: 14px;
    position: relative;
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    gap: 8px;
  }

  .services-list li:last-child {
    border-bottom: none;
  }

  .services-list li::before {
    content: "";
    position: absolute;
    left: 0;
    top: 12px;
    width: 4px;
    height: 4px;
    background: rgb(180, 160, 140);
    border-radius: 50%;
  }

  .service-name {
    flex: 1;
    line-height: 1.3;
  }

  .service-details {
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    font-size: 12px;
    white-space: nowrap;
  }

  .service-duration {
    color: rgb(150, 130, 110);
    font-size: 11px;
  }

  .service-price {
    color: rgb(38, 25, 17);
    font-weight: 500;
  }

  /* Card CTA */
  .card-cta {
    display: block;
    text-align: center;
    font-family: "Priamos", serif;
    font-size: 14px;
    color: rgb(249, 246, 239);
    background: rgb(38, 25, 17);
    text-decoration: none;
    padding: 13px 24px;
    border-radius: 10px;
    letter-spacing: 1px;
    margin-top: auto;
  }

  .card-cta:hover {
    background: rgb(60, 45, 35);
  }

  /* ===== CTA SECTION ===== */
  .cta-section {
    text-align: center;
    margin: 50px 20px 0;
    padding: 50px 30px;
    background: rgb(38, 25, 17);
    border-radius: 20px;
    max-width: 1000px;
    margin-left: auto;
    margin-right: auto;
  }

  .cta-section p {
    font-family: "Priamos", serif;
    font-size: 18px;
    color: rgb(200, 180, 160);
    margin: 0 0 25px 0;
    letter-spacing: 1px;
  }

  .main-cta {
    display: inline-block;
    font-family: "Priamos", serif;
    font-size: 16px;
    color: rgb(38, 25, 17);
    background-color: rgb(249, 246, 239);
    text-decoration: none;
    padding: 16px 40px;
    border-radius: 10px;
    letter-spacing: 1px;
  }

  .main-cta:hover {
    background-color: rgb(220, 210, 200);
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
  @media (max-width: 1400px) {
    .slider {
      justify-content: flex-start;
      overflow-x: auto;
      -webkit-overflow-scrolling: touch;
      scrollbar-width: thin;
      scrollbar-color: rgba(102, 74, 50, 0.3) transparent;
    }

    .slider::-webkit-scrollbar {
      height: 6px;
    }

    .slider::-webkit-scrollbar-track {
      background: rgba(102, 74, 50, 0.1);
      border-radius: 10px;
    }

    .slider::-webkit-scrollbar-thumb {
      background: rgba(102, 74, 50, 0.3);
      border-radius: 10px;
    }

    .scroll-hint {
      display: flex;
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

    .brand {
      font-size: 22px;
    }

    .logo {
      height: 40px;
    }

    main {
      padding: 30px 0;
    }

    .title {
      font-size: 32px;
    }

    .prestations-header {
      margin-bottom: 20px;
    }

    .scroll-hint {
      padding: 0 20px 12px;
    }

    .slider {
      padding: 10px 20px 25px 20px;
      gap: 18px;
    }

    .prestation-card {
      width: 290px;
    }

    .card-image-wrapper {
      height: 160px;
    }

    .card-body {
      padding: 18px 18px 20px;
    }

    .card-header h2 {
      font-size: 15px;
    }

    .services-list li {
      font-size: 12px;
      padding: 6px 0 6px 12px;
    }

    .service-details {
      font-size: 11px;
    }

    .service-duration {
      font-size: 10px;
    }

    .cta-section {
      margin: 40px 15px 0;
      padding: 40px 20px;
      border-radius: 16px;
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

    .prestation-card {
      width: 280px;
    }

    .card-image-wrapper {
      height: 145px;
    }

    .card-body {
      padding: 15px 16px 18px;
    }

    .card-header {
      margin-bottom: 14px;
      gap: 8px;
    }

    .card-header h2 {
      font-size: 14px;
    }

    .card-icon {
      width: 24px;
      height: 24px;
    }

    .services-list {
      margin-bottom: 16px;
    }

    .services-list li {
      font-size: 11px;
      padding: 5px 0 5px 10px;
    }

    .service-details {
      font-size: 10px;
    }

    .service-duration {
      font-size: 9px;
    }

    .card-cta {
      padding: 11px 20px;
      font-size: 13px;
      border-radius: 8px;
    }

    .cta-section p {
      font-size: 16px;
    }

    .main-cta {
      font-size: 14px;
      padding: 14px 30px;
    }
  }
</style>
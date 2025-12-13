<script>
  import { onMount } from "svelte";
  import { link, querystring, push } from "svelte-spa-router";

  let menuOpen = $state(false);
  let calLoaded = $state(false);
  let selectedService = $state(null);
  let calLink = $state("salon-ellesse/rendez-vous");

  // Map duration strings to Cal.com event type slugs
  function getCalLinkFromDuration(duration) {
    if (!duration) return "salon-ellesse/rendez-vous";
    
    // Normalize duration string
    const normalized = duration.toLowerCase().trim();
    
    // Map of duration strings to cal.com slugs
    const durationMap = {
      "10min": "salon-ellesse/10min",
      "15min": "salon-ellesse/15min",
      "20min": "salon-ellesse/20min",
      "30min": "salon-ellesse/30min",
      "45min": "salon-ellesse/45min",
      "1h": "salon-ellesse/1h",
      "1h15": "salon-ellesse/1h15",
      "1h20": "salon-ellesse/1h20",
      "1h30": "salon-ellesse/1h30",
      "2h": "salon-ellesse/2h",
      "2h30": "salon-ellesse/2h30",
      "3h": "salon-ellesse/3h",
    };
    
    return durationMap[normalized] || "salon-ellesse/rendez-vous";
  }

  // Parse query string to get service info
  $effect(() => {
    if ($querystring) {
      const params = new URLSearchParams($querystring);
      const serviceName = params.get('service');
      const duration = params.get('duration');
      const price = params.get('price');
      if (serviceName) {
        selectedService = {
          name: decodeURIComponent(serviceName),
          duration: duration ? decodeURIComponent(duration) : null,
          price: price ? decodeURIComponent(price) : null
        };
        // Update calLink based on duration
        calLink = getCalLinkFromDuration(duration ? decodeURIComponent(duration) : null);
      }
    }
  });

  // Clear selected service and go back to prestations
  function clearSelection() {
    push('/prestations');
  }

  // Function to reload Cal.com embed with new event type
  function reloadCalendar(newCalLink) {
    const calEmbed = document.getElementById('cal-embed');
    if (calEmbed) {
      calEmbed.innerHTML = '';
    }
    
    if (window.Cal) {
      Cal("inline", {
        elementOrSelector: "#cal-embed",
        calLink: newCalLink,
        config: {
          theme: "light",
          hideEventTypeDetails: false,
          layout: "month_view",
          locale: "fr",
        },
      });
    }
  }

  // Injection et initialisation de Cal.com
  onMount(() => {
    // Check if service is selected, otherwise redirect to prestations
    const hasService = window.location.hash.includes('service=');
    if (!hasService) {
      push('/prestations');
      return;
    }

    // Parse query params first to get the right calLink
    let initialCalLink = "salon-ellesse/rendez-vous";
    if (window.location.hash.includes('?')) {
      const queryStr = window.location.hash.split('?')[1];
      const params = new URLSearchParams(queryStr);
      const duration = params.get('duration');
      if (duration) {
        initialCalLink = getCalLinkFromDuration(decodeURIComponent(duration));
      }
    }

    (function (C, A, L) {
      let p = function (a, ar) {
        a.q.push(ar);
      };
      let d = C.document;
      C.Cal =
        C.Cal ||
        function () {
          let cal = C.Cal;
          let ar = arguments;
          if (!cal.loaded) {
            cal.ns = {};
            cal.q = cal.q || [];
            d.head.appendChild(d.createElement("script")).src = A;
            cal.loaded = true;
          }
          if (ar[0] === L) {
            const api = function () {
              p(api, arguments);
            };
            const namespace = ar[1];
            api.q = api.q || [];
            typeof namespace === "string"
              ? (cal.ns[namespace] = api) && p(api, ar)
              : p(cal, ar);
            return;
          }
          p(cal, ar);
        };
    })(window, "https://app.cal.com/embed/embed.js", "init");

    Cal("init", { origin: "https://cal.com" });

    Cal("inline", {
      elementOrSelector: "#cal-embed",
      calLink: initialCalLink,
      config: {
        theme: "light",
        hideEventTypeDetails: false,
        layout: "month_view",
        locale: "fr",
      },
    });
    Cal("ui", {
      styles: { branding: { brandColor: "#261911" } },
    });
    calLoaded = true;
  });

  // Menu responsive
  function toggleMenu() {
    menuOpen = !menuOpen;
  }

  function closeMenu() {
    menuOpen = false;
  }
</script>

<svelte:head>
  <!-- SEO de base -->
  <title>Rendez-vous en ligne | Ellessé - Cabinet d'esthétique à Toulouse</title>
  <meta name="description" content="Prenez rendez-vous en ligne au cabinet d'esthétique Ellessé à Toulouse. Réservez votre soin de beauté à la russe en quelques clics. Disponible 24h/24." />
  <meta name="keywords" content="rendez-vous esthétique Toulouse, réservation soin beauté, prendre rdv institut beauté, cabinet esthétique 31200, booking beauté russe" />
  <link rel="canonical" href="https://ellesse-beaute.fr/rendez-vous" />
  
  <!-- Open Graph / Facebook -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://ellesse-beaute.fr/rendez-vous" />
  <meta property="og:title" content="Prendre rendez-vous | Ellessé - Cabinet d'esthétique à Toulouse" />
  <meta property="og:description" content="Réservez votre soin de beauté à la russe en ligne. Confirmation immédiate par email." />
  <meta property="og:image" content="https://ellesse-beaute.fr/Ellesse/accueil_01.webp" />
  <meta property="og:image:width" content="1200" />
  <meta property="og:image:height" content="630" />
  <meta property="og:locale" content="fr_FR" />
  <meta property="og:site_name" content="Ellessé" />
  
  <!-- Twitter -->
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:title" content="Prendre rendez-vous | Ellessé - Esthétique à Toulouse" />
  <meta name="twitter:description" content="Réservez votre soin de beauté à la russe en ligne." />
  <meta name="twitter:image" content="https://ellesse-beaute.fr/Ellesse/accueil_01.webp" />
  
  <!-- Schema.org page réservation -->
  {@html `<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "WebPage",
    "@id": "https://ellesse-beaute.fr/rendez-vous#webpage",
    "url": "https://ellesse-beaute.fr/rendez-vous",
    "name": "Prendre rendez-vous - Ellessé Cabinet d'esthétique",
    "description": "Réservez votre soin de beauté en ligne au cabinet Ellessé à Toulouse. Soins visage, manucure russe, beauté sibérienne au 73 Bd Silvio Trentin, 31200 Toulouse.",
    "isPartOf": {
      "@id": "https://ellesse-beaute.fr/#organization"
    },
    "breadcrumb": {
      "@type": "BreadcrumbList",
      "itemListElement": [
        {
          "@type": "ListItem",
          "position": 1,
          "name": "Accueil",
          "item": "https://ellesse-beaute.fr/"
        },
        {
          "@type": "ListItem",
          "position": 2,
          "name": "Rendez-vous",
          "item": "https://ellesse-beaute.fr/rendez-vous"
        }
      ]
    },
    "potentialAction": {
      "@type": "ReserveAction",
      "target": {
        "@type": "EntryPoint",
        "urlTemplate": "https://ellesse-beaute.fr/rendez-vous",
        "actionPlatform": [
          "http://schema.org/DesktopWebPlatform",
          "http://schema.org/MobileWebPlatform"
        ]
      },
      "result": {
        "@type": "Reservation",
        "name": "Réservation soin esthétique"
      }
    }
  }
  </script>`}
</svelte:head>

<div class="page-wrapper">
  <header>
    <div class="left">
      <a href="/" use:link aria-label="Accueil Ellessé">
        <img
          src="./logo.png"
          alt="Logo Ellessé"
          class="logo"
          width="50"
          height="50"
        />
      </a>
      <span class="brand">Ellessé</span>
    </div>

    <nav class:open={menuOpen}>
      <a href="/" use:link onclick={closeMenu}>Accueil</a>
      <a href="/galerie" use:link onclick={closeMenu}>Galerie</a>
      <a href="/prestations" use:link class="active" onclick={closeMenu}>Prestations</a>
      <a href="/contact" use:link onclick={closeMenu}>Contact</a>
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
          Choisissez votre soin et le créneau qui vous convient. Vous recevrez
          une confirmation par email.
        </p>
      </div>

      {#if selectedService}
        <div class="selected-service">
          <div class="selected-service-content">
            <div class="selected-service-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <path d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
            </div>
            <div class="selected-service-info">
              <span class="selected-label">Prestation sélectionnée</span>
              <h3>{selectedService.name}</h3>
              <div class="selected-details">
                {#if selectedService.duration}
                  <span class="detail-item">
                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                      <path d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                    </svg>
                    {selectedService.duration}
                  </span>
                {/if}
                {#if selectedService.price}
                  <span class="detail-item">
                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                      <path d="M14.121 15.536c-1.171 1.952-3.07 1.952-4.242 0-1.172-1.953-1.172-5.119 0-7.072 1.171-1.952 3.07-1.952 4.242 0M8 10.5h4m-4 3h4m9-1.5a9 9 0 11-18 0 9 9 0 0118 0z" />
                    </svg>
                    {selectedService.price}
                  </span>
                {/if}
              </div>
            </div>
            <button class="clear-selection" onclick={clearSelection} aria-label="Retirer la sélection">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
          <div class="selected-hint-row">
            <p class="selected-hint">Le calendrier affiche les créneaux disponibles pour cette prestation.</p>
            <a href="/prestations" use:link class="change-service-link">Changer de prestation</a>
          </div>
        </div>
      {/if}

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
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.5"
          >
            <path d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
          <div>
            <h3>Horaires</h3>
            <p>Lundi - Samedi : 9h30 - 21h</p>
          </div>
        </div>
        <div class="info-item">
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.5"
          >
            <path
              d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z"
            />
          </svg>
          <div>
            <h3>Contact</h3>
            <p>+33 6 38 93 66 48</p>
          </div>
        </div>
        <div class="info-item">
          <svg
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.5"
          >
            <path
              d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"
            />
            <path d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
          </svg>
          <div>
            <h3>Adresse</h3>
            <p>73 Bd Silvio Trentin<br />31200 Toulouse</p>
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

  /* ===== SELECTED SERVICE ===== */
  .selected-service {
    background: linear-gradient(135deg, rgb(38, 25, 17) 0%, rgb(60, 45, 35) 100%);
    border-radius: 16px;
    padding: 24px;
    margin-bottom: 30px;
    color: rgb(249, 246, 239);
  }

  .selected-service-content {
    display: flex;
    align-items: center;
    gap: 20px;
  }

  .selected-service-icon {
    width: 50px;
    height: 50px;
    background: rgba(249, 246, 239, 0.1);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }

  .selected-service-icon svg {
    width: 28px;
    height: 28px;
    color: rgb(200, 180, 160);
  }

  .selected-service-info {
    flex: 1;
  }

  .selected-label {
    font-family: "Priamos", serif;
    font-size: 12px;
    color: rgb(200, 180, 160);
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .selected-service-info h3 {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 20px;
    font-weight: 300;
    margin: 5px 0 10px 0;
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .selected-details {
    display: flex;
    gap: 20px;
    flex-wrap: wrap;
  }

  .detail-item {
    display: flex;
    align-items: center;
    gap: 6px;
    font-family: "Priamos", serif;
    font-size: 14px;
    color: rgb(200, 180, 160);
  }

  .detail-item svg {
    width: 16px;
    height: 16px;
  }

  .clear-selection {
    width: 36px;
    height: 36px;
    background: rgba(249, 246, 239, 0.1);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.3s;
    flex-shrink: 0;
  }

  .clear-selection:hover {
    background: rgba(249, 246, 239, 0.2);
  }

  .clear-selection svg {
    width: 18px;
    height: 18px;
    color: rgb(200, 180, 160);
  }

  .selected-hint-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 15px;
    margin-top: 15px;
    padding-top: 15px;
    border-top: 1px solid rgba(249, 246, 239, 0.1);
    flex-wrap: wrap;
  }

  .selected-hint {
    font-family: "Priamos", serif;
    font-size: 13px;
    color: rgb(180, 160, 140);
    margin: 0;
  }

  .change-service-link {
    font-family: "Priamos", serif;
    font-size: 13px;
    color: rgb(200, 180, 160);
    text-decoration: underline;
    transition: color 0.3s;
  }

  .change-service-link:hover {
    color: rgb(249, 246, 239);
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
    to {
      transform: rotate(360deg);
    }
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
    line-height: 1.4;
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

    .selected-service {
      padding: 20px;
    }

    .selected-service-content {
      flex-wrap: wrap;
    }

    .selected-service-icon {
      width: 40px;
      height: 40px;
    }

    .selected-service-icon svg {
      width: 22px;
      height: 22px;
    }

    .selected-service-info h3 {
      font-size: 16px;
    }

    .selected-details {
      gap: 12px;
    }

    .detail-item {
      font-size: 13px;
    }

    .selected-hint-row {
      flex-direction: column;
      align-items: flex-start;
      gap: 10px;
    }

    .booking-info {
      flex-direction: column;
      gap: 25px;
      align-items: center;
    }

    .info-item {
      flex-direction: column;
      align-items: center;
      text-align: center;
      gap: 10px;
    }

    .info-item svg {
      margin-top: 0;
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

    .selected-service-info h3 {
      font-size: 14px;
    }

    .selected-hint {
      font-size: 12px;
    }

    .change-service-link {
      font-size: 12px;
    }
  }
</style>
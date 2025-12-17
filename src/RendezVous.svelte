<script>
  import { onMount } from "svelte";
  import { link, querystring, push } from "svelte-spa-router";
  import Header from "./lib/Header.svelte";
  import Footer from "./lib/Footer.svelte";

  let calLoaded = $state(false);
  let selectedService = $state(null);
  let calLink = $state("salon-ellesse/rendez-vous");

  // Map duration strings to Cal.com event type slugs
  function getCalLinkFromDuration(duration) {
    if (!duration) return "salon-ellesse/rendez-vous";
    
    const normalized = duration.toLowerCase().trim();
    
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
        calLink = getCalLinkFromDuration(duration ? decodeURIComponent(duration) : null);
      }
    }
  });

  // Clear selected service and go back to prestations
  function clearSelection() {
    push('/prestations');
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
</script>

<svelte:head>
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
    "description": "Réservez votre soin de beauté en ligne au cabinet Ellessé à Toulouse.",
    "isPartOf": {
      "@id": "https://ellesse-beaute.fr/#organization"
    },
    "breadcrumb": {
      "@type": "BreadcrumbList",
      "itemListElement": [
        {"@type": "ListItem", "position": 1, "name": "Accueil", "item": "https://ellesse-beaute.fr/"},
        {"@type": "ListItem", "position": 2, "name": "Prestations", "item": "https://ellesse-beaute.fr/prestations"},
        {"@type": "ListItem", "position": 3, "name": "Rendez-vous", "item": "https://ellesse-beaute.fr/rendez-vous"}
      ]
    },
    "potentialAction": {
      "@type": "ReserveAction",
      "target": {
        "@type": "EntryPoint",
        "urlTemplate": "https://ellesse-beaute.fr/rendez-vous",
        "actionPlatform": ["http://schema.org/DesktopWebPlatform", "http://schema.org/MobileWebPlatform"]
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
  <Header activePage="prestations" />

  <main>
    <section class="booking-section">
      <div class="booking-header">
        <p class="subtitle">Réservation en ligne</p>
        <h1 class="title">Prendre rendez-vous</h1>
        <p class="description">
          Choisissez votre soin et le créneau qui vous convient. Vous recevrez une confirmation par email.
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
                      <path d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                    </svg>
                    {selectedService.price}
                  </span>
                {/if}
              </div>
            </div>
            <button class="clear-selection" onclick={clearSelection} aria-label="Changer de prestation">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
          <div class="selected-hint-row">
            <p class="selected-hint">Sélectionnez un créneau disponible ci-dessous</p>
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
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
            <path d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
          </svg>
          <div>
            <h3>Confirmation</h3>
            <p>Par email immédiatement</p>
          </div>
        </div>
        <div class="info-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
            <path d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
          <div>
            <h3>Annulation</h3>
            <p>Gratuite 24h avant</p>
          </div>
        </div>
        <div class="info-item">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
            <path d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" />
            <path d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
          </svg>
          <div>
            <h3>Adresse</h3>
            <p>73 Bd Silvio Trentin, 31200 Toulouse</p>
          </div>
        </div>
      </div>
    </section>
  </main>

  <Footer variant="dark" />
</div>

<style>
  .page-wrapper {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    background-color: var(--color-cream, rgb(249, 246, 239));
  }

  main {
    padding: 50px 20px;
    flex: 1;
  }

  .booking-section {
    max-width: 900px;
    margin: 0 auto;
  }

  .booking-header {
    text-align: center;
    margin-bottom: 40px;
  }

  .subtitle {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 16px;
    color: var(--color-brown, rgb(102, 74, 50));
    margin: 0 0 10px 0;
    letter-spacing: 2px;
  }

  .title {
    font-family: var(--font-display, "LittleMicroSans", sans-serif);
    font-size: 48px;
    font-weight: 300;
    text-transform: uppercase;
    color: var(--color-dark, rgb(38, 25, 17));
    margin: 0 0 15px 0;
    letter-spacing: 3px;
  }

  .description {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 16px;
    color: var(--color-brown, rgb(102, 74, 50));
    margin: 0;
    max-width: 500px;
    margin: 0 auto;
    line-height: 1.6;
  }

  /* ===== SELECTED SERVICE ===== */
  .selected-service {
    background: var(--color-dark, rgb(38, 25, 17));
    border-radius: var(--radius-large, 16px);
    padding: 25px 30px;
    margin-bottom: 30px;
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
    width: 26px;
    height: 26px;
    color: #4ade80;
  }

  .selected-service-info {
    flex: 1;
  }

  .selected-label {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 13px;
    color: var(--color-tan, rgb(200, 180, 160));
    letter-spacing: 0.5px;
  }

  .selected-service-info h3 {
    font-family: var(--font-display, "LittleMicroSans", sans-serif);
    font-size: 18px;
    font-weight: 400;
    text-transform: uppercase;
    color: var(--color-cream, rgb(249, 246, 239));
    margin: 5px 0 10px 0;
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
    font-family: var(--font-body, "Priamos", serif);
    font-size: 14px;
    color: var(--color-tan, rgb(200, 180, 160));
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
    color: var(--color-tan, rgb(200, 180, 160));
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
    font-family: var(--font-body, "Priamos", serif);
    font-size: 13px;
    color: var(--color-light-tan, rgb(180, 160, 140));
    margin: 0;
  }

  .change-service-link {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 13px;
    color: var(--color-tan, rgb(200, 180, 160));
    text-decoration: underline;
    transition: color 0.3s;
  }

  .change-service-link:hover {
    color: var(--color-cream, rgb(249, 246, 239));
  }

  /* ===== CALENDAR ===== */
  .calendar-container {
    background: white;
    border-radius: var(--radius-small, 8px);
    box-shadow: var(--shadow-medium, 0 4px 20px rgba(38, 25, 17, 0.1));
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
    border-top-color: var(--color-brown, rgb(102, 74, 50));
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
    font-family: var(--font-body, "Priamos", serif);
    color: var(--color-brown, rgb(102, 74, 50));
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
    color: var(--color-brown, rgb(102, 74, 50));
    flex-shrink: 0;
    margin-top: 2px;
  }

  .info-item h3 {
    font-family: var(--font-display, "LittleMicroSans", sans-serif);
    font-size: 14px;
    font-weight: 400;
    text-transform: uppercase;
    color: var(--color-dark, rgb(38, 25, 17));
    margin: 0 0 5px 0;
    letter-spacing: 1px;
  }

  .info-item p {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 14px;
    color: var(--color-brown, rgb(102, 74, 50));
    margin: 0;
    line-height: 1.4;
  }

  /* ===== RESPONSIVE ===== */
  @media (max-width: 768px) {
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

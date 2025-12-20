<script>
  import { link } from "svelte-spa-router";
  import Header from "./lib/Header.svelte";
  import Footer from "./lib/Footer.svelte";

  let sliderEl;

  // Generate booking URL with service info
  function getBookingUrl(service) {
    const params = new URLSearchParams();
    const cleanName = service.name.replace(/<br>/g, " - ");
    params.set("service", cleanName);
    if (service.duration) params.set("duration", service.duration);
    if (service.price) params.set("price", service.price);
    return `/rendez-vous?${params.toString()}`;
  }

  const prestations = [
    {
      id: "ongles-mains",
      title: "Beauté des ongles mains",
      icon: "nail.png",
      image: "/Ellesse/ongles.webp",
      services: [
        { name: "Manucure russe seule", duration: "30min", price: "15€" },
        { name: "Manucure russe<br>+ Soins", duration: "1h", price: "35€" },
        { name: "Pose vernis semi-permanent", duration: "30min", price: "25€" },
        {
          name: "Vernis semi-permanent<br>+ Manucure russe",
          duration: "1h20",
          price: "40€",
        },
        {
          name: "Forfait pose et dépose<br>Semi-permanent",
          duration: "45min",
          price: "40€",
        },
        { name: "Dépose semi-permanent", duration: "15min", price: "15€" },
        {
          name: "Renforcement gel<br>+ Manucure russe",
          duration: "1h30",
          price: "50€",
        },
        {
          name: "Remplissage gel<br>+ Manucure russe",
          duration: "1h30",
          price: "45€",
        },
        {
          name: "Remplissage gel<br>D'un autre institut",
          duration: "1h30",
          price: "50€",
        },
        {
          name: "Rallongement extension<br>+ Manucure russe",
          duration: "2h",
          price: "65€",
        },
        { name: "Supplément french", duration: "30min", price: "10€" },
        { name: "Dépose gel ou résine", duration: "30min", price: "20€" },
      ],
    },
    {
      id: "pieds",
      title: "Beauté des pieds",
      icon: "nail.png",
      image: "/Ellesse/gallery/pieds.jpg",
      services: [
        {
          name: "Vernis semi-permanent<br>+ Cuticules",
          duration: "30min",
          price: "29€",
        },
        { name: "Vernis semi-permanent", duration: "20min", price: "20€" },
        {
          name: "Technique russe<br>+ Dépose & pose VSP",
          duration: "1h",
          price: "47€",
        },
        {
          name: "Technique russe<br>+ Pose VSP",
          duration: "45min",
          price: "38€",
        },
        { name: "Supplément french", duration: "15min", price: "5€" },
      ],
    },
    {
      id: "cils",
      title: "Extensions de cils",
      icon: "eyelashes.png",
      image: "/Ellesse/gallery/extension_cils.jpg",
      services: [
        {
          name: "Cil à cil naturel<br>Pose complète",
          duration: "1h",
          price: "60€",
        },
        {
          name: "Cil à cil naturel<br>Remplissage",
          duration: "45min",
          price: "40€",
        },
        {
          name: "Cil à cil volumineux<br>Pose complète",
          duration: "1h30",
          price: "70€",
        },
        {
          name: "Cil à cil volumineux<br>Remplissage",
          duration: "1h",
          price: "45€",
        },
        {
          name: "Extensions mixte<br>Pose complète",
          duration: "1h30",
          price: "90€",
        },
        {
          name: "Extensions mixte<br>Remplissage",
          duration: "1h15",
          price: "55€",
        },
        {
          name: "Volume russe<br>Pose complète",
          duration: "2h",
          price: "100€",
        },
        { name: "Volume russe<br>Remplissage", duration: "1h30", price: "80€" },
        { name: "Dépose extensions", duration: "20min", price: "10€" },
      ],
    },
    {
      id: "rehaussement",
      title: "Rehaussement de cils",
      icon: "eyelashes.png",
      image: "/Ellesse/cils.webp",
      services: [
        { name: "Rehaussement<br>+ Teinture", duration: "1h", price: "55€" },
        {
          name: "Rehaussement<br>Sans teinture",
          duration: "45min",
          price: "50€",
        },
        { name: "Teinture cils haut", duration: "15min", price: "10€" },
        {
          name: "Teinture cils<br>Haut + bas",
          duration: "15min",
          price: "15€",
        },
      ],
    },
    {
      id: "levres",
      title: "Semi-permanent lèvres",
      icon: "lips.png",
      image: "/Ellesse/levres.webp",
      services: [
        {
          name: "Candy lips<br>Remplissage complet",
          duration: "3h",
          price: "200€",
        },
        {
          name: "Contour des lèvres<br>Retouche incluse",
          duration: "1h",
          price: "150€",
        },
        { name: "Retouche fixatrice", duration: "3h", price: "50€" },
        {
          name: "Candy lips<br>Retouche annuelle",
          duration: "2h",
          price: "110€",
        },
        {
          name: "Contour des lèvres<br>Retouche annuelle",
          duration: "1h",
          price: "80€",
        },
      ],
    },
    {
      id: "sourcils",
      title: "Semi-permanent sourcils",
      icon: "eyebrows.png",
      image: "/Ellesse/gallery/microblading.jpg",
      services: [
        {
          name: "Microblading<br>Pose complète",
          duration: "2h",
          price: "190€",
        },
        {
          name: "Microblading<br>Retouche fixatrice",
          duration: "1h",
          price: "50€",
        },
        {
          name: "Microshading / Mixt<br>Pose complète",
          duration: "3h",
          price: "200€",
        },
        {
          name: "Microshading<br>Retouche fixatrice",
          duration: "2h",
          price: "50€",
        },
        { name: "Entretien 3-6 mois", duration: "45min", price: "70€" },
        { name: "Retouche annuelle", duration: "1h30", price: "100€" },
        { name: "Retouche nouveau client", duration: "1h", price: "140€" },
      ],
    },
    {
      id: "yeux",
      title: "Semi-permanent yeux",
      icon: "eyelashes.png",
      image: "/Ellesse/gallery/ras_de_cils.jpg",
      services: [
        {
          name: "Ras de cils simple<br>Retouche incluse",
          duration: "1h",
          price: "180€",
        },
        {
          name: "Ras de cils simple<br>Retouche annuelle",
          duration: "30min",
          price: "80€",
        },
        {
          name: "Ras de cils double<br>Retouche incluse",
          duration: "1h30",
          price: "210€",
        },
        {
          name: "Ras de cils double<br>Retouche annuelle",
          duration: "1h",
          price: "130€",
        },
        { name: "Eye liner", duration: "30min", price: "230€" },
      ],
    },
    {
      id: "entretien-sourcils",
      title: "Entretien sourcils",
      icon: "eyebrows.png",
      image: "/Ellesse/gallery/entretien_sourcils.jpg",
      services: [
        { name: "Épilation simple sourcils", duration: "15min", price: "10€" },
        { name: "Teinture sourcils", duration: "30min", price: "25€" },
        { name: "Restructuration sourcils", duration: "30min", price: "25€" },
        {
          name: "Restructuration<br>+ Teinture",
          duration: "45min",
          price: "45€",
        },
        { name: "Airbrush sourcils", duration: "45min", price: "40€" },
        { name: "Browlift", duration: "30min", price: "40€" },
        { name: "Browlift<br>+ Teinture", duration: "45min", price: "45€" },
        {
          name: "Browlift + teinture<br>+ Restructuration",
          duration: "45min",
          price: "60€",
        },
      ],
    },
    {
      id: "visage",
      title: "Soins visage",
      icon: "nail.png",
      image: "/Ellesse/gallery/soins_visage.jpg",
      services: [
        { name: "Soin visage hydratant", duration: "1h", price: "50€" },
        { name: "Gommage<br>Et dermaplaning", duration: "30min", price: "65€" },
        { name: "Microneedling", duration: "1h", price: "80€" },
        {
          name: "Microneedling<br>Forfait 4 séances",
          duration: "45min",
          price: "290€",
        },
        { name: "BB glow", duration: "30min", price: "80€" },
        {
          name: "BB glow<br>Forfait 4 séances",
          duration: "30min",
          price: "290€",
        },
        { name: "Microglow", duration: "1h", price: "110€" },
        { name: "Hydrafacial", duration: "1h", price: "90€" },
        { name: "Hydraneedling", duration: "1h20", price: "150€" },
        { name: "Hydraglow", duration: "1h20", price: "150€" },
      ],
    },
    {
      id: "radiofrequence",
      title: "Radiofréquence visage",
      icon: "nail.png",
      image: "/Ellesse/gallery/radiofrequence.jpg",
      services: [
        {
          name: "Radiofréquence visage<br>1 séance",
          duration: "30min",
          price: "60€",
        },
        {
          name: "Radiofréquence visage<br>Forfait 6 séances",
          duration: "30min",
          price: "250€",
        },
        {
          name: "Radiofréquence visage<br>Forfait 10 séances",
          duration: "30min",
          price: "450€",
        },
      ],
    },
    {
      id: "corps",
      title: "Soins corps",
      icon: "nail.png",
      image: "/Ellesse/gallery/soins_corps.jpg",
      services: [
        {
          name: "Lipocavitation<br>Ventre et taille",
          duration: "1h30",
          price: "120€",
        },
        { name: "Lipocavitation<br>Cuisses", duration: "1h", price: "99€" },
        {
          name: "Drainage lymphatique<br>Corps entier",
          duration: "1h30",
          price: "100€",
        },
        {
          name: "Drainage lymphatique<br>Cure 3 séances",
          duration: "1h30",
          price: "240€",
        },
      ],
    },
    {
      id: "bronzage",
      title: "Bronzage sans UV",
      icon: "nail.png",
      image: "/Ellesse/gallery/bronzage.jpg",
      services: [
        { name: "Visage et décolleté", duration: "10min", price: "15€" },
        { name: "Visage + buste<br>+ Bras", duration: "15min", price: "25€" },
        { name: "Jambes", duration: "15min", price: "25€" },
        {
          name: "Corps entier<br>Visage inclus",
          duration: "30min",
          price: "40€",
        },
      ],
    },
    {
      id: "autres",
      title: "Autres prestations",
      icon: "nail.png",
      image: "/Ellesse/gallery/dents.jpg",
      services: [
        { name: "Blanchiment dentaire", duration: "1h", price: "180€" },
        {
          name: "Grain de beauté<br>Retouche incluse",
          duration: "15min",
          price: "50€",
        },
        { name: "Taches de rousseur", duration: "1h", price: "120€" },
      ],
    },
  ];
</script>

<svelte:head>
  <title
    >Nos Prestations | Ellessé - Maquillage permanent, Cils, Ongles à Toulouse</title
  >
  <meta
    name="description"
    content="Découvrez nos prestations : microblading, candy lips, extension de cils volume russe, manucure russe, pose de gel. Cabinet d'esthétique à Toulouse."
  />
  <meta
    name="keywords"
    content="microblading Toulouse, candy lips, extension cils volume russe, manucure russe Toulouse, pose gel, maquillage semi permanent"
  />
  <link rel="canonical" href="https://ellesse-beaute.fr/prestations" />

  <!-- Open Graph -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://ellesse-beaute.fr/prestations" />
  <meta
    property="og:title"
    content="Nos Prestations | Ellessé - Maquillage permanent, Cils, Ongles à Toulouse"
  />
  <meta
    property="og:description"
    content="Découvrez nos prestations : microblading, candy lips, extension de cils, manucure russe."
  />
  <meta
    property="og:image"
    content="https://ellesse-beaute.fr/Ellesse/sourcils.webp"
  />

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
  <Header activePage="prestations" />

  <main>
    <section class="prestations-section">
      <div class="prestations-header">
        <p class="subtitle">Nos soins</p>
        <h1 class="title">Prestations</h1>
        <p class="description">
          Découvrez notre gamme complète de soins esthétiques inspirés des
          traditions russes
        </p>
      </div>

      <div class="scroll-hint">
        <span>Glissez pour découvrir</span>
        <svg
          width="18"
          height="18"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
        >
          <line x1="5" y1="12" x2="19" y2="12"></line>
          <polyline points="12 5 19 12 12 19"></polyline>
        </svg>
      </div>

      <div class="slider-container">
        <div class="slider" bind:this={sliderEl}>
          {#each prestations as prestation}
            <article class="prestation-card">
              <div class="card-image-wrapper">
                <img
                  src={prestation.image}
                  alt={prestation.title}
                  class="card-image"
                  loading="lazy"
                />
                <div class="card-gradient"></div>
              </div>

              <div class="card-body">
                <div class="card-header">
                  <img
                    src={prestation.icon}
                    alt={prestation.title}
                    class="card-icon"
                  />
                  <h2>{prestation.title}</h2>
                </div>

                <ul class="services-list">
                  {#each prestation.services as service}
                    <li>
                      <div class="service-info">
                        <span class="service-name">{@html service.name}</span>
                        {#if service.price}
                          <span class="service-details">
                            {#if service.duration}<span class="service-duration"
                                >{service.duration}</span
                              >{/if}
                            <span class="service-price">{service.price}</span>
                          </span>
                        {/if}
                      </div>
                      {#if service.price}
                        <a
                          href={getBookingUrl(service)}
                          use:link
                          class="service-book-btn"
                          aria-label="Réserver {service.name}"
                        >
                          <svg
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="2"
                          >
                            <path
                              d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
                            />
                          </svg>
                        </a>
                      {/if}
                    </li>
                  {/each}
                </ul>
              </div>
            </article>
          {/each}
        </div>
      </div>

      <div class="cta-section">
        <p>Une question sur nos prestations ?</p>
        <a href="/contact" use:link class="main-cta">Nous contacter</a>
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
    padding: 50px 0;
    flex: 1;
  }

  .prestations-section {
    max-width: 100%;
    overflow: hidden;
  }

  .prestations-header {
    text-align: center;
    margin-bottom: 30px;
    padding: 0 20px;
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
    max-width: 600px;
    margin: 0 auto;
    line-height: 1.6;
  }

  /* ===== SCROLL HINT ===== */
  .scroll-hint {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    padding: 0 30px 15px;
    max-width: 1400px;
    margin: 0 auto;
  }

  .scroll-hint span {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 13px;
    color: var(--color-brown, rgb(102, 74, 50));
    letter-spacing: 0.5px;
  }

  .scroll-hint svg {
    color: var(--color-brown, rgb(102, 74, 50));
    animation: bounceRight 1.5s ease-in-out infinite;
  }

  @keyframes bounceRight {
    0%,
    100% {
      transform: translateX(0);
    }
    50% {
      transform: translateX(5px);
    }
  }

  /* ===== SLIDER ===== */
  .slider-container {
    width: 100%;
    overflow: hidden;
  }

  .slider {
    display: flex;
    gap: 25px;
    padding: 15px 30px 30px 30px;
    overflow-x: auto;
    scroll-snap-type: x mandatory;
    scrollbar-width: none;
    -ms-overflow-style: none;
  }

  .slider::-webkit-scrollbar {
    display: none;
  }

  /* ===== CARD ===== */
  .prestation-card {
    flex: 0 0 auto;
    width: 340px;
    background: white;
    border-radius: var(--radius-large, 16px);
    box-shadow: var(--shadow-soft, 0 4px 20px rgba(38, 25, 17, 0.08));
    overflow: hidden;
    scroll-snap-align: start;
    transition:
      transform 0.3s ease,
      box-shadow 0.3s ease;
  }

  .prestation-card:hover {
    transform: translateY(-5px);
    box-shadow: var(--shadow-medium, 0 8px 30px rgba(38, 25, 17, 0.12));
  }

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

  .card-body {
    padding: 20px 22px 25px;
  }

  .card-header {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 18px;
    padding-bottom: 15px;
    border-bottom: 1px solid rgba(102, 74, 50, 0.15);
  }

  .card-icon {
    width: 28px;
    height: 28px;
    object-fit: contain;
    filter: brightness(0) saturate(100%);
  }

  .card-header h2 {
    font-family: var(--font-display, "LittleMicroSans", sans-serif);
    font-size: 16px;
    font-weight: 400;
    text-transform: uppercase;
    color: var(--color-dark, rgb(38, 25, 17));
    margin: 0;
    letter-spacing: 1px;
  }

  /* ===== SERVICES LIST ===== */
  .services-list {
    list-style: none;
    margin: 0 0 20px 0;
    padding: 0;
    max-height: 280px;
    overflow-y: auto;
    scrollbar-width: thin;
    scrollbar-color: rgba(102, 74, 50, 0.3) transparent;
  }

  .services-list::-webkit-scrollbar {
    width: 4px;
  }

  .services-list::-webkit-scrollbar-track {
    background: transparent;
  }

  .services-list::-webkit-scrollbar-thumb {
    background: rgba(102, 74, 50, 0.3);
    border-radius: 2px;
  }

  .services-list li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 10px;
    padding: 10px 0 10px 15px;
    font-family: var(--font-body, "Priamos", serif);
    font-size: 13px;
    color: var(--color-dark, rgb(38, 25, 17));
    border-left: 2px solid transparent;
    transition: all 0.2s ease;
  }

  .services-list li:hover {
    background: rgba(249, 246, 239, 0.8);
    border-left-color: var(--color-brown, rgb(102, 74, 50));
  }

  .service-info {
    display: flex;
    flex-direction: column;
    gap: 3px;
    flex: 1;
  }

  .service-name {
    line-height: 1.3;
  }

  .service-details {
    display: flex;
    gap: 10px;
    font-size: 12px;
  }

  .service-duration {
    color: var(--color-brown, rgb(102, 74, 50));
  }

  .service-price {
    color: var(--color-dark, rgb(38, 25, 17));
    font-weight: 500;
  }

  .service-book-btn {
    width: 32px;
    height: 32px;
    background: var(--color-dark, rgb(38, 25, 17));
    border: none;
    border-radius: var(--radius-small, 8px);
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
    flex-shrink: 0;
  }

  .service-book-btn:hover {
    background: var(--color-brown, rgb(102, 74, 50));
    transform: scale(1.05);
  }

  .service-book-btn svg {
    width: 16px;
    height: 16px;
    color: var(--color-cream, rgb(249, 246, 239));
  }

  /* ===== CTA SECTION ===== */
  .cta-section {
    text-align: center;
    margin: 50px 20px 0;
    padding: 50px 30px;
    background: var(--color-dark, rgb(38, 25, 17));
    border-radius: var(--radius-xl, 20px);
    max-width: 1000px;
    margin-left: auto;
    margin-right: auto;
  }

  .cta-section p {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 18px;
    color: var(--color-tan, rgb(200, 180, 160));
    margin: 0 0 25px 0;
    letter-spacing: 1px;
  }

  .main-cta {
    display: inline-block;
    font-family: var(--font-body, "Priamos", serif);
    font-size: 16px;
    color: var(--color-dark, rgb(38, 25, 17));
    background-color: var(--color-cream, rgb(249, 246, 239));
    text-decoration: none;
    padding: 16px 40px;
    border-radius: var(--radius-medium, 10px);
    letter-spacing: 1px;
    transition: all 0.3s ease;
  }

  .main-cta:hover {
    background-color: rgb(220, 210, 200);
  }

  /* ===== RESPONSIVE ===== */
  @media (max-width: 768px) {
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
      padding: 8px 0 8px 12px;
    }

    .service-details {
      font-size: 11px;
    }

    .service-duration {
      font-size: 10px;
    }

    .service-book-btn {
      width: 28px;
      height: 28px;
      border-radius: 6px;
    }

    .service-book-btn svg {
      width: 14px;
      height: 14px;
    }

    .cta-section {
      margin: 40px 15px 0;
      padding: 40px 20px;
      border-radius: var(--radius-large, 16px);
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
      padding: 7px 0 7px 10px;
    }

    .service-details {
      font-size: 10px;
    }

    .service-duration {
      font-size: 9px;
    }

    .service-book-btn {
      width: 26px;
      height: 26px;
    }

    .service-book-btn svg {
      width: 12px;
      height: 12px;
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

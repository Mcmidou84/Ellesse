<script>
  import { onMount } from "svelte";
  import { link } from "svelte-spa-router";
  import Header from "./lib/Header.svelte";
  import Footer from "./lib/Footer.svelte";

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
</script>

<svelte:head>
  <!-- SEO de base -->
  <title>Ellessé | Cabinet d'esthétique à la russe à Toulouse - Soins de beauté</title>
  <meta
    name="description"
    content="Ellessé, cabinet d'esthétique à Toulouse spécialisé dans l'art de la beauté à la russe. Découvrez nos soins inspirés des traditions impériales et des secrets de beauté sibériens."
  />
  <meta
    name="keywords"
    content="esthétique, beauté russe, soins visage, institut beauté Toulouse, manucure russe, soins naturels, beauté sibérienne, cabinet esthétique 31200"
  />
  <link rel="canonical" href="https://ellesse-beaute.fr/" />

  <!-- Open Graph -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://ellesse-beaute.fr/" />
  <meta property="og:title" content="Ellessé | Cabinet d'esthétique à la russe à Toulouse" />
  <meta
    property="og:description"
    content="L'art de la beauté à la russe. Soins inspirés des traditions impériales et des secrets de beauté sibériens."
  />
  <meta property="og:image" content="https://ellesse-beaute.fr/Ellesse/accueil_01.webp" />
  <meta property="og:image:width" content="1200" />
  <meta property="og:image:height" content="630" />
  <meta property="og:locale" content="fr_FR" />
  <meta property="og:site_name" content="Ellessé" />

  <!-- Twitter -->
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:title" content="Ellessé | Cabinet d'esthétique à la russe à Toulouse" />
  <meta
    name="twitter:description"
    content="L'art de la beauté à la russe. Soins inspirés des traditions impériales."
  />
  <meta name="twitter:image" content="https://ellesse-beaute.fr/Ellesse/accueil_01.webp" />

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
  <Header activePage="accueil" />

  <main id="main-content">
    <article class="hero">
      <p class="subtitle">Cabinet d'esthétique</p>
      <h1 class="title">
        <span>L'art de la beauté</span>
        <span>à la russe</span>
      </h1>
      <a
        href="/prestations"
        use:link
        class="cta-button"
        aria-label="Découvrir nos prestations et prendre rendez-vous"
      >
        Prendre rendez-vous
      </a>
    </article>
  </main>

  <Footer variant="light" showValues={true} />
</div>

<style>
  .page-wrapper {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
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
    font-family: var(--font-body, "Priamos", serif);
    font-size: 18px;
    color: var(--color-cream, rgb(249, 246, 239));
    margin: 0 0 15px 0;
    letter-spacing: 2px;
  }

  .title {
    font-family: var(--font-display, "LittleMicroSans", sans-serif);
    font-size: 90px;
    font-weight: 100;
    text-transform: uppercase;
    color: var(--color-cream, rgb(249, 246, 239));
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
    font-family: var(--font-body, "Priamos", serif);
    font-size: 16px;
    color: var(--color-dark, rgb(38, 25, 17));
    background-color: var(--color-cream, rgb(249, 246, 239));
    border: none;
    text-decoration: none;
    letter-spacing: 1px;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .cta-button:hover,
  .cta-button:focus {
    background-color: var(--color-dark, rgb(38, 25, 17));
    color: var(--color-cream, rgb(249, 246, 239));
    outline: 2px solid var(--color-cream, rgb(249, 246, 239));
    outline-offset: 2px;
  }

  /* ===== RESPONSIVE ===== */
  @media (max-width: 768px) {
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
  }
</style>

<script>
  import { onMount } from "svelte";
  import { link } from "svelte-spa-router";

  let menuOpen = $state(false);
  let activeCategory = $state("all");
  let lightboxOpen = $state(false);
  let lightboxImage = $state(null);
  let countersVisible = $state(false);
  let countersSection;

  // Compteurs animés
  let countClientes = $state(0);
  let countAnnees = $state(0);
  let countAvis = $state(0);

  // Slider avant/après
  let sliderPositions = $state({
    sourcils: 50,
    levres: 50,
    cils: 50,
    ongles: 50
  });
  let activeSlider = $state(null);

  // Catégories style Stories
  const categories = [
    { id: "all", name: "Tout", icon: "✨" },
    { id: "sourcils", name: "Sourcils", image: "/Ellesse/sourcils.webp" },
    { id: "levres", name: "Lèvres", image: "/Ellesse/levres.webp" },
    { id: "cils", name: "Cils", image: "/Ellesse/cils.webp" },
    { id: "ongles", name: "Ongles", image: "/Ellesse/ongles.webp" }
  ];

  // Images galerie avec témoignages
  const galleryItems = [
    {
      id: 1,
      category: "sourcils",
      image: "/Ellesse/gallery/sourcils_01.webp",
      title: "Microblading naturel",
      testimonial: "Résultat parfait, très naturel !",
      client: "Sophie M.",
      rating: 5
    },
    {
      id: 2,
      category: "sourcils",
      image: "/Ellesse/gallery/sourcils_02.webp",
      title: "Powder Brows",
      testimonial: "Je ne pouvais plus vivre sans mes sourcils parfaits.",
      client: "Marie L.",
      rating: 5
    },
    {
      id: 3,
      category: "levres",
      image: "/Ellesse/gallery/levres_01.webp",
      title: "Candy Lips",
      testimonial: "Des lèvres sublimes, effet naturel garanti.",
      client: "Camille R.",
      rating: 5
    },
    {
      id: 4,
      category: "levres",
      image: "/Ellesse/gallery/levres_02.webp",
      title: "Blush Lips",
      testimonial: "Le résultat dépasse mes attentes !",
      client: "Léa D.",
      rating: 5
    },
    {
      id: 5,
      category: "cils",
      image: "/Ellesse/gallery/cils_01.webp",
      title: "Volume Russe",
      testimonial: "Un regard de biche, je suis conquise.",
      client: "Emma P.",
      rating: 5
    },
    {
      id: 6,
      category: "cils",
      image: "/Ellesse/gallery/cils_02.webp",
      title: "Cil à cil naturel",
      testimonial: "Subtil et élégant, exactement ce que je voulais.",
      client: "Julie B.",
      rating: 5
    },
    {
      id: 7,
      category: "ongles",
      image: "/Ellesse/gallery/ongles_01.webp",
      title: "Manucure Russe",
      testimonial: "La meilleure manucure que j'ai jamais eue !",
      client: "Clara H.",
      rating: 5
    },
    {
      id: 8,
      category: "ongles",
      image: "/Ellesse/gallery/ongles_02.webp",
      title: "Nail Art",
      testimonial: "De vraies œuvres d'art sur mes ongles.",
      client: "Inès K.",
      rating: 5
    },
    {
      id: 9,
      category: "sourcils",
      image: "/Ellesse/gallery/sourcils_03.webp",
      title: "Microshading",
      testimonial: "Effet ombré magnifique et durable.",
      client: "Anaïs V.",
      rating: 5
    },
    {
      id: 10,
      category: "cils",
      image: "/Ellesse/gallery/cils_03.webp",
      title: "Lash Lift",
      testimonial: "Mes cils naturels n'ont jamais été aussi beaux.",
      client: "Manon F.",
      rating: 5
    },
    {
      id: 11,
      category: "ongles",
      image: "/Ellesse/gallery/ongles_03.webp",
      title: "Pose Gel",
      testimonial: "Tenue impeccable pendant 3 semaines.",
      client: "Chloé T.",
      rating: 5
    },
    {
      id: 12,
      category: "levres",
      image: "/Ellesse/gallery/levres_03.webp",
      title: "Contour lèvres",
      testimonial: "Finies les retouches rouge à lèvres !",
      client: "Sarah N.",
      rating: 5
    }
  ];

  // Avant/Après data
  const beforeAfter = [
    {
      id: "sourcils",
      title: "Microblading Sourcils",
      before: "/Ellesse/before_after/sourcils_before.webp",
      after: "/Ellesse/before_after/sourcils_after.webp"
    },
    {
      id: "levres",
      title: "Candy Lips",
      before: "/Ellesse/before_after/levres_before.webp",
      after: "/Ellesse/before_after/levres_after.webp"
    },
    {
      id: "cils",
      title: "Volume Russe",
      before: "/Ellesse/before_after/cils_before.webp",
      after: "/Ellesse/before_after/cils_after.webp"
    },
    {
      id: "ongles",
      title: "Manucure Russe",
      before: "/Ellesse/before_after/ongles_before.webp",
      after: "/Ellesse/before_after/ongles_after.webp"
    }
  ];

  // Témoignages complets
  const testimonials = [
    {
      id: 1,
      name: "Aurélie M.",
      prestation: "Microblading",
      quote: "Je n'aurais jamais imaginé avoir des sourcils aussi parfaits. Le travail est d'une précision incroyable, et l'ambiance du cabinet est tellement apaisante. Je recommande à 200% !",
      rating: 5,
      date: "Novembre 2024"
    },
    {
      id: 2,
      name: "Charlotte D.",
      prestation: "Volume Russe",
      quote: "Mes extensions de cils sont absolument magnifiques. Un regard intense mais naturel. L'équipe est aux petits soins et très professionnelle.",
      rating: 5,
      date: "Octobre 2024"
    },
    {
      id: 3,
      name: "Nadia B.",
      prestation: "Candy Lips",
      quote: "J'hésitais depuis longtemps... et je regrette de ne pas l'avoir fait plus tôt ! Mes lèvres sont sublimes, la couleur est exactement ce que je voulais.",
      rating: 5,
      date: "Décembre 2024"
    },
    {
      id: 4,
      name: "Marine L.",
      prestation: "Manucure Russe",
      quote: "La manucure russe, c'est vraiment autre chose. Mes cuticules n'ont jamais été aussi nettes et le vernis tient une éternité. Je suis cliente fidèle maintenant !",
      rating: 5,
      date: "Novembre 2024"
    },
    {
      id: 5,
      name: "Élodie P.",
      prestation: "Microshading",
      quote: "Après des années à dessiner mes sourcils chaque matin, je peux enfin me réveiller parfaite. Le résultat est bluffant de naturel.",
      rating: 5,
      date: "Septembre 2024"
    }
  ];

  let currentTestimonial = $state(0);
  let testimonialInterval;

  // Filtrer les images
  let filteredGallery = $derived(
    activeCategory === "all"
      ? galleryItems
      : galleryItems.filter(item => item.category === activeCategory)
  );

  onMount(() => {
    // Observer pour les compteurs
    const observer = new IntersectionObserver(
      (entries) => {
        if (entries[0].isIntersecting && !countersVisible) {
          countersVisible = true;
          animateCounters();
        }
      },
      { threshold: 0.5 }
    );

    if (countersSection) {
      observer.observe(countersSection);
    }

    // Auto-scroll témoignages
    testimonialInterval = setInterval(() => {
      currentTestimonial = (currentTestimonial + 1) % testimonials.length;
    }, 6000);

    return () => {
      observer.disconnect();
      clearInterval(testimonialInterval);
    };
  });

  function animateCounters() {
    const duration = 2000;
    const steps = 60;
    const stepTime = duration / steps;

    const targets = { clientes: 850, annees: 5, avis: 4.9 };
    let step = 0;

    const interval = setInterval(() => {
      step++;
      const progress = step / steps;
      const easeOut = 1 - Math.pow(1 - progress, 3);

      countClientes = Math.round(targets.clientes * easeOut);
      countAnnees = Math.round(targets.annees * easeOut);
      countAvis = Math.round(targets.avis * easeOut * 10) / 10;

      if (step >= steps) {
        clearInterval(interval);
        countClientes = targets.clientes;
        countAnnees = targets.annees;
        countAvis = targets.avis;
      }
    }, stepTime);
  }

  function toggleMenu() {
    menuOpen = !menuOpen;
  }

  function closeMenu() {
    menuOpen = false;
  }

  function openLightbox(item) {
    lightboxImage = item;
    lightboxOpen = true;
    document.body.style.overflow = "hidden";
  }

  function closeLightbox() {
    lightboxOpen = false;
    lightboxImage = null;
    document.body.style.overflow = "";
  }

  function handleSliderMove(e, sliderId) {
    if (activeSlider !== sliderId) return;
    
    const rect = e.currentTarget.getBoundingClientRect();
    const x = e.type.includes('touch') ? e.touches[0].clientX : e.clientX;
    const position = ((x - rect.left) / rect.width) * 100;
    sliderPositions[sliderId] = Math.max(0, Math.min(100, position));
  }

  function startSlider(sliderId) {
    activeSlider = sliderId;
  }

  function endSlider() {
    activeSlider = null;
  }

  function nextTestimonial() {
    currentTestimonial = (currentTestimonial + 1) % testimonials.length;
  }

  function prevTestimonial() {
    currentTestimonial = (currentTestimonial - 1 + testimonials.length) % testimonials.length;
  }

  function renderStars(rating) {
    return "★".repeat(rating) + "☆".repeat(5 - rating);
  }
</script>

<svelte:head>
  <title>Galerie | Ellessé - Nos réalisations et témoignages à Toulouse</title>
  <meta
    name="description"
    content="Découvrez nos réalisations en maquillage permanent, extension de cils et manucure russe. Photos avant/après et avis de nos clientes satisfaites à Toulouse."
  />
  <meta
    name="keywords"
    content="galerie esthétique Toulouse, photos microblading, avant après candy lips, avis extension cils, témoignages manucure russe"
  />
  <link rel="canonical" href="https://ellesse-beaute.fr/galerie" />

  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://ellesse-beaute.fr/galerie" />
  <meta property="og:title" content="Galerie | Ellessé - Nos réalisations" />
  <meta property="og:description" content="Découvrez nos réalisations et les témoignages de nos clientes." />
  <meta property="og:image" content="https://ellesse-beaute.fr/Ellesse/accueil_01.webp" />

  {@html `<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "ImageGallery",
    "@id": "https://ellesse-beaute.fr/galerie#webpage",
    "url": "https://ellesse-beaute.fr/galerie",
    "name": "Galerie - Ellessé Cabinet d'esthétique",
    "description": "Galerie photos des réalisations du cabinet Ellessé : microblading, candy lips, extension de cils, manucure russe.",
    "isPartOf": {
      "@id": "https://ellesse-beaute.fr/#organization"
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
      <a href="/galerie" use:link class="active" onclick={closeMenu}>Galerie</a>
      <a href="/prestations" use:link onclick={closeMenu}>Prestations</a>
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
    <!-- HERO SECTION -->
    <section class="hero-section">
      <div class="hero-background">
        <img src="/Ellesse/accueil_01.webp" alt="Ambiance Ellessé" />
        <div class="hero-overlay"></div>
      </div>
      <div class="hero-content">
        <p class="hero-subtitle">Nos réalisations</p>
        <h1 class="hero-title">L'excellence<br/>en images</h1>
        <p class="hero-description">
          Découvrez le savoir-faire Ellessé à travers les transformations de nos clientes
        </p>
      </div>
    </section>

    <!-- COMPTEURS -->
    <section class="counters-section" bind:this={countersSection}>
      <div class="counter">
        <span class="counter-number">{countClientes}+</span>
        <span class="counter-label">Clientes satisfaites</span>
      </div>
      <div class="counter-divider"></div>
      <div class="counter">
        <span class="counter-number">{countAnnees}</span>
        <span class="counter-label">Années d'expertise</span>
      </div>
      <div class="counter-divider"></div>
      <div class="counter">
        <span class="counter-number">{countAvis}<span class="star">★</span></span>
        <span class="counter-label">Note moyenne</span>
      </div>
    </section>

    <!-- CATEGORIES STORIES -->
    <section class="categories-section">
      <div class="categories-wrapper">
        {#each categories as cat}
          <button
            class="category-bubble"
            class:active={activeCategory === cat.id}
            onclick={() => activeCategory = cat.id}
          >
            <div class="bubble-ring">
              <div class="bubble-inner">
                {#if cat.image}
                  <img src={cat.image} alt={cat.name} />
                {:else}
                  <span class="bubble-icon">{cat.icon}</span>
                {/if}
              </div>
            </div>
            <span class="bubble-label">{cat.name}</span>
          </button>
        {/each}
      </div>
    </section>

    <!-- AVANT/APRÈS -->
    <section class="before-after-section">
      <div class="section-header">
        <p class="section-subtitle">Transformations</p>
        <h2 class="section-title">Avant / Après</h2>
        <p class="section-description">Faites glisser pour révéler la transformation</p>
      </div>

      <div class="before-after-grid">
        {#each beforeAfter as item}
          <div class="before-after-card">
            <h3>{item.title}</h3>
            <div 
              class="slider-compare"
              onmousemove={(e) => handleSliderMove(e, item.id)}
              ontouchmove={(e) => handleSliderMove(e, item.id)}
              onmousedown={() => startSlider(item.id)}
              ontouchstart={() => startSlider(item.id)}
              onmouseup={endSlider}
              ontouchend={endSlider}
              onmouseleave={endSlider}
              role="slider"
              aria-label="Comparateur avant après"
              tabindex="0"
            >
              <div class="compare-before">
                <img src={item.before} alt="Avant {item.title}" />
                <span class="compare-label">Avant</span>
              </div>
              <div class="compare-after" style="clip-path: inset(0 {100 - sliderPositions[item.id]}% 0 0)">
                <img src={item.after} alt="Après {item.title}" />
                <span class="compare-label">Après</span>
              </div>
              <div class="compare-handle" style="left: {sliderPositions[item.id]}%">
                <div class="handle-line"></div>
                <div class="handle-circle">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="15 18 9 12 15 6"></polyline>
                  </svg>
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="9 18 15 12 9 6"></polyline>
                  </svg>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </section>

    <!-- GALERIE MASONRY -->
    <section class="gallery-section">
      <div class="section-header">
        <p class="section-subtitle">Portfolio</p>
        <h2 class="section-title">Nos réalisations</h2>
      </div>

      <div class="masonry-grid">
        {#each filteredGallery as item, index (item.id)}
          <button
            class="masonry-item"
            class:tall={index % 5 === 0}
            onclick={() => openLightbox(item)}
          >
            <img src={item.image} alt={item.title} loading="lazy" />
            <div class="item-overlay">
              <span class="item-category">{item.category}</span>
              <h4 class="item-title">{item.title}</h4>
              <p class="item-testimonial">"{item.testimonial}"</p>
              <span class="item-client">— {item.client}</span>
            </div>
          </button>
        {/each}
      </div>
    </section>

    <!-- TÉMOIGNAGES -->
    <section class="testimonials-section">
      <div class="section-header light">
        <p class="section-subtitle">Avis clients</p>
        <h2 class="section-title">Elles racontent</h2>
      </div>

      <div class="testimonials-carousel">
        <button class="carousel-nav prev" onclick={prevTestimonial} aria-label="Témoignage précédent">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="15 18 9 12 15 6"></polyline>
          </svg>
        </button>

        <div class="testimonial-card">
          <div class="quote-mark">"</div>
          <p class="testimonial-quote">{testimonials[currentTestimonial].quote}</p>
          <div class="testimonial-rating">
            {renderStars(testimonials[currentTestimonial].rating)}
          </div>
          <div class="testimonial-author">
            <div class="author-avatar">
              {testimonials[currentTestimonial].name.charAt(0)}
            </div>
            <div class="author-info">
              <span class="author-name">{testimonials[currentTestimonial].name}</span>
              <span class="author-prestation">{testimonials[currentTestimonial].prestation}</span>
            </div>
          </div>
          <span class="testimonial-date">{testimonials[currentTestimonial].date}</span>
        </div>

        <button class="carousel-nav next" onclick={nextTestimonial} aria-label="Témoignage suivant">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="9 18 15 12 9 6"></polyline>
          </svg>
        </button>
      </div>

      <div class="carousel-dots">
        {#each testimonials as _, i}
          <button
            class="dot"
            class:active={currentTestimonial === i}
            onclick={() => currentTestimonial = i}
            aria-label="Aller au témoignage {i + 1}"
          ></button>
        {/each}
      </div>
    </section>

    <!-- INSTAGRAM FEED -->
    <section class="instagram-section">
      <div class="section-header">
        <p class="section-subtitle">Suivez-nous</p>
        <h2 class="section-title">@ellesse.institut</h2>
      </div>

      <div class="instagram-widget">
        <script src="https://cdn.lightwidget.com/widgets/lightwidget.js"></script>
        <iframe 
          src="//lightwidget.com/widgets/a07cac652e325c25ba995af84a3b0b7c.html" 
          scrolling="no" 
          allowtransparency="true" 
          class="lightwidget-widget"
          title="Feed Instagram Ellessé"
        ></iframe>
      </div>

      <a href="https://www.instagram.com/ellesse.institut" target="_blank" rel="noopener noreferrer" class="instagram-button">
        Voir plus sur Instagram
      </a>
    </section>

    <!-- CTA FINAL -->
    <section class="cta-section">
      <div class="cta-content">
        <h2>Prête pour votre transformation ?</h2>
        <p>Rejoignez nos 850+ clientes satisfaites</p>
        <a href="/rendez-vous" use:link class="cta-button">Prendre rendez-vous</a>
        <div class="cta-contact">
          <span>📍 73 Bd Silvio Trentin, Toulouse</span>
          <span>📞 06 38 93 66 48</span>
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

<!-- LIGHTBOX -->
{#if lightboxOpen && lightboxImage}
  <div class="lightbox" onclick={closeLightbox} role="dialog" aria-modal="true">
    <button class="lightbox-close" onclick={closeLightbox} aria-label="Fermer">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <line x1="18" y1="6" x2="6" y2="18"></line>
        <line x1="6" y1="6" x2="18" y2="18"></line>
      </svg>
    </button>
    <div class="lightbox-content" onclick={(e) => e.stopPropagation()}>
      <img src={lightboxImage.image} alt={lightboxImage.title} />
      <div class="lightbox-info">
        <h3>{lightboxImage.title}</h3>
        <p class="lightbox-testimonial">"{lightboxImage.testimonial}"</p>
        <div class="lightbox-client">
          <span class="lightbox-rating">{renderStars(lightboxImage.rating)}</span>
          <span>— {lightboxImage.client}</span>
        </div>
        <a href="/rendez-vous" use:link class="lightbox-cta">Réserver ce soin</a>
      </div>
    </div>
  </div>
{/if}

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
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
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
    margin-top: 90px;
  }

  /* ===== HERO SECTION ===== */
  .hero-section {
    position: relative;
    height: 70vh;
    min-height: 500px;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
  }

  .hero-background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  .hero-background img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .hero-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(
      to bottom,
      rgba(38, 25, 17, 0.4) 0%,
      rgba(38, 25, 17, 0.7) 100%
    );
  }

  .hero-content {
    position: relative;
    z-index: 1;
    text-align: center;
    color: rgb(249, 246, 239);
    padding: 0 20px;
  }

  .hero-subtitle {
    font-family: "Priamos", serif;
    font-size: 18px;
    letter-spacing: 3px;
    margin: 0 0 15px 0;
    opacity: 0.9;
  }

  .hero-title {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 72px;
    font-weight: 100;
    text-transform: uppercase;
    margin: 0 0 25px 0;
    line-height: 1.1;
    letter-spacing: 4px;
  }

  .hero-description {
    font-family: "Priamos", serif;
    font-size: 18px;
    margin: 0;
    opacity: 0.9;
    max-width: 500px;
    margin: 0 auto;
  }

  /* ===== COUNTERS ===== */
  .counters-section {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 60px;
    padding: 50px 20px;
    background: rgb(38, 25, 17);
  }

  .counter {
    text-align: center;
  }

  .counter-number {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 48px;
    color: rgb(249, 246, 239);
    display: block;
    letter-spacing: 2px;
  }

  .counter-number .star {
    color: rgb(255, 193, 7);
    font-size: 36px;
  }

  .counter-label {
    font-family: "Priamos", serif;
    font-size: 14px;
    color: rgb(200, 180, 160);
    letter-spacing: 1px;
  }

  .counter-divider {
    width: 1px;
    height: 60px;
    background: rgba(200, 180, 160, 0.3);
  }

  /* ===== CATEGORIES STORIES ===== */
  .categories-section {
    padding: 50px 20px;
    overflow-x: auto;
  }

  .categories-wrapper {
    display: flex;
    justify-content: center;
    gap: 30px;
  }

  .category-bubble {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 12px;
    background: none;
    border: none;
    cursor: pointer;
    transition: transform 0.3s ease;
  }

  .category-bubble:hover {
    transform: scale(1.05);
  }

  .bubble-ring {
    width: 85px;
    height: 85px;
    border-radius: 50%;
    padding: 3px;
    background: linear-gradient(135deg, rgb(200, 180, 160), rgb(102, 74, 50));
    opacity: 0.5;
    transition: opacity 0.3s ease;
  }

  .category-bubble.active .bubble-ring {
    opacity: 1;
    animation: pulse 2s infinite;
  }

  @keyframes pulse {
    0%, 100% { box-shadow: 0 0 0 0 rgba(102, 74, 50, 0.4); }
    50% { box-shadow: 0 0 0 10px rgba(102, 74, 50, 0); }
  }

  .bubble-inner {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: rgb(249, 246, 239);
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .bubble-inner img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .bubble-icon {
    font-size: 32px;
  }

  .bubble-label {
    font-family: "Priamos", serif;
    font-size: 14px;
    color: rgb(102, 74, 50);
    letter-spacing: 1px;
  }

  .category-bubble.active .bubble-label {
    color: rgb(38, 25, 17);
    font-weight: 600;
  }

  /* ===== SECTION HEADERS ===== */
  .section-header {
    text-align: center;
    margin-bottom: 40px;
    padding: 0 20px;
  }

  .section-header.light .section-subtitle,
  .section-header.light .section-title {
    color: rgb(249, 246, 239);
  }

  .section-header.light .section-subtitle {
    opacity: 0.8;
  }

  .section-subtitle {
    font-family: "Priamos", serif;
    font-size: 16px;
    color: rgb(102, 74, 50);
    margin: 0 0 10px 0;
    letter-spacing: 2px;
  }

  .section-title {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 42px;
    font-weight: 100;
    text-transform: uppercase;
    color: rgb(38, 25, 17);
    margin: 0;
    letter-spacing: 3px;
  }

  .section-description {
    font-family: "Priamos", serif;
    font-size: 16px;
    color: rgb(102, 74, 50);
    margin: 15px 0 0 0;
  }

  /* ===== BEFORE/AFTER ===== */
  .before-after-section {
    padding: 60px 20px;
    background: rgb(249, 246, 239);
  }

  .before-after-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 30px;
    max-width: 1000px;
    margin: 0 auto;
  }

  .before-after-card {
    background: white;
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0 4px 20px rgba(38, 25, 17, 0.08);
  }

  .before-after-card h3 {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 16px;
    text-transform: uppercase;
    color: rgb(38, 25, 17);
    text-align: center;
    padding: 20px;
    margin: 0;
    letter-spacing: 1px;
  }

  .slider-compare {
    position: relative;
    width: 100%;
    height: 300px;
    cursor: ew-resize;
    overflow: hidden;
    user-select: none;
  }

  .compare-before,
  .compare-after {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  .compare-before img,
  .compare-after img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .compare-label {
    position: absolute;
    bottom: 15px;
    padding: 6px 14px;
    background: rgba(38, 25, 17, 0.8);
    color: rgb(249, 246, 239);
    font-family: "Priamos", serif;
    font-size: 12px;
    letter-spacing: 1px;
    border-radius: 20px;
  }

  .compare-before .compare-label {
    left: 15px;
  }

  .compare-after .compare-label {
    right: 15px;
  }

  .compare-handle {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 4px;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .handle-line {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 3px;
    background: rgb(249, 246, 239);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  }

  .handle-circle {
    position: relative;
    width: 44px;
    height: 44px;
    background: rgb(249, 246, 239);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
  }

  .handle-circle svg {
    width: 16px;
    height: 16px;
    color: rgb(38, 25, 17);
  }

  /* ===== GALLERY MASONRY ===== */
  .gallery-section {
    padding: 60px 20px;
  }

  .masonry-grid {
    columns: 4;
    column-gap: 20px;
    max-width: 1400px;
    margin: 0 auto;
  }

  .masonry-item {
    break-inside: avoid;
    margin-bottom: 20px;
    position: relative;
    border-radius: 16px;
    overflow: hidden;
    cursor: pointer;
    border: none;
    padding: 0;
    background: none;
    width: 100%;
  }

  .masonry-item img {
    width: 100%;
    height: auto;
    display: block;
    transition: transform 0.5s ease;
  }

  .masonry-item:hover img {
    transform: scale(1.05);
  }

  .item-overlay {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(
      to top,
      rgba(38, 25, 17, 0.95) 0%,
      rgba(38, 25, 17, 0.7) 40%,
      rgba(38, 25, 17, 0) 100%
    );
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    padding: 25px;
    opacity: 0;
    transition: opacity 0.3s ease;
  }

  .masonry-item:hover .item-overlay {
    opacity: 1;
  }

  .item-category {
    font-family: "Priamos", serif;
    font-size: 12px;
    color: rgb(200, 180, 160);
    text-transform: uppercase;
    letter-spacing: 2px;
    margin-bottom: 8px;
  }

  .item-title {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 18px;
    color: rgb(249, 246, 239);
    margin: 0 0 12px 0;
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .item-testimonial {
    font-family: "Priamos", serif;
    font-size: 14px;
    color: rgb(200, 180, 160);
    font-style: italic;
    margin: 0 0 8px 0;
    line-height: 1.5;
  }

  .item-client {
    font-family: "Priamos", serif;
    font-size: 13px;
    color: rgb(249, 246, 239);
  }

  /* ===== TESTIMONIALS ===== */
  .testimonials-section {
    padding: 80px 20px;
    background: rgb(38, 25, 17);
  }

  .testimonials-carousel {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 30px;
    max-width: 900px;
    margin: 0 auto;
  }

  .carousel-nav {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background: rgba(249, 246, 239, 0.1);
    border: 1px solid rgba(249, 246, 239, 0.2);
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s ease;
    flex-shrink: 0;
  }

  .carousel-nav:hover {
    background: rgba(249, 246, 239, 0.2);
  }

  .carousel-nav svg {
    width: 24px;
    height: 24px;
    color: rgb(249, 246, 239);
  }

  .testimonial-card {
    background: rgba(249, 246, 239, 0.05);
    border-radius: 20px;
    padding: 50px 40px;
    text-align: center;
    position: relative;
    flex: 1;
    max-width: 600px;
  }

  .quote-mark {
    font-family: Georgia, serif;
    font-size: 120px;
    color: rgba(249, 246, 239, 0.1);
    position: absolute;
    top: -20px;
    left: 30px;
    line-height: 1;
  }

  .testimonial-quote {
    font-family: "Priamos", serif;
    font-size: 20px;
    color: rgb(249, 246, 239);
    line-height: 1.7;
    margin: 0 0 25px 0;
    position: relative;
    z-index: 1;
  }

  .testimonial-rating {
    color: rgb(255, 193, 7);
    font-size: 20px;
    letter-spacing: 4px;
    margin-bottom: 25px;
  }

  .testimonial-author {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 15px;
  }

  .author-avatar {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background: linear-gradient(135deg, rgb(200, 180, 160), rgb(102, 74, 50));
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: "LittleMicroSans", sans-serif;
    font-size: 20px;
    color: rgb(249, 246, 239);
  }

  .author-info {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  .author-name {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 16px;
    color: rgb(249, 246, 239);
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .author-prestation {
    font-family: "Priamos", serif;
    font-size: 14px;
    color: rgb(200, 180, 160);
  }

  .testimonial-date {
    font-family: "Priamos", serif;
    font-size: 13px;
    color: rgba(200, 180, 160, 0.6);
    margin-top: 20px;
    display: block;
  }

  .carousel-dots {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-top: 30px;
  }

  .dot {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background: rgba(249, 246, 239, 0.3);
    border: none;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .dot.active {
    background: rgb(249, 246, 239);
    transform: scale(1.2);
  }

  /* ===== INSTAGRAM ===== */
  .instagram-section {
    padding: 60px 20px;
  }

  .instagram-widget {
    max-width: 1200px;
    margin: 0 auto 30px;
  }

  .instagram-widget iframe {
    width: 100%;
    border: 0;
    overflow: hidden;
  }

  .instagram-button {
    display: block;
    width: fit-content;
    margin: 0 auto;
    padding: 14px 35px;
    font-family: "Priamos", serif;
    font-size: 15px;
    color: rgb(38, 25, 17);
    background: transparent;
    border: 2px solid rgb(38, 25, 17);
    border-radius: 30px;
    text-decoration: none;
    letter-spacing: 1px;
    transition: all 0.3s ease;
  }

  .instagram-button:hover {
    background: rgb(38, 25, 17);
    color: rgb(249, 246, 239);
  }

  /* ===== CTA SECTION ===== */
  .cta-section {
    background: linear-gradient(135deg, rgb(38, 25, 17) 0%, rgb(60, 45, 35) 100%);
    padding: 100px 20px;
    text-align: center;
  }

  .cta-content h2 {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 42px;
    font-weight: 100;
    text-transform: uppercase;
    color: rgb(249, 246, 239);
    margin: 0 0 15px 0;
    letter-spacing: 3px;
  }

  .cta-content p {
    font-family: "Priamos", serif;
    font-size: 18px;
    color: rgb(200, 180, 160);
    margin: 0 0 35px 0;
  }

  .cta-button {
    display: inline-block;
    padding: 18px 50px;
    font-family: "Priamos", serif;
    font-size: 16px;
    color: rgb(38, 25, 17);
    background: rgb(249, 246, 239);
    text-decoration: none;
    border-radius: 30px;
    letter-spacing: 1px;
    transition: all 0.3s ease;
  }

  .cta-button:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  }

  .cta-contact {
    margin-top: 40px;
    display: flex;
    justify-content: center;
    gap: 40px;
    flex-wrap: wrap;
  }

  .cta-contact span {
    font-family: "Priamos", serif;
    font-size: 15px;
    color: rgb(200, 180, 160);
  }

  /* ===== FOOTER ===== */
  footer {
    background-color: rgb(38, 25, 17);
    padding: 25px 30px;
    border-top: 1px solid rgba(200, 180, 160, 0.2);
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

  /* ===== LIGHTBOX ===== */
  .lightbox {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.95);
    z-index: 1000;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 40px;
  }

  .lightbox-close {
    position: absolute;
    top: 20px;
    right: 20px;
    width: 50px;
    height: 50px;
    background: rgba(255, 255, 255, 0.1);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.3s ease;
  }

  .lightbox-close:hover {
    background: rgba(255, 255, 255, 0.2);
  }

  .lightbox-close svg {
    width: 24px;
    height: 24px;
    color: rgb(249, 246, 239);
  }

  .lightbox-content {
    display: flex;
    gap: 40px;
    max-width: 1100px;
    max-height: 80vh;
    align-items: center;
  }

  .lightbox-content img {
    max-width: 60%;
    max-height: 80vh;
    object-fit: contain;
    border-radius: 16px;
  }

  .lightbox-info {
    color: rgb(249, 246, 239);
    max-width: 350px;
  }

  .lightbox-info h3 {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 28px;
    text-transform: uppercase;
    margin: 0 0 20px 0;
    letter-spacing: 2px;
  }

  .lightbox-testimonial {
    font-family: "Priamos", serif;
    font-size: 18px;
    font-style: italic;
    color: rgb(200, 180, 160);
    line-height: 1.6;
    margin: 0 0 20px 0;
  }

  .lightbox-client {
    display: flex;
    align-items: center;
    gap: 15px;
    margin-bottom: 30px;
  }

  .lightbox-rating {
    color: rgb(255, 193, 7);
    font-size: 18px;
  }

  .lightbox-cta {
    display: inline-block;
    padding: 14px 30px;
    font-family: "Priamos", serif;
    font-size: 15px;
    color: rgb(38, 25, 17);
    background: rgb(249, 246, 239);
    text-decoration: none;
    border-radius: 25px;
    letter-spacing: 1px;
    transition: all 0.3s ease;
  }

  .lightbox-cta:hover {
    transform: scale(1.05);
  }

  /* ===== RESPONSIVE ===== */
  @media (max-width: 1200px) {
    .masonry-grid {
      columns: 3;
    }
  }

  @media (max-width: 968px) {
    .before-after-grid {
      grid-template-columns: 1fr;
      max-width: 500px;
    }

    .masonry-grid {
      columns: 2;
    }

    .counters-section {
      gap: 30px;
    }

    .counter-number {
      font-size: 36px;
    }

    .lightbox-content {
      flex-direction: column;
      gap: 20px;
    }

    .lightbox-content img {
      max-width: 100%;
      max-height: 50vh;
    }
  }

  @media (max-width: 768px) {
    header {
      padding: 15px 20px;
    }

    main {
      margin-top: 70px;
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

    .hero-title {
      font-size: 42px;
    }

    .hero-section {
      height: 50vh;
      min-height: 400px;
    }

    .counters-section {
      flex-direction: column;
      gap: 25px;
    }

    .counter-divider {
      width: 60px;
      height: 1px;
    }

    .categories-wrapper {
      justify-content: flex-start;
      padding: 0 10px;
      overflow-x: auto;
    }

    .bubble-ring {
      width: 70px;
      height: 70px;
    }

    .section-title {
      font-size: 32px;
    }

    .slider-compare {
      height: 250px;
    }

    .masonry-grid {
      columns: 2;
      column-gap: 12px;
    }

    .masonry-item {
      margin-bottom: 12px;
    }

    .testimonials-carousel {
      flex-direction: column;
      gap: 20px;
    }

    .carousel-nav {
      display: none;
    }

    .testimonial-card {
      padding: 40px 25px;
    }

    .testimonial-quote {
      font-size: 17px;
    }

    .cta-content h2 {
      font-size: 28px;
    }
  }

  @media (max-width: 480px) {
    .hero-title {
      font-size: 32px;
    }

    .hero-subtitle,
    .hero-description {
      font-size: 15px;
    }

    .counter-number {
      font-size: 32px;
    }

    .bubble-ring {
      width: 60px;
      height: 60px;
    }

    .bubble-label {
      font-size: 12px;
    }

    .section-title {
      font-size: 26px;
    }

    .masonry-grid {
      columns: 1;
    }

    .cta-section {
      padding: 60px 20px;
    }

    .cta-content h2 {
      font-size: 24px;
    }

    .cta-contact {
      flex-direction: column;
      gap: 15px;
    }
  }
</style>
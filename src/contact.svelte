<script>
  import { link } from "svelte-spa-router";

  let menuOpen = $state(false);
  let formStatus = $state("");

  function toggleMenu() {
    menuOpen = !menuOpen;
  }

  function closeMenu() {
    menuOpen = false;
  }

  function handleSubmit(e) {
    e.preventDefault();
    // Ici tu peux ajouter la logique d'envoi du formulaire
    formStatus = "Message envoyé ! Nous vous répondrons rapidement.";
    e.target.reset();
    setTimeout(() => formStatus = "", 5000);
  }
</script>

<svelte:head>
  <title>Contact | Ellessé - Cabinet d'esthétique à Toulouse</title>
  <meta
    name="description"
    content="Contactez Ellessé, votre cabinet d'esthétique à Toulouse. Retrouvez-nous au 73 Bd Silvio Trentin, 31200 Toulouse. Suivez-nous sur Instagram et Facebook."
  />
  <meta
    name="keywords"
    content="contact esthétique Toulouse, Ellessé adresse, institut beauté 31200, rendez-vous esthétique Toulouse"
  />
  <link rel="canonical" href="https://ellesse-beaute.fr/contact" />

  <!-- Open Graph -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://ellesse-beaute.fr/contact" />
  <meta property="og:title" content="Contact | Ellessé - Cabinet d'esthétique à Toulouse" />
  <meta property="og:description" content="Contactez-nous pour prendre rendez-vous. 73 Bd Silvio Trentin, 31200 Toulouse." />
  <meta property="og:image" content="https://ellesse-beaute.fr/logo.png" />

  <!-- Schema.org ContactPage -->
  {@html `<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "ContactPage",
    "@id": "https://ellesse-beaute.fr/contact#webpage",
    "url": "https://ellesse-beaute.fr/contact",
    "name": "Contact - Ellessé Cabinet d'esthétique",
    "description": "Page de contact du cabinet d'esthétique Ellessé à Toulouse",
    "isPartOf": {
      "@id": "https://ellesse-beaute.fr/#organization"
    },
    "mainEntity": {
      "@type": "BeautySalon",
      "name": "Ellessé",
      "telephone": "+33638936648",
      "email": "ellesse.institut@gmail.com",
      "address": {
        "@type": "PostalAddress",
        "streetAddress": "73 Bd Silvio Trentin",
        "addressLocality": "Toulouse",
        "postalCode": "31200",
        "addressCountry": "FR"
      }
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
      <a href="/prestations" use:link onclick={closeMenu}>Prestations</a>
      <a href="/contact" use:link class="active" onclick={closeMenu}>Contact</a>
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
    <section class="contact-section">
      <div class="contact-header">
        <p class="subtitle">Restons en contact</p>
        <h1 class="title">Contact</h1>
        <p class="description">
          Retrouvez-nous sur les réseaux sociaux ou venez nous rendre visite
        </p>
      </div>

      <!-- Réseaux sociaux -->
      <div class="social-section">
        <h2 class="section-title">Suivez-nous</h2>
        <div class="social-links">
          <a href="https://www.instagram.com/ellesse.institut" target="_blank" rel="noopener noreferrer" class="social-card instagram">
            <img src="instagram.png" alt="Instagram" class="social-icon" />
            <div class="social-info">
              <span class="social-name">Instagram</span>
              <span class="social-handle">@ellesse.institut</span>
            </div>
          </a>
          <a href="https://www.facebook.com/ellesse.institut" target="_blank" rel="noopener noreferrer" class="social-card facebook">
            <img src="facebook.png" alt="Facebook" class="social-icon" />
            <div class="social-info">
              <span class="social-name">Facebook</span>
              <span class="social-handle">Ellessé Institut</span>
            </div>
          </a>
        </div>
      </div>

      <!-- Infos contact et Map -->
      <div class="contact-content">
        <div class="info-section">
          <div class="info-card">
            <img src="location.png" alt="Adresse" class="info-icon" />
            <div class="info-details">
              <h3>Adresse</h3>
              <p>73 Bd Silvio Trentin</p>
              <p>31200 Toulouse</p>
            </div>
          </div>

          <div class="info-card">
            <img src="phone.png" alt="Téléphone" class="info-icon" />
            <div class="info-details">
              <h3>Téléphone</h3>
              <a href="tel:+33638936648">06 38 93 66 48</a>
            </div>
          </div>

          <div class="info-card">
            <img src="email.png" alt="Email" class="info-icon" />
            <div class="info-details">
              <h3>Email</h3>
              <a href="mailto:ellesse.institut@gmail.com">ellesse.institut@gmail.com</a>
            </div>
          </div>

          <div class="info-card">
            <img src="clock.png" alt="Horaires" class="info-icon" />
            <div class="info-details">
              <h3>Horaires</h3>
              <p>Lundi - Samedi : 9h30 - 21h</p>
              <p>Dimanche : Fermé</p>
              <p>Dimanche : Fermé</p>
            </div>
          </div>
        </div>

        <div class="map-section">
          <iframe
            src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2888.057417787547!2d1.4290469130396506!3d43.62616387098294!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x12aebb4daa110649%3A0x1e11ef9a68c98b66!2s73%20Bd%20Silvio%20Trentin%2C%2031200%20Toulouse!5e0!3m2!1sfr!2sfr!4v1765613142531!5m2!1sfr!2sfr"
            width="100%"
            height="100%"
            style="border:0;"
            allowfullscreen=""
            loading="lazy"
            referrerpolicy="no-referrer-when-downgrade"
            title="Localisation Ellessé sur Google Maps"
          ></iframe>
        </div>
      </div>

      <!-- Formulaire de contact -->
      <div class="form-section">
        <h2 class="section-title">Envoyez-nous un message</h2>
        <form class="contact-form" onsubmit={handleSubmit}>
          <div class="form-row">
            <div class="form-group">
              <label for="name">Nom</label>
              <input type="text" id="name" name="name" required placeholder="Votre nom" />
            </div>
            <div class="form-group">
              <label for="email">Email</label>
              <input type="email" id="email" name="email" required placeholder="votre@email.com" />
            </div>
          </div>
          <div class="form-group">
            <label for="phone">Téléphone</label>
            <input type="tel" id="phone" name="phone" placeholder="06 00 00 00 00" />
          </div>
          <div class="form-group">
            <label for="message">Message</label>
            <textarea id="message" name="message" rows="5" required placeholder="Votre message..."></textarea>
          </div>
          <button type="submit" class="submit-btn">Envoyer</button>
          {#if formStatus}
            <p class="form-success">{formStatus}</p>
          {/if}
        </form>
      </div>

      <!-- CTA -->
      <div class="cta-section">
        <p>Prêt(e) pour votre soin ?</p>
        <a href="/prestations" use:link class="main-cta">Prendre rendez-vous</a>
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

  .contact-section {
    max-width: 1200px;
    margin: 0 auto;
  }

  .contact-header {
    text-align: center;
    margin-bottom: 50px;
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

  .section-title {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 24px;
    font-weight: 100;
    text-transform: uppercase;
    color: rgb(38, 25, 17);
    text-align: center;
    margin: 0 0 30px 0;
    letter-spacing: 2px;
  }

  /* ===== SOCIAL SECTION ===== */
  .social-section {
    margin-bottom: 60px;
  }

  .social-links {
    display: flex;
    justify-content: center;
    gap: 30px;
    flex-wrap: wrap;
  }

  .social-card {
    display: flex;
    align-items: center;
    gap: 20px;
    padding: 25px 40px;
    background: white;
    border-radius: 16px;
    text-decoration: none;
    box-shadow: 0 4px 20px rgba(38, 25, 17, 0.08);
    transition: all 0.3s ease;
    min-width: 280px;
  }

  .social-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 30px rgba(38, 25, 17, 0.15);
  }

  .social-icon {
    width: 50px;
    height: 50px;
    object-fit: contain;
  }

  .social-info {
    display: flex;
    flex-direction: column;
  }

  .social-name {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 18px;
    text-transform: uppercase;
    color: rgb(38, 25, 17);
    letter-spacing: 1px;
  }

  .social-handle {
    font-family: "Priamos", serif;
    font-size: 14px;
    color: rgb(102, 74, 50);
    margin-top: 4px;
  }

  /* ===== CONTACT CONTENT ===== */
  .contact-content {
    display: grid;
    grid-template-columns: 1fr 1.5fr;
    gap: 40px;
    margin-bottom: 60px;
  }

  .info-section {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .info-card {
    display: flex;
    align-items: flex-start;
    gap: 20px;
    padding: 25px;
    background: white;
    border-radius: 16px;
    box-shadow: 0 4px 20px rgba(38, 25, 17, 0.08);
  }

  .info-icon {
    width: 32px;
    height: 32px;
    object-fit: contain;
    flex-shrink: 0;
    margin-top: 5px;
  }

  .info-details h3 {
    font-family: "LittleMicroSans", sans-serif;
    font-size: 14px;
    text-transform: uppercase;
    color: rgb(38, 25, 17);
    margin: 0 0 10px 0;
    letter-spacing: 1px;
  }

  .info-details p {
    font-family: "Priamos", serif;
    font-size: 15px;
    color: rgb(102, 74, 50);
    margin: 0;
    line-height: 1.6;
  }

  .info-details a {
    font-family: "Priamos", serif;
    font-size: 15px;
    color: rgb(102, 74, 50);
    text-decoration: none;
    transition: color 0.3s;
  }

  .info-details a:hover {
    color: rgb(38, 25, 17);
  }

  .map-section {
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 4px 20px rgba(38, 25, 17, 0.08);
    min-height: 400px;
  }

  .map-section iframe {
    display: block;
  }

  /* ===== FORM SECTION ===== */
  .form-section {
    background: white;
    padding: 50px;
    border-radius: 20px;
    box-shadow: 0 4px 20px rgba(38, 25, 17, 0.08);
    margin-bottom: 60px;
  }

  .contact-form {
    max-width: 700px;
    margin: 0 auto;
  }

  .form-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
  }

  .form-group {
    margin-bottom: 20px;
  }

  .form-group label {
    display: block;
    font-family: "LittleMicroSans", sans-serif;
    font-size: 12px;
    text-transform: uppercase;
    color: rgb(38, 25, 17);
    margin-bottom: 8px;
    letter-spacing: 1px;
  }

  .form-group input,
  .form-group textarea {
    width: 100%;
    padding: 15px 20px;
    font-family: "Priamos", serif;
    font-size: 15px;
    color: rgb(38, 25, 17);
    background: rgb(249, 246, 239);
    border: 2px solid transparent;
    border-radius: 10px;
    transition: all 0.3s ease;
    box-sizing: border-box;
  }

  .form-group input:focus,
  .form-group textarea:focus {
    outline: none;
    border-color: rgb(102, 74, 50);
  }

  .form-group input::placeholder,
  .form-group textarea::placeholder {
    color: rgb(180, 160, 140);
  }

  .form-group textarea {
    resize: vertical;
    min-height: 120px;
  }

  .submit-btn {
    display: block;
    width: 100%;
    padding: 16px;
    font-family: "Priamos", serif;
    font-size: 16px;
    color: rgb(249, 246, 239);
    background: rgb(38, 25, 17);
    border: none;
    border-radius: 10px;
    cursor: pointer;
    letter-spacing: 1px;
    transition: all 0.3s ease;
  }

  .submit-btn:hover {
    background: rgb(60, 45, 35);
  }

  .form-success {
    font-family: "Priamos", serif;
    font-size: 15px;
    color: rgb(34, 139, 34);
    text-align: center;
    margin-top: 20px;
  }

  /* ===== CTA SECTION ===== */
  .cta-section {
    text-align: center;
    padding: 50px 30px;
    background: rgb(38, 25, 17);
    border-radius: 20px;
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
    transition: all 0.3s ease;
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
  @media (max-width: 968px) {
    .contact-content {
      grid-template-columns: 1fr;
    }

    .map-section {
      min-height: 350px;
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
      padding: 30px 15px;
    }

    .title {
      font-size: 32px;
    }

    .section-title {
      font-size: 20px;
    }

    .social-card {
      padding: 20px 25px;
      min-width: auto;
      flex: 1;
    }

    .social-icon {
      width: 40px;
      height: 40px;
    }

    .social-name {
      font-size: 16px;
    }

    .social-handle {
      font-size: 13px;
    }

    .form-section {
      padding: 30px 20px;
    }

    .form-row {
      grid-template-columns: 1fr;
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

    .social-links {
      flex-direction: column;
      gap: 15px;
    }

    .social-card {
      justify-content: center;
    }

    .info-card {
      padding: 20px;
    }

    .info-icon {
      width: 28px;
      height: 28px;
    }

    .map-section {
      min-height: 280px;
    }

    .cta-section {
      padding: 40px 20px;
    }
  }
</style>
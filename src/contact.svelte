<script>
  import { link } from "svelte-spa-router";
  import Header from "./lib/Header.svelte";
  import Footer from "./lib/Footer.svelte";

  let formStatus = $state("");
  let isSubmitting = $state(false);

  async function handleSubmit(e) {
    e.preventDefault();
    isSubmitting = true;
    
    const formData = new FormData(e.target);
    
    try {
      const response = await fetch("https://formspree.io/f/xwpgyaqj", {
        method: "POST",
        body: formData,
        headers: {
          Accept: "application/json"
        }
      });
      
      if (response.ok) {
        formStatus = "Message envoyé ! Nous vous répondrons rapidement.";
        e.target.reset();
      } else {
        formStatus = "Erreur lors de l'envoi. Veuillez réessayer.";
      }
    } catch (error) {
      formStatus = "Erreur lors de l'envoi. Veuillez réessayer.";
    }
    
    isSubmitting = false;
    setTimeout(() => formStatus = "", 5000);
  }
</script>

<svelte:head>
  <title>Contact | Ellessé - Cabinet d'esthétique à Toulouse</title>
  <meta
    name="description"
    content="Contactez Ellessé, votre cabinet d'esthétique à Toulouse. Retrouvez-nous au 73 Bd Silvio Trentin, 31200 Toulouse. Suivez-nous sur Instagram."
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
      "email": "ellesse.contact@gmail.com",
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
  <Header activePage="contact" />

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
              <a href="mailto:ellesse.contact@gmail.com">ellesse.contact@gmail.com</a>
            </div>
          </div>

          <div class="info-card">
            <img src="clock.png" alt="Horaires" class="info-icon" />
            <div class="info-details">
              <h3>Horaires</h3>
              <p>Lundi - Samedi : 9h30 - 21h</p>
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
          <button type="submit" class="submit-btn" disabled={isSubmitting}>
            {isSubmitting ? "Envoi en cours..." : "Envoyer"}
          </button>
          {#if formStatus}
            <p class="form-status" class:success={formStatus.includes("envoyé")} class:error={formStatus.includes("Erreur")}>
              {formStatus}
            </p>
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

  .contact-section {
    max-width: 1100px;
    margin: 0 auto;
  }

  .contact-header {
    text-align: center;
    margin-bottom: 50px;
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

  .section-title {
    font-family: var(--font-display, "LittleMicroSans", sans-serif);
    font-size: 24px;
    font-weight: 300;
    text-transform: uppercase;
    color: var(--color-dark, rgb(38, 25, 17));
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
    gap: 25px;
    flex-wrap: wrap;
  }

  .social-card {
    display: flex;
    align-items: center;
    gap: 18px;
    background: white;
    padding: 25px 35px;
    border-radius: var(--radius-large, 16px);
    text-decoration: none;
    box-shadow: var(--shadow-soft, 0 4px 20px rgba(38, 25, 17, 0.08));
    transition: all 0.3s ease;
    min-width: 260px;
  }

  .social-card:hover {
    transform: translateY(-5px);
    box-shadow: var(--shadow-medium, 0 8px 30px rgba(38, 25, 17, 0.12));
  }

  .social-icon {
    width: 50px;
    height: 50px;
    object-fit: contain;
  }

  .social-info {
    display: flex;
    flex-direction: column;
    gap: 4px;
  }

  .social-name {
    font-family: var(--font-display, "LittleMicroSans", sans-serif);
    font-size: 18px;
    text-transform: uppercase;
    color: var(--color-dark, rgb(38, 25, 17));
    letter-spacing: 1px;
  }

  .social-handle {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 14px;
    color: var(--color-brown, rgb(102, 74, 50));
  }

  /* ===== CONTACT CONTENT ===== */
  .contact-content {
    display: grid;
    grid-template-columns: 1fr 1.2fr;
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
    background: white;
    padding: 25px;
    border-radius: var(--radius-large, 16px);
    box-shadow: var(--shadow-soft, 0 4px 20px rgba(38, 25, 17, 0.08));
  }

  .info-icon {
    width: 32px;
    height: 32px;
    object-fit: contain;
    flex-shrink: 0;
  }

  .info-details h3 {
    font-family: var(--font-display, "LittleMicroSans", sans-serif);
    font-size: 14px;
    text-transform: uppercase;
    color: var(--color-dark, rgb(38, 25, 17));
    margin: 0 0 10px 0;
    letter-spacing: 1px;
  }

  .info-details p {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 15px;
    color: var(--color-brown, rgb(102, 74, 50));
    margin: 0;
    line-height: 1.6;
  }

  .info-details a {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 15px;
    color: var(--color-brown, rgb(102, 74, 50));
    text-decoration: none;
    transition: color 0.3s;
  }

  .info-details a:hover {
    color: var(--color-dark, rgb(38, 25, 17));
  }

  .map-section {
    border-radius: var(--radius-large, 16px);
    overflow: hidden;
    box-shadow: var(--shadow-soft, 0 4px 20px rgba(38, 25, 17, 0.08));
    min-height: 400px;
  }

  .map-section iframe {
    display: block;
  }

  /* ===== FORM SECTION ===== */
  .form-section {
    background: white;
    padding: 50px;
    border-radius: var(--radius-xl, 20px);
    box-shadow: var(--shadow-soft, 0 4px 20px rgba(38, 25, 17, 0.08));
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
    font-family: var(--font-display, "LittleMicroSans", sans-serif);
    font-size: 12px;
    text-transform: uppercase;
    color: var(--color-dark, rgb(38, 25, 17));
    margin-bottom: 8px;
    letter-spacing: 1px;
  }

  .form-group input,
  .form-group textarea {
    width: 100%;
    padding: 15px 20px;
    font-family: var(--font-body, "Priamos", serif);
    font-size: 15px;
    color: var(--color-dark, rgb(38, 25, 17));
    background: var(--color-cream, rgb(249, 246, 239));
    border: 2px solid transparent;
    border-radius: var(--radius-medium, 10px);
    transition: all 0.3s ease;
    box-sizing: border-box;
  }

  .form-group input:focus,
  .form-group textarea:focus {
    outline: none;
    border-color: var(--color-brown, rgb(102, 74, 50));
  }

  .form-group input::placeholder,
  .form-group textarea::placeholder {
    color: var(--color-light-tan, rgb(180, 160, 140));
  }

  .form-group textarea {
    resize: vertical;
    min-height: 120px;
  }

  .submit-btn {
    display: block;
    width: 100%;
    padding: 16px;
    font-family: var(--font-body, "Priamos", serif);
    font-size: 16px;
    color: var(--color-cream, rgb(249, 246, 239));
    background: var(--color-dark, rgb(38, 25, 17));
    border: none;
    border-radius: var(--radius-medium, 10px);
    cursor: pointer;
    letter-spacing: 1px;
    transition: all 0.3s ease;
  }

  .submit-btn:hover:not(:disabled) {
    background: rgb(60, 45, 35);
  }

  .submit-btn:disabled {
    opacity: 0.7;
    cursor: not-allowed;
  }

  .form-status {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 15px;
    text-align: center;
    margin-top: 20px;
  }

  .form-status.success {
    color: rgb(34, 139, 34);
  }

  .form-status.error {
    color: rgb(220, 53, 69);
  }

  /* ===== CTA SECTION ===== */
  .cta-section {
    text-align: center;
    padding: 50px 30px;
    background: var(--color-dark, rgb(38, 25, 17));
    border-radius: var(--radius-xl, 20px);
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
  @media (max-width: 968px) {
    .contact-content {
      grid-template-columns: 1fr;
    }

    .map-section {
      min-height: 350px;
    }
  }

  @media (max-width: 768px) {
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
  }
</style>

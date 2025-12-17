<script>
  // Props pour personnaliser le footer
  let {
    variant = "dark", // "dark" ou "light"
    showValues = false, // Afficher les valeurs (page d'accueil)
  } = $props();

  const values = [
    {
      icon: "elegant.png",
      title: "Élégance",
      text: "La grâce et le raffinement inspirés de l'esthétique impériale russe",
    },
    {
      icon: "tradition.png",
      title: "Tradition",
      text: "Un savoir-faire ancestral transmis de génération en génération",
    },
    {
      icon: "diamond.png",
      title: "Excellence",
      text: "La quête de la perfection dans chaque geste et chaque soin",
    },
    {
      icon: "pure.png",
      title: "Pureté",
      text: "Des soins naturels inspirés des secrets de beauté sibériens",
    },
  ];
</script>

<footer class:light={variant === "light"} class:dark={variant === "dark"}>
  {#if showValues}
    <section class="values-container" aria-label="Nos valeurs">
      {#each values as value}
        <article class="value-item">
          <img src={value.icon} alt={value.title} class="value-icon" />
          <h2 class="value-title">{value.title}</h2>
          <p class="value-text">{value.text}</p>
        </article>
      {/each}
    </section>
  {/if}

  <div class="footer-bottom">
    <p>© 2025 Ellessé — Cabinet d'esthétique</p>
  </div>
</footer>

<style>
  footer {
    padding: 25px 30px;
    margin-top: auto;
  }

  footer.dark {
    background-color: var(--color-dark, rgb(38, 25, 17));
  }

  footer.light {
    background-color: var(--color-cream, rgb(249, 246, 239));
    padding: 20px 30px 10px;
  }

  /* ===== VALUES SECTION ===== */
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
    width: 50px;
    height: 50px;
    color: var(--color-brown, rgb(102, 74, 50));
    margin-bottom: 10px;
    transition: color 0.3s ease;
  }

  .value-item:hover .value-icon {
    color: var(--color-dark, rgb(38, 25, 17));
  }

  .value-title {
    font-family: var(--font-display, "LittleMicroSans", sans-serif);
    text-transform: uppercase;
    font-size: 16px;
    font-weight: 300;
    color: var(--color-dark, rgb(38, 25, 17));
    margin: 0 0 10px 0;
    letter-spacing: 2px;
  }

  .value-text {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 14px;
    color: var(--color-brown, rgb(102, 74, 50));
    margin: 0;
    line-height: 1.6;
  }

  /* ===== FOOTER BOTTOM ===== */

  .footer-bottom {
    text-align: center;
  }

  footer.light .footer-bottom {
    margin-top: 20px;
    padding-top: 10px;
    border-top: 1px solid rgba(102, 74, 50, 0.3);
  }

  .footer-bottom p {
    font-family: var(--font-body, "Priamos", serif);
    font-size: 10px;
    margin: 0;
  }

  footer.dark .footer-bottom p {
    color: var(--color-tan, rgb(200, 180, 160));
  }

  footer.light .footer-bottom p {
    color: var(--color-brown, rgb(102, 74, 50));
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
    footer.light {
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

    footer.light .footer-bottom {
      margin-top: 20px;
      padding-top: 15px;
    }

    .footer-bottom p {
      font-size: 10px;
    }
  }

  @media (max-width: 480px) {
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

    footer.light {
      padding: 20px 10px 12px;
    }

    footer.light .footer-bottom {
      margin-top: 15px;
      padding-top: 10px;
    }
  }
</style>

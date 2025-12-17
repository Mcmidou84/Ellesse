<script>
  import { link } from "svelte-spa-router";

  // Props
  let { activePage = "", fixed = false } = $props();

  // State
  let menuOpen = $state(false);

  function toggleMenu() {
    menuOpen = !menuOpen;
  }

  function closeMenu() {
    menuOpen = false;
  }

  // Navigation items
  const navItems = [
    { path: "/", label: "Accueil", id: "accueil" },
    { path: "/galerie", label: "Galerie", id: "galerie" },
    { path: "/prestations", label: "Prestations", id: "prestations" },
    { path: "/contact", label: "Contact", id: "contact" }
  ];
</script>

<header class:fixed>
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
    {#each navItems as item}
      <a
        href={item.path}
        use:link
        class:active={activePage === item.id}
        onclick={closeMenu}
      >
        {item.label}
      </a>
    {/each}
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

<style>
  header {
    background-color: var(--color-dark, rgb(38, 25, 17));
    padding: 20px 30px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: relative;
    z-index: 100;
  }

  header.fixed {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
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
    font-family: var(--font-display, "LittleMicroSans", sans-serif);
    text-transform: uppercase;
    font-size: 28px;
    font-weight: 300;
    color: var(--color-cream, rgb(249, 246, 239));
  }

  nav {
    display: flex;
    gap: 50px;
  }

  nav a {
    font-family: var(--font-body, "Priamos", serif);
    text-decoration: none;
    color: var(--color-tan, rgb(200, 180, 160));
    font-weight: 300;
    font-size: 16px;
    letter-spacing: 1px;
    transition: color 0.3s;
  }

  nav a:hover,
  nav a:focus {
    color: var(--color-cream, rgb(249, 246, 239));
  }

  nav a.active {
    color: var(--color-cream, rgb(249, 246, 239));
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
    background-color: var(--color-cream, rgb(249, 246, 239));
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
      background-color: var(--color-dark, rgb(38, 25, 17));
      flex-direction: column;
      padding: 80px 30px 30px;
      gap: 0;
      box-shadow: var(--shadow-nav, -5px 0 20px rgba(0, 0, 0, 0.4));
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
  }
</style>

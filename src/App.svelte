<script>
</script>

<header>
  <div class="left">
    <img src="./logo.png" alt="Logo" class="logo">
    <span class="brand">Ellessé</span>
  </div>
  
  <nav>
    <a href="/">Accueil</a>
    <a href="/galerie">Galerie</a>
    <a href="/a-propos">À propos</a>
    <a href="/contact">Contact</a>
    <a href="/boutique">Boutique</a>
  </nav>

  <div class="spacer"></div>
</header>

<main>
  <section class="hero">
    <p class="subtitle">Institut de beauté</p>
    <h1 class="title">L'art de la beauté à la russe</h1>
  </section>
  
  <slot />
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Oswald:wght@400;700&family=Quattrocento:wght@400;700&display=swap');

  :global(body) {
    margin: 0;
    background-image: url('/fond.jpg');
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    background-attachment: fixed;
    min-height: 100vh;
  }

  header {
    background-color: rgb(249, 246, 239);
    padding: 20px 30px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .left {
    display: flex;
    align-items: center;
    gap: 15px;
    flex: 1;
  }

  .spacer {
    flex: 1;
  }

  .logo {
    height: 50px;
  }

  .brand {
    font-family: 'Oswald', sans-serif;
    font-size: 28px;
    font-weight: 700;
    color: rgb(38, 25, 17);
  }

  nav {
    display: flex;
    gap: 30px;
  }

  nav a {
    font-family: 'Quattrocento', serif;
    text-decoration: none;
    color: rgb(102, 74, 50);
    font-weight: 700;
    font-size: 16px;
  }

  nav a:hover {
    color: rgb(38, 25, 17);
  }

  main {
    padding: 20px;
  }

  .hero {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 70vh;
    text-align: center;
  }

  .subtitle {
    font-family: 'Quattrocento', serif;
    font-size: 18px;
    text-transform: uppercase;
    color: rgb(249, 246, 239);
    margin: 0 0 15px 0;
    letter-spacing: 2px;
  }

  .title {
    font-family: 'Oswald', sans-serif;
    font-size: 90px;
    font-weight: 100;
    text-transform: uppercase;
    color: rgb(249, 246, 239);
    margin: 0;
    max-width: 1200px;
    line-height: 1.2;
    text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
  }
</style>
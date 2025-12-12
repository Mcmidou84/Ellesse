<script>
  import Router, { link } from 'svelte-spa-router';
  import Accueil from './Accueil.svelte';
  import RendezVous from './RendezVous.svelte';

  // Définition des routes
  const routes = {
    '/': Accueil,
    '/rendez-vous': RendezVous,
    // Ajoute tes autres pages ici :
    // '/galerie': Galerie,
    // '/a-propos': APropos,
    // '/contact': Contact,
    // '/boutique': Boutique,
  };
</script>

<Router {routes} />

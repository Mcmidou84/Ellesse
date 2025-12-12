<script>
  import Router from 'svelte-spa-router';
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

<svelte:head>
  <!-- Meta globales -->
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="author" content="Ellessé" />
  <meta name="robots" content="index, follow" />
  <meta name="theme-color" content="#261911" />
  
  <!-- Favicon -->
  <link rel="icon" type="image/png" href="/logo.png" />
  <link rel="apple-touch-icon" href="/logo.png" />
  
  <!-- Preconnect pour performances -->
  <link rel="preconnect" href="https://app.cal.eu" />
  
  <!-- Schema.org global - Organisation -->
  {@html `<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "BeautySalon",
    "@id": "https://ellesse-beaute.fr/#organization",
    "name": "Ellessé",
    "description": "Cabinet d'esthétique spécialisé dans l'art de la beauté à la russe. Soins inspirés des traditions impériales et des secrets de beauté sibériens.",
    "url": "https://ellesse-beaute.fr",
    "logo": "https://ellesse-beaute.fr/logo.png",
    "image": "https://ellesse-beaute.fr/Ellesse/accueil_01.webp",
    "telephone": "+33638936648",
    "email": "contact@ellesse-beaute.fr",
    "address": {
      "@type": "PostalAddress",
      "streetAddress": "73 Bd Silvio Trentin",
      "addressLocality": "Toulouse",
      "postalCode": "31200",
      "addressCountry": "FR"
    },
    "geo": {
      "@type": "GeoCoordinates",
      "latitude": "43.6180",
      "longitude": "1.4280"
    },
    "openingHoursSpecification": [
      {
        "@type": "OpeningHoursSpecification",
        "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
        "opens": "09:00",
        "closes": "19:00"
      }
    ],
    "priceRange": "€€",
    "currenciesAccepted": "EUR",
    "paymentAccepted": "Cash, Credit Card",
    "areaServed": {
      "@type": "City",
      "name": "Toulouse"
    },
    "sameAs": [
      "https://www.instagram.com/ellesse_beaute",
      "https://www.facebook.com/ellessebeaute"
    ]
  }
  </script>`}
</svelte:head>

<Router {routes} />
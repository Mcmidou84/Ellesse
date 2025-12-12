<script>
  import Router from 'svelte-spa-router';
  import Accueil from './Accueil.svelte';
  import RendezVous from './RendezVous.svelte';
  import Prestations from './Prestations.svelte';

  // Définition des routes
  const routes = {
    '/': Accueil,
    '/rendez-vous': RendezVous,
    '/prestations': Prestations,
    // Ajoute tes autres pages ici :
    // '/galerie': Galerie,
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
    "description": "Cabinet d'esthétique à Toulouse spécialisé dans l'art de la beauté à la russe. Maquillage semi permanent sourcils, microblading, candy lips, extension de cils volume russe, manucure russe, pose de gel.",
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
    "hasOfferCatalog": {
      "@type": "OfferCatalog",
      "name": "Prestations esthétiques",
      "itemListElement": [
        {
          "@type": "OfferCatalog",
          "name": "Maquillage semi permanent sourcils",
          "itemListElement": [
            {"@type": "Offer", "itemOffered": {"@type": "Service", "name": "Microblading Toulouse"}},
            {"@type": "Offer", "itemOffered": {"@type": "Service", "name": "Microshading Toulouse"}},
            {"@type": "Offer", "itemOffered": {"@type": "Service", "name": "Brow shading Toulouse"}},
            {"@type": "Offer", "itemOffered": {"@type": "Service", "name": "Powder brows Toulouse"}}
          ]
        },
        {
          "@type": "OfferCatalog",
          "name": "Maquillage semi permanent lèvres",
          "itemListElement": [
            {"@type": "Offer", "itemOffered": {"@type": "Service", "name": "Candy Lips Toulouse"}},
            {"@type": "Offer", "itemOffered": {"@type": "Service", "name": "Blush Lips Toulouse"}}
          ]
        },
        {
          "@type": "OfferCatalog",
          "name": "Extension de cils",
          "itemListElement": [
            {"@type": "Offer", "itemOffered": {"@type": "Service", "name": "Volume russe Toulouse"}},
            {"@type": "Offer", "itemOffered": {"@type": "Service", "name": "Pose cil à cil Toulouse"}},
            {"@type": "Offer", "itemOffered": {"@type": "Service", "name": "Lash botox Toulouse"}}
          ]
        },
        {
          "@type": "OfferCatalog",
          "name": "Soins ongles",
          "itemListElement": [
            {"@type": "Offer", "itemOffered": {"@type": "Service", "name": "Manucure russe Toulouse"}},
            {"@type": "Offer", "itemOffered": {"@type": "Service", "name": "Pose de gel Toulouse"}},
            {"@type": "Offer", "itemOffered": {"@type": "Service", "name": "Nail art Toulouse"}}
          ]
        }
      ]
    },
    "sameAs": [
      "https://www.instagram.com/ellesse_beaute",
      "https://www.facebook.com/ellessebeaute"
    ]
  }
  </script>`}
</svelte:head>

<Router {routes} />

---
title: "Bonus: Flask til App"
aliases:
  - Flask til App
lang: nb-NO
authors:
  - Sondre Grønås
tags:
  - Python
  - Flask
  - App
created: 2023-10-04 20:26:56
updated: 2023-10-04 20:27:02
---
# Bonus: Flask til App
Dette er en kort innføring i hvordan du kan gjøre ditt nettsted om til en installerbar applikasjon. Dette er ikke eksklusivt for Flask, og kan brukes til å forvandle alle web applikasjoner til installerbare apper.

Denne siden går ikke inn på hvordan [service workers](https://web.dev/learn/pwa/service-workers/) fungerer i sin helhet. Funksjonalitet som varslinger og 'bakgrunnskommunikasjon' dekkes ikke.

## Hva trenger du?
Som mye i kode-verden, så spørs det egentlig hva du har behov for. Nedenfor er en liste over enkle ting du kommer til å trenge, men det er fullt mulig du vil trenge mer.

- Et ikon i ulike formater: `512x512`, `192x192` + favicon `.ico` fil (`32x32` er standard), det er dog mulig å bruke vanlig `png`.
- En `manifest.json` fil - navn er forsåvidt valgfritt
- En `service_worker.js` fil - navn er forsåvidt valgfritt.

## Ikon
Et ikon er viktig når man lager en app - det er tross alt det brukeren forbinder med applikasjonen. Det finnes en rekke standarder som er utviklet for at flest mulig skal kunne bruke appen, med tanke på bakoverkompatabilitet for eldre enheter, eller enheter med andre typer skjermer. Apple App store krever for eksempel ikon i `1024x1024` oppløsning.

> [!NOTE]+ Ett ikon - flere format
> Når vi tilpasser oss flere plattformer må man ofte opprette flere varianter av et ikon (ja - dette kan gjøres via kode). I dette eksempelet forholder vi oss til litt færre alternativer som bør være nok for de fleste moderne enheter.

Ikonene bør være tilgjengelig i `static` mappen, med for eksempel følgende navngivning:

- `/static/favicon.ico` (`32x32` størrelse, kan være `.png`)
- `/static/icons/icon-512x512.png`
- `/static/icons/icon-192x192.png`
- Eventuelt ekstra ikoner i flere størrelser.

> [!TECH]+ Android App Sizes Guidelines
> ![[Android App Icon Sizes.png]] 
> (Kilde: https://logosbynick.com/sizes-guidelines-for-designing-app-icons-ios-android/)

## manifest.json fil
Manifest filen er metadata som er knyttet til appen i [[JSON]] format, og må også ligge tilgjengelig i `static` mappen. Filen bør se noenlunde slik ut:

```json title="/static/manifest.json"
{  
  "short_name": "App Navn",  
  "name": "App Navn",
  "icons": [  
    {  
      "src": "/static/favicon.ico",  
      "sizes": "64x64 32x32 24x24 16x16",  
      "type": "image/x-icon"  
    },  
    {  
      "src": "/static/icons/icon-192x192.png",  
      "type": "image/png",  
      "sizes": "192x192"  
    },  
    {  
      "src": "/static/icons/icon-512x512.png",  
      "type": "image/png",  
      "sizes": "512x512"  
    }  
  ],  
  "scope": "/",  
  "start_url": "/",  
  "display": "standalone",  
  "theme_color": "#1e2129",  
  "background_color": "#1e2129"  
}
```

## service_worker.js fil
Under er et enkelt eksempel av en `service_worker.js` fil - denne kan utvides til å gjøre mye forskjellig - i dette eksempelet brukes den kun til installasjon.

```js title="/static/service_worker.js"
const cacheName = "App Navn";  
const assets = ["/"];  // Eventuelle ekstra sider som "kan fungere offline", ikke nødvendig
self.addEventListener("install", (installEvent) => {  
  installEvent.waitUntil(  
    caches.open(cacheName).then((cache) => {  
      cache.addAll(assets);  
    })  
  );  
});
```

## Sett det i hop!
Resten gjøres i enkel HTML. Vi må legge til `manifest.json` i `<head>` seksjonen, samt en liten JavaScript snippet på siden for å aktivere `service_worker.js`:

```html
<head>
  <!-- Din normale head fil -->
  <link rel="manifest" href="/static/manifest.json">
  <script>
    if ('serviceWorker' in navigator) {
      window.addEventListener('load', function () {
        navigator.serviceWorker.register('/static/service_worker.js');
      });
    }
  </script>
</head>
```

## Nyt resultatet 😎
Dett var dett! Neste gang noen besøker siden din vil de få valget om å installere nettsiden som en applikasjon (Progressive Web App / PWA) - på desktop, iPhone, Android og flere plattformer. Kult, ikke sant?

> [!TECH]- Installasjonsknapp Windows
> ![[Installer PWA Desktop.png]]

> [!TECH]- Installasjonsknapp Android
>> [!NOTE]+
>> Obs: Det kan hende nettleseren spør bruker om å installere appen automatisk når brukeren besøker nettsiden, slik at de ikke må grave frem i menyen. Eventuelt kan en "Installer app" knapp opprettes.
> ![[Installer PWA Android.png]]

> [!TIP]+ Egen "Installer app" knapp
 > Det er fullt mulig å legge til en knapp på siden med for eksempel teksten `Installer app`, for å gjøre installasjonen litt enklere for endebruker.
 > 
 > Se: https://web.dev/customize-install/#in-app-flow

> [!CODE]+ Se også - Bubblewrap / PWABuilder - App store publisering
> - Link Google: https://developers.google.com/codelabs/pwa-in-play#0
> - Link PWABuilder: https://www.pwabuilder.com/
> 
> Bubblewrap lar deg konvertere PWA (Progressiv Web App) til en fullverdig Android Applikasjon som kan publiseres til Google Play store.
> 
> PWABuilder fikser dette eventuelt for deg, og lar deg publisere til flere app-stores, inkluder Microsoft, Apple, Google & Meta.

## Referanser
- https://developer.android.com/distribute/google-play/resources/icon-design-specifications
- https://support.google.com/chrome/answer/9658361
- https://web.dev/learn/pwa/

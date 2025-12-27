# Linkek

- [Vscode](https://code.visualstudio.com/)
- [Bootstrap](https://getbootstrap.com/) | [Bootstrap icons](https://icons.getbootstrap.com/) | [Favicon generátor](https://favicon.io/)
- [Vite](https://vite.dev/) | [VueJs](https://vuejs.org/) | [VueJs W3 schools](https://www.w3schools.com/vue/index.php) | [Vue router](https://router.vuejs.org/) | [Pinia](https://pinia.vuejs.org/)
- [VueJs examples 1](https://vuejs.org/examples/#cells) | [VueJs examples 2](https://vuejsexamples.com/) | [Avesome VueJs](https://github.com/vuejs/awesome-vue?tab=readme-ov-file#official-resources)
- [Vue dev tools](https://github.com/vuejs/devtools) | [Vue dev tools Crome plugin](https://chromewebstore.google.com/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)
- [Cypress web test](https://www.cypress.io/#create) | [Cypress component test](https://docs.cypress.io/app/component-testing/get-started) | [Vue test utils](https://test-utils.vuejs.org/)

---

# Környezet telepítése

branch: 01_adatkotes_data

- vite vuejs keretrendeszr fejlesztőeszköz:

```console
npm create vue@latest
```

- bootstrap:

```console
npm i bootstrap@5.3.3
```

- icons:

```console
npm i bootstrap-icons
```

- node_moduls:

```console
npm install
```

- `main.js`-ben (import bootstrap, icons):

```js
//main.js
//Bootstrap: css, js
import "bootstrap/dist/css/bootstrap.min.css";
import "bootstrap";
//Icons: css
import "bootstrap-icons/font/bootstrap-icons.min.css";
```

# Indítás, scripts

- fejlesztőeszköz indítása

```console
npm run dev
```

- js-js fordítás a dist mappába (Produktum)

```console
npm run build
```

- Produktum kipróbálása

```console
npm run preview
```

# Fontos függőségek

- vue: VueJs keretrendszer fejlesztőeszköz
- vue-router: route kezelés
- pinia: Állapot kezelés
- bootstrap
- bootstrap-icons

# Webalkamazás felépítés

## Alakalmazás indító main.js

main.js

```js
import { createApp } from "vue";
import { createPinia } from "pinia";

import App from "./App.vue";
import router from "./router";
//Bootstrap: css, js
import "bootstrap/dist/css/bootstrap.min.css";
import "bootstrap";
//Icons: css
import "bootstrap-icons/font/bootstrap-icons.min.css";

const app = createApp(App);

app.use(createPinia());
app.use(router);

app.mount("#app");
```

## Belépési pont: App.vue

```vue
<template>
  <!-- Head -->
  <h1>Vue alkalmazás</h1>

  <!-- Menü -->
  <ul>
    <li><RouterLink to="/">Home</RouterLink></li>
    <li><RouterLink to="/about">About</RouterLink></li>
  </ul>

  <!-- Ide töltődnek be az oldalak -->
  <RouterView />
</template>
```

## Weboldalak: views

- Az alkalmazásunk weboldalai a views mappában találhatók
  - AboutView.vue, HomeView.vue
  - Ezek töltődnek be az App.vue <RouterView/> dobozába
  - Helyük a src/views mappa

például: AboutView.vue

```vue
<template>
  <div>
    <h1>About</h1>
  </div>
</template>
```

## Router: route/index.js

- Itt rendeljük össze a rout-okat az oldalakkal
- Megoljuk a title kiírásokat
- Megoldjuk a 404-es oldalt

router/index.js

```js
import { createRouter, createWebHistory } from "vue-router";
import HomeView from "@/views/HomeView.vue";

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: "/",
      name: "home",
      component: HomeView,
      meta: {
        title: (route) => "Home",
      },
    },
    {
      path: "/about",
      name: "about",
      component: () => import("@/views/AboutView.vue"),
      meta: {
        title: (route) => "About",
      },
    },
    {
      path: "/:pathMatch(.*)*",
      name: "NotFound",
      component: () => import("@/views/404.vue"),
      meta: {
        title: (route) => "404",
      },
    },
  ],
});

router.beforeEach((to, from, next) => {
  //Az oldal címkéjébe töltsd be az adott route objektum meta kulcsán található függvény által visszaadott értéket
  document.title = "Valami - " + to.meta.title(to);
  //mehetsz tovább az oldalra
  next();
});

export default router;
```

# Vujs Publikálás

## Elmélet

A Vue.js egy Single Page Application (SPA) alkalmazás:

- Környezet az apach szerveren:
  - A www/proba mappába kerül az index.html
  - Tehát aweboldal a .../proba mint gyökérből indul
- Amikor a felhasználó először betölti az alkalmazást, **a szerver mindig csak egyetlen HTML fájlt küld el** (ez az index.html).
- **Az összes "oldalváltást"** az alkalmazáson belül (pl. /rolunk, /termekek) **a Vue Router kezeli** a böngészőben, anélkül, hogy a szerverhez kellene fordulnia.
- Az URL címsávban az útvonal változik, de **a böngésző nem tölt be új oldalt**.
- Mindezt az apache szerverrel a .htaccess fájlban adjuk meg
  - Amikor a böngésző kéri a .../proba/rolunk oldalt, akkor
    1. Ilyen nincs a szerveren, az apache 404-es hibát ad vissza
    2. A `.htaccess` konfiguráció biztosítja,
       - hogy minden nem létező útvonal visszairányításra kerüljön az index.html-re,
       - ami elindítja a Vue.js alkalmazást
       - Ezután a Vue Router elolvassa az URL-t, és betölti a megfelelő proba/rolunk komponenst.

## .htaccess konfiguráció

public/.htaccess

```htaccess
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /proba
RewriteRule ^/proba/index\.html$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /proba/index.html [L]
</IfModule>
```

Kommentelve:

```htaccess
# Feltétel: Csak akkor fut le a blokk, ha az Apache-ban engedélyezve van a mod_rewrite modul.
<IfModule mod_rewrite.c>

# Beállítás: Beindítja az útvonal-átírási (URL rewriting) motort.
RewriteEngine On

# Alapútvonal:
# Beállítja az útvonalak alapkönyvtárát /proba-ra.
# Ez azt jelenti, hogy a további útvonalak a /proba/ gyökérhez képest értelmeződnek
RewriteBase /proba

# Szabály 1:
# Kivétel: Ha a kérés az index.html-re érkezik, akkor ne csináljon semmit (-),
# hanem álljon le ([L]).
# Megakadályozza, hogy az index.html folyamatosan saját magára irányítsa át a kéréseket.
RewriteRule ^/proba/index\.html$ - [L]

# Feltétel1:
# Csak akkor menjen tovább a következő szabályra, ha a kért útvonal NEM egy létező fájl (!-f).
# (Pl. Ne irányítsa át a style.css vagy a logo.png fájlokat.)
RewriteCond %{REQUEST_FILENAME} !-f

# Feltétel 2:
# Csak akkor menjen tovább, ha a kért útvonal NEM egy létező könyvtár (!-d).
# (Pl. Ne irányítsa át a képek/ mappát.)
RewriteCond %{REQUEST_FILENAME} !-d

# Szabály 2: Az Összefoglaló Szabály
# Ha a kérés nem létező fájl ÉS nem létező könyvtár, akkor a rendszer az összes kérést (.)
# A /proba/index.html fájlra irányítja át.
# Az [L] (Last) leállítja a további szabályok feldolgozását.
RewriteRule . /proba/index.html [L]

</IfModule>
```

## Az apach szerver beállításai

http.conf: Engedélyezve legyen a mod_rewrite modul

```conf
LoadModule rewrite_module modules/mod_rewrite.so
```

## Fordítás: vite.config.js

vite.config.js

```js
import { fileURLToPath, URL } from "node:url";
import { defineConfig, loadEnv } from "vite"; // loadEnv importálása
import vue from "@vitejs/plugin-vue";

// A konfigurációt már nem objektumként, hanem függvényként exportáljuk.
export default defineConfig(({ mode }) => {
  // 1. Környezeti Változók Betöltése
  // A Vite API-t használjuk a mode és az aktuális mappa alapján.
  // Csak a 'VITE_' kezdetű változókat töltjük be.
  const env = loadEnv(mode, process.cwd(), "VITE_");

  // 2. Visszatérés a konfigurációs objektummal
  return {
    plugins: [vue()],
    resolve: {
      alias: {
        "@": fileURLToPath(new URL("./src", import.meta.url)),
      },
    },

    //A fordítási mappa megadása
    build: {
      outDir: env.VITE_BUILD_DIR,
      //Előtte takarítsa-e a mappát
      emptyOutDir: false,
    },

    //Az alkalmazás mappaneve attól függően, hogy milyen módban vagyunk
    base: mode === "development" ? "/" : env.VITE_WEB_DIR,
  };
});
```

## Az alkalmazás üzemmódjai

Az alkalmazásnak 3 módja létezik:

- **development**: Amikor fejlesztünk, a fejlesztőpi rendszer fut
- **production**: A lefordított alkalmazás fut
- **test**: Amikor a tesztek futnak

Ezeket a NODE_ENV környezeti változóba állítja ve a vite attól függően hogy:

- dev (npm run dev)
- view (npm run viewer)
- test (npm run test)
  üzemmódban indítottuk az alkalmazást

NODE_ENV értékének kiolvasása

```js
//Régi szintaxis
if (process.env.NODE_ENV !== "production") {
  console.log("Ez a napló csak fejlesztői módban látható.");
}

//modernebb szintaxis
const mode = import.meta.env.MODE; //'development' vagy 'production'
const isDev = import.meta.env.DEV; //true ha 'development'
const isProd = import.meta.env.PROD; //true ha 'production'
const isSsr = import.meta.env.SSR; //true ha a rendszer SSR-el működik
```

### SSR, CSR

A webfejelesztésben három féle megközelítést használhatunk

1. A Server-Side Rendering (SSR)

- Az weboldalakat a server generálja, és html-t küld vissza
  - Hátrány: jóval nagyobb a server terhelése
  - Előny: Az első betöltődés gyors
  - Előny: keresőbarátabb

2. Client-Side Rendering (CSR)

- A bönésző megkapja a komplett webalkalmazást a megnyitáskor
- A webalkalmazás innentől kezdve a szervertől ajax kérésekkel csak azadatokat kéri a dinamikus tartalmakhoz
  - Előny: kevés a szerver terhelése
  - Hátrány: hosszabb az első betöltődés
  - Hátrány: A kereső motorok nehezebben derítik fel

3.  Hibrid Megoldások (SSG)

- Az SSR és CSR keverékét használja úgy hogy mindkettő előnyét próbálja optimálisan kihasználni.

A VueJs alapban a CSR technológiát preferálja

### .env fájlok és rendszerváltozók

- Célszerű saját rendszerváltozókat használni, amiket a **.env fájlokban** hozhatunk létre
- .env fájlok:
  - `.env`: Minden módban betöltődik: itt adhatjuk meg az alkalmazásra vonatkozó általános dolgokat
  - `.env.development`t: dev módban töltődik be
  - `.env.production`: éles környezet ben töltődik be.
  - `.env.github`: éles környezetben: github fordításnál töltődik be.
- A .env fájlok rendszerváltozóinak névadási szabálya:
  - A VITE\_ előtag kötelező, csak ezek olvashatók be a kódból
  - A VITE\_ előtaggal ellátott változók nyilvánosan elérhetők a böngészőben futó front-end kódban.
  - A VITE\_ előtag célja a nyilvánosság
  - Ne használj VITE\_ előtag nélküli változókat

### Példa .env fájlokra

.env: Az alkalmazás címe, verziója stb.

```env
VITE_APP_TITLE = Iskola webalkalmazás
VITE_APP_VER = 1.0.0
VITE_BUILD_DIR = C:/wamp64/www/proba
VITE_WEB_DIR = /proba/
```

.env.github: Az alkalmazás címe, verziója stb.

```env
VITE_BUILD_DIR = ../vuealapismeretekwww
VITE_WEB_DIR = /vuealapismeretekwww/
VITE_API_URL = http://xyz.com:8000/api
```

.env.development: Az API címe fejlesztői módban

```env
VITE_API_URL = http://localhost:3000/api
```

.env.production: Az API címe produkciós környezetban

```env
VITE_API_URL = http://akarmi.com:3000/api
```

### npm script: buildgithub

```json
"scripts": {
    "dev": "vite",
    "build": "vite build",
    "buildgithub": "vite build --mode github",
    "preview": "vite preview"
  },
```

### Környezeti változók elérése

Nem kell beírni, hogy melyik .env fájlból akarjuk beolvasni

```js
//.env-ből olvassa be
const appTitle = import.meta.env.VITE_APP_TITLE;
const appVer = import.meta.env.VITE_APP_VER;
//.env.development, vagy .env.production-ból olvassa be
const apiUrl = import.meta.env.VITE_API_URL;
```

# A projekt mappái

- `dist`: Alapértelmezetten ide fordítjuk le a projektet
- `src`: A projekt fejlesztői gyökér könyvtára
- `public`: Statikusfájlok
  - például képek, amikre közvetlenül tudunk hivatkozni
  - .htaccess konfigurációs fájl
  - favicon.icon
  - Fordításkor ennek a tartalma egy az egyben bemásolódik a dist mappa gyökerébe
- src/`assets`: Szintén statikus fájlok
  - .css fájlok
  - képek
  - Fordításkor beépíti a js fájlokba úgy hogy pl. a képek nevét egyedi azonosítóval egészíti ki.
- src/`components`
  - Ide kerülnek a komponensek: újrahasznosítható oldalak, amik weboldalakba ágyzódhatnak be.
- src/`route`
  - index.js: A route-view hozzárendelés valamint egyéb routelási mechanizmusok
- src/`stores`
  - állapotkezelés (pinia)
- src/`views`
  - Ide kerülnek az alkalmazás weboldalai


## public vs assets

├── public/
│ └── public-logo.png <-- A build nem dolgozza fel
├── src/
│ ├── assets/
│ │ └── assets-logo.png <-- A build befordítja a js fájlba és hash-eli
│ └── components/
│ └── LogoDisplay.vue
└── ...

- Képek használata

```vue
<template>
  <h2>1. Kép az `assets` mappából (Ajánlott)</h2>
  <img :src="assetsLogoUrl" />

  <h2>2. Kép a `public` mappából (Közvetlen elérési út)</h2>
  <img src="/public-logo.png"/>
</template>

<script>
import assetsLogoUrl from "@/assets/assets-logo.png";
//...
</script>
```

# Direktívák


# Programozási stílusok
- Composition API
```js
<script setup>

</script>
```

- Options API
```js
<script>
export default {
  //..
}
</script>
```


## Az Options API logikai sorrendje
A javasolt sorrend:

1. name (kereséshez, debugoláshoz fontos)
2. components
3. props
4. emits
5. data
6. computed
7. watch
8. lifecycle hooks (pl. created, mounted)
9. methods

# Projektstruktúra
src/
├── assets/             # Képek, globális CSS (pl. tailwind, scss)
├── components/         # Kisebb, újrahasznosítható elemek (Gomb, Input, Kártya)
│   ├── UserAvatar.vue
│   └── BaseButton.vue
├── stores/             # Pinia store-ok (globális állapot)
│   └── userStore.js
├── utils/              # Sima JS függvények (formázás, matek)
│   └── dateHelper.js
├── views/              # "Oldalak" (amik több komponenst fognak össze)
│   ├── HomeView.vue
│   └── LoginView.vue
├── App.vue             # A fő keret (itt van a RouterView és a Navigáció)
└── main.js             # Itt inicializálod a Vue-t és a Piniát

1. Szétválasztás: A components mappában vannak a "buta" elemek, amik csak megjelenítenek. A views mappában vannak az "okos" oldalak, amik a Store-ral beszélgetnek.

2. Kereshetőség: Ha egy adatot módosítani kell, tudod, hogy a stores-ban keresd. Ha egy gomb színét, akkor a components-ben.

1. Mi kerüljön a Store-ba? (CRUD és üzleti logika)
- Minden, ami adattal kapcsolatos, és nem csak a grafikus megjelenítésre tartozik:
- API hívások: fetch, axios lekérések (Create, Read, Update, Delete).
- Adatok tárolása: A lista, amit az API-tól kaptál.
- Adat-manipuláció: Például egy elem törlése a listából vagy egy új hozzáadása.
- Globális állapot: Ki van-e jelentkezve a felhasználó, mi van a kosárban, stb.

2. Mi maradjon a View-ban? (UI logika)
- Csak az, ami szorosan a felhasználói felülethez (User Interface) kötődik:
- Űrlapok állapota: Amíg a felhasználó gépel egy inputba, az az adat nyugodtan maradhat a komponens data()-jában. Csak a Mentés gomb megnyomásakor küldjük el a Store-nak.
- Modális ablakok/Lenyílók: Nyitva van-e egy ablak, vagy sem.
- Validáció: Mielőtt elküldenéd az adatot a Store-nak, ellenőrizheted a komponensben, hogy ki van-e töltve minden mező.

Dobozos felépítés:
Doboz 1: UI logika (View)
Doboz 2: Adat logika (Store)
Doboz 3: Megjelenítés (Components)

## .vue nevezéktan
src/
├── components/ <-- Mapába strukturáljuk
│   ├── Base/
│   │   ├── BaseInput.vue  <-- Az oldalon egyszer használt buta elemek
│   │   └── BaseToast.vue
│   ├── Table/
│   │   ├── GenericTable.vue        <-- A "Szuper-táblázatunk"
│   │   └── GenericTableHeader.vue
│   ├── Todo/
│   │   ├── TodoList.vue            <-- Szülő
│   │   ├── TodoListItem.vue        <-- Gyerek (Szülő nevével kezdődik)
│   │   └── TodoListItemButton.vue  <-- Gyerek gyereke
│   └── Layout/
│       ├── TheNavbar.vue           <-- Egypéldányos komponensek
│       └── TheFooter.vue
└── views/
    ├── ProductView.vue <-- Weboldalak View utótag kötelező
    ├── SportView.vue
    └── HomeView.vue

  # Computed
  ## set-get computed
  ```js
computed: {
    //Író olvasó computed
    valamiBizgeto: {
      get() {
        return this.valami;
      },
      set(value) {
        this.valami=value;
      }
    }
  ```

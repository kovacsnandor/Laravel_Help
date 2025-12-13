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
import "bootstrap/dist/css/bootstrap.min.css"
import "bootstrap"
//Icons: css
import "bootstrap-icons/font/bootstrap-icons.min.css"
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
import { createApp } from 'vue'
import { createPinia } from 'pinia'

import App from './App.vue'
import router from './router'
//Bootstrap: css, js
import "bootstrap/dist/css/bootstrap.min.css"
import "bootstrap"
//Icons: css
import "bootstrap-icons/font/bootstrap-icons.min.css"

const app = createApp(App)

app.use(createPinia())
app.use(router)

app.mount('#app')
```

## Belépési pont: App.vue
```vue
<template>
  <!-- Head -->
  <h1>Vue alkalmazás</h1>

  <!-- Menü -->
  <ul>
    <li> <RouterLink to="/">Home</RouterLink> </li>
    <li> <RouterLink to="/about">About</RouterLink> </li>
  </ul>

  <!-- Ide töltődnek be az oldalak -->
   <RouterView/>
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
import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '@/views/HomeView.vue'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'home',
      component: HomeView,
      meta:{
        title: (route) => 'Home'
      }
    },
    {
      path: '/about',
      name: 'about',
      component: () => import('@/views/AboutView.vue'),
      meta:{
        title: (route) => 'About'
      }
    },
    { path: "/:pathMatch(.*)*", 
      name: "NotFound", 
      component: () => import('@/views/404.vue'),
      meta:{
        title: (route) => '404'
      }
    },
  ],
})

router.beforeEach((to,from, next) => {
  //Az oldal címkéjébe töltsd be az adott route objektum meta kulcsán található függvény által visszaadott értéket
  document.title = 'Valami - ' + to.meta.title(to);
  //mehetsz tovább az oldalra
  next();
});

export default router
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

## Fordítás
- Alias útvonal beállítás: @/ -> src mappa
- 
vite.config.js
```js
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
  ],
  //Alias útvonla: @-al kezdődően az src mappát tekintve abszolút hivatkozásokat használhatunk relatív helyett.
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  },
  //Ebbe a mappába fordítsa a webalkalmaézást
  //ilyenkor az apach szerver: www/proba mappába kell tenni
  build: {
    outDir: './dist/proba',
  },

  //Ez egy belső környezeti változó, ami:
  //npm run dev hatására: development
  //npm run build hatására: production
  //A base: mondja meg, hogy honnan idul a projekt
  //developent módban: / -> src mappa
  //product módban: a dist/proba mappa, vagy élesben a szerveren: www/proba (a lefordított alakalmazás)
  base: process.env.NODE_ENV === 'development' ? '/' : '/proba/',
})

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

Ezek értéke kiolvasható:
```js
if (process.env.NODE_ENV !== 'production') {
  console.log('Ez a napló csak fejlesztői módban látható.');
}
```
### .env fájlok és rendszerváltozók
- Célszerű saját rendszerváltozókat használni, amiket a **.env fájlokban** hozhatunk létre
- .env fájlok:
  - `.env`: Minden módban betöltődik: itt adhatjuk meg az alkalmazásra vonatkozó általános dolgokat
  - `.env.development`t: dev módban töltődik be
  - `.env.production`: éles környezet ben töltődik be.
- A .env fájlok rendszerváltozóinak névadási szabálya:  
  - A VITE_ előtag kötelező, csak ezek olvashatók be a kódból
  - A VITE_ előtaggal ellátott változók nyilvánosan elérhetők a böngészőben futó front-end kódban.
  - A VITE_ előtag célja a nyilvánosság
  - Ne használj VITE_ előtag nélküli változókat

### Példa env fájlokra
Példa az alkalmazás címe, verziója stb.
```env
VITE_APP_TITLE = Iskola webalkalmazás
VITE_APP_VER = 3.7.2
```

.env.development: Az API címe fejlesztői módban
```env
VITE_API_URL = http://localhost:3000/api
```
.env.production: Az API címe produkciós környezetban
```env
VITE_API_URL = http://akarmi.com:3000/api
```
### Környezeti változók elérése
Nem kell beírni, hogy melyik .env fájlból akarjuk beolvasni
```js
//.env-ből olvassa be
const appTitle = import.meta.env.VITE_APP_TITLE;
//.env.development, vagy .env.production-ból olvassa be
const apiUrl = import.meta.env.VITE_API_URL;
```




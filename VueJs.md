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
## Belépési pont
App.vue
```vue
<script setup></script>

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

<style scoped></style>
```

## Alakalmazás indító
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

## Weboldalak: views
- Az alkalmazásunk weboldalai
    - Ezek töltődnek be az App.vue <RouterView/> dobozába
    - Helye a scc/views mappa
    
például: About.vue
```vue
<template>
  <div>
    <h1>About</h1>
  </div>
</template>

<script>
export default {

}
</script>

<style></style>
```


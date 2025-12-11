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

# Indítás
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

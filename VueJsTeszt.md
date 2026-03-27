# Tesztek futtatása
## ~ Vitest futtatási módok:

- Alap futtatás (Watch mód):
```console
npm run test:unit
```

- Egyszeri futtatás:
```console
npx vitest run
```

- Futtatás watch módban:
```console
npx vitest run
```
Ha módosítasz bármit a Store-ban vagy a tesztfájlban, a Vitest a másodperc tört része alatt újra futtatja a teszteket és kiírja az eredményt.


- Grafikus eredmény flület
```console
npx vitest --ui
```

- Konkrét teszt fájl futtatása
```console
npx vitest useSportStore
```
## ~ Teszt report generálás
- HTML woeboldal riport generálása:
```console
npx vitest --reporter=html
```
Ez létrehoz egy html mappát a projektgyökérben, amit böngészőben megnyitva egy interaktív felületet kapsz az összes tesztről, futási időről és hibáról.

- Generálás text fájlba:
```console
npx vitest run --reporter=verbose --no-color > vitest-results.txt
```

- Géppel olvasható XML (JUnit formátum)
```console
npx vitest run --reporter=default --reporter=junit --outputFile=vitest-results.xml
```


## ~ Vitest lefedettség vizsgálat
- Csomag telepítés
```console
npm install -D @vitest/coverage-v8
```
- kódod hány százaléka van tesztelve
```console
npx vitest --coverage
```
Keletkezik egy coverage mappa, amely statikus weboldalon meg lehet nézni, hogy mik maradtak ki.


# Teszt eszközök
## Unit és Komponens tesztelés: **Vitest**
- A Vitest a Vite motorját használja, így a tesztkörnyezet pontosan ugyanúgy látja a kódodat (importok, alias-ok, pluginok), mint a böngésző.
- Vue komponensek logikájának, segédfüggvényeknek (helper functions) és a Pinia/Vuex store-ok tesztelésére.
- Szükséges csomagok: vitest, @vue/test-utils (a komponensek rendereléséhez), és opcionálisan @vitejs/plugin-vue.
## E2E (End-to-End) tesztelés: Playwright vagy Cypress
### Playwright (Ajánlott)
- A Microsoft fejlesztése, ami jelenleg a leggyorsabb és legmodernebb E2E keretrendszer.
- Előnye: Natívan támogatja a párhuzamos tesztfuttatást, és kiválóan kezeli a modern webes várakozásokat (nem kell manuálisan wait-eket beírnod).
- Laravel tipp: Nagyon jól együttműködik a CI/CD folyamatokkal.

### Cypress
- A legnépszerűbb és legbarátságosabb felülettel rendelkező eszköz.
- Előnye: Van egy vizuális tesztfuttatója, ahol látod lépésről lépésre, mi történik a böngészőben. Nagyon könnyű vele elkezdeni a munkát.
- Hátránya: Kicsit lassabb lehet, mint a Playwright, és a több böngészőablakos tesztelés nehézkes benne.

# Unit és Komponens tesztelés (Vitest)
## A unit tesztek helye
src/__tests__/unit/

## vitest.config.js
A tesztek futtatásához kell egy konfigurációs fájl
```js
import { fileURLToPath } from 'node:url'
import { defineConfig } from 'vitest/config'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  test: {
    // Ez szimulálja a böngészőt (fontos a DOM műveletekhez)
    environment: 'jsdom',
    // Így nem kell minden fájlba importálni a describe, it, expect függvényeket
    globals: true,
    // Kizárjuk az E2E teszteket a unit tesztek közül
    exclude: ['**/node_modules/**', '**/dist/**', '**/e2e/**'],
    root: fileURLToPath(new URL('./', import.meta.url)),
  },
  resolve: {
    alias: {
      // Ez oldja fel a @/ hivatkozásokat a tesztekben
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})
```

## Példa egy teszt fájlra
A teszt fájl célja, hogy szmulálja a sport tábla crud műveleteit.
- Az adatokat nem az adatbázisból vesszük, hanem mock-oljuk
- A teszt fájl helye és neve: src/__tests__/unit/stores/useSportStore.spec.js

```js
import { setActivePinia, createPinia } from 'pinia';
import { describe, it, expect, beforeEach, vi } from 'vitest';
import { useSportStore } from '@/stores/sportStore';
import apiClient from '@/api/axiosClient';

// 1. Az API kliens szimulálása (Mockolás)
vi.mock('@/api/axiosClient', () => ({
  default: {
    get: vi.fn(),
    post: vi.fn(),
    patch: vi.fn(),
    delete: vi.fn(),
    interceptors: {
      request: { use: vi.fn() },
      response: { use: vi.fn() }
    }
  }
}));

describe('SportStore CRUD műveletek', () => {
  beforeEach(() => {
    setActivePinia(createPinia()); // Minden teszt előtt tiszta Pinia kell
    vi.clearAllMocks(); // Töröljük az előző teszt hívásait
  });

  // READ teszt
  it('getAll - sikeresen lekéri a sportokat', async () => {
    const store = useSportStore();
    const mockSports = { data: [{ id: 1, sportNev: 'Kosárlabda' }] };
    apiClient.get.mockResolvedValue(mockSports);

    await store.getAll();

    expect(apiClient.get).toHaveBeenCalledWith('/sports');
    expect(store.items).toHaveLength(1);
    expect(store.items[0].sportNev).toBe('Kosárlabda');
  });

  // CREATE teszt
  it('create - új sportot ad hozzá és frissíti a listát', async () => {
    const store = useSportStore();
    const newSport = { sportNev: 'Tenisz' };
    
    // Szimuláljuk a sikeres mentést, majd a lista újratöltését
    apiClient.post.mockResolvedValue({ data: { id: 2, ...newSport } });
    apiClient.get.mockResolvedValue({ data: [{ id: 2, ...newSport }] });

    const success = await store.create(newSport);

    expect(success).toBe(true);
    expect(apiClient.post).toHaveBeenCalled();
    expect(store.items[0].sportNev).toBe('Tenisz');
  });

  // DELETE teszt speciális hibaágra (MySQL 1451)
  it('delete - kezeli a kényszerfeltétel hibát (1451)', async () => {
    const store = useSportStore();
    const errorResponse = {
      response: {
        status: 500,
        data: { message: "1451-es hiba üzenete" }
      }
    };
    
    apiClient.delete.mockRejectedValue(errorResponse);

    try {
      await store.delete(1);
    } catch (e) {
      // Az interceptorod dobja tovább, itt elkapjuk
    }

    expect(store.error).toBeDefined();
    expect(store.loading).toBe(false);
  });
});
```

## A Vitest építőkövei
- Építőkövek: a vitest függvényei. Ezek segítségével használjuk a Vitest-et.
- Az építőkövek importálása:
```js
import { describe, it, expect, beforeEach, vi } from 'vitest';
```

### describe – Tesztek csoportosítása
A describe arra való, hogy logikai egységekbe rendezze a tesztjeidet. Nem hajt végre ellenőrzést, csak címkéz.

```js
describe('SportStore CRUD műveletek', () => {
  beforeEach(() => {...});

  // READ teszt
  it('getAll - sikeresen lekéri a sportokat', async () => {...});

  // CREATE teszt
  it('create - új sportot ad hozzá és frissíti a listát', async () => {...});

  // DELETE teszt speciális hibaágra (MySQL 1451)
  it('delete - kezeli a kényszerfeltétel hibát (1451)', async () => {...});
});
```

### it: Egy teszt függvény
```js
//...
// READ teszt
it('getAll - sikeresen lekéri a sportokat', async () => {
  const store = useSportStore();
  const mockSports = { data: [{ id: 1, sportNev: 'Kosárlabda' }] };
  apiClient.get.mockResolvedValue(mockSports);

  await store.getAll();

  expect(apiClient.get).toHaveBeenCalledWith('/sports');
  expect(store.items).toHaveLength(1);
  expect(store.items[0].sportNev).toBe('Kosárlabda');
});
//...
```

### except: Elvárás megvalósítása
Összehasonlítja a kapott eredményt az általad elvárt eredménnyel.
```js
//...
expect(apiClient.get).toHaveBeenCalledWith('/sports');
expect(store.items).toHaveLength(1);
expect(store.items[0].sportNev).toBe('Kosárlabda');
//...
```

### beforeEach – A "Takarító" (Előkészítés)
Biztosítja a "tiszta lapot". Például minden teszt előtt alaphelyzetbe állítja a Store-t, hogy az előző teszt adatai ne zavarják össze a következőt.

```js
describe('SportStore CRUD műveletek', () => {
  beforeEach(() => {
      setActivePinia(createPinia()); // Minden teszt előtt tiszta Pinia kell
      vi.clearAllMocks(); // Töröljük az előző teszt hívásait
    });
  //...
});
```

### vi – A "Bűvész" (Utility objektum / Mockolás)
Leggyakrabban mockolásra (helyettesítésre) használjuk. Segítségével létrehozhatsz "kém" függvényeket, amik figyelik, hányszor hívták meg őket, vagy teljesen lecserélhetsz velük külső fájlokat (mint az axiosClient).

```js
//...
// 1. Az API kliens szimulálása (Mockolás)
vi.mock('@/api/axiosClient', () => ({
  default: {
    get: vi.fn(),
    post: vi.fn(),
    patch: vi.fn(),
    delete: vi.fn(),
    interceptors: {
      request: { use: vi.fn() },
      response: { use: vi.fn() }
    }
  }
}));
//...
```

# Cypress (End-to-End) teszt
## Telepítés
- Cypress csomag telepítése a projektbe
```console
npm install cypress --save-dev
```
- Cypress program telepítése a windows-ra
```console
npx cypress install
```

## Indítás
- Full indítás
```console
npx cypress open
```

- Melyik böngészővel induljon
```console
npx cypress open --browser electron
```

- Milyen teszttel és böngészővel
```console
npx cypress open --e2e --browser electron
```


- Ez létrehozza a szükséges mappaszerkezetet (cypress/ mappa) 
- és konfigurációs fájlokat. 
- Elindul a Cypress ablak
  - Válaszd az **E2E Testing** opciót.
  - Válaszd az Electron böngészőt

## Cypress nyelvezet

### should (Elvárás)
```js
//Kiszelektál egy elemet, majd vizsgál
cy.get(szelektor) . should ( 'parancs (assertion)' , 'mi legyen az értéke' )

//Példák
//Valami látható-e a képernyőn
cy.get('.invalid-feedback').should('be.visible')

//Van-e ilyen osztálya a szelektált elemnek (pl. form)
cy.get('form').should('have.class', 'was-validated')

//A kiszelektált inputba ez van-e írva
cy.get('#email').should('have.value', 'teszt@elek.hu')

//A kiszelektált elem tartalmazza-e ezt a szót
cy.get('.card-header').should('contain', 'Login')

//Le van-e tiltva egy gomb
cy.get('button').should('be.disabled')
```

### Láncolás
A kiszelektált elemre több elvárást is összefűzhetünk az and-el
```js
cy.get('#email')
  .should('be.visible')       // Legyen látható
  .and('have.class', 'form-control') // Legyen rajta a form-control osztály
  .and('have.value', '');     // Ne legyen beleírva semmi
```

### Beírás egy mezőbe (type)
```js
cy.get('#email').type('teszt@elek.hu');
```

### Katttintás (esemény kiváltás)
```js
cy.get('button[type="submit"]').click();
```
### Képernyőkép készítés
- A tesztkódod bármelyik pontján kérhetsz egy pillanatképet a cy.screenshot() paranccsal
- A projekt gyökerében létrejön egy cypress/screenshots mappa, benne a tesztfájlod nevével ellátott almappával és a .png fájllal.
```js
it('Bejelentkezés és screenshot', () => {
  cy.visit('/login');
  cy.get('#email').type('teszt@elek.hu');
  
  // Készítünk egy képet a kitöltött űrlapról
  cy.screenshot('kitoltott-urlap'); 

  cy.get('button[type="submit"]').click();
});
```


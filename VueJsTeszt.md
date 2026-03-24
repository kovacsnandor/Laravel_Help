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

- Grafikus eredmény flület
```console
npx vitest --ui
```

- Konkrét teszt fájl futtatása
```console
npx vitest useSportStore
```

- HTML riport generálása:
```console
npx vitest --reporter=html
```
Ez létrehoz egy html mappát a projektgyökérben, amit böngészőben megnyitva egy interaktív felületet kapsz az összes tesztről, futási időről és hibáról.

## ~ Vitest lefedettség vizsgálat
- Csomag telepítés
```console
npm install -D @vitest/coverage-v8
```
- kódod hány százaléka van tesztelve
```console
npx vitest --coverage
```

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

```
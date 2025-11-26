# Linkek
[Laravel tesztelés](https://laravel.com/docs/master/http-tests)
[Laravel adatbázis tesztelés](https://laravel.com/docs/master/database-testing)


# Tesztelés
## Tesztelés alapfogalmak
- Mi a Fehér dobozos tesztelés
    - Rendelkezésre áll a forráskód
    - A kódot valmilyen módon futtatjuk, és nézzük, hogy mi módon reagál

- A fehér dobozos tesztelés fajtái:
    - Kézi teszt (pingelés: pl. request.rest)
    - Unit teszt (Egységteszt) (70%)
    - Funkcionális teszt (20%)
    - Integrációs teszt (10%)

### Unit teszt (egységteszt)
Helye: **test/Unit** mappa
A programozásban az egységteszt (unit test) a szoftvertesztelés egyik módszere, amelynek során a forráskód egyes egységeit (pl. **függvények, metódusok, osztályok**) külön-külön teszteljük annak érdekében, hogy ellenőrizzük, megfelelően működnek-e.
- Segédfüggvények ellenőrzése
- Konroller függvények ellenőrzése
- Adatbázis, táblák, kapcsolatok ellenőrzése


### Funkcionális (Feature teszt) tesztelés
Helye: **test/Feature** mappa
A funkcionális tesztelés a szoftverfejlesztés egyik kritikus szakasza, amelynek során a szoftver funkcionalitását a specifikációk alapján ellenőrzik. **Úgy működik-e a program, ahogy elő van írva.**
- get, post, patch, delet kérések tesztelése


### Integrációs tesztelés
Helye: **test/Feature** és a **test/Unit** mappa
Az integrációs tesztelés a szoftverfejlesztés egyik kritikus fázisa, amelynek célja annak ellenőrzése, hogy **a különböző szoftvermodulok vagy alrendszerek megfelelően működnek-e együtt**. Ez a tesztelési típus az egységtesztelés után következik, és a rendszertesztelés előtt zajlik.

# Tesztek létrehozása és futtatása laravel-ben
- A Laravel alapértelmezetten a **PHPUnit** tesztelési keretrendszert használja, amely már a projekted **vendor** könyvtárában megtalálható.
- A tesztelés teljes irányítása a **php artisan** parancsokkal történik.

## Teszt osztályok létrehozása
### Unit teszt létrehozása
- Létrehozza a **tests/Unit/DatabaseServiceTest.php** fájlt, ami egy **teszt osztály**.
    - A teszt osztály akármennyi **teszt metódust** (Ezt nevezzük **teszt**-nek) tartalmazhat
    - Ezek a metódusok futnak a tesztek során
    - Elvileg különböző osztályokban lehet ugyanolyan nevű teszt
    - Céljuk, hogy teszteljék az adott funkciót
    - Lefutásuk után közlik (a konzolon), hogy sikeres, vagy sikertelen volt-e a teszt és mutatják, hol a probléma
```php
php artisan make:test DatabaseServiceTest --unit
```
### Funkcionális (Feature) teszt létrehozása
- Létrehozza a **tests/Feature/ProductApiTest.php** fájlt.
```php
php artisan make:test ProductApiTest
```

## Tesztek futtatása
- Az összes teszt futtatása
```console
php artisan test
```
- Csak a Unit tesztek futtatása
```console
php artisan test --testsuite=Unit
```
- Csak a Feature tesztek futtatása
```console
php artisan test --testsuite=Feature
```
- Egy teszt osztály összes tetjének futtatása
```console
php artisan test tests/Feature/ProductApiTest.php
```
- Egy teszt metódus futtatása (ha több osztályban van ugyanilyen nevű lefut mind)
```console
php artisan test --filter test_can_create_new_product
```
- Egy osztály teszt metódusának futtatása
```console
php artisan test tests/Feature/ProductApiTest.php --filter test_can_create_new_product
```

## Teszt metódusok névadási követelményei
### Prefix módszer (ez az ajánlott) 
- A legfontosabb szabály, hogy egy teszt metódus neve a **test** szóval kezdődjön.
- Ha egy metódus nem a test szóval kezdődik, az egy segédfüggvény, és nem futtatják a `php artisan test` parancsok.
Példa egy teszt oszályon belüli névadásra:
```php
//Ez egy teszt metódus 
public function test_user_can_be_created()
{
    // Teszt kód, ami lefuttatja a tesztet
    $this->assertTrue(true);
}

//Ez egy segéd metódus
public function createUserWithDefaults()
{
    // Segéd logika, például egy felhasználó létrehozása az adatbázisban
    // A PHPUnit nem futtatja le tesztként
}
```

### PHP Annotáció 
Tesztként kezeli a rendszer azt a függvényt, aminek a kommentjében benne van a @test annotáció
Példa:
```php
/** @test */
public function it_returns_404_when_product_not_found()
{
    // Teszt kód
    $this->get('/api/products/999')->assertStatus(404);
}
```

### Különleges segédfüggvények
- Vannak olyan speciális segédfüggvények, amik automatikusan lefutnak
- setUp(): Automatikusan lefut az osztály minden tesztje előtt egyszer
- tearDown(): Automatikusan lefut az osztály minden tesztje után egyszer
- Fontos a ezek használatakor kötelező:
    - parent::setUp() a függvény elején
    - parent::tearDown() a függvény végén

Példa: Szerenénk, hogy az osztály tesztjei előtt bejelentkezzen egy user:
```php
use Tests\TestCase;

class ProductApiTest extends TestCase
{
    // A PHPUnit minden teszt előtt automatikusan meghívja
    protected function setUp(): void
    {
        //Meg kell hogy hívja az ősosztály (TestCase) ugyanilyen nevű metódusát
        parent::setUp();
        // Példa segéd metódus hívása
        $this->createAdminUser(); 
    }

    /** @test */
    public function a_product_can_be_retrieved_by_admin()
    {
        // ... teszt kód
    }

    // Ez egy segéd metódus, nem fut le tesztként.
    protected function createAdminUser()
    {
        // Itt jön az admin felhasználó létrehozásának logikája
        // pl.: User::factory()->admin()->create();
    }

    protected function tearDown(): void
{
    // 1. Saját tisztítás: Töröld a temp fájlokat, külső mock szerver kapcsolatot
    $this->deleteTestFiles(); 

    // 2. Szülő hívása: Végleges keretrendszer tisztítás, mock-ok ellenőrzése
    parent::tearDown(); 
}
}
```

# Főbb assert Kategóriák Laravelben
A Laravel tesztosztályok két nagy csoportra osztják az **assert** metódusokat.
- HTTP Válasz Ellenőrzők
- JSON Ellenőrzők

## HTTP Válasz Ellenőrzők
Parancs,Leírás,Példa
- `assertJson(array $data)`: Ellenőrzi, hogy a válasz tartalmazza a megadott JSON adatszerkezetet (részleges egyezés).
```php
$response->assertJson(['name' => 'Laptop']);
```
- `assertJsonFragment(array $data)`: Ellenőrzi, hogy a válasz JSON-jének bármelyik része tartalmazza a megadott töredéket.
```php
$response->assertJsonFragment(['total' => 1000]);
```
- `assertExactJson(array $data)`: Ellenőrzi, hogy a válasz pontosan megegyezik a megadott JSON-nal."
```ph
$response->assertExactJson([...]);
```
- `assertJsonStructure(array $structure)`: Ellenőrzi, hogy a válasz JSON-je tartalmazza a megadott kulcsokat (szerkezet ellenőrzés).
```php
$response->assertJsonStructure(['data' => ['id', 'name']]);"
```
- `assertJsonCount(int $count, string $key = null)`: Ellenőrzi egy JSON tömb elemeinek számát.
```php
$response->assertJsonCount(3, 'products');"
```
    - 
## JSON Ellenőrzők
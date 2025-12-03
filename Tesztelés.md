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

# Adatbázis beállítása teszteléshez
Hogy a teszt futtatásakor mi legyen a környezet, például mi legyen a teszt adatbázis stb. két fájl határozza meg:
- `phpunit.xml` (ennek a beállításai az **elsődlegesek**)
- `.env.testing` (ezek a **másodlagosak**, de ami nincs a phpunit.xml-ben, azt .env.testing-ből olvassa be)
- Egy jó stratégia: mindent üssünk ki a phpunit.xml-ből, amit a .env.testing-ből akarunk irányítani.
    - például, hogy mi legyen a teszt futtatásakor a használt adatbázis
## phpunit.xml javasolt beállítása
Alapvetően ebben a fájlban definiáljuk a teszt kötnyezetet.
- <env name="APP_ENV" value="testing"/>: az állítja be, hogy a teszteléhez használhatjuk a .env.testing fájlt, annak beállításait
- Ezeket most azért vesszük ki, mert ezek felülírnák .env.testing-ben lévő ugyanilyen beállításokat:
    <!-- <env name="DB_CONNECTION" value="sqlite"/> -->
    <!-- <env name="DB_DATABASE" value=":memory:"/> -->

```xml

    <testsuites>
        <testsuite name="Unit">
            <directory>tests/Unit</directory>
        </testsuite>
        <testsuite name="Feature">
            <directory>tests/Feature</directory>
        </testsuite>
    </testsuites>
    <source>
        <include>
            <directory>app</directory>
        </include>
    </source>
    <php>
        <env name="APP_ENV" value="testing"/>
        <env name="APP_MAINTENANCE_DRIVER" value="file"/>
        <env name="BCRYPT_ROUNDS" value="4"/>
        <env name="BROADCAST_CONNECTION" value="null"/>
        <env name="CACHE_STORE" value="array"/>

        <!-- <env name="DB_CONNECTION" value="sqlite"/> -->
        <!-- <env name="DB_DATABASE" value=":memory:"/> -->

        <env name="MAIL_MAILER" value="array"/>
        <env name="QUEUE_CONNECTION" value="sync"/>
        <env name="SESSION_DRIVER" value="array"/>
        <env name="PULSE_ENABLED" value="false"/>
        <env name="TELESCOPE_ENABLED" value="false"/>
        <env name="NIGHTWATCH_ENABLED" value="false"/>
    </php>

```
## .env.testing
Az egy jó stratégia, hogy a .env.testing beállításai adják a teszt környezetet.
A tesztek az itt beállított adatbázison fognak futni.
1. Hozzuk létre ezt a fájlt a `.env` lemásolásával
2. Nevezzük át `.env.testing` névre.
3. Ha ugyanazon az adatbáziskon tesztelünk, akkor kész.
    - Ha nem, akkor írjuk be annak az adatbázisnak a nevét.

Most úgy állítottuk be, hogy a teszt esetén az éles adatbázisunkat használjuk: school
.env.testing részlete
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=school
DB_USERNAME=root
DB_PASSWORD=
```

## Tesztek Tranzakciókezeléssel
Ahhoz,hogy a tesztek ne befolyásolják az adatokat a laravel automatikus tranzakciókezelést tesz lehetővé minden tesztosztályban
```php

use Illuminate\Foundation\Testing\DatabaseTransactions;
use Tests\TestCase;

class UserTest extends TestCase
{
    //Tranzakciókezelés bekapcsolása
    use DatabaseTransactions;
    //...
}
```


# Tesztek létrehozása és futtatása laravel-ben
- A Laravel alapértelmezetten a **PHPUnit** tesztelési keretrendszert használja, amely már a projekted **vendor** könyvtárában megtalálható.
- A tesztelés teljes irányítása a **php artisan** parancsokkal történik.

## Teszt osztályok létrehozása
- A teszt osztály akármennyi **teszt metódust** (Ezt nevezzük **teszt**-nek) tartalmazhat
- Ezek a metódusok futnak a tesztek során
- Elvileg különböző osztályokban lehet ugyanolyan nevű teszt
- Céljuk, hogy teszteljék az adott funkciót
- Lefutásuk után közlik (a konzolon), hogy sikeres, vagy sikertelen volt-e a teszt és mutatják, hol a probléma

### A laravel teszt osztályai
A laravel tesztekhez használjuk a 
- `Test\TestCase` könyvtárat a 
- `PHPUnit\Framework\TestCase` helyett (ebben csak a legalapvetőbb asert függvények vannak). 

```php
// use PHPUnit\Framework\TestCase;
use Tests\TestCase;

class ExampleTest extends TestCase
{
    //...
}    
```

### ~ Unit teszt létrehozása
```console
php artisan make:test DatabaseServiceTest --unit
```
- Létrehozza a **tests/Unit/DatabaseServiceTest.php** fájlt, ami egy **teszt osztály**.
### ~ Funkcionális (Feature) teszt létrehozása
```console
php artisan make:test ProductApiTest
```
- Létrehozza a **tests/Feature/ProductApiTest.php** fájlt.

## ~ Teszt futtatása konzolra
A teszt eredményei megjelennek a konzolon.

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
- Egy teszt osztály összes tetszjének futtatása
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

## ~ Teszt futtatása fájlba
A teszt kimenetés különböző formátumú fájlba is irányíthatjuk a megfelelő artisan paranccsal

- JUnit XML formátum:
```console
php artisan test --log-junit test-results.xml
```
- Agile dokumentáció (TestDox) text:
```console
php artisan test --testdox-text test-results.txt
```

- Agile dokumentáció (TestDox) html:
```console
php artisan test --testdox-html test-results.html
```

## Teszt névadási követelmények
1. A teszt osztályoknak, fájlneveinek a **Test** szóval kell végződni: pl. `DatabaseTest`
2. A teszt metódusoknak a test szóval kell kezdődnie: pl.: test_user_can_be_created()
3. A teszt osztályok alapértelmezésben ABC sorrrendben futnak le.
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
### Teszt átlépése, kiiktatása (skipped)
```php
public function test_this_one_is_too_slow()
{
    $this->markTestSkipped('Ideiglenesen kiiktatva, a teszt túl lassú.');

    // Ez a kód már nem fog lefutni
    // ... 
}
```

# Főbb assert Kategóriák Laravelben
A teszteléshez az ún. assert parancsolkat használjuk. Assert jelentése = állítani valamit. Ha ez ez állítás igaz, akkor a teszt jól lefutott.
A Laravel tesztosztályok több nagy csoportra osztják az **assert** metódusokat.
- HTTP Válasz Ellenőrzők
- JSON Ellenőrzők
- Adatbázis Ellenőrzők
- Általános PHPUnit Ellenőrzők

## Általános PHPUnit Ellenőrzők
A Laravel tesztjeid a PHPUnit alapvető ellenőrző metódusait is használhatják (ezek a TestCase osztálytól öröklődnek). Ezeket általában a belső logikák tesztelésére használják (Unit Tests).
- `assertTrue($condition)`: Ellenőrzi, hogy egy feltétel igaz-e.
```php
$this->assertTrue(is_array($data));
```
- `assertFalse($condition)`: Ellenőrzi, hogy egy feltétel hamis-e.
```php
$this->assertFalse($user->isAdmin());
```
- `assertEquals($expected, $actual)`: Ellenőrzi, hogy két változó értéke azonos-e.
```php
$this->assertEquals(5, count($items));
```
- `assertCount(int $expectedCount, $haystack)`: Ellenőrzi egy tömb vagy gyűjtemény méretét
```php
$this->assertCount(10, $products);
```
- `assertNull($variable)`: Ellenőrzi, hogy egy változó null értékű-e.
```php
$this->assertNull($result);
```
- `assertNotNull($variable)`: Ellenőrzi, hogy egy változó nem null értékű-e.
```php
$this->assertNotNull($user->api_token);
```
- `assertInstanceOf($expected, $actual)`: Ellenőrzi, hogy egy objektum egy adott osztály példánya-e.
```php
$this->assertInstanceOf(User::class, $user);
```


## HTTP Válasz Ellenőrzők
- Ezek a metódusok a 
    - `this->get(...)` vagy 
    - `this->postJson(...)` 
    - hívások eredményeként kapott HTTP válasz ($response objektum) ellenőrzésére szolgálnak.

- `assertSee('xxx');`: Ellenőrzi a válasz HTTP tartalmazza-e az 'xxx' szöveget.
```php
$response->assertSee('xxx');
```
- `assertStatus(int $code)`: Ellenőrzi a válasz HTTP státuszkódját (pl. 200, 201, 404).
```php
$response->assertStatus(200);
```
- `assertOk()`: Ellenőrzi, hogy a státusz 200 OK-e.
```php
$response->assertOk();
```
- `assertCreated()`: Ellenőrzi, hogy a státusz 201 Created-e.
```php
$response->assertCreated();
```
- `assertNotFound()`: Ellenőrzi, hogy a státusz 404 Not Found-e.
```php
$response->assertNotFound();
```
- `assertUnauthorized()`: Ellenőrzi, hogy a státusz 401 Unauthorized-e.
```php
$response->assertUnauthorized();
```
- `assertForbidden()`: Ellenőrzi, hogy a státusz 403 Forbidden-e.
```php
$response->assertForbidden();
```
- `assertRedirect(string $uri = null)`: Ellenőrzi, hogy a válasz átirányítás-e (301, 302).
```php
$response->assertRedirect('/login');
```
- `assertSessionHasErrors(array $keys = [])`: Ellenőrzi a validációs hibákat (API-nál jellemzően a 422 Unprocessable Entity válaszhoz kapcsolódik).
```php
$response->assertSessionHasErrors(['email', 'password']);
```

## JSON Ellenőrzők
API fejlesztés során a válasz JSON tartalmának helyességét kell ellenőrizni.
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

## Adatbázis Ellenőrzők
- `assertDatabaseHas(string $table, array $data)`: Ellenőrzi, hogy az adott táblában létezik-e a feltételeknek megfelelő sor.
```php
$this->assertDatabaseHas('products', ['name' => 'New Product']);
```
- `assertDatabaseMissing(string $table, array $data)`: Ellenőrzi, hogy az adott táblában nem létezik-e a feltételeknek megfelelő sor (pl. törlés után).
```php
$this->assertDatabaseMissing('users', ['id' => 1]);
```
- `assertDeleted($model, $table = null)`: Ellenőrzi, hogy egy modell példány törölve lett.
```php
$this->assertDeleted($user);"
```



## Adatstruktúrák Assert Metódusai
Ezek a metódusok tömbök és objektumok szerkezetének ellenőrzésére ideálisak.
- `assertArrayHasKey($key, $array)`: Ellenőrzi, hogy a tömb tartalmazza-e a megadott kulcsot.
```php
$this->assertArrayHasKey('name', $userData);
```
- `assertArrayNotHasKey($key, $array)`: Ellenőrzi, hogy a tömb nem tartalmazza-e a kulcsot.
```php
$this->assertArrayNotHasKey('password', $responseArray);
```
- `assertContains($needle, $haystack)`: Ellenőrzi, hogy egy érték benne van-e a tömbben/gyűjteményben.
```php
$this->assertContains('admin', $userRoles);
```
- `assertCount($expectedCount, $arrayOrCollection)`: Ellenőrzi, hogy a tömb/gyűjtemény elemeinek száma megegyezik-e a várt értékkel.
```php
$this->assertCount(3, $cartItems);
```
- `assertIsArray($variable)`: Ellenőrzi, hogy a változó tömb-e.
```php
$this->assertIsArray($config);
```
- `assertIsString($variable)`: Ellenőrzi, hogy a változó string-e.
```php
$this->assertIsString($token);
```
- `assertInstanceOf($expectedClass, $actualObject)`: Ellenőrzi, hogy az objektum a megadott osztály egy példánya.
```php
$this->assertInstanceOf(User::class, $student);
```
# Verzió: 1.2.1

Szemantikus Verziózás (Semantic Versioning: SemVer), amely három szintet használ:
Major.Minor.Patch:

- Major (fő verzió): Alapvető változtatás(ok)
- Minor (alverzió): Új fejezet(ek)
- Patch (javítás): Kisebb javítások

# Laravel parancs összefoglaló

## Alap telepítés

- `composer create-project laravel/laravel laravel-rest-api`
  - Laravel **laravel-rest-api** nevű (ez lesz a projekt mappája) projekt létrehozása

## Artisan parancsok

- Az Artisan a Laravel keretrendszer parancssori felülete (CLI - Command-Line Interface).
- Minden Artisan parancsot a Laravel projekt gyökérkönyvtárából kell futtatni a php artisan előtaggal.
- Segítségével tudjuk a keretrendszert bővíteni, működtetni
- A Laravel gyökerében található **artisan** fájl maga az Artisan parancssori eszköz belépési pontja.
  - Amikor a terminálban kiadod a php artisan [parancs] utasítást, a PHP értelmező a artisan fájlt futtatja le először.
  - A fájl feladata, hogy
    - betöltse a Laravel keretrendszert,
    - elindítsa a console kernelt, majd
    - átadja a megadott parancsot (pl. make:model) feldolgozásra.

## Help használata

- ```
php artisan list
```
  - Összes parancs listája
- `php artisan help make:model` vagy `php artisan make:model --help`
  - Help egy konkrét parancsról

## Laravel verzió lekérdezése

- `php artisan --version`
  - Csak a verziószám
- `php artisan -v`
  - Verziószám és parancslista

## Szerver indítás

- `php artisan serve`
  - Szerver indítás
- `php artisan serve --port=8001`
  - Szerver indítás adott porton

## API telepítés

- `php artisan install:api`
  - Az api támogatást le kell telepíteni

## Adatbázis létrehozás

- `php artisan db:create`
  - A .ini fájlban megadott adatbázis hozza létre
  - A config/database.php-ben megadott kódolással

## Migráció

- `php artisan migrate`
  - Migráció futtatása

### Egyéb migrációs parancsok:

- `php artisan migrate:rollback`
  - Az utolsó migrációs csomag visszavonása (down())
- `php artisan migrate:rollback --step=1`
  - Az utolsó migráció visszavonása (down())
- `php artisan migrate:rollback --step=3`
  - Az utolsó 3 migráció visszavonása (down())
- `php artisan migrate:reset`
  - Az összes migráció visszavonása
- `php artisan migrate:refresh`
  - Visszavonja az összes migrációt (down()) majd újra lefuttatja őket (up())
- `php artisan migrate:refresh --seed`
  - Visszavonja az összes migrációt majd újra lefuttatja őket és a seedereket
- `php artisan migrate:fresh`
  - Törli az összes táblát és újra migrál (nem fut a down)
- `php artisan migrate:fresh --seed`
  - Törli az összes táblát és újra migrál és a seedel
- `php artisan migrate --path=database/migrations/2025_01_20_123456_create_products_table.php`
  - Konkrét Migráció Futtatása (up())
- `php artisan migrate:rollback --path=database/migrations/2025_01_20_123456_create_products_table.php`
  - Konkrét Migráció Visszavonása (down())
- `php artisan migrate:refresh --path=database/migrations/2025_01_20_123456_create_products_table.php`
  - Konkrét Migráció Frissítése (Újraépítése) (down(), up())

### Tábla módosítás

- `php artisan make:migration add_unique_index_to_produscts_name_column --table=products`
  - Utólagos táblamódosítás migrációs fájl létrehozás

## Seeder

- `php artisan make:seeder UserSeeder`
  - Seeder osztály készítés (UserSeeder osztály) (database/seeders/UserSeeder.php)
- `php artisan db:seed`
  - Seeder futtatása
- `php artisan db:seed --class=ProductSeeder`
  - Konkrét seeder osztály futtatása

## Tábla CRUD parancs

- `php artisan make:model Product -a --api`
  - Egy tábla CRUD előkészítése (minden fájlt létrehoz)

## Konroller készítő parancsok

Ajánlott parancs: `php artisan make:controller UserController --resource --model=User --requests`

- A parancsok utólga is kiadhatók, a meglévő fájlokat nem törlik.
- A tábla nevet egyesszámban adjuk meg: **User**

- `php artisan make:controller UserController`
  - app/Http/Controllers/UserController.php (nem hozza létre a metódusokat)
- `php artisan make:controller UserController --resource`
  - app/Http/Controllers/UserController.php és létrehozza a metódusokat (**--resource**)
- `php artisan make:controller UserController --resource --model=User --requests`
  - app/Http/Controllers/UserController.php (**make:controller**)
  - index, create stb metódusok (**--resource**)
  - Automatikusan "befűzi" (type-hinteli) a megadott modellt a vezérlő metódusaiba (**--model=User**)
  - app/Http/Requests/StoreUserRequest.php, UpdateUserRequest.php (**--requests**)
- `php artisan make:model User -mcr --requests`
  - m: Létrehozza a migrációt (migration).
  - c: Létrehozza a kontrollert (controller).
  - r: A kontrollert resource (erőforrás) stílusban hozza létre.
  - --requests: Létrehozza a **StoreUserRequest** és **UpdateUserRequest** validációs osztályokat is!

## Request osztályok létrehozása

- Store (post), és Udate (patch) műveltekhez szabályokat fogalmazhatunk meg bennük.
- Szerkezetileg ugyanazok, csak a nevükben és a szabályokban különböznek.
- `php artisan make:request StoreUserRequest`
- `php artisan make:request UpdateUserRequest`
- `php artisan make:request LoginUserRequest`
  - Update, Store vagy speciális például Login osztály létrehozás

## cors

- `php artisan config:publish cors`
  - A cors beállítás létrehozása: **config/cors.php**

# Laravel

[Laravel](https://laravel.com/)
[Laravel readouble](https://readouble.com/laravel/11.x/en/)
[Available Column Types](https://laravel.com/docs/11.x/migrations#available-column-types)

# Laravel projekt létrehozás

1. Laravel **laravel-rest-api** nevű (ez bármi lehet, ez lesz a projekt mappája) projekt létrehozása: `composer create-project laravel/laravel laravel-rest-api`

   - **laravel-rest-api** lesz a projekt mappája
   - Átmenni a **laravel-rest-api** mappába
   - Ellenőrzés: `php artisan serve`

2. Kapcsolódás az adatbázishoz: .env fájlban:
   Az adatbázis neve: laravel-rest-api

```.env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel-rest-api
DB_USERNAME=root
DB_PASSWORD=
```

2. Adatbázis kódolás beállítás (ez az alap, nem kell hozzányúlni):
   [Helyes adatbázis kódolás](https://gemini.google.com/share/4ef922c14e85)

Kódolás összehasonlítás:

- **utf8**: csak 3 bájtos ékezetes betűk (elavult)
- **utf8mb4**: 4 bájtos ékezetes betűk (**ajánlott**)
- **utf8mb4_hugarian_ci**: 4 bájtos magyar ékezetes sorbarandezési szabályok (csak, ha kizárólag magyar szöveget akarunk)
- **utf8mb4_unicode_ci**: 4 bájtos általános ékezetes sorbarandezési szabályok (**ajánlott**)

Ajánlott adatbázis beállítás:

```sql
CREATE DATABASE `database`
	CHARACTER SET utf8mb4
	COLLATE utf8mb4_unicode_ci;
```

// config/database.php

```php
 'mysql' => [
            'driver' => 'mysql',
            'url' => env('DB_URL'),
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'laravel'),
            'username' => env('DB_USERNAME', 'root'),
            'password' => env('DB_PASSWORD', ''),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => env('DB_CHARSET', 'utf8mb4'),
            'collation' => env('DB_COLLATION', 'utf8mb4_unicode_ci'),
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => true,
            'engine' => null,
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
            ]) : [],
        ],
```

3. Az api támogatást le kell telepíteni:
   `php artisan install:api`
   A végén kérdezi, hogy letelepítse-e a táblákat: Enter (yes)

!!!! Itt hibaüzenet van.
A hiba oka: [karakterhossz hiba](https://gemini.google.com/share/01a06eea5bdf)

app/Providers/AppServiceProvider.php fájba írjuk be:

```php
//Tegyük be még ezt is a tetejére:
use Illuminate\Support\Facades\Schema;

//Ez marad, e fölé
use Illuminate\Support\ServiceProvider;

//...

//Ebbe a függvénybe:
public function boot(): void
{
    //
    // Az alapértelmezett string hossza 191 karakterre csökkentése
    Schema::defaultStringLength(191);
}
```

- Migráció újrafuttatása:
  `php artisan migrate:fresh`

vagy
`php artisan migrate:rollback`
`php artisan migrate`

vagy töröjük le az összes táblát az adatbázisban és Futtassuk le a migrációt:
`php artisan migrate`

Létrejönnek a táblák (egy csomó, velük együtt a **user**)

4. Létre kel hozni azt a usert, akivel majd csatlakozni akarunk a seeder-el.
   **database/seeders/DatabaseSeeder.php** fájlban:

```php
public function run(): void
{
    User::factory()->create([
        'name' => 'test',
        'email' => 'test@example.com',
        'password' => '123',
    ]);
}
```

Lefuttatjuk a seeder-t: `php artisan db:seed` és létrejön egy user

5. Az api tesztelése
   routes/api.php

```php
//visszajön az api szöveg
Route::get('/', function(){
    return 'API';
});
```

request.rest fájl létrehozás:

```rest
### változók
@protocol = http://
@hostname = localhost
@port = 8000
@host = {{protocol}}{{hostname}}:{{port}}
### @token = 12|ibiwTdr77o8ppToh1tBxIkckBNb8ECjIjLgaweTaa757cdef

### teszt
get {{host}}/api/


### login
# @name login
POST {{host}}/api/users/login
Content-Type: application/json

{
    "email": "test@example.com",
    "password": "123"
}

###
@token = {{login.response.body.user.token}}

### get users
GET  {{host}}/api/users
Accept: application/json
Authorization: Bearer {{token}}


### get products
GET  {{host}}/api/products

### post products
POST  {{host}}/api/products
Accept: application/json
Content-Type: application/json
Authorization: Bearer {{token}}

{
    "category": "Bogyós",
    "name": "Málna2",
    "description": "Kézzel termelt egészség",
    "picture": "https:\/\/hur.webmania.cc\/img\/malna.jpg",
    "price": 8000,
    "stock": 500
}


### patch products
PATCH  {{host}}/api/products/20
Accept: application/json
Content-Type: application/json
Authorization: Bearer {{token}}

{
    "category": "Bogyósok",
    "price": 4000
}

### delete product by id
DELETE  {{host}}/api/products/21
Accept: application/json
Content-Type: application/json
Authorization: Bearer {{token}}
```

# Egy tábla CRUD

Egy **product** nevű tábla esetén

- `php artisan make:model Product -a --api`
- Létrehozza a kontrollert az össze metódussal, a modellt és a migrációs fájlt.
  - migrations\2025_11_01_191501_create_products_table.php
  - app\Models\product.php
  - seeders\ProductSeeder.php
  - database\factories\ProductFactory.php
  - app\Http\Controllers\ProductController.php
  - app\Http\Requests\StoreproductRequest.php
  - app\Http\Requests\UpdateproductRequest.php
  - app\Policies\ProductPolicy.php

## Migráció

A migráció célja, hogy a táblát (táblákat) létrehozzuk
A migrációs fájlban adjuk meg a tábla definícióját az up() metódusban

migrations\2025_11_01_191501_create_products_table.php

```php
public function up(): void
{
    Schema::create('products', function (Blueprint $table) {
        $table->integer('id')->autoIncrement();
        $table->primary('id');
        $table->string('category', 255)->notNull();
        $table->string('name', 191)->notNull();
        $table->string('description', 255);
        $table->string('picture', 255);
        $table->Integer('price');
        $table->Integer('stock');
        //Minta mezők

        // --- 2. Logikai (BOOLEAN) Alapértelmezett Értékkel
        // Alapértelmezés: FALSE (0)
        // TINYINT(1)
        $table->boolean('is_published')->default(false);

        // --- 3. Dátum (DATE)
        $table->date('start_date')->nullable()->default(null);

        // --- 4. Dátum és Idő (DATETIME) Alapértelmezett Értékkel
        // Alapértelmezés: NULL
        $table->dateTime('scheduled_posting')->nullable()->default(null);

        // --- 5. Idő (TIME)
        // Alapértelmezés: '09:00:00'
        $table->time('delivery_time')->default('09:00:00');

        // --- 6. Decimális (DECIMAL) Alapértelmezett Értékkel
        // Ár oszlop: 10 számjegy összesen, 2 tizedesjegy pontossággal.
        // Alapértelmezés: 0.00
        $table->decimal('final_price', 10, 2)->default(0.00);
        // $table->timestamps();
    });
}
```

A Modell-t is nézni kell:
app\Models\product.php

```php
    //...
    //Ne keresd a prodact táblákban a timestamps mezőket
    public $timestamps = false;

    //Ha létrehozunk egy új terméket, akkor milyen adatokat tudunk neki megadni
    //Ez kötelező
    protected $fillable = [
        'category',
        'name',
        'description',
        'picture',
        'price',
        'stock',
        'is_published'
    ];

    //Ezeket kívülről nem tudjuk feltölteni.
    //Nem kötelező
    protected $guarded = [
        'start_date',
        'scheduled_posting',
        'delivery_time',
        'final_price'
    ];

    //Ha az egész tála összes mezőjét visszaküldjük, akkor ezek rejtve maradnak
    //Nem kötlező
    protected $hidden = [
        'final_price'
    ];

    protected function casts(): array
    {
        return [
            'is_published' => 'boolean',
            'price' => 'integer'
        ];
    }
    //...
```

Hozzuk létre a táblát: `php artisan migrate`

## Egyéb migrációs parancsok:

Az utolsó migráció visszvonása: `php artisan migrate:rollback`
Az utolsó migráció visszvonása: `php artisan migrate:rollback --step=1`
Az utolsó 3 migráció visszvonása: `php artisan migrate:rollback --step=3`
Az összes migráció vissazvonása: `php artisan migrate:reset`
Visszavonja az összes migrációt (down()) majd újra lefuttatja őket (up()): `php artisan migrate:refresh`
Visszavonja az összes migrációt majd újra lefuttatja őket és a seedereket: `php artisan migrate:refresh --seed`
Törli az összes táblát és újra migrál (nem fut a down): `php artisan migrate:fresh`
Törli az összes táblát és újra migrál és a seedel: `php artisan migrate:fresh --seed`

Konkrét Migráció Futtatása (up metódus):
`php artisan migrate --path=database/migrations/2025_01_20_123456_create_products_table.php`

Konkrét Migráció Visszavonása (DOWN metódus)
`php artisan migrate:rollback --path=database/migrations/2025_01_20_123456_create_products_table.php`

Konkrét Migráció Frissítése (Újraépítése) (down(), up())
`php artisan migrate:refresh --path=database/migrations/2025_01_20_123456_create_products_table.php`

## Tábla terv utólagos módosítása (alter)

1. új migrációs módosító fájl létrehozása:
   `php artisan make:migration add_unique_index_to_produscts_name_column --table=products`

```php
public function up(): void
{
    Schema::table('products', function (Blueprint $table) {
        //Egyedi indexet teszek a name mezőre
        $table->unique('name', 'products_name_unique');
        //Beteszek egy új oszlopot
        $table->boolean('is_published2')->default(false);
        //Módosítom a mező méretét
        $table->string('category', 200)->change();
    });
}

/**
 * Reverse the migrations.
 */
public function down(): void
{
    Schema::table('products', function (Blueprint $table) {
        // Eltávolítja az egyedi indexet az 'email' oszlopról
        $table->dropUnique(['name']);
        // VAGY az index nevével:
        // $table->dropUnique('products_name_unique');
        $table->dropColumn('is_published2');

            // 2. A string oszlop méretének visszaállítása az eredeti 100-ra
            // (Az "posts" tábla eredeti oszlop mérete feltételezve: 100)
        $table->string('category', 255)->change();
    });
}
```

Megjegyzés: a **Schema::create** helyett itt már a **Schema::table** szerepel.

2. Migráció futtatása: `php artisan migrate`

# Seedelés

A seedelés intézi el a táblák adatokkal való feltöltését.
5 esetet szoktunk:

- sql script
- php tömbbel
- src fájlból
- teszt feltöltés factory-val véletlen kiválsztással saját tömbből
- Faker könyvtárral

## Tábla seederek

- Minden táblának külön seeder osztályt érdemes készíteni.
  Készítsünk egy UserSeeder osztályt (database/seeders/UserSeeder.php):  
  `php artisan make:seeder UserSeeder`

database/seeders/UserSeeder.php

```php
public function run(): void
{
    if (User::count()===0) {
        # code...
        User::factory()->create([
            'name' => 'test',
            'email' => 'test@example.com',
            'password' => '123',
        ]);
    }
}
```

Seederek indítása: database/seeders/DatabaseSeeder.php

```php
class DatabaseSeeder extends Seeder
{
    use WithoutModelEvents;


    //Előtte érdemes a táblákat letörölni:
    DB::statement('DELETE FROM products');
    DB::statement('DELETE FROM users');


    public function run(): void
    {
        //Ezeket a seedereket futtatja le sorban
        $this->call([
            UserSeeder::class,
            ProductSeeder::class,
        ]);

    }
}
```

Seeder futtatása: `php artisan db:seed`
Konkrét seeder osztály futtatása: `php artisan db:seed --class=ProductSeeder`

## Tömbös kézi

Egy tömbbe tesszük az adatokat és ebből generáljuk:

database/seeders/ProductSeeder.php

```php
//Tömbös kézi $data
$data =
[
    [
        'category' => 'Bogyós',
        'name' => 'Málna',
        'description' => 'Kézzel termelt egészség',
        'picture' => 'https://hur.webmania.cc/img/malna.jpg',
        'price' => 3800,
        'stock' => 500,
    ],
    [
        'category' => 'Bogyós',
        'name' => 'Áfonya',
        'description' => 'Az erdő kincse az otthonodba',
        'picture' => 'https://hur.webmania.cc/img/afonya.jpg',
        'price' => 3250,
        'stock' => 120,
    ],
    [
        'category' => 'Bogyós',
        'name' => 'Szeder',
        'description' => 'A hagyományos csemege',
        'picture' => 'https://hur.webmania.cc/img/szeder.jpg',
        'price' => 1700,
        'stock' => 40,
    ],
    [
        'category' => 'Bogyós',
        'name' => 'Eper',
        'description' => 'Egy tavaszi harapás',
        'picture' => 'https://hur.webmania.cc/img/eper.jpg',
        'price' => 1440,
        'stock' => 0,
    ],
    [
        'category' => 'Bogyós',
        'name' => 'Homoktövis',
        'description' => 'Mezei csemege',
        'picture' => 'https://hur.webmania.cc/img/homoktovis.jpg',
        'price' => 3200,
        'stock' => 100,
    ],
    [
        'category' => 'Bogyós',
        'name' => 'Som',
        'description' => 'A fanyar gyönyör',
        'picture' => 'https://hur.webmania.cc/img/som.jpg',
        'price' => 900,
        'stock' => 10,
    ],
    [
        'category' => 'Bogyós',
        'name' => 'Fanyarka',
        'description' => 'Édes mint a méz',
        'picture' => 'https://hur.webmania.cc/img/fanyarka.jpg',
        'price' => 990,
        'stock' => 25,
    ],
    [
        'category' => 'Bogyós',
        'name' => 'Piszke',
        'description' => 'Egres',
        'picture' => 'https://hur.webmania.cc/img/piszke.jpg',
        'price' => 750,
        'stock' => 100,
    ],
    [
        'category' => 'Bogyós',
        'name' => 'Ribizli',
        'description' => 'Fanyar, vasban gazdag',
        'picture' => 'https://hur.webmania.cc/img/ribizli.jpg',
        'price' => 1300,
        'stock' => 170,
    ],
    [
        'category' => 'Magyaros',
        'name' => 'Meggy',
        'description' => 'A falusi kincs',
        'picture' => 'https://hur.webmania.cc/img/meggy.jpg',
        'price' => 600,
        'stock' => 300,
    ],
    [
        'category' => 'Magyaros',
        'name' => 'Cseresznye',
        'description' => 'A falusi kincs',
        'picture' => 'https://hur.webmania.cc/img/cseresznye.jpg',
        'price' => 900,
        'stock' => 300,
    ],
    [
        'category' => 'Magyaros',
        'name' => 'Szilva',
        'description' => 'A falusi kincs',
        'picture' => 'https://hur.webmania.cc/img/szilva.jpg',
        'price' => 770,
        'stock' => 200,
    ],
];

if (Product::count() === 0) {
    Product::factory()->createMany($data);
}

```

## csv fájlból olvassuk fel

database/csv/products.csv

```csv
category;name;description;picture;price;stock
Bogyós;Málna;Kézzel termelt egészség;https://hur.webmania.cc/img/malna.jpg;3800;500
Bogyós;Áfonya;Az erdő kincse az otthonodba;https://hur.webmania.cc/img/afonya.jpg;3250;120
Bogyós;Szeder;A hagyományos csemege;https://hur.webmania.cc/img/szeder.jpg;1700;40
Bogyós;Eper;Egy tavaszi harapás;https://hur.webmania.cc/img/eper.jpg;1440;0
Bogyós;Homoktövis;Mezei csemege;https://hur.webmania.cc/img/homoktovis.jpg;3200;100
Bogyós;Som;A fanyar gyönyör;https://hur.webmania.cc/img/som.jpg;900;10
Bogyós;Fanyarka;Édes mint a méz;https://hur.webmania.cc/img/fanyarka.jpg;990;25
Bogyós;Piszke;Egres;https://hur.webmania.cc/img/piszke.jpg;750;100
Bogyós;Ribizli;Fanyar, vasban gazdag;https://hur.webmania.cc/img/ribizli.jpg;1300;170
Magyaros;Meggy;A falusi kincs;https://hur.webmania.cc/img/meggy.jpg;600;300
Magyaros;Cseresznye;A falusi kincs;https://hur.webmania.cc/img/cseresznye.jpg;900;300
Magyaros;Szilva;A falusi kincs;https://hur.webmania.cc/img/szilva.jpg;770;200
```

database/seeders/ProductSeeder.php

```php
//$data: Felöltés fájlból (database/csv/produts.csv)

// $filePath = database_path(path: 'csv\products.csv');
// $data = [];
// $rows = file($filePath, FILE_IGNORE_NEW_LINES | FILE_SKIP_EMPTY_LINES);
// for ($i = 1; $i < count($rows); $i++) {
//     $cols = explode(';', $rows[$i]);
//     $data[] = [
//         'category' => $cols[0],
//         'name' => $cols[1],
//         'description' => $cols[2],
//         'picture' => $cols[3],
//         'price' => $cols[4],
//         'stock' => $cols[5]
//     ];
// }

 //Profibb megoldás (nagyon nagy fájlok esetén):
if (($handle = fopen($filePath, 'r')) !== false) {
    // 1. Beolvassuk a fejléceket (ha vannak)
    $header = fgetcsv($handle, 0, ';');

    // 2. Soronként beolvassuk az adatokat (0 azt jelenti, hogy nincs korlát a beolvasott sorra)
    while (($cols = fgetcsv($handle, 0, ';')) !== false) {
        if (count($header) === count($cols)) {
            // Asszociatív tömb létrehozása (jobb olvashatóság!)
            $data[] = array_combine($header, $cols);
        }
    }
    // 3. Zárjuk a fájlt (itt kötelező!)
    fclose($handle);
}

if (Product::count() === 0) {
    Product::factory()->createMany($data);
}

```

## Faker könyvtár, factory használatával

A lényeg, hogy létezik egy Faker könyvtár, ami a Laravel része.
Ennek segítségével lehet véletlen neveket, számokat, városokat stb. generálni.
[Mire jó a Faker](https://gemini.google.com/share/05a2d76cd792)

Az egész erre egyszerűsödik:
database/seeders/ProductSeeder.php

```php
//100 véletlen termék generálása
Product::factory()->count(100)->create();
```

database/factories/ProductFactory.php

```php
namespace Database\Factories;

use Illuminate\Database\Eloquent\Factories\Factory;


/**
 * @extends \Illuminate\Database\Eloquent\Factories\Factory<\App\Models\product>
 */
class ProductFactory extends Factory
{
    /**
     * Define the model's default state.
     *
     * @return array<string, mixed>
     */
    protected array $productData = [
        ['category' => 'Bogyós', 'name' => 'Málna', 'description' => 'Kézzel termelt egészség', 'picture' => 'https://hur.webmania.cc/img/malna.jpg'],
        ['category' => 'Bogyós', 'name' => 'Áfonya', 'description' => 'Az erdő kincse az otthonodba', 'picture' => 'https://hur.webmania.cc/img/afonya.jpg'],
        ['category' => 'Bogyós', 'name' => 'Szeder', 'description' => 'A hagyományos csemege', 'picture' => 'https://hur.webmania.cc/img/szeder.jpg'],
        ['category' => 'Bogyós', 'name' => 'Eper', 'description' => 'Egy tavaszi harapás', 'picture' => 'https://hur.webmania.cc/img/eper.jpg'],
        ['category' => 'Bogyós', 'name' => 'Homoktövis', 'description' => 'Mezei csemege', 'picture' => 'https://hur.webmania.cc/img/homoktovis.jpg'],
        ['category' => 'Bogyós', 'name' => 'Som', 'description' => 'A fanyar gyönyör', 'picture' => 'https://hur.webmania.cc/img/som.jpg'],
        ['category' => 'Bogyós', 'name' => 'Fanyarka', 'description' => 'Édes mint a méz', 'picture' => 'https://hur.webmania.cc/img/fanyarka.jpg'],
        ['category' => 'Bogyós', 'name' => 'Piszke', 'description' => 'Egres', 'picture' => 'https://hur.webmania.cc/img/piszke.jpg'],
        ['category' => 'Bogyós', 'name' => 'Ribizli', 'description' => 'Fanyar, vasban gazdag', 'picture' => 'https://hur.webmania.cc/img/ribizli.jpg'],
        ['category' => 'Magyaros', 'name' => 'Meggy', 'description' => 'A falusi kincs', 'picture' => 'https://hur.webmania.cc/img/meggy.jpg'],
        ['category' => 'Magyaros', 'name' => 'Cseresznye', 'description' => 'A falusi kincs', 'picture' => 'https://hur.webmania.cc/img/cseresznye.jpg'],
        ['category' => 'Magyaros', 'name' => 'Szilva', 'description' => 'A falusi kincs', 'picture' => 'https://hur.webmania.cc/img/szilva.jpg'],
    ];


    //A nyelv megadás
    //Ez a függvény a $this->faker első meghívásakor hívódik meg egyszer
    protected function withFaker()
    {
        // Manuális beállítás az app config felülírására
        return \Faker\Factory::create('hu_HU');
    }

    //Ez hívódik meg 100-szor és a visszadott asszociatív tömbbel generálja az adatokat
    public function definition(): array
    {

        $randomProduct = $this->faker->randomElement($this->productData);
        //A gyümölcs nevét keverjük egy nem ismétlődő véletlen számmal.
        $randomUniqueNumber = $this->faker->unique()->randomNumber(5, true);
        $randomName = $randomProduct['name'].' #'.$randomUniqueNumber;
        // ...
        return [
            'category' => $randomProduct['category'],

            // Ha nem akarsz fix nevet, használhatsz véletlent (ritka a terméknél, de a példa kedvéért)
            // 'name' => $this->faker->lastName() . ' ' . $this->faker->firstName(),
            // 'name' => $this->faker->unique()->name,
            'name' => $randomName,
            'description' => $this->faker->sentence(), //véletlenszerű lorem mondat
            'picture' => $this->faker->imageUrl(),


            // ...
            'price' => $this->faker->numberBetween(500, 5000),
            'stock' => $this->faker->numberBetween(0, 1000),
        ];
    }
}
```

# CRUD

## Endpointok

routes/api.php

```php
Route::get('products', [ProductController::class, 'index']);
Route::get('products/{id}', [ProductController::class, 'show']);
Route::post('products', [ProductController::class, 'store']);
Route::patch('products/{id}', [ProductController::class, 'update']);
Route::delete('products/{id}', [ProductController::class, 'destroy']);
```

## Lekérdezések futtatása

Laravel-ben a kontrollerekben három fő módon futtathatsz SQL lekérdezéseket:

- **Eloquent ORM** (Object-Relational Mapper): A leggyakoribb és ajánlottabb módszer. Objektumokat használ az adatbázis-táblák reprezentálására.

- **Query Builder**: A nyers SQL-hez legközelebb álló, de mégis PHP szintaktikát használó interfész.

- **Nyers SQL** (Raw SQL): Közvetlenül írhatsz SQL parancsokat.

### Eloquent ORM

1. Eloquent ORM (Ajánlott)
   Az Eloquent az adatok kezelésének Laravel-es módja. Feltételezi, hogy létrehoztad a megfelelő modellt a tábládhoz (pl. App\Models\Termek a termek táblához).

- Összes termék: Termek::all();
- Lekérdezés feltétellel: Termek::where('ar', '>', 5000)->get();
- Egy elem: Termek::find(1);
- Új tarmék: Termek::create(['megnevezes' => 'Laptop']);

### Query Builder

A Query Builder-t akkor használd, ha az Eloquent modellezés nem szükséges (pl. aggregáció, összetett illesztések), de nem akarsz nyers SQL-t írni.

- Ehhez a DB Facade-ot (vagy a `use Illuminate\Support\Facades\DB`-t) kell használni.

- MűveletQuery Builder KódÖsszes lekérdezése: DB::table('termek')->get();
- Lekérdezés feltételekkel: DB::table('termek')->where('ar', 50000)->first();
- Új bejegyzés: DB::table('termek')->insert(['megnevezes' => 'Monitor']);
- Csoportosítás (GROUP BY): DB::table('termek')->select('darab')->groupBy('darab')->get();

### Nyers SQL

A Nyers SQL-t csak akkor használd, ha nincs más megoldás, mivel fennáll az SQL Injection veszélye, ha nem paraméterezed megfelelően.

- Ehhez a DB Facade-ot (vagy a `use Illuminate\Support\Facades\DB`-t) kell használni.

- MűveletNyers SQL KódLekérdezés (SELECT): DB::select('SELECT \* FROM users WHERE active = ?', [1]);
- Beszúrás (INSERT): DB::insert('INSERT INTO users (id, name) VALUES (?, ?)', [1, 'Péter']);
- Módosítás (UPDATE/DELETE): DB::update('UPDATE users SET name = "Kata" WHERE id = ?', [2]);

## Kontrollerek

app/Http/Controllers

```php

```

# Cors kezelés

[cors (gemini)](https://g.co/gemini/share/477e6307e707)
A CORS egy olyan biztonsági mechanizmus, amely lehetővé teszi, hogy különböző domain-ekről származó weboldalak egymás erőforrásait hozzáférjék. Ez azért fontos, mert a böngészők alapértelmezett beállítása szerint csak az azonos domainről származó kéréseket engedélyezik.
Ha ezt nem állítanánk be, a kliens oldal nem férne hozzá az API erőforrásaihoz.
A cors beállítások a http kérésre adott válasz fejlécében érkeznek, és ennek alapján befolyásolja a böngésző működését.

- A cors beállítás létrehozása: `php artisan config:publish cors`
- Létrejön egy **app/config/cors.php** fájl. Itt állíthatjuk be, hogy milyen **metódusokat**, illetve milyen **doménról** szolgálhat ki az API

```php
return [
    'paths' => ['api/*', 'sanctum/csrf-cookie'],

    // 'allowed_methods' => ['*'],
    'allowed_methods' => ['OPTIONS','GET','POST','PATCH','DELETE'],

    //*: Ez a beállítás engedélyezi a CORS-t bármely domainről érkező kérésekhez.
    //A valóságban itt egy adott (egy-vagy több) domaint engedélyezünk.
    'allowed_origins' => ['*'],

    'allowed_origins_patterns' => [],

    //Mely fejlécek engedélyezettek a kérésekben
    'allowed_headers' => ['*'],

    'exposed_headers' => [],

    'max_age' => 0,

    'supports_credentials' => false,
];
```

# Helper fájlok

Elhelyezés: app/Helpers mappában.
Használat: A helpers.php fájlban definiált függvényeket bárhonnan meghívhatod az alkalmazásban.
Regisztrálás: A config/app.php fájlban az aliases tömbben kell regisztrálni a helper fájlt.

# Hitelesítés

[Konroller létrehozás](https://gemini.google.com/share/a9ee1e38a913)

## Users controller

Konroller készítő parancsok:

- `php artisan make:controller UserController`
  - app/Http/Controllers/UserController.php (nem hozza létre a metódusokat)
- `php artisan make:controller UserController --resource`
  - app/Http/Controllers/UserController.php és létrehozza a metódusokat (**--resource**)
- `php artisan make:controller UserController --resource --model=User --requests`
  - app/Http/Controllers/UserController.php (**make:controller**)
  - index, create stb metódusok (**--resource**)
  - Automatikusan "befűzi" (type-hinteli) a megadott modellt a vezérlő metódusaiba (**--model=User**)
    -- app/Http/Requests/StoreUserRequest.php, UpdateUserRequest.php (**--requests**)
- `php artisan make:model User -mcr --requests`
  - m: Létrehozza a migrációt (migration).
  - c: Létrehozza a kontrollert (controller).
  - r: A kontrollert resource (erőforrás) stílusban hozza létre.
  - --requests: Létrehozza a **StoreUserRequest** és **UpdateUserRequest** validációs osztályokat is!

1. User Conroller, valamint Request osztályok létrehozása:

- `php artisan make:controller UserController --resource --model=User --requests`
  - app/Http/Controllers/UserController.php (kontoller az összes CRUD függvénnyel)
  - app/Http/Request/StoreUserRequest.php (Store Request osztály: POST szabályokhoz)
  - app/Http/Request/UpdateUserRequest.php (Update Request osztály: PATCH szabályokhoz)

2. A Login POST szabályaihoz hozzunk létre egy **LoginUserRequest** nevű Request osztályt:

- `php artisan make:request LoginUserRequest`
  - app/Http/Request/LoginUserRequest.php

3. Írjuk meg a **LoginUserRequest.php** kódját

LoginUserRequest.php

```php
public function authorize(): bool
{
    //Bárki használhatja
    return true;
}

public function rules(): array
{
    // A login egy POST művelet, és kötelező ezeket megadni a bejelentkezéskor
    return [
        'email' => 'required|email',
        'password' => 'required',
    ];
}
```

4. Írjuk meg a user-hez tartozó összes request.rest parancsot

request.rest

```rest
### login
# @name login
POST {{host}}/api/users/login
Accept: application/json
Content-Type: application/json

{
    "email": "test@example.com",
    "password": "123"
}

###
@token = {{login.response.body.data.token}}

### logout user
POST  {{host}}/api/users/logout
Accept: application/json
Authorization: Bearer {{token}}


### get users
GET  {{host}}/api/users
Accept: application/json
Authorization: Bearer {{token}}

### get user by id
GET  {{host}}/api/users/4
Accept: application/json
Authorization: Bearer {{token}}

### post user
POST {{host}}/api/users
Content-Type: application/json
Accept: application/json
Authorization: Bearer {{token}}

{
    "name":  "test2",
    "email": "test2@example.com",
    "password": "123"
}

### patch user
PATCH {{host}}/api/users/5
Content-Type: application/json
Accept: application/json
Authorization: Bearer {{token}}

{
    "password": "1234"
}

### delete user
DELETE {{host}}/api/users/4
Content-Type: application/json
Accept: application/json
Authorization: Bearer {{token}}
```

5. Login elkészítése

### Token

**UsersControllers.php**

```php
public function login(LoginUsersRequest $request)
{
    //Eltároljuk az adatokat változókba
    $email = $request->input(('email'));
    $password = $request->input(('password'));

    //Az email alapján megkeressük a usert
    $user = User::where('email', $email)->first();

    //Stimmel-e az email és a jelszó?
    if (!$user || !Hash::check($password, $password ? $user->password : '')) {
        return response()->json([
            'message' => 'invalid email or password'
        ], 401);
    }

    //Jó az email és a jelszó
    //Kitöröljük az esetleges tokenjeit
    //$user->tokens()->delete();

    //itt adjuk az új tokent időkorlát nélkül
    //$user->token = $user->createToken('access')->plainTextToken;

    //Lejárati idővel
    // $expirationTime = Carbon::now()->addSeconds(10);
    // $name ="10sec";
    // $expirationTime = Carbon::now()->addMinutes(30);
    // $name ="30min";
    // $expirationTime = Carbon::now()->addHours(4);;
    // $name ="4hours";
    $expirationTime = Carbon::now()->addDays(1);
    $name ="1day";
    $abilities = ['*'];

    $user->token = $user->createToken(
        $name,
        $abilities,
        $expirationTime
    )->plainTextToken;

    //visszaadjuk a usert, ami a tokent is tartalmazni fogja
     $data = [
            'message' => 'ok',
            'data' => $user
        ];
        $status = 200;

        //visszaadjuk a usert, ami a tokent is tartalmazni fogja
        return response()->json($data, $status, options: JSON_UNESCAPED_UNICODE);
}
```

- Mi a token

  - A Laravel Sanctum által generált token nem JWT (JSON Web Token), hanem egy adatbázis-alapú token, ami szorosan egy adott felhasználóhoz van rendelve.
  - personal_access_tokens táblában van

- Lejárati idő:

  - Ha nem adjuk meg, akkor bármeddig felhasználható
  - Magas Biztonság Rövid Munkamenet API-k, Pénzügyi Funkciók 15 perctől 1 óráig
  - Egyensúlyozott Általános Webes SPA, API 1 naptól 7 napig A leggyakoribb beállítás.
  - Kényelem, Hosszú Munkamenet, Mobilalkalmazások, Dedikált Kliensek 30 naptól 1 évig

- Lejárt tokenek törlése artisan paranccsal (A megadott óránál kisebblejáratúakat törli):
  `php artisan sanctum:prune-expired --hours=0`

- Adott user törlése artisan paranccsal (Tinker):
  `php artisan tinker`
  `$user = App\Models\User::find(1);`
  `$user->tokens()->delete();`
  `exit`

- Lejárt tokenek automatikus törlése

6. User model kiegészítése

```php
namespace App\Models;

// use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    /** @use HasFactory<\Database\Factories\UserFactory> */
    use HasApiTokens, HasFactory, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var list<string>
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var list<string>
     */
    protected $hidden = [
        'password',
        'remember_token',
        'email_verified_at',
        'created_at',
        'updated_at',
    ];

    /**
     * Get the attributes that should be cast.
     *
     * @return array<string, string>
     */
    protected function casts(): array
    {
        return [
            'email_verified_at' => 'datetime',
            'password' => 'hashed',
        ];
    }
}

```

6. Endpointok elkészítése

```php
//region users
Route::post('users/login', [UserController::class, 'login']);
Route::post('users/logout', [UserController::class, 'logout']);
Route::get('users', [UserController::class, 'index'])
    ->middleware('auth:sanctum');

Route::get('users/{id}', [UserController::class, 'show'])
    ->middleware('auth:sanctum');
Route::post('users', [UserController::class, 'store']);

Route::patch('users/{id}', [UserController::class, 'update'])
    ->middleware('auth:sanctum');
Route::delete('users/{id}', [UserController::class, 'destroy'])
    ->middleware('auth:sanctum');
//endregion

```

### Logout

```php
public function logout(Request $request)
{
    // Megkeresi a tokent és törli ---------------------
    $token = $request->bearerToken(); // Kivonjuk a bearer tokent a kérésből

    // Megkeressük a token modellt
    $personalAccessToken = PersonalAccessToken::findToken($token);

    if ($personalAccessToken) {
        $personalAccessToken->delete();
        return response()->json(['message' => 'Successfully logged out']);
    } else {
        return response()->json(['message' => 'Token not found'], 404);
    }
}
```

### User crud

```php
//Visszaadja a usereket
public function index()
{
   try {
            //code...
            $rows = User::all();
            $status = 200;
            $data = [
                'message' => 'OK',
                'data' => $rows
            ];
        } catch (\Exception $e) {
            //throw $th;
            $status = 500;
            $data = [
                'message' => "Server error {$e->getCode()}",
                'data' => $rows
            ];
        }

        return response()->json($data, $status, options: JSON_UNESCAPED_UNICODE);
}

public function store(StoreUsersRequest $request)
{
    try {
            $row = User::create($request->all());

            $data = [
                'message' => 'ok',
                'data' => $row
            ];
            // Sikeres válasz: 201 Created kód ajánlott új erőforrás létrehozásakor
            return response()->json($data, 201, options: JSON_UNESCAPED_UNICODE);
        } catch (QueryException $e) {
            // Ellenőrizzük, hogy ez egy "Duplicate entry for key" hiba-e (MySQL hibakód: 23000 vagy 1062)
            if ($e->getCode() == 23000 || str_contains($e->getMessage(), 'Duplicate entry')) {
                $data = [
                    'message' => 'Insert error: The given name already exists, please choose another one',
                    'data' => [
                        'name' => $request->input('name') // Visszaküldhetjük, mi volt a hibás
                    ]
                ];
                // Kliens hiba, ami jelzi a kérés érvénytelenségét
                return response()->json($data, 409, options: JSON_UNESCAPED_UNICODE); // 409 Conflict ajánlott
            }
            // Ha nem ez a hiba volt, dobjuk tovább az eredeti kivételt, vagy kezeljük másképp
            throw $e;
        }
}

public function show(int $id)
{
    $row = User::find($id);
    if ($row) {
        # code...
        $status = 200;
        $data = [
            'message' => 'OK',
            'data' => $row
        ];
    } else {
        # code...
        $status = 404;
        $data = [
            'message' => "Not found id: $id",
            'data' => null
        ];
    }

    return response()->json($data, $status, options: JSON_UNESCAPED_UNICODE);
}

public function update(UpdateUsersRequest $request,  $id)
{
    $row = User::find($id);
    if ($row) {
        # code...
        $status = 200;
        $row->update($request->all());

        $data = [
            'message' => 'OK',
            'data' => [
                'data' => $row
            ]
        ];
    } else {
        # code...
        $status = 404;
        $data = [
            'message' => "Patch error. Not found id: $id",
            'data' => $id
        ];
    }
    return response()->json($data, $status, options: JSON_UNESCAPED_UNICODE);
}

public function destroy(int $id)
{
    $row = User::find($id);
    if ($row) {
        # code...
        $status = 200;
        $row->delete();

        $data = [
            'message' => 'OK',
            'data' => [
                'id' => $id
            ]
        ];
    } else {
        # code...
        $status = 404;
        $data = [
            'message' => "Delete error. Not found id: $id",
            'data' => null
        ];
    }
    return response()->json($data, $status, options: JSON_UNESCAPED_UNICODE);
}
```

## User validátorok

**Requests/LoginUsersRequest.php**

```php
public function rules(): array
{
    return [
        'email' => 'required|email',
        'password' => 'required',
    ];
}
```

**Requests/StoreUsersRequest.php**

```php
public function rules(): array
{
    return [
        'name' => 'required|string',
        'email' => 'required|email',
        'password' => 'required',
    ];
}
```

**Requests/UpdateUsersRequest.php**

```php
public function rules(): array
{
    return [
        'name' => 'nullable|string',
        'email' => 'nullable|email',
        'password' => 'nullable',
    ];
}
```

## Users endpoint

**route/api.php**

```php
Route::post('users/login', [UsersController::class, 'login']);
Route::post('users/logout', [UsersController::class, 'logout']);
Route::get('users', [UsersController::class, 'index'])
    ->middleware('auth:sanctum');
Route::get('users/{id}', [UsersController::class, 'show'])
    ->middleware('auth:sanctum');
Route::post('users', [UsersController::class, 'store'])
    ->middleware('auth:sanctum');
Route::patch('users/{id}', [UsersController::class, 'update'])
    ->middleware('auth:sanctum');
Route::delete('users/{id}', [UsersController::class, 'destroy'])
    ->middleware('auth:sanctum');
```

## Token élettatam beállítás egységesen

Ezt csak akkor édemes, ha mindeninek egységesen azt akarjuk adni
**app/config/sanctum.php**

```php
'expiration' => null,
//Token élettartam percben
//'expiration' => 1,

```

# Role (szerepkör) hitelesítés

## User tábla bővítés, szerepkörök

1. vegyünk fel egy role nevű új mezőt a user táblába

- Tervezett szerepkörök:

  - 1: admin
  - 2: raktáros
  - 3: vásárló

- `php artisan make:migration add_role_to_users_table --table=users`

2025_11_17_164436_add_role_to_users_table.php

```php
public function up(): void
{
    Schema::table('users', function (Blueprint $table) {
        // role mező hozzáadása: integer, alapértelmezett értéke 3
        $table->integer('role')->default(3)->after('email');
    });
}

/**
 * Reverse the migrations.
 */
public function down(): void
{
    Schema::table('users', function (Blueprint $table) {
        // A role mező eltávolítása visszavonáskor
        $table->dropColumn('role');
    });
}
```

2. Migráció futtatása

- `php artisan migrate`

3. Módosítsuk a seeder-t és hozzunk létre 3 különböző szerepű felhasználót

database/seeders/UserSeeder.php

```php
public function run(): void
{
    //
    User::factory()->create([
        'name' => 'Admin',
        'email' => 'admin@example.com',
        'password' => '123',
        'role' => 1,
    ]);
    User::factory()->create([
        'name' => 'Raktáros',
        'email' => 'raktaros@example.com',
        'password' => '123',
        'role' => 2,
    ]);
    User::factory()->create([
        'name' => 'Vásárló1',
        'email' => 'vasarlo1@example.com',
        'password' => '123',
        'role' => 3,
    ]);
}
```

4. Futtassuk a Seedert-t
   `php artisan migrate:fresh --seed` (minden alaphelyzetbe)
   vagy
   `php artisan db:seed` (minden maradjon)

## Role terv

- 1: admin
  minden művelet
- 2: raktáros
  user: login, logout
  products: read, create, update, delete
- 3: vásárló
  user: login, logout
  products: read

## Role kiosztás megvalósítása

- A role kiosztást a token abilities paraméterében adjuk meg
- Ez a tokenben tárolódik

app/Http/Controllers/UserController.php

```php
public function login(LoginUserRequest $request)
{
 //...
    $expirationTime = Carbon::now()->addDays(1);
    $role = $user->role;
    $name = "1day-role:$role";
    switch ($role) {
        case 1:
            //Admin
            $abilities = ['*'];
            break;
        case 2:
            //Raktáros
            $abilities = [
                'products:create',
                'products:delete',
                'products:update',
            ];
            break;
        default:
            //Vásárló
            $abilities = [
            ];
            break;
    }


    $user->token = $user->createToken(
        $name,
        $abilities,
        $expirationTime
    )->plainTextToken;
 //...
}
```

Token képzés

- A token egy fix méretű egyedi véletleszerű sztring
- Az abilities nem benne tárolódik, hanem
- a **personal_access_tokens** tábla abilities (TEXT típusú) mezőjében
- string formátumban.
- pl.: ['products:create','products:delete','products:update']
- Az abilities kifejezés lehet nagyon hosszú is

Sanctum Logika:

- A Laravel Sanctum CheckAbilities middleware-je ellenőrzi, hogy
- az érkező token rendelkezik-e a megadott képességgel (course:create).
- A Rendszergazda tokenje tartalmazza a \* képességet, tehát mindent csinálhat.

Abilities ellenőrzés működése:

- Amikor a Laravel találkozik az 'ability:course:delete' karaktersorozattal, azt kettéosztja:
- Middleware Alias: ability (ami a CheckAbilities::class osztályra mutat).
- Paraméter: products:delete (ez az, amit átad a CheckAbilities osztály handle metódusának).
- A CheckAbilities middleware ezután megvizsgálja a bejelentkezett felhasználó tokenjét és megnézi, hogy az átadott paraméter (products:delete) szerepel-e a tokenjéhez rendelt képességek listájában.

## Jogosultság kiosztás

- Adott user token kiosztásnál az abilities-ben felsoroltuk, hogy melyik táblával mit csinálhat: **táblanév:művelet** formában.

  - Raktáros: $abilities = ['products:create','products:delete','products:update'];

- Az endpointoknál (ahol korlátozás van) az van megnevezve, hogy ő mit csinál
  - 'ability:táblanév:művelet' formában.
- Amit mindenki csinálhat nem korlátozunk
- A rendszergazda ['*'] ability-je pedig mindenre jogosít

routes/api.php

```php
//Mindenki
Route::get('products', [ProductController::class, 'index']);
Route::get('products/{id}', [ProductController::class, 'show']);

//Admin és Raktáros
Route::post('products', [ProductController::class, 'store'])
    ->middleware('auth:sanctum', 'ability:products:create');
Route::delete('products/{id}', [ProductController::class, 'destroy'])
    ->middleware('auth:sanctum', 'ability:products:delete');
Route::patch('products/{id}', [ProductController::class, 'update'])
    ->middleware('auth:sanctum', 'ability:products:update');
```

## Middleware regisztráció

Hohoz hogy a sanctum felismerje az ability: bejegyzésket, regisztrálni kell.
app/Providers/AppServiceProvider.php

```php
namespace App\Providers;

use Illuminate\Support\Facades\Schema;
use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Response;
use Illuminate\Support\Facades\Route; // <--- EZT KELL HOZZÁADNI
use Laravel\Sanctum\Http\Middleware\CheckAbilities; // <--- EZT KELL HOZZÁADNI
use Symfony\Component\HttpKernel\Exception\AccessDeniedHttpException;
use Illuminate\Support\Facades\Exceptions;

class AppServiceProvider extends ServiceProvider
{
    //...

    public function boot(): void
    {
        // Az alapértelmezett string hossza 191 karakterre csökkentése
        Schema::defaultStringLength(191);
        //Middleware regisztráció
        Route::aliasMiddleware('ability', CheckAbilities::class);

        // 2. KIVÉTELKEZELÉS REGISZTRÁCIÓJA
        Exceptions::renderable(function (AccessDeniedHttpException $e, $request) {
            $message = $e->getMessage() ?? 'Access denied.';
            // Csak API kérésekre fusson le
            if ($request->is('api/*')) {
                return response()->json([
                    'message' => $message
                ], 403);
            }
        });
    }
}
```

# User Profil konrollerek

Olyan user kezelést kell megvalósítani hogy csak a bejelentkezett user:

- Módosíthasa saját jelszavát, profil adatait
- Törölhesse magát
- Módosíthassa a profil adatait: user név, email
  A rendszergazda ne tudja saját role-ját megváltoztatni.

1. Hozzunk létre egy User policy-t:
   `php artisan make:policy UserPolicy --model=User`

app/Policies/UserPolicy.php

```php
//...
public function view(User $user, User $model): Response
{
    if ($user->id !== $model->id) {
        return Response::deny('Csak a saját profilodat nézheted.');
    }
    return Response::allow();
}

public function update(User $user, User $model): Response
{
    // Először ellenőrizzük, hogy a bejelentkezett felhasználó azonos-e a módosítandóval.
    if ($user->id !== $model->id) {
        // Ha nem önmódosítás, és nem adminról van szó (az adminra vonatkozik a before() metódus),
        // megtagadjuk. De a before() miatt ez a rész főleg a nem-adminokra érvényes.
        if ($user->role !== 1) {
            return Response::deny('Csak a saját profilodat módosíthatod.');
        }
    }

    // 3. ADMIN SPECIÁLIS SZABÁLY: Megnézzük, próbál-e a user a saját 'role' mezőjén módosítani.
    // Ezt az ellenőrzést csak akkor végezzük el, ha valóban az admin magát módosítja.
    if ($user->role === 1 && $user->id === $model->id) {

        // Ha az admin megpróbálja a bemeneti adatokkal megváltoztatni a role mezőt:
        $request = request();
        if ($request->has('role') && (int)$request->input('role') !== $user->role) {
            return Response::deny('Admin: Nem módosíthatod a saját szerepkörödet.');
        }
    }

    // Ha minden ellenőrzésen átment (önmódosítás, és nem sérti az admin korlátozásokat).
    return Response::allow();

}

public function delete(User $user, User $model): Response
{
    // Csak akkor engedélyezzük a törlést, ha a bejelentkezett felhasználó azonos a törlendővel.

    return $user->id === $model->id
        ? Response::allow()
        : Response::deny('Csak a saját profilodat törölheted.');
}

//...
//Az admin önmaga kizárása elleni Policy rutinok:
 //Az admin nem tudja saját role-ját megváltoztatni
public function updateAdmin(User $user, User $model): Response
{

    // Amin  aját 'role' mezőjét nem módosíthatja.
    if ($user->role === 1 && $user->id === $model->id) {

        // Ha az admin megpróbálja a bemeneti adatokkal megváltoztatni a role mezőt:
        $request = request();
        if ($request->has('role') && (int)$request->input('role') !== $user->role) {
            return Response::deny('Admin: Nem módosíthatod a saját szerepkörödet.');
        }
    }

    // Ha minden ellenőrzésen átment (önmódosítás, és nem sérti az admin korlátozásokat).
    return Response::allow();
}

//Az Admin ne tudja önmagát letörölni
public function deleteAdmin(User $user, User $model): Response
{

    // Öntörlés kizárva
    return $user->id !== $model->id
        ? Response::allow()
        : Response::deny('Csak a saját profilodat törölheted.');
}

```

## User kontroller kiegészítések
```php
public function update(UpdateUserRequest $request, int $id)
{
    $row = User::find($id);
    
    if ($row) {
        # code...
        $status = 200;
        //Szabd-e ezt nekem?
        $userToUpdate = $row;
        $this->authorize('updateAdmin', $userToUpdate);

        $row->update($request->all());
        //...
    }
    //...
}      

public function destroy(int $id)
{
    $row = User::find($id);
    if ($row) {
        # code...
        $status = 200;
        //Az admin önmagát nem törölheti
        $userToDestroy = $row;
        $this->authorize('deleteAdmin', $userToDestroy);
        $row->delete();
        //...
    }
    //...
}

```

## Policy regisztráció

Ahhoz hogy a konrollerekben műkötethessük a policy hvásokat, regisztrálni kell Policy-t

app/Providers/AppServiceProvider.php

```php
// ...

public function boot(): void
{
    //...

    // Policy Regisztráció
    Gate::policy(User::class, UserPolicy::class);
}
```

## Role változtatás kizárás
Saját magunk role-ját ne tudjuk megváltoztatni

1. készítsünk erre egy Request-et
   `php artisan make:request UpdateUserSelfRequest`

2. Zárjuk ki a role mezőt

Http\Requests\UpdateUserSelfRequest.php

```php
public function rules(): array
{
    return [
        'name' => 'nullable|string',
        'email' => 'nullable|email',
        'password' => 'nullable|string',
        // Tiltott mező: Ha a role mező megérkezik a kérésben, a validáció elbukik.
        'role' => 'prohibited',
    ];
}
```

3. A UserControllerben a updateSelf(UpdateUserSelfRequest $request) paraméterbe tegyük be, hogy updateSelf esetén átfusson s rendszer a szabályon

app/Http/Controllers/UserController.php

```php
    public function destroySelf(Request $request)
    {
        //Kivesszük a törlendő user-t
        $userToDestroy = $request->user();
        // A Policy-t használjuk:
        $this->authorize('delete', $userToDestroy);
        // ... törlés logika
        //A user tokenjeinek törlése
        $userToDestroy->tokens()->delete();
        //A user törlése
        $userToDestroy->delete();

        $status = 404;
        $data = [
            'message' => "Sikeresen törölted a fiókodat",
            'data' => null
        ];
        return response()->json($data, $status, options: JSON_UNESCAPED_UNICODE);
    }


public function updateSelf(UpdateUserSelfRequest $request)
{

   //Kivesszük a módosítandó user-t
    $userToDestroy = $request->user();
    // A Policy-t használjuk:
    $this->authorize('update', $userToDestroy);

    $status = 200;
    $userToDestroy->update($request->all());

    $data = [
        'message' => 'OK',
        'data' => [
            'data' => $userToDestroy
        ]
    ];

    return response()->json($data, $status, options: JSON_UNESCAPED_UNICODE);
}

public function indexSelf(Request $request)
{

    //Kivesszük a megmutatandó usert
    $userToDestroy = $request->user();
    // A Policy-t használjuk:
    $this->authorize('view', $userToDestroy);

    $status = 200;
    $data = [
        'message' => 'OK',
        'data' => [
            'data' => $userToDestroy
        ]
    ];

    return response()->json($data, $status, options: JSON_UNESCAPED_UNICODE);
}



```

## User route beállítások

- Bejelentkezés nélkül: login, logout, post user (regisztrálás)
- Admin: get users, get users/id, patch users/id, delete users/id (bármelyik userrel bármit)
  - kivéve saját role-ját
  - Admin nem törölheti önmagát
- Raktáros, Vásárló: get usersme, delete usersme, patch usersme
  - role módosítás tiltva

route/api.php
```php
//region users
//User kezelés, login, logout
//Mindenki
Route::post('users/login', [UserController::class, 'login']);
Route::post('users/logout', [UserController::class, 'logout']);
Route::post('users', [UserController::class, 'store']);

//Admin: 
//minden user lekérdezése
Route::get('users', [UserController::class, 'index'])
    ->middleware('auth:sanctum', 'ability:admin');
//Egy user lekérése    
Route::get('users/{id}', [UserController::class, 'show'])
    ->middleware('auth:sanctum', 'ability:admin');
//User adatok módosítása      
Route::patch('users/{id}', [UserController::class, 'update'])
->middleware('auth:sanctum', 'ability:admin');
//User törlés
Route::delete('users/{id}', [UserController::class, 'destroy'])
->middleware('auth:sanctum', 'ability:admin');  

//User self (Amit a user önmagával csinálhat) parancsok
Route::delete('usersme', [UserController::class, 'destroySelf'])
->middleware('auth:sanctum', 'ability:usersme:delete');

Route::patch('usersme', [UserController::class, 'updateSelf'])
->middleware('auth:sanctum', 'ability:usersme:patch');

Route::get('usersme', [UserController::class, 'indexSelf'])
    ->middleware('auth:sanctum', 'ability:usersme:get'); 
//endregion

//region products
//Mindenki
Route::get('products', [ProductController::class, 'index']);
Route::get('products/{id}', [ProductController::class, 'show']);

//Admin és Raktáros
Route::post('products', [ProductController::class, 'store'])
    ->middleware('auth:sanctum', 'ability:products:create');
Route::delete('products/{id}', [ProductController::class, 'destroy'])
    ->middleware('auth:sanctum', 'ability:products:delete');
Route::patch('products/{id}', [ProductController::class, 'update'])
    ->middleware('auth:sanctum', 'ability:products:update');
//endregion    
```

## Token és ability kiosztás

app/Http/Controllers/UserController.php
```php
public function login(LoginUserRequest $request)
{
    //Eltároljuk az adatokat változókba
    $email = $request->input(('email'));
    $password = $request->input(('password'));

    //Az email alapján megkeressük a usert
    $user = User::where('email', $email)->first();

    //Stimmel-e az email és a jelszó?
    if (!$user || !Hash::check($password, $password ? $user->password : '')) {
        return response()->json([
            'message' => 'invalid email or password'
        ], 401);
    }

    $expirationTime = Carbon::now()->addDays(1);
    $role = $user->role;
    $name = "1day-role:$role";
    switch ($role) {
        case 1:
            //Admin
            $abilities = ['*'];
            break;
        case 2:
            //Raktáros
            $abilities = [
                'usersme:delete',
                'usersme:patch',
                'usersme:get',
                'products:create',
                'products:delete',
                'products:update',
            ];
            break;
        default:
            //Vásárló
            $abilities = [
                'usersme:delete',
                'usersme:patch',
                'usersme:get',
            ];
            break;
    }
    //Token és ability kiadás (abiliy: personal_access_tokens tába ability mezőjébe kerül)
    $user->token = $user->createToken(
        $name,
        $abilities,
        $expirationTime
    )->plainTextToken;

    //visszaadjuk a usert, ami a tokent is tartalmazni fogja
    $data = [
        'message' => 'ok',
        'data' => $user
    ];
    $status = 200;

    return response()->json($data, $status, options: JSON_UNESCAPED_UNICODE);
}

```

## userme pingek

request.rest

```rest
### User csak önmagát törölheti
DELETE {{host}}/api/usersme
Content-Type: application/json
Accept: application/json
Authorization: Bearer {{token}}

### A bejelentkezett user a jelszavát módosítja
PATCH {{host}}/api/usersme
Content-Type: application/json
Accept: application/json
Authorization: Bearer {{token}}

{
    "password": "1234"
}


### A bejelentkezett user egy adatát módosítja. A role tiltott
PATCH {{host}}/api/usersme
Content-Type: application/json
Accept: application/json
Authorization: Bearer {{token}}

{
    "name": "TEST",
    "role": 1
}

### A bejelentkezett user önmagát lekérheti
GET  {{host}}/api/usersme
Accept: application/json
Authorization: Bearer {{token}}
```

## Erős jelszó validálás

[Kiszivárgott jelszavak ellenőrzése](https://haveibeenpwned.com/Passwords)

[Gemini beszélgetés](https://gemini.google.com/share/8449c8e12d2c)

app/Http/Requests/UpdateUserSelfRequest.php

```php
 public function rules(): array
    {
        return [
            'name' => 'nullable|string',
            'email' => 'nullable|email',
            'password' => [
                'nullable',
                'string',
                Password::min(10) // Minimum 10 karakter
                    ->mixedCase() // Kevert kis- és nagybetű
                    ->letters()   // Legalább egy betű
                    ->numbers()   // Legalább egy szám
                    ->symbols()   // Legalább egy szimbólum
                    ->uncompromised(), // Ne legyen kiszivárgott
            ],
            // Tiltott mező: Ha a role mező megérkezik a kérésben, a validáció elbukik.
            'role' => 'prohibited',
        ];

    }
```

[profibb user kezelés: törlés és módosítás](https://gemini.google.com/share/35bee94261ae)

# Ütemezés

# Tinker

A tinker lhetővé teszik hogy prancssoról adjunk ki php parancsokat.
Példa:

- Adott user törlése artisan paranccsal (Tinker):
  `php artisan tinker`
  `$user = App\Models\User::find(1);`
  `$user->tokens()->delete();`
  `exit`

# Egy tábla eltávolítása

Néha fontos, hogy egy feleslegessé vált táblát eltávolítsunk a rendszerből.
Nincs egyetlen olyan beépített Artisan parancs, mint a php artisan make:model Product -a --api (ami létrehoz mindent), amelyik automatikusan visszavonja az összes létrehozott fájlt (Modell, Controller, Migration, Factory, Seeder)

Az eltávolítás lépései:

1. Vegyük ki a táblára való hivatkozásokat:

- Endpointok: **routes/api.ph**p fájlból tröljük az endpointokat
- Seeders: **database/seeders/DatabaseSeeder.php**: Tisztítsuk ki belőle

2. Kézzel egyenként töröljük le a tábla kezeléséhez létrehozott osztályokat:

- migrations\2025_11_01_191501_create_products_table.php
- app\Models\Product.php
- seeders\ProductSeeder.php
- database\factories\ProductFactory.php
- app\Http\Controllers\ProductController.php
- app\Http\Requests\StoreproductRequest.php
- app\Http\Requests\UpdateproductRequest.php
- app\Policies\ProductPolicy.php

3. Hozzunk létre egy tábla törlő módosító migrációt

- Készísünk egy tábla törlő migrációt:
  - `php artisan make:migration delete_produscts_table --table=products`
- Készítsük el törlés és az esetleges visszaállítás kódját (opcionális)

```php
public function up(): void
    {
        //Ha voltak kapcsolatai, akkor azokat törölni kell
        //Ebben a példában a products volt a több oldalon, és a valami táblához kapcsolódott
        //valami_id idegen kulccsal
        Schema::table('products', function (Blueprint $table) {
            // Feltételezve, hogy a kapcsolat neve product_valami_id_foreign
            $table->dropForeign('product_valami_id_foreign');
            //Ezután törölheted az oszlopot is, ha már nincs rá szükség
            $table->dropColumn('valami_id');
        });

        // Csak akkor törli, ha létezik
        Schema::dropIfExists('products');
    }

    //Ez elhagyható, ha biztos hogy nem akarjuk újracsinálni
    public function down(): void
    {
        // Ide írhatja a tábla újra létrehozásának logikáját,
        // ha valaha is vissza akarja vonni ezt a migrációt.
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            // ... egyéb oszlopok
            $table->timestamps();
        });
    }
```

- Futtassuk a migrációt, ami letörli fizikailag a táblát:
  - `php artisan migrate --path=database/migrations/2025_01_20_123456_delete_produscts_table.php`

# Saját artisan parancs

[Saját artisan parancs](https://gemini.google.com/share/66c64af2b6a8)

1. Parancstároló osztály létrehozása:
   `php artisan make:command CleanupProductResource`

- Létrejön: **app\Console\Commands\CleanupProductResource.php**

```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;
use Illuminate\Support\Facades\File;
use Illuminate\Support\Facades\Artisan; // Az Artisan parancsok futtatásához

class CleanupProductResource extends Command
{
    // A parancs neve és argumentuma (pl. php artisan cleanup:resource Product)
    protected $signature = 'my_cleanup:resource {name}';

    // A parancs rövid leírása
    protected $description = 'Törli a táblát és az összes hozzá tartozó fájlt (modell, controller, request, stb.) egy adott erőforráshoz.';

    public function handle()
    {
        $name = ucfirst($this->argument('name')); // Erőforrás neve (pl. Product)
        if (!$this->confirm("Biztosan törölni akarod az összes fájlt és a(z) '{$name}' táblát?")) {
            $this->info('Művelet megszakítva.');
            return self::FAILURE;
        }
        $lowerName = strtolower($name); // Kisbetűs változat (pl. product)

        // 1. A tábla törlése (migráció visszavonása)
        // A migrációs fájlt a fájlrendszerben is meg kell keresni, mivel az időbélyeg miatt változik a neve.
        // Minta: Keresd meg az összes migrációs fájlt, ami a 'lowerName's-re illeszkedik.
        $searchPattern = database_path("migrations/*_{$lowerName}s_*.php");
        $migrationFiles = glob($searchPattern);

        if (!empty($migrationFiles)) {
            // 1. Rendezés fordított sorrendben
            // Mivel az időbélyeg a fájlnév elején van, a sztringek fordított rendezése 
            // biztosítja, hogy a legújabb fájlok (legnagyobb időbélyegek) kerüljenek előre.
            rsort($migrationFiles);

            $this->comment("Talált migrációs fájlok száma a(z) '{$lowerName}' erőforráshoz: " . \count($migrationFiles));

            // 2. Végigiterálás a fordított sorrendben rendezett tömbön
            foreach ($migrationFiles as $filePath) {
                $fileName = basename($filePath);

                $this->line("Rollback futtatása a(z) {$fileName} fájlra...");

                // Futtatjuk a rollback-et a migráció visszavonásához
                Artisan::call('migrate:rollback', [
                    // A rollbacket az egyedi fájlra kényszerítjük a --path segítségével
                    '--path' => 'database/migrations/' . $fileName,
                    '--force' => true
                ], $this->output);

                $this->info("A(z) {$fileName} migráció visszavonva.");
            }

            $this->info("A tábla ({$lowerName}s) összes kapcsolódó migrációja sikeresen visszavonva, a helyes sorrendben.");
        } else {
            $this->warn("A migrációs fájlok nem találhatók a(z) '{$lowerName}' erőforráshoz a következő minta alapján: {$searchPattern}");
            $this->warn("Folytatom a többi fájl kézi törlését.");
        }

        // 2. Fájlok listája
        $filesToDelete = [
            // A glob() eredményét vesszük alapul a pontos migrációs fájl névhez
            !empty($migrationFile) ? $migrationFile[0] : null,
            // Egyéb fájlok
            app_path("Models/{$name}.php"),
            database_path("seeders/{$name}Seeder.php"),
            database_path("factories/{$name}Factory.php"),
            app_path("Http/Controllers/{$name}Controller.php"),
            app_path("Http/Requests/Store{$name}Request.php"),
            app_path("Http/Requests/Update{$name}Request.php"),
            app_path("Policies/{$name}Policy.php"),
            // Lehetnek még: Resource, Test-ek, View-k, stb.
            // API/Web Resource
            app_path("Http/Resources/{$name}Resource.php"),
            // Teszt fájlok
            base_path("tests/Feature/{$name}Test.php"),
            base_path("tests/Unit/{$name}Test.php"),
        ];

        // 3. Fájlok törlése
        $deletedCount = 0;
        foreach (array_filter($filesToDelete) as $file) {
            if (File::exists($file)) {
                if (File::delete($file)) {
                    $this->info("Sikeresen törölve: " . basename($file));
                    $deletedCount++;
                } else {
                    $this->error("Hiba történt a törlés során: " . basename($file));
                }
            } else {
                $this->warn("A fájl nem található: " . basename($file));
            }
        }

        if ($deletedCount > 0) {
            $this->comment("\n✅ Tisztítás befejezve a(z) '{$name}' erőforrásra. Törölt fájlok száma: {$deletedCount}");
        } else {
            $this->warn("\n⚠️ Nincs törlendő fájl a(z) '{$name}' erőforráshoz.");
        }

        // Utólagos javaslatok
        $this->newLine();
        $this->warn('--- Kézi tisztítás szükséges! ---');
        $this->warn("1. Távolítsd el a '{$lowerName}' útvonalat a routes/api.php fájlból.");
        $this->warn("2. Ha használtál Policy-t, távolítsd el a Policy regisztrációt az App\Providers\AuthServiceProvider.php fájlból.");
        return self::SUCCESS;

    }
}
```

A parancs futtatása a megadott alias (my_cleanup:resource) segítségével:
`php artisan my_cleanup:resource Product`

## Visszavonás

1. Létrehozzuk a törlő rutint, Commitolunk
2. lefuttatjuk a törlő rutint, Commitolunk
   Tegyük fel, hogy vissza akarjuk vonni, mert olyasmit törölt, amit nem akartunk
3. Keressük meg a törlés utáni commit SHA-ját
   `git log --oneline`

4. Állítsuk vissza az SHA alapján   
`git revert a1b2c3f`



[jogrendszer](https://gemini.google.com/share/b51868a4aea4)


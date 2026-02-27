# Clone
1. Egy projekt klónozása
```console
git clone <url>
```

# Kapcsolódás guthub repóhoz
1. hozz létre egy teljesen üres repót a githubon
2. Hozz létre egy helyi repót és kommitolj valamit
3. Kapcsolódj a távoli repóhoz (az origin (származás) név hozzárendelése az url-hez)
```console
git remote add origin <url>
```
- Kapcsolódás visszavonása
```console
git remote remove origin
```
- A távoli repo url-jének lekérdezése (hova vagy kapcsolódva)
```console
git remote -v
```

# Konfiguráció
- Konfiguráció listázása: 
```console
git config -l
```
**Email** megadása **lokálisan** (csak arra a mappára):
```console
git config user.email "xy.gmail.com"`
```
**Usernév** megadása **lokálisan**:
```console
git config user.name "xy"
```

# pull
- Letöltjük a változásokat
```console
git pull
```
## Ütközés kezelés
- Amikor egy `git merge`, `git pull`, vagy `git rebase` parancs ütközést észlel, leállítja a folyamatot, és a fájlokat különleges jelölésekkel látja el.
```
<<<<<<< HEAD: A **jelenlegi** ágon lévő változtatás (amit éppen használsz).

=======: Elválasztó vonal.

>>>>>>> branch-neve vagy a commit hash-e: A bejövő ág változtatása.
```
- Négy fő lehetőség jelenik meg gombok formájában: Valamelyikre kattints
    - Accept **Current** Change: **Jelenlegi** Változás Elfogadása (a tied győz)
    - Accept **Incoming** Change: Bejövő Változás Elfogadása (a bejövő győz)
    - Accept Both Changes: Mindkét Változás Elfogadása (maradjon mind a kettő)
    - Compare Changes: Változások Összehasonlítása (egy külön nézetben mutatja a két változatot, hogy segítsen a döntésben)
- Nyomd meg a fájl mellett a + jelet: tedd fel a színpadra (stage)
- Kommitold (rá fog kérdezni, hogy tényleg akarod-e)
- Push

## Kényszerített pull (fetch): 
- Rá akarjuk húzni a távoli repót a munkánkra (mindent felülír)
1. Lehúzzuk az összes változást, de ezt még nem hajtja végre (eredet lekérés) Szaggatott le nyíl
```console
git fetch origin
```
2. Áganként felülírhatjuk saját kódunkat a letöltött tartalommal
```console
git reset --hard origin/<ág_neve>
```
3. Ha csináltál új fájlokat, mappákat ebben az ágban, akkor ezt külön le kell takarítani
```console
git clean -fd
```
### Egy lépésben
- Az ág nevét be kell írni
```console
git fetch origin && git reset --hard origin/<ág_neve> && git clean -fd
```
- Az ág nevét automatikusan kifigyeli
```console
git fetch origin && git reset --hard origin/$(git branch --show-current) && git clean -fd
```


# Ág kezelés
- Helyi ágak lekérdezése:
```console
git branch
```
- Helyi és az őt követő távoli ágak lekérdezése + utolsó commit:
```console
git branch -vv
```
- Ugrás a megadott ágra:
```console
git checkout <ágnév>
```
- Ág átnevezés (mindegy melyik ágon vagy)
```console
git branch -m <réginév> <újnév>
```
- Merge: Mindíg azon az ágon legyél, amibe mergelni akarsz!!!
1. Állj rá az ágra amibe mergelni akarsz
```console
git checkout main
```
2. Merge parancs:
```console
git merge <mergelendő ág>
```
- Egy ág feltöltése a github-ra
```console
git push origin -u <ágnév>
```
- Egy ág törlése a github-ról
```console
git push origin --delete <ágnév>
```

# Push
- Az első push
    - Githubon létrejön  a main ág
    - Feltöltődik az összes commit
    - -u: --set-upstream: beállítja, kiépíti a kapcsolatot main (helyi) - origin/main (távoli) main ágak között
    - Ezt minden ágnál így kell csinálni
```console
git push -u origin main
```
- Többi push
```console
git push
```
## Minden ág push
- A sima push csak az aktuális ágat nyomja fel.
- Ez akkor kell, ha csináltunk egy üres github repót és
- A helyi repón vannak ágaink amiket mind fel akarunk tölteni
```console
git push -u origin --all
```

# .gitignore
A .gitignore fájlban határzohatjuk meg, hogy mi ne legyen verziózva.
## .gitignore hierarchia
- A projekt bármely mappájába lehet .gitinore fájl.
- A főkönyvtártól lefelé haladva az előbb lévő szabályok öröklődnek
- Az almappába rakott .gitinore csak attól a mappától kezdve működik, a szülő felé nem.
- Az almappában lévő .gitinore felülbírálhatja, módosíthatja a szülő .gitignore szabályait.

## .gitignore szintaktika
- config.json: Mindenhol ignorálja a config.json nevű fájlokat.
- node_modules/: A teljes mappát és annak tartalmát ignorálja (a perjel fontos!).
- node_modules: A teljes mappát és annak tartalmát ignorálja, valamint ha van ilyen nevű fájl, azt is.
- /logs/debug.log: Csak a gyökérkönyvtárban lévő logs mappa tartalmát nézi, az almappákét nem.
- *.log	Minden .log végű fájlt ignorál.
- !important.log: Ha a *.log aktív, ez az egy fájl mégis követve lesz.
- Kivétel létrehozás példa: Titunk egy mappa minden fájlját, de benne néhány dolgot megengedünk

```.gitignore
# 1. Tiltsd le a mappa tartalmát (de ne magát a mappát!)
config/*

# 2. Most már tehetsz kivételt a mappán belül
!config/settings.json

# 3. (Opcionális) Ha vannak almappák a config-on belül, 
# azokat is engedned kell, hogy lássa a Git:
!config/subfolder/
```

## Gitignore parancsok
- már verziózva vannak, és utólag kerültek .gitignore-ba (.gitignor túlélők)
    - -i (--ignored): Csak azokat mutasd, amik a .gitignore-ban szerepelnek.
    - -c (--cached): Csak azokat a fájlokat nézd, amiket a Git már követ (vagyis benne vannak az indexben/tárolóban).
    - --exclude-standard: Használd a projekt összes .gitignore fájlját az ellenőrzéshez.
```console
git ls-files -i -c --exclude-standard
```

- .gitignor túlélő kitakarítása
1. Eltávolítás a követésből
```console
git rm --cached <fájlnév>
```
2. commitolni kell, majd push és kikerül a távoli repóból is

- Mely fájlokat tilt a gitignore bárhola projektben (a gyökérből)

```console
git ls-files --others --ignored --exclude-standard
```

- Egy konkrét fájl ignorálásának felderítése az egész projektben (linux, bash)
```console
git ls-files -oi --exclude-standard | grep "<fájlnév>"
```

- .gitignor sor felderítése: melyik .gitignore fájlban és hol van letiltva a fájl
```console
git check-ignore -v <útvonal/fájlnév>
```

# Mappa letöltése github-ról
[DownGit](https://minhaskamal.github.io/DownGit/#/home)
- DownGit:
    - Másold ki a böngésző címsorából a letölteni kívánt mappa teljes URL címét.
    - Illeszd be a DownGit weboldalára.
    - Kattints a Download gombra, és máris generál neked egy ZIP-et csak abból a mappából.
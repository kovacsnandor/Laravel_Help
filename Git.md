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
# xClone
1. Egy projekt klónozása
```console
git clone <url>
```

# Kapcsolódás guthub repóhoz
1. hozz létre egy teljesen üres repót a githubon
2. Hozz létre egy helyi reót és kommitolj valamit
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

# Egy projekt minden ágának push-olása
A push csak az aktuális ágat rakja fel.
- Minden ág push
```console
git push -u origin --all
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
- Kommitold a változást

## Kényszerített pull (fetch): 
- Rá akarjuk húzni a távoli repót a munkánkra (mindent felülír)
1. Lehúzzuk az összes válotzást, de ezt még nem hajtja végre (eredet lekérés)
```console
git fetch origin
```
2. Áganként felülírhatjuk saját kódunkat a letöltött tartalommal
```console
git reset --hard origin/<ág_neve>
```

# Ág kezelés
- Ugrás a megadott ágra:
```console
git checkout <ágnév>
```
- Ág átnevezés (mindegy melyik ágon vagy)
```console
git branch -m <réginév> <újnév>
```
- Merge
1. Állj rá az ágra amibe mergelni akarsz
```console
git checkout main
```
2. Merge parancs:
```console
git merge <mergelendő ág>
```

# Minden ág push
Ha már vannak ágaink, ezzel minden ág felpusholódik
```console
git push --all origin --set-upstream
```

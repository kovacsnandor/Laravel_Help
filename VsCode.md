# VsCode download
[VsCode download](https://code.visualstudio.com/download)

# Ajánlott bővítmények
## Editor
- Auto Close Tag
- Auto Rename Tag
- change-case
- Jupyter
- Jupyter Cell Tags
- Jupyter Keymap
- Jupyter Notebook Renderers
- Jupyter Slide Show

## Laravel
- Laravel Blade formatter
- Laravel Blade Snippets
- Laravel Extra Intellisence


## Composer
- Composer

## Docker
- Container Tools
- Dev Containers
- Docker
- Docker DX
- WSL

## Formázó
- ESLint
- Prettier - Code formatter
- SCSS Formatter

## Pyton
- isort
- Pylance
- Python
- Python Debugger
- Python Environments


## Html megjelenítés
- Live Server

## Markdown
- Markdown Preview Enhanced

## Egyéb
- Path Intellisence
- Sass (.sass only)

## PHP
- PHP
- PHP Debug
- PHP Intelephense
- PHP Profiler

## API
- REST Client

## Adatbázis
- SQLite Viewer

## Vuejs
- Vetur
- Vue (Official)
- Vue Extension Box

## AI
- Github Copilot
- IntelliPHP - AI Autocomplete for PHP

## Bővítmény magyarázatok (gemini)
[A bővítményekről](https://gemini.google.com/share/a69cfeb8d5b7)


# VsCode önletörlés megszüntetése
1. File/Preferences
2. Keresés: update
- [ ] Enable Windows Background Updates
    - Ne legyen kipipálva

# Gyorsbillentyűk
## 0. Vs Code Blokkok zárása nyitása
[Nyitás-zárás stackoverflow](https://stackoverflow.com/questions/42660670/collapse-all-methods-in-visual-studio-code)
- **Minden szint kinyitása**: `Ctrl - K - J`
- Adott szint bezárása: `Ctrl - K - n`
    - ahol n: 0,1,2,3,4,5,6,7,8,9
        - 0: Minden szint:  (namespace, class, method, and block)
        - 1: Névterek
        - 2: **függvények**
        - 3: blokkok
        - 4: al blokkok

## Vs kód gyorbillentyűk help
-  `Ctrl - K S`

# Fejlesztői környezet inítása
1. A projekt gyökerében készíts egy `.vscode` mappát.
2. Ebbe készíts egy tasks.json nevű fájlt

├── .vscode  
│ └── tasks.json  
│   
├── client  
│   
└── server  
  

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "🚀 INDÍTÁS",
            "dependsOn": ["S-PHP", "S-BASH", "C-JS", "C-BASH"],
            "group": { "kind": "build", "isDefault": true }
        },
        {
            "label": "S-PHP",
            "type": "shell",
            "command": "\"C:\\Program Files\\Git\\bin\\bash.exe\" -c 'cd server && php artisan serve; exec bash'"
        },
        {
            "label": "S-BASH",
            "type": "shell",
            "command": "\"C:\\Program Files\\Git\\bin\\bash.exe\" -c 'cd server; exec bash'"
        },
        {
            "label": "C-JS",
            "type": "shell",
            "command": "\"C:\\Program Files\\Git\\bin\\bash.exe\" -c 'cd client && npm run dev; exec bash'"
        },
        {
            "label": "C-BASH",
            "type": "shell",
            "command": "\"C:\\Program Files\\Git\\bin\\bash.exe\" -c 'cd client; exec bash'"
        }
    ]
}
```

3. Indítás: Ctrl-Shift-B

### A bash.exe helyének lekérdezése
konzol ablakban:
```console
where bash
```


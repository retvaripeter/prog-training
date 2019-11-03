# prog-training

# Programozás TypeScriptben

## Bevezetés

> Szükséges eszközök: Telepített Node (Már valószínűleg van) és egy szövegszerkesztő (Lehetőleg VSCode, ez is tuti hogy van)

1. készíts egy repót GitHub-on (Javasolt név: training, prog-training) Kérj hozzá egy MIT licenszt, readme-t és egy node-os gitignore-t

> A repók nevei konvencionálisan kebab-case-el íródnak.

A repót töltsd le és készítsd elő arra, hogy különféle typescript feladatokat fogsz benne elvégezni.

Gyakorlatilag egy klasszikus Node projektet kell csinálj TypeScript supportal.

> Könnyen letölthetnéd az én egyik repómat de ez a feladat arra szolgál, hogy a projektstruktúrát átlásd

Legyen egy package.json-öd a repó gyökerében, (nézz utána hogy kell inicializálni egy ilyen projekted, ezt a fájlt ne kézzel készítsd)
és egy src mappa.

2. készíts egy Hello World-öt.

az src-mappában készíts egy day01 (nevezheted ahogy szeretnéd csak sorba lehessen őket tenni) nevű mappát, ilyen csokrokba fogjuk szervezni a fealadatokat.
Ezt a leírást belerakhatod ebbe a mappába `readme.md` néven.

> a readme.md fileokat a github automatikusan megmutatja ha az adott mappába van egy, azért nevezzük így

Ezen a mappán belül jöhet egy hello-world.ts nevű file

Futtasd le először a terminálból! (Node-al tudod futtatni) de úgy hogy abba a mappába állsz ahol a projekt is van (ha a vs-code belső terminálját használod akkor ez azt jelenti hogy úgy hagyod ahogy megnyitottad, ne használj `cd` commandokat)

Majd pedig próbáld meg a VSCode debuggerével is lefuttatni!

Na de hogy?

Kis segítség vs code konfigurációhoz:
csinálj egy .vscode nevű mappát és azon belül egy launch.json nevű fájlt

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Current File",
      "type": "node",
      "request": "launch",
      "args": ["${relativeFile}"],
      "cwd": "${workspaceRoot}",
      "protocol": "inspector"
    }
  ]
}
```

Ha ez a debug config van kiválasztva akkor a zöld nyilacska a debug menüben és az F5 ezt fogja indítani. Ez egy olyan commandot fog összerakni és lefuttatni ami node-al kezdődik ("type": "node",), belép előtte abba a mappába amit vs-code al is megnyitottál ("cwd": "${workspaceRoot}") és paraméternek megadja a file ehhez képesti útvonalát ("args": ["${relativeFile}"],) ez a command gyakorlatilag ugyanúgy fog kinézni mint amit az előbb kézzel beírtál, csak a debug is rá lesz csatolva és tudsz breakpointokat betenni, meg kényelmesebb mint kézzel indíthatni terminálból

3. csinálj egy (új fileban) függvényt aminek paraméterül beadsz egy string-et (fontos hogy megadd, hogy string típusú a függvény első argumentuma, valahogy így: `param: string`) és azt egyből ki is írja a konzolra. Majd pedig hívd meg ezt a függvényt kétszer, két különböző szöveggel.

És próbáld meg lefuttatni.

Nem fog sikerülni mert a node nem fogta fel, hogy mi az a `: string` típusozás, ezt csak a typescript tudja felfogni.

Készítsük elő a repót arra, hogy typescript kompatibilis legyen:

a Typescript egy dev-dependencia, telepítsük fel a legfrissebb verzióját dev-dependenciaként. Ne kézzel írd bele a package.jsonbe, nézz utána hogy kell.

ha megvan csináljunk egy tsconfig.json filet is a repó gyökerébe ezzel a tartalommal:

```json
{
  "compilerOptions": {
    "lib": ["es2017", "esnext.asynciterable"],
    "module": "commonjs",
    "target": "esnext",
    "rootDir": "./src/",
    "outDir": "./dist/",
    "sourceMap": true,
    "noUnusedLocals": true,
    "declaration": false,
    "moduleResolution": "node",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "resolveJsonModule": true,
    "importHelpers": true,
    "noImplicitAny": true,
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noUnusedParameters": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictPropertyInitialization": true,
    "baseUrl": "./",
    "typeRoots": ["./node_modules/@types"]
  },
  "include": ["./src/**/*"],
  "exclude": ["**/node_modules/**/*"]
}
```

Majd később kitérek arra hogy mi mit csinál, a lényeg hogy ezek a létező legszigorúbb typescriptbeállítások. Jó hogyha elve ehhez vagy szokva.

Próbáljuk meg futtatni a filet, de most se fog sikerülni. Miért? Pedig felraktuk a TypeScriptet.

Azért mert még nem használjuk.

Nézzük meg először azt hogy command lineból hogy lehetne használni

A node önmagában nem képes typescriptet futtatni, a typescriptet előbb átt kell fordítani javascriptté. Ezt a `tsc` (typescript compiler) meg is tudja tenni

Próbáljuk is ki, írd be a konzolba, hogy `tsc` paraméterül meg a typscript fájlt amiben a feladat van.

> Ha nem találja a `tsc`-t a számítógéped mint command akkor globálisan is fel kell telepíteni a typescripteta `npm i -g typescript` commanddal

Ez kiköpött egy .js filet, amit már tuti megeszik a node. Megnézheted ezt a filet de úgyse fogjuk ezt kézzel jászani.

Van egy `ts-node` nevű package ami elvégzi a fordítás és futtatást nekünk egy lépésben

Ezt is telepítsd fel dev-dependenciaként (és pluszba akár globálisan is)

Próbáld ki, hogy ezzel futtatod a terminálból a .ts fileodat ami eddig nem ment node-al (most `node` helyett a `ts-node` commandot fogod használni)

Szuper! Most nézzük meg hogy a debuggerrel is tudod e futtatni (Tipp: nem fog)

Meg kell mondani a vs-code-nak hogy ő is a ts-node-ot használja a debuggerrel. Na jó de nem cserélheted ki a konfigban a "type": "node" -ot "type": "ts-node",-ra mert ilyen nincs. Ez a "type" sor nem azt jelenti hogy ezt fogja a command lineba beírni hanem egy ennél picit összetetteb debugkonfigurációnak a neve aminek a kommandjai úgy kezdődnek hogy node. Ebből azt tudjuk leszűrni hogy rá vagyunk kényszerülve a node kommand használatára

Szerencsére a ts-node-ot lehet node-on keresztül is használni ha úgymond "regisztráljuk"

Tehát az hogy `ts-node ./hello.ts` ugyanazt csinálja mint a `node -r ts-node/register ./hello.ts` (Ahhoz hogy ez működjön a mappában amiben vagy helyben kell telepítve legyen a ts-node szóval csak úgy a terminálból lehet nem tudod kipróbálni, de nem is kell)

cseréljük ki a launch.json-ünket erre:

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Current TS File",
      "type": "node",
      "request": "launch",
      "env": {
        "TS_NODE_PROJECT": "./tsconfig.json",
        "TS_NODE_TRANSPILE_ONLY": "true",
        "TS_NODE_FILES": "true"
      },
      "args": ["${relativeFile}"],
      "runtimeArgs": ["-r", "ts-node/register", "-r", "tsconfig-paths/register"],
      "cwd": "${workspaceRoot}",
      "protocol": "inspector",
      "internalConsoleOptions": "openOnSessionStart"
    }
  ]
}
```

Ezzel a konfigurációval már tudjuk majd futtatni a typescript fájlokat a debuggerünkkel

4. Kényelmi eszközök

Automatikus formázás prettierr-el.

Csinálj a repó gyökerébe egy `.prettierrc` fájlt és legyen ez a tartalma:

```json
{
  "tabWidth": 4,
  "useTabs": true,
  "singleQuote": true,
  "printWidth": 120,
  "eslintIntegration": true,
  "jsonEnable": true,
  "overrides": [
    {
      "files": "*.md",
      "options": {
        "tabWidth": 2
      }
    }
  ]
}
```

és csinálj egy `.editorconfig` nevűt is ezzel:

```toml
# Editor configuration, see https://editorconfig.org
root = true

[*]
charset = utf-8
indent_style = tab
indent_size = 4
insert_final_newline = true
trim_trailing_whitespace = true

[*.md]
max_line_length = off
trim_trailing_whitespace = false

[*.txt]
insert_final_newline = false

[*.yml]
indent_style = space
indent_size = 4

```

A .vscode mappában egy `settings.json` nevűt ezzel:

```json
{
  "tslint.alwaysShowRuleFailuresAsWarnings": true,
  "editor.formatOnSave": true,
  "editor.autoIndent": true,
  "editor.quickSuggestions": true,
  "editor.tabSize": 4,
  "material-icon-theme.files.associations": {
    "*.animation.ts": "purescript",
    ".mocharc": "mocha"
  },
  "typescript.tsdk": "node_modules\\typescript\\lib",
  "mochaExplorer.optsFile": "./mocha.opts",
  "mochaExplorer.files": "src/**/*.spec.ts",
  "mochaExplorer.mochaPath": "node_modules/mocha",
  "editor.codeActionsOnSave": {
    "source.fixAll": true
  },
  "editor.mouseWheelZoom": true,
  "files.associations": { ".mocharc": "json" }
}
```

és ugyanide a .vscode mappába egy `extensions.json` fájlt is ezzel:

```json
{
  "recommendations": [
    "jasonlhy.hungry-delete",
    "ms-vscode.vscode-typescript-tslint-plugin",
    "esbenp.prettier-vscode",
    "pkief.material-icon-theme",
    "msjsdiag.debugger-for-chrome",
    "wix.vscode-import-cost",
    "npxms.hide-gitignored",
    "hbenl.vscode-mocha-test-adapter",
    "juanmnl.vscode-theme-1984",
    "daylerees.rainglow",
    "equinusocio.vsc-material-theme"
  ]
}
```

A felajánlott kiegészítőket meg telepítsd is fel (Ahogy elmented ezt a fájlt fel kell hogy ajánlja őket, ha nem vs-code bezárás újramegnyitás után felkell)

A prettier lokálisan is tegyük fel, dev dependenciaként telepítsd a `prettier` csomagot, továbbá dev dependenciaként ugyanúgy a `@types/node` csomagot. (utóbbi a nodeos cuccok típusozásait adják)

5. Lintelés

Az utolsó feladat a lintelés support beállítása. Hatalmas segítség lesz ha a lintelő tud majd segíteni az egyszerű hibák kiszűrésében

Dev dependenciaként telepítsd a `tslint` és a `tslint-plugin-prettier` csomagokat

ehhez kelleni fog konfiguráció is. Csinálj egy `tslint.json` nevű fájlt a repó gyökerébe ezzel a tartalommal:

```json
{
  "defaultSeverity": "error",
  "extends": ["tslint:recommended"],
  "jsRules": {},
  "rules": {
    "array-type": false,
    "arrow-parens": false,
    "arrow-return-shorthand": true,
    "callable-types": true,
    "class-name": true,
    "comment-format": [true, "check-space"],
    "curly": false,
    "deprecation": {
      "severity": "warn"
    },
    "eofline": true,
    "forin": true,
    "import-blacklist": [true, "rxjs/Rx"],
    "import-spacing": true,
    "indent": [true, "tabs"],
    "interface-name": false,
    "interface-over-type-literal": true,
    "label-position": true,
    "max-line-length": [true, 120],
    "member-access": false,
    "member-ordering": [
      true,
      {
        "order": ["static-field", "instance-field", "static-method", "instance-method"]
      }
    ],
    "no-arg": true,
    "no-bitwise": false,
    "no-console": [true, "debug", "info", "time", "timeEnd", "trace"],
    "no-construct": true,
    "no-debugger": true,
    "no-duplicate-super": true,
    "no-empty": false,
    "no-empty-interface": true,
    "no-eval": true,
    "no-inferrable-types": [true, "ignore-params"],
    "no-misused-new": true,
    "no-non-null-assertion": true,
    "no-redundant-jsdoc": true,
    "no-shadowed-variable": true,
    "no-string-literal": false,
    "no-string-throw": true,
    "no-submodule-imports": false,
    "no-switch-case-fall-through": true,
    "no-trailing-whitespace": true,
    "no-unnecessary-initializer": true,
    "no-unused-expression": true,
    "no-var-keyword": true,
    "object-literal-key-quotes": false,
    "object-literal-sort-keys": false,
    "one-line": [true, "check-open-brace", "check-catch", "check-else", "check-whitespace"],
    "prefer-const": true,
    "quotemark": [true, "single"],
    "radix": true,
    "semicolon": [true, "always"],
    "trailing-comma": false,
    "triple-equals": [true, "allow-null-check"],
    "typedef-whitespace": [
      true,
      {
        "call-signature": "nospace",
        "index-signature": "nospace",
        "parameter": "nospace",
        "property-declaration": "nospace",
        "variable-declaration": "nospace"
      }
    ],
    "unified-signatures": true,
    "variable-name": {
      "options": ["check-format", "require-const-for-all-caps", "allow-leading-underscore", "ban-keywords"]
    },
    "whitespace": [true, "check-branch", "check-decl", "check-operator", "check-separator", "check-type"]
  },
  "rulesDirectory": ["node_modules/tslint-plugin-prettier"]
}
```

Ezek csak ajánlott beállítások, ha gondolod utánuk nézegethetsz hogy mi mit állít be

Bónusz:

Telepítsd a `npm-check-updates` csomagot is dev dependenciaként, az `rxjs` csomagot meg sima dependenciaként. utóbbit már ismered, egyelőre nem fogjuk használni de elfér ott. Előbbivel meg időnként meg lehet nézni hogy jött e ki valamelyik csomagból új.

Később majd csinálunk teszteket is de egyelőre nem fontos.

Jó munkát!
Ha kész vagy (vagy valami probléma van) küldd el a repódat messengeren!

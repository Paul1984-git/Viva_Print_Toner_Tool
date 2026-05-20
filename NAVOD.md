# Vivantis PT Manager – Návod na zprovoznění

Tento návod tě provede od nuly až po funkční appku přístupnou přes odkaz v prohlížeči.
Celkem to zabere asi **20–30 minut**, vše je klikací, žádné programování.

---

## Co budeš potřebovat

- Google účet (pro Firebase)
- GitHub účet (pro hosting) – na github.com
- Textový editor (Poznámkový blok / Notepad stačí)

---

## KROK 1 – Založ si GitHub účet

1. Jdi na **https://github.com**
2. Klikni na **Sign up**
3. Zadej email, heslo a uživatelské jméno (např. `vivantis-it`)
4. Potvrď email

---

## KROK 2 – Vytvoř GitHub repozitář

1. Po přihlášení klikni na zelené tlačítko **New** (nebo **+** vpravo nahoře → New repository)
2. Vyplň:
   - **Repository name:** `vptmt`
   - **Visibility:** ✅ **Private** (soukromý – vidí jen pozvaní)
3. Klikni **Create repository**

---

## KROK 3 – Založ si Firebase projekt

1. Jdi na **https://console.firebase.google.com**
2. Přihlas se Google účtem
3. Klikni **Přidat projekt** (Add project)
4. Název projektu: `vivantis-vptmt`
5. Google Analytics: můžeš vypnout, klikni **Vytvořit projekt**
6. Počkej chvíli, pak klikni **Pokračovat**

### Vytvoř databázi

7. V levém menu klikni na **Realtime Database**
8. Klikni **Vytvořit databázi**
9. Lokalita: vyber **europe-west1 (Belgie)** – nejblíže ČR
10. Režim zabezpečení: vyber **Testovací režim** (test mode) – pro začátek stačí
    > ⚠️ Testovací režim vyprší za 30 dní. Viz Krok 6 pro nastavení pravidel.
11. Klikni **Povolit**

### Získej konfigurační údaje

12. V levém menu klikni na ozubené kolečko ⚙️ → **Nastavení projektu**
13. Posuň dolů na sekci **Vaše aplikace**
14. Klikni na ikonu `</>` (Web)
15. Přezdívka aplikace: `vptmt-web`, klikni **Zaregistrovat aplikaci**
16. Zobrazí se blok kódu `firebaseConfig` – **nechej tuto stránku otevřenou**, potřebuješ odtud hodnoty

---

## KROK 4 – Vyplň Firebase konfiguraci v appce

1. Otevři soubor **index.html** v Poznámkovém bloku (pravý klik → Otevřít v → Poznámkový blok)
2. Najdi sekci (Ctrl+F → hledej `TVUJ_API_KEY`):

```
const firebaseConfig = {
    apiKey:            "TVUJ_API_KEY",
    authDomain:        "TVUJ_PROJECT.firebaseapp.com",
    databaseURL:       "https://TVUJ_PROJECT-default-rtdb.europe-west1.firebasedatabase.app",
    projectId:         "TVUJ_PROJECT",
    storageBucket:     "TVUJ_PROJECT.appspot.com",
    messagingSenderId: "TVOJE_SENDER_ID",
    appId:             "TVOJE_APP_ID"
};
```

3. Zkopíruj hodnoty z Firebase konzole (stránka otevřená z Kroku 3) a nahraď všechny `TVUJ_*` hodnoty svými skutečnými hodnotami
4. Ulož soubor

**Příklad jak to vypadá po vyplnění:**
```
const firebaseConfig = {
    apiKey:            "AIzaSyBxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    authDomain:        "vivantis-vptmt.firebaseapp.com",
    databaseURL:       "https://vivantis-vptmt-default-rtdb.europe-west1.firebasedatabase.app",
    ...
};
```

---

## KROK 5 – Nahraj soubory na GitHub

1. Jdi do svého repozitáře na GitHubu (`github.com/tvoje-jmeno/vptmt`)
2. Klikni na **uploading an existing file** (nebo přetáhni soubory)
3. Nahraj tyto soubory ze složky `vptmt`:
   - `index.html`
   - `manifest.json`
   - `sw.js`
   - složku `icons` (oba soubory icon-192.png a icon-512.png)
4. Dole klikni **Commit changes**

---

## KROK 6 – Zapni GitHub Pages

1. V repozitáři klikni na **Settings** (nastavení)
2. V levém menu klikni na **Pages**
3. Pod **Branch** vyber **main** a složku **/ (root)**
4. Klikni **Save**
5. Po chvíli (1–2 minuty) se zobrazí odkaz ve tvaru:
   `https://tvoje-jmeno.github.io/vptmt`

**Tento odkaz pošleš šéfovi a kolegům.**

> 💡 Repozitář je privátní (soukromý), ale GitHub Pages u privátních repozitářů je veřejně přístupný přes odkaz. Kdo nemá odkaz, appku nenajde. Pokud chceš plnou ochranu heslem, je potřeba GitHub Pro (placené) – pro interní firemní nástroj bez citlivých dat to ale není nutné.

---

## KROK 7 – Nastav Firebase pravidla (po 30 dnech)

Testovací režim vyprší. Před vypršením nastav tato pravidla:

1. Firebase konzole → **Realtime Database** → záložka **Pravidla**
2. Nahraď obsah tímto:

```json
{
  "rules": {
    "vptmt": {
      ".read": true,
      ".write": true
    }
  }
}
```

3. Klikni **Publikovat**

> Tato pravidla umožňují čtení i zápis komukoli kdo zná URL databáze. Pro firemní sklad tonerů je to dostačující. Pokud bys chtěl přidat přihlašování, dej vědět.

---

## Jak pozvat šéfa / kolegy

Jednoduše jim pošli odkaz z Kroku 6. Otevřou ho v prohlížeči, žádná instalace není nutná. Všichni vidí stejná data v reálném čase díky Firebase.

Pokud si chtějí appku přidat na plochu (jako ikonu), v Chrome/Edge se jim zobrazí výzva k instalaci automaticky.

---

## Stručný přehled – co kde běží

| Co | Kde | Cena |
|----|-----|------|
| Soubory appky (HTML) | GitHub Pages | Zdarma |
| Sdílená data | Firebase Realtime Database | Zdarma (do 1 GB dat, 10 GB přenosu/měsíc) |
| Hosting domény | github.io | Zdarma |

Pro váš objem dat (desítky tiskáren, stovky pohybů ročně) jsou limity Firebase více než dostatečné.

---

## Potřebuješ pomoc?

Pokud narazíš na problém v kterémkoli kroku, pošli screenshot chybové hlášky do Claudu a pomůžu ti to vyřešit.

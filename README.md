# Valmentaja — treenivalmius Apple Healthista

Sivu lukee unen, leposykkeen ja aktiivisuuden Apple Shortcutin kautta suoraan URL:sta, laskee treenivalmiuden (0–100) ja antaa treenin tai lepopäivän. Painonpudotuksen tavoite 103 → 93 kg.

**Sivun osoite julkaisun jälkeen:** `https://jertsa97-cmd.github.io/valmentaja/`

---

## 1. Julkaise GitHub Pagesiin

1. Luo uusi repo: **valmentaja** (Public).
2. Lisää tämä `index.html` repon juureen (raahaa GitHubin web-käyttöliittymässä "Add file → Upload files", tai gitillä).
3. Repo → **Settings → Pages** → Source: **Deploy from a branch** → Branch: **main** / **/ (root)** → Save.
4. Odota ~1 min. Sivu on osoitteessa `https://jertsa97-cmd.github.io/valmentaja/`.

Gitillä:
```bash
git init
git add index.html README.md
git commit -m "Valmentaja"
git branch -M main
git remote add origin https://github.com/jertsa97-cmd/valmentaja.git
git push -u origin main
```

---

## 2. Apple Shortcut — täysin automaattinen

Sivu lukee URL-parametrit, joten Shortcut voi avata sen valmiiksi täytettynä. Ei liittämistä.

Avaa **Komennot** → **+** → lisää toiminnot:

### Uni
- **Etsi terveysnäytteet** → Unen analyysi (Asleep), suodata: tänään → **Laske tilasto** → Summa kestoista
- Muunna tunneiksi tarvittaessa (**Laske**: ÷ 3600 jos sekunteina)
- **Aseta muuttuja**: `uni`

### Leposyke
- **Etsi terveysnäytteet** → Leposyke, viimeisin (lajittele laskevasti, rajoita 1)
- **Aseta muuttuja**: `syke`

### Eilinen kuormitus (0–3)
- **Etsi terveysnäytteet** → Aktiivinen energia, suodata: eilen → **Laske tilasto** → Summa → muuttuja `kcal`
- **Jos**-haaroilla: <200 → `0`, 200–450 → `1`, 450–750 → `2`, >750 → `3` → muuttuja `kuorma`

### Paino (valinnainen)
- **Etsi terveysnäytteet** → Paino, viimeisin → muuttuja `paino`

### Avaa sivu parametreilla
- **Teksti**-toiminto:
  ```
  https://jertsa97-cmd.github.io/valmentaja/?sleepHours=[uni]&restingHR=[syke]&yesterdayLoad=[kuorma]&weight=[paino]
  ```
  (hakasulkeisiin valitaan vastaavat muuttujat)
- **Avaa URL-osoitteet**

Aja Shortcut aamulla → sivu avautuu valmiiksi täytettynä → **Tallenna päivä**.

### Automaatio
Komennot → Automaatio → "Kello 7:00" tai "Herätys lopetettu" → aja tämä → "Suorita heti".

---

## Parametrit

| Avain | Arvo | Lähde |
|-------|------|-------|
| `sleepHours` | tunnit, 7.2 | Unen analyysi |
| `restingHR` | bpm, 55 | Leposyke |
| `yesterdayLoad` | 0–3 | Aktiivinen energia muunnettuna |
| `weight` | kg, 101.3 | Paino (valinnainen) |
| `sleepQuality` | 1–5 | ei Watchista — säädä käsin tai jätä pois |
| `soreness` | 1–5 | subjektiivinen — säädä käsin tai jätä pois |

Data tallentuu selaimen localStorageen, joten historia ja baseline-leposyke säilyvät. Käytä samaa selainta (esim. Safari) joka kerta.

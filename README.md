# Startlijst

Bijhouden wie je hebt gevraagd om mee te racen, met status **ja / nee / geen reactie**.
Statische site (GitHub Pages) + Firebase (Firestore voor de data, Authentication
voor het gedeelde wachtwoord). Iedereen met de link kan de lijst *zien*; alleen
wie het wachtwoord invoert kan iets *wijzigen*.

## 1. Firebase-project aanmaken (eenmalig, ~5 min)

1. Ga naar https://console.firebase.google.com en klik **Project toevoegen**.
   Geef het een naam (bv. `startlijst`), Google Analytics kun je uitzetten.
2. Ga in het project naar **Build → Firestore Database → Database maken**.
   Kies een regio in de buurt (bv. `eur3 (europe-west)`) en start in
   **productiemodus**.
3. Ga naar **Build → Authentication → Aan de slag** → tabblad **Sign-in method**
   → schakel **E-mail/wachtwoord** in.
4. Nog steeds bij Authentication → tabblad **Users** → **Gebruiker toevoegen**.
   - E-mailadres: mag alles zijn, hoeft geen echte inbox te zijn, bv.
     `team@startlijst.app` (moet wel gelijk zijn aan `SHARED_LOGIN_EMAIL` in
     `index.html`, zie stap 6).
   - Wachtwoord: **dit is het wachtwoord dat je met je team deelt.**
5. Ga naar de **projectinstellingen** (tandwiel linksboven) → tabblad
   **Algemeen** → scroll naar **Jouw apps** → klik het `</>` (web) icoon →
   geef de app een naam en registreer 'm (Firebase Hosting overslaan). Je
   krijgt een `firebaseConfig` object te zien met `apiKey`, `authDomain`, etc.
6. Open `index.html` in dit mapje en plak die waarden in het `firebaseConfig`
   object bovenin het `<script>`-gedeelte (zoek naar `VUL_HIER`). Als je in
   stap 4 een ander e-mailadres koos dan `team@startlijst.app`, pas dan ook
   `SHARED_LOGIN_EMAIL` aan.
7. Terug in Firestore: tabblad **Regels** → vervang de inhoud door die van
   `firestore.rules` (in dit mapje) → **Publiceren**.

Dat is alle Firebase-configuratie — er is verder niks te installeren of te
deployen aan de Firebase-kant.

## 2. Naar GitHub pushen

Maak op github.com een nieuwe (lege) repository aan, bijvoorbeeld `startlijst`.
**Openbaar (public)** — GitHub Pages op een gratis account werkt alleen bij
publieke repo's. Dat is geen probleem: er staan geen geheimen in de code (de
Firebase `apiKey` is niet geheim, de beveiliging zit in de Firestore-regels
en het wachtwoord).

Voer dan in dit mapje uit:

```bash
git init
git add .
git commit -m "Startlijst"
git branch -M main
git remote add origin https://github.com/<jouw-gebruikersnaam>/startlijst.git
git push -u origin main
```

## 3. GitHub Pages aanzetten

1. Ga naar de repository op GitHub → **Settings → Pages**.
2. Bij **Source** kies je **Deploy from a branch**, branch **main**, map
   **/ (root)** → **Save**.
3. Na ongeveer een minuut staat de site live op
   `https://<jouw-gebruikersnaam>.github.io/startlijst/`.

## Gebruik

- Iedereen die de link opent ziet de lijst (alleen-lezen).
- Klik rechtsboven op **🔒 Alleen-lezen · ontgrendelen**, vul het gedeelde
  wachtwoord in om te ontgrendelen. Daarna kun je rijders toevoegen, de
  status wijzigen (Wacht / Ja / Nee) en rijders verwijderen.
- **🔓 Bewerken aan · vergrendelen** klikken logt je weer uit.

## Later aanpassen

- **Wachtwoord wijzigen**: Firebase Console → Authentication → Users → naast
  de gebruiker → wachtwoord opnieuw instellen.
- **Extra mensen laten meebeslissen** zonder het wachtwoord te delen: maak
  een extra Auth-gebruiker aan met een eigen wachtwoord — de regels in
  `firestore.rules` staan iedere ingelogde gebruiker toe te schrijven.
- **Site aanpassen**: bewerk `index.html`, commit en push — GitHub Pages
  update vanzelf binnen een minuut.

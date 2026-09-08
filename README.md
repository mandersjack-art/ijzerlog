# IJzerlog

Persoonlijke krachttraining-tracker. Log sets, reps en gewicht per gymbezoek, importeer een trainingsschema uit Markdown, en volg je voortgang per oefening.

Werkt direct zonder verdere configuratie (data blijft dan lokaal in de browser van het toestel waarop je hem gebruikt). Voor synchronisatie tussen meerdere toestellen (bv. telefoon + laptop) kun je gratis cloud-opslag via Firebase aanzetten — zie hieronder.

## Cloud-sync instellen (optioneel)

1. Ga naar [console.firebase.google.com](https://console.firebase.google.com) en maak een nieuw (gratis) project aan.
2. Ga naar **Build > Firestore Database** → **Create database** → kies **Production mode**.
3. Ga naar **Build > Authentication** → **Get started** → zet **Google** aan als sign-in-methode.
4. Ga naar **Project settings** (tandwiel) → **Algemeen** → scroll naar **Jouw apps** → klik **</> Web app toevoegen** → geef een naam → kopieer het `firebaseConfig`-object dat verschijnt.
5. Open `index.html` in dit project, zoek naar `FIREBASE_CONFIG` (bovenaan het `<script>`-blok) en plak daar je eigen waarden in.
6. Zet `OWNER_EMAIL` op je eigen Google-mailadres (staat al ingevuld op basis van je account).
7. Ga in Firebase naar **Firestore Database > Regels** en plak:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if request.auth != null && request.auth.token.email == "JOUW_EMAIL_HIER";
       }
     }
   }
   ```

8. Publiceer de regels, commit en push je wijzigingen in `index.html`.
9. Open de site en log in met Google — vanaf nu synct alles tussen apparaten waarop je met hetzelfde account inlogt.

**Let op:** de `FIREBASE_CONFIG`-waarden zijn niet geheim (ze staan sowieso zichtbaar in de broncode van elke Firebase-webapp) — de beveiliging zit in de Firestore-regels hierboven, die alleen jouw e-mailadres toegang geven.

## Lokaal testen

Open `index.html` gewoon in een browser, of start een lokale server:

```
python -m http.server 8000
```

en open `http://localhost:8000`.

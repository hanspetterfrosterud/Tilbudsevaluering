# Oppsett – Tilbudsevaluering (nettbasert, lokal lagring)

Denne versjonen trenger IKKE Supabase eller noen database. Prosjekter lagres i nettleseren din, og du kan eksportere/importere prosjektfiler for sikkerhetskopi og flytting mellom PC-er. Det eneste du trenger er en Anthropic API-nøkkel for AI-funksjonene.

## 1 – API-nøkkel for AI (Anthropic)

1. Gå til console.anthropic.com -> logg inn -> API Keys -> Create Key.
2. Kopier nøkkelen (vises kun én gang).
3. I appen: gå til Oppsett-fanen, lim inn nøkkelen, trykk Lagre nøkkel. Den lagres kun lokalt i din nettleser.

Kostnad: hvert AI-kall koster litt (typisk øre-beløp per tilbud). Du betaler Anthropic direkte etter bruk. Sett gjerne et forbrukstak under Billing -> Limits i Anthropic-konsollen.

## 2 – Kjøre appen

To muligheter:

A. Lokalt paa PC-en (enklest): bare dobbeltklikk tilbudsevaluering.html, saa aapnes den i nettleseren. Alt fungerer - AI og lagring.

B. Paa GitHub Pages (naas fra flere enheter):
1. Lag et repo paa github.com, gjoer det Public (Pages er gratis kun paa offentlige repo). Noeklene ligger ikke i koden, saa det er trygt.
2. Last opp fila og gi den navnet index.html.
3. Settings -> Pages -> Source: Deploy from a branch -> main / root -> Save.
4. Adressen blir https://<brukernavn>.github.io/<repo>/

## 3 – Bruk

1. Oppsett-fanen: lim inn Anthropic-noekkelen og lagre.
2. Lag et prosjekt (skriv navn -> Nytt prosjekt).
3. Last opp funksjonsbeskrivelse og tilbud i Evaluering, kjoer AI-evaluering.
4. Trykk Lagre prosjekt - prosjektet lagres i nettleseren og dukker opp i lista i Oppsett-fanen.

## Sikkerhetskopi og flytting mellom PC-er

Lokal lagring ligger kun i nettleseren paa den maskinen du bruker, og forsvinner hvis du toemmer nettleserdata. Derfor:

- Eksporter prosjekt: Oppsett-fanen -> Eksporter gjeldende prosjekt (.json). Lagre fila et trygt sted (Novaform-serveren eller OneDrive).
- Importer prosjekt: Oppsett-fanen -> Importer prosjektfil -> velg .json-fila. Fungerer ogsaa paa en annen PC.

Anbefaling: eksporter en prosjektfil naar du er ferdig med en viktig evaluering, som backup.

## Feilsoeking

- AI svarer ikke / 401: Anthropic-noekkelen mangler eller er feil - lim inn paa nytt i Oppsett.
- CORS-feil paa AI-kall: Anthropic tillater nettleserkall (appen sender riktig header), men blokkeres det, si ifra - da lager vi en liten proxy.
- Nettleserlageret er fullt: mange store prosjekter med bilde-PDF-er kan fylle lageret. Eksporter gamle prosjekter til fil og slett dem fra lista.
- Prosjekt borte etter toemming av nettleserdata: bruk eksport/import som backup.

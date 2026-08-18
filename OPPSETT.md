# Oppsett – Tilbudsevaluering (nettbasert)

Denne guiden setter opp appen som en ekte nettapp med lagring. Tre deler: Supabase (lagring), API-nøkkel (AI), og GitHub Pages (publisering). Regn med 20–30 minutter første gang.

## Del 1 – Supabase (lagring)

1. Gå til supabase.com, logg inn (eller lag gratis konto), og trykk **New project**.
2. Gi det navn (f.eks. `tilbudsevaluering`), velg region **Europe (eu-north / Stockholm)**, sett et databasepassord (lagre det et trygt sted). Trykk **Create new project** og vent ~2 min.
3. Når prosjektet er klart: gå til **Project Settings → API**. Noter to verdier – du trenger dem i appen:
   - **Project URL** (ser ut som `https://xxxx.supabase.co`)
   - **anon public** nøkkel (lang tekst under "Project API keys")

### Lag tabellen
4. Gå til **SQL Editor** (venstremenyen) → **New query**, lim inn dette og trykk **Run**:

```sql
create table if not exists prosjekter (
  id uuid primary key default gen_random_uuid(),
  navn text not null,
  data jsonb not null,
  oppdatert timestamptz default now()
);

alter table prosjekter enable row level security;

-- Privat bruk: åpen tilgang med anon-nøkkel (kun du har nøkkelen).
create policy "anon full access" on prosjekter
  for all using (true) with check (true);
```

### Lag lagringsbøtte for filer
5. Gå til **Storage** → **New bucket**. Navn: `tilbudsfiler`. Sett den som **Public** (enk
lest for privat bruk). Trykk **Create bucket**.
6. Gå til **Storage → Policies** for `tilbudsfiler` → **New policy → For full customization**, og lag én policy som tillater alt:
   - Policy name: `anon all`
   - Allowed operations: huk av **SELECT, INSERT, UPDATE, DELETE**
   - Target roles: `anon`
   - USING expression: `true`   |   WITH CHECK expression: `true`
   - Trykk **Review** → **Save policy**.

> Merk: dette er bevisst åpent fordi bare du har nøkkelen og bruken er privat. Del aldri anon-nøkkelen offentlig. Skal flere bruke appen senere, må vi legge på innlogging.

## Del 2 – API-nøkkel for AI (Anthropic)

1. Gå til console.anthropic.com → logg inn → **API Keys** → **Create Key**. Kopier nøkkelen (vises kun én gang).
2. Du legger den IKKE inn i koden. Du limer den inn i appen (feltet «API-nøkkel» øverst) første gang – den lagres lokalt i nettleseren din.

> Kostnad: hvert AI-kall koster litt (typisk øre-beløp per tilbud). Du betaler Anthropic direkte etter bruk. Sett gjerne et forbrukstak i Anthropic-konsollen under **Billing → Limits**.

## Del 3 – Publiser på GitHub Pages

1. Lag et **privat** repo på github.com (f.eks. `tilbudsevaluering`). Privat er tryggest.
2. Last opp `tilbudsevaluering.html` (gi den gjerne nytt navn `index.html`).
3. Repo → **Settings → Pages** → Source: **Deploy from a branch** → Branch: `main` / `/root` → **Save**.
4. Vent 1–2 min. Adressen blir `https://<brukernavn>.github.io/tilbudsevaluering/`.

> Privat repo + GitHub Pages: Pages-siden blir teknisk sett tilgjengelig for den som har lenken, men repoet (koden) er privat. Siden ingen nøkler ligger i koden (de ligger i nettleseren din), er dette trygt for privat bruk. Vil du stenge helt: kjør appen lokalt ved å bare åpne HTML-fila fra egen PC – da funker Supabase og AI like fullt.

## Førstegangsbruk i appen

1. Åpne appen. Øverst: lim inn **Supabase URL**, **anon key** og **Anthropic API-nøkkel**. Trykk **Lagre oppsett** (lagres lokalt).
2. Lag et prosjekt (f.eks. «Døves Kultursenter»), last opp tilbud, kjør evaluering.
3. Trykk **Lagre prosjekt** – nå ligger alt i Supabase og kan åpnes igjen senere, også fra en annen PC med samme nøkler.

## Feilsøking

- **«Failed to fetch» / rød feil ved lagring:** sjekk at URL og anon-nøkkel er riktige, og at SQL-en i Del 1 er kjørt.
- **AI svarer ikke / 401:** Anthropic-nøkkelen mangler eller er feil – lim inn på nytt.
- **Filer lastes ikke opp:** sjekk at Storage-bøtta heter nøyaktig `tilbudsfiler` og at policy-en i steg 6 er lagret.
- **CORS-feil på AI-kall fra nettleser:** se merknad i appen; Anthropic tillater nettleserkall, men blokkeres kallet, må vi rute det via en Supabase Edge Function (si ifra, så lager jeg den).

# Kristallgruvan – publik version

Ett mattespel för åk 3–6. Spelaren skriver ett smeknamn och väljer årskurs, sedan anpassar sig nivån själv. Det finns en topplista med tre flikar: veckans poäng, vagnrace-rekord och all-time.

## Filerna

| Fil | Vad den är |
|---|---|
| `index.html` | Hela spelet |
| `config.js` | Dina två Supabase-värden (fylls i i steg 1) |
| `supabase.sql` | Databasen, körs en gång i Supabase |
| `README.md` | Den här guiden |

Spelet fungerar direkt utan Supabase, men då sparas allt bara i webbläsaren och topplistan visar bara spelare på samma enhet.

---

## Steg 1: Supabase (databasen för topplista och framsteg), cirka 10 minuter

1. Gå till **supabase.com** och skapa ett gratis konto.
2. Klicka **New project**. Ge det ett namn, till exempel `kristallgruvan`, och välj ett databaslösenord. Spara lösenordet någonstans, men spelet behöver det inte. Välj en region i Europa, gärna Stockholm om den finns.
3. När projektet är klart: gå till **SQL Editor** → **New query**. Klistra in hela innehållet i `supabase.sql` och klicka **Run**. Det ska stå "Success".
4. Gå till **Project Settings** → **API Keys** (eller **API**). Kopiera två saker:
   - **Project URL**, till exempel `https://abcdefghijkl.supabase.co`
   - den **publika** nyckeln, som heter **publishable** (börjar med `sb_publishable_`) eller **anon public**
5. Öppna `config.js` i en texteditor och klistra in värdena mellan citattecknen:

```js
window.KG_CONFIG = {
  supabaseUrl: 'https://abcdefghijkl.supabase.co',
  supabaseAnonKey: 'sb_publishable_xxxxxxxx',
};
```

**Viktigt:** använd aldrig nyckeln som heter **secret** eller **service_role**. Den publika nyckeln får synas på webben. Databasen är byggd så att den nyckeln bara kan spara och läsa via spelets egna funktioner, inte läsa hela tabellen.

**Bra att veta om gratisplanen:** projektet pausas om ingen spelar på cirka en vecka. Då står det "paused" i Supabase, och du klickar **Restore**. Under tiden fungerar spelet ändå, och poängen skickas när databasen är igång igen.

---

## Steg 2: GitHub Pages (själva webbsidan), cirka 5 minuter

1. Skapa ett gratis konto på **github.com**.
2. Klicka **+** uppe till höger → **New repository**. Döp det till `kristallgruvan` och välj **Public**. Klicka **Create repository**.
3. Klicka **uploading an existing file**. Dra in `index.html` och `config.js` (de andra filerna behövs inte på webben). Klicka **Commit changes**.
4. Gå till **Settings** → **Pages**. Under **Build and deployment** väljer du **Deploy from a branch**, branch **main** och mapp **/ (root)**. Klicka **Save**.
5. Vänta 1–2 minuter och ladda om sidan. Där står adressen, till exempel `https://DITTNAMN.github.io/kristallgruvan/`.

**Uppdatera spelet senare:** ladda upp en ny `index.html` på samma sätt, så skrivs den gamla över.

---

## Hur det fungerar

- **Spelkod:** varje spelare får en hemlig spelkod som sparas i webbläsaren. Under **Min spelkod** kan spelaren kopiera den och fortsätta på en annan enhet.
- **Poäng till topplistan:** varje rätt svar ger nivå × 10 poäng, så svårare uppgifter ger mer. Det blir 5 extra vid tre rätt i rad och 100 för att besegra veckobossen. Vagnracet ger 5 per rätt och har en egen rekordlista.
- **Veckans poäng** nollställs varje måndag (svensk tid).
- **Mot fusk:** servern godtar högst cirka 240 poäng per minut och vagnrace-rekord upp till 80.
- **Smeknamn** kontrolleras mot en lista med fula ord, både i spelet och i databasen. Listan finns i `supabase.sql` (funktionen `kg_name_ok`) och kan byggas ut.
- **Radera mig:** spelaren kan själv ta bort all sin data under **Min spelkod**.

## Om du behöver ta bort ett olämpligt namn

I Supabase: **Table Editor** → **players** → markera raden → **Delete**.

## Personuppgifter

Spelet sparar bara smeknamn, årskurs, poäng och spelets framsteg, och ber spelarna använda påhittade smeknamn. Om spelet blir publikt för andra barn än dina egna är du ändå den som ansvarar för datat. Läs gärna IMY:s vägledning. Detta är ingen juridisk rådgivning.

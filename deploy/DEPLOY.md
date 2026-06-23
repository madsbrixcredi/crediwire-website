# Deploy crediwire.com til Cloudflare Pages

Denne mappe indeholder alt, der skal deployes til Cloudflare Pages for at gå live på **https://www.crediwire.com**.

## Indhold i mappen

- `index.html` — forsiden
- `404.html` — pænt fallback hvis nogen rammer en død URL
- `shared.css` — al styling
- `CW_logo_lilla_rgb.png` — logo og favicon
- `_redirects` — Cloudflare-redirects (fx /login → app.crediwire.com)
- `robots.txt` — fortæller søgemaskiner hvad de må indeksere
- `sitemap.xml` — liste over sider til Google

## Sådan deployes det

1. Log ind på https://dash.cloudflare.com
2. Gå til **Workers & Pages** → projektet **newsite-2pz**
3. Klik **Create new deployment**
4. Træk hele `deploy/`-mappen ind
5. Klik **Save and Deploy**. Vent 20-30 sekunder.

Sitet er nu live på den eksisterende pages.dev URL.

## Sådan kobles crediwire.com på

Efter deployment, mens du stadig er i projektet:

1. Klik **Custom domains** (i højre menu)
2. Klik **Set up a domain**
3. Skriv `www.crediwire.com` og klik **Continue**
4. Cloudflare vil bede dig om at tilføje en CNAME-record. Hvis DNS for crediwire.com allerede er på Cloudflare, sker det automatisk. Hvis ikke, skal du manuelt tilføje:
   - **Type:** CNAME
   - **Navn:** www
   - **Mål:** newsite-2pz.pages.dev
5. Gentag for `crediwire.com` (uden www) — det skal redirecte til www-versionen
6. Vent 5-15 minutter på DNS propagation
7. Cloudflare udsteder automatisk SSL-certifikat

## Tjek efter switch

- [ ] https://www.crediwire.com loader det nye site
- [ ] https://crediwire.com (uden www) redirecter til www
- [ ] HTTPS-låsen er grøn (gyldigt certifikat)
- [ ] Footer-linkene til crediwire.dk virker (cookie policy, privacy etc.)
- [ ] /login redirecter til app.crediwire.com/login
- [ ] /register redirecter til app.crediwire.com/register
- [ ] Test på mobil i privat fane

## SEO-opfølgning (når domænet er live)

1. **Google Search Console:**
   - Gå til https://search.google.com/search-console
   - Tilføj property `https://www.crediwire.com`
   - Bekræft via DNS TXT-record (Cloudflare guider dig)
   - Submit sitemap: `https://www.crediwire.com/sitemap.xml`

2. **Verificér i Search Console at:**
   - Sitet bliver crawlet
   - Der ikke er fejl
   - Mobile usability er OK

3. **Hvis du tidligere havde crediwire.com indekseret med andet indhold:**
   - Brug "Removed URLs"-værktøjet til at fjerne gamle URLs
   - Eller tilføj specifikke redirects i `_redirects` til at sende dem videre til crediwire.dk

## Vigtige noter

- **crediwire.dk** påvirkes IKKE af denne deployment. Det fortsætter med det eksisterende indhold og fungerer som arkiv/back-end for de juridiske sider.
- **app.crediwire.com** (login, register) er separat og påvirkes IKKE.
- Hvis du vil tilføje flere redirects (fx hvis crediwire.com tidligere havde specifikke sider), så åbn `_redirects` og tilføj linjer i formatet:
  ```
  /gammel-side  https://www.crediwire.dk/gammel-side  301
  ```
- Hver gang du ændrer noget lokalt: gentag deploy-trinnene.

## Hvis noget går galt

- Cloudflare Pages gemmer ALLE tidligere deployments. Du kan altid rulle tilbage til en forrige deployment via dashboard.
- Du kan rulle DNS tilbage ved at fjerne custom domain igen — det tager 5-15 minutter.

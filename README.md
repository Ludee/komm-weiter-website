# KommWeiter – Webseite

Prototyp der Projektwebseite, gebaut mit MkDocs Material und veröffentlicht über GitHub Pages.

## Lokal starten

```bash
pip install -r requirements.txt
mkdocs serve
```

## Aufbau

| Pfad                          | Inhalt                                                                         |
|-------------------------------|--------------------------------------------------------------------------------|
| `mkdocs.yml`                  | Navigation, Projektdaten für den Fußbereich (`extra`)                          |
| `docs/`                       | Seiten in Markdown; Startseite: Hero-Texte im Front Matter von `docs/index.md` |
| `docs/stylesheets/tokens.css` | Corporate Design: Farben und Schriften                                         |
| `docs/stylesheets/site.css`   | Layout; greift nur auf die Tokens zu                                           |
| `overrides/`                  | Templates für Startseite (`home.html`) und Fußbereich (`main.html`)            |
| `docs/aktuelles/posts/`       | Meldungen (Blog-Plugin)                                                        |

## Corporate Design einpflegen

1. Farben in `docs/stylesheets/tokens.css` unter `:root` ersetzen.
2. Schriftdateien (`.woff2`) nach `docs/assets/fonts/` legen, `@font-face`-Blöcke in `tokens.css` anpassen und `--kw-font-text` bzw. `--kw-font-display` setzen. Keine Google Fonts o. ä. einbinden.
3. Logo als `docs/assets/logo.svg`, Favicon als `docs/assets/favicon.svg` ersetzen.
4. Offizielles BMWE-Förderlogo nach `docs/assets/` legen und in `overrides/main.html` an der markierten Stelle einbinden.

## Veröffentlichung

Einmalig: Repository › Settings › Pages › Source: **GitHub Actions**. Danach baut jeder Push auf `main` die Seite (`.github/workflows/pages.yml`).

## Umzug auf komm-weiter.info

Die Seite ist so angelegt, dass beim Umzug keine Inhalte geändert werden müssen:

- `site_url` wird beim Build aus GitHub Pages übernommen und folgt der eigenen Domain automatisch.
- Alle internen Links sind relativ; keine Pfade mit Repository-Namen in Markdown oder Templates schreiben.

Schritte:

1. DNS beim Domain-Anbieter: für `komm-weiter.info` A-Records auf `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`; für `www` einen CNAME auf `<organisation>.github.io`.
2. Settings › Pages › Custom domain: `komm-weiter.info` eintragen, nach der DNS-Prüfung „Enforce HTTPS“ aktivieren. Eine `CNAME`-Datei ist beim Deployment über Actions nicht nötig.
3. Workflow erneut ausführen. Aufrufe der alten github.io-Adresse leitet GitHub auf die Domain um.
4. Domain zusätzlich in den Organisationseinstellungen verifizieren (schützt vor Übernahme durch fremde Repositories).

Soll die Seite später auf einen anderen Server (z. B. bei einem Partnerinstitut), genügt `mkdocs build` mit gesetzter Variable `SITE_URL`; das Ergebnis in `site/` ist statisch.

## Hinweise zur Kompatibilität

- `mkdocs` ist auf `<2` festgelegt: MkDocs 2.0 bricht Plugins und Theme-Overrides.
- Templates und Plugins sind so gewählt, dass ein späterer Wechsel zu Zensical (`theme.variant: classic`) möglich bleibt: keine `hooks`, keine eigenen Python-Plugins, Blog- und Suche-Plugin werden von Zensical unterstützt.

# REVIER – Statische Website

Professionelle Neuauflage der Website für **REVIER Alu Stahl Glas Service GmbH** (Stoob) als rein statische Seite: HTML, CSS, lokales JavaScript, lokale Bilder und Schriften.

**Version:** `1.1.1` (siehe [`VERSION`](VERSION), Git-Tag `v1.1.1`)
**Changelog:** [CHANGELOG.md](CHANGELOG.md)
**Security:** [SECURITY.md](SECURITY.md)  
**Install & Update:** [docs/DEPLOY.md](docs/DEPLOY.md)  
**Quelle der Inhalte:** [http://www.re-vier.at](http://www.re-vier.at) (Stand Recherche 2026).  
**Repository:** [github.com/WPreier/Re-Vier](https://github.com/WPreier/Re-Vier)  
**Lokaler Sync-Ordner:** Google Drive `Meine Ablage/Re-Vier/re-vier/theweb`

## Server (Install / Update)

Vollständige Anleitung für neuen Server und Updates: **[`docs/DEPLOY.md`](docs/DEPLOY.md)**  
(Document-Root immer auf `theweb/`, Nginx- oder Apache-Beispiel, HTTPS, Git-Update.)

## Schnellstart

Die Seite braucht keinen Build-Schritt. Alle Site-Dateien liegen in diesem Ordner (`theweb/`).

1. Ordner `theweb` öffnen  
2. `index.html` im Browser öffnen  
   oder lokal einen einfachen Server starten, z. B.:

```powershell
cd "c:\Users\Preier.PPC\Meine Ablage\Re-Vier\re-vier\theweb"
npx --yes serve -l 8080
```

Dann [http://localhost:8080](http://localhost:8080) öffnen.

## Struktur

```text
theweb/
  index.html          Start
  service.html        Service & Wartung
  ueber-uns.html      Über uns
  produktion.html     Produktion
  referenzen.html     Referenzen
  team.html           Team & Mitarbeiterkontakte
  impressum.html
  datenschutz.html
  css/styles.css
  js/main.js          Mobile-Navigation, Scroll-Header, Reveal
  assets/images/      Logos & Projektfotos (lokal)
  assets/fonts/       Self-hosted Outfit + Source Sans 3
  docs/CONTENT.md     Textquellen / Mapping Alt → Neu
  docs/DEPLOY.md      Installations- und Update-Anleitung
  VERSION
  SECURITY.md
  CHANGELOG.md
  .htaccess
  _headers
```

## Kontakt (bewusst ohne Formular)

Es gibt **kein** Kontaktformular. Kontakt nur über:

- `tel:`-Links (Büro und Mobil)
- `mailto:`-Links (`office@re-vier.at` sowie Mitarbeiter-Adressen)

## Design & Technik

- Markenfarben aus dem REVIER-Logo (Lila / Petrol), CTA-Akzent Orange  
- Responsive Layout, Sticky-Header, Mobile-Menü  
- Schriften lokal unter `assets/fonts/` (kein Google-Fonts-Request zur Laufzeit)  
- Keine Tracking-Skripte, keine Cookies durch die Seite selbst

## Sicherheit & Backup

- Keine Secrets oder Zugangsdaten im Repo  
- Backup über Git-Historie und Google-Drive-Desktop-Sync dieses Ordners  
- Live-Domain `re-vier.at`: HTTPS (Port 443) war zum Erstellungszeitpunkt nicht erreichbar; die Altseite lief nur über HTTP. Ein Go-Live der neuen Seite erfordert separate DNS-/Hosting-Umstellung.

## GitHub Pages (optional)

Kurz: Settings → Pages → Branch `main`, Ordner `/theweb`.  
Details und eigener Server: [`docs/DEPLOY.md`](docs/DEPLOY.md).

## Pflege

- Texte in den jeweiligen HTML-Dateien anpassen  
- Bilder unter `assets/images/` ersetzen und Pfade in HTML prüfen  
- Inhaltsmapping und Altquellen: [`docs/CONTENT.md`](docs/CONTENT.md)

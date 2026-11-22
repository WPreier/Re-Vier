# REVIER – Statische Website

Professionelle Neuauflage der Website für **REVIER Alu Stahl Glas Service GmbH** (Stoob) als rein statische Seite: HTML, CSS, lokales JavaScript, lokale Bilder und Schriften.

**Quelle der Inhalte:** [http://www.re-vier.at](http://www.re-vier.at) (Stand Recherche 2026).  
**Repository:** [github.com/WPreier/Re-Vier](https://github.com/WPreier/Re-Vier)  
**Lokaler Sync-Ordner:** Google Drive `Meine Ablage/Re-Vier/re-vier`

## Schnellstart

Die Seite braucht keinen Build-Schritt.

1. Ordner `re-vier` öffnen  
2. `index.html` im Browser öffnen  
   oder lokal einen einfachen Server starten, z. B.:

```powershell
cd "c:\Users\Preier.PPC\Meine Ablage\Re-Vier\re-vier"
python -m http.server 8080
```

Dann [http://localhost:8080](http://localhost:8080) öffnen.

## Struktur

```text
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

1. Repo-Settings → Pages  
2. Source: Branch `main`, Ordner `/` (Root)  
3. Speichern – die Site ist dann unter  
   `https://wpreier.github.io/Re-Vier/` erreichbar (oder Custom Domain nach DNS-Setup)

Pages ist **nicht** automatisch aktiviert; bei Bedarf manuell einschalten.

## Pflege

- Texte in den jeweiligen HTML-Dateien anpassen  
- Bilder unter `assets/images/` ersetzen und Pfade in HTML prüfen  
- Inhaltsmapping und Altquellen: [`docs/CONTENT.md`](docs/CONTENT.md)

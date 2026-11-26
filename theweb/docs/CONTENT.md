# Inhaltsquellen – Mapping Alt → Neu

**Site-Version:** 1.1.1  


**Pfad im Repo:** `theweb/` (seit v1.1.1)

Quelle: WordPress-Site `http://www.re-vier.at` (Theme „First“, Abruf über HTTP).

| Neue Datei | Alte URL / Seite | Inhalt |
|---|---|---|
| `index.html` | `/` und Beitrag „Herzlich Willkommen“ (`?p=54`) | Willkommenstext, Leistungs-Teaser, Referenz-Teaser |
| `service.html` | `?page_id=2` Service & Wartung | 24h-Service, Geschäftsbereiche, Mobilnummern |
| `ueber-uns.html` | `?page_id=34` Über uns | Schwerpunkte + Differenzierung |
| `produktion.html` | `?page_id=30` Produktion | Angebot + Kooperation TESIGN |
| `referenzen.html` | `?page_id=32` Referenzen | Referenzliste + lokale Projektfotos |
| `team.html` | `?page_id=118` Team + `?page_id=121` Kontaktdaten Mitarbeiter | Firmendaten, Alexander Stibi, Ing. Rudolf Stibi |
| `impressum.html` | *(neu)* | Aus Firmendaten der Altseite; Registerdaten ggf. ergänzen |
| `datenschutz.html` | *(neu)* | Statische Site, mailto/tel only, lokale Fonts |

## Firmendaten

- **Name:** REVIER Alu Stahl Glas Service GmbH  
- **Adresse:** Fabrikstraße 12, A-7344 Stoob  
- **Telefon:** +43 (0) 2612 / 43852  
- **E-Mail:** office@re-vier.at  

### Mitarbeiter (öffentlich wie auf der Altseite)

- **Alexander Stibi** – T Büro, M +43 664 3941321, a.stibi@re-vier.at  
- **Ing. Rudolf Stibi** – T Büro, M +43 664 2802826, r.stibi@re-vier.at  

## Bilder (lokal unter `assets/images/`)

Herkunft: `http://www.re-vier.at/wp-content/uploads/…`

| Datei | Verwendung |
|---|---|
| `logo_revier.jpg` | Hero (Marke + Fassade) |
| `REVIER_Logo_02.jpg` | Header-Logo |
| `cropped-logo_revier_01.jpg` | Favicon-Quelle |
| `produktion.jpg` | Produktion |
| `TZ-Guessing.jpg` | Start / Referenzen |
| `Raiffeisenlandesbank-BGLD.jpg` / `Raiffeisenlandesblank-BGLD.jpg` | Referenzen |
| `Ikea-SCS.jpg` / `Ikea-SCS2.jpg` | Referenzen |
| `Albert-Schulz-Eishalle.jpg` / `…2.jpg` | Referenzen |
| `Glastausch.jpg` / `Glastausch-2.jpg` | Service / Referenzen |

## Nicht übernommen

- WordPress-Kontaktformular (`?page_id=76` – defekt: „Contact form not found“)  
- RSS, Comments, Captcha-Plugins, externe Theme-CSS/JS  
- Google Fonts zur Laufzeit (Fonts lokal gespiegelt)

## Hinweis Live-Hosting

Zum Zeitpunkt der Migration: HTTPS auf `www.re-vier.at:443` nicht erreichbar; HTTP Port 80 ok. Go-Live der neuen Site = Hosting + DNS getrennt von diesem Repo-Setup.

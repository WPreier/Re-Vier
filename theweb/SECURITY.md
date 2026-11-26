# Security – REVIER static site

**Version:** 1.1.1

## Maßnahmen in den HTML-Seiten

In jeder `.html`-Datei:

| Maßnahme | Umsetzung |
|---|---|
| Content-Security-Policy | `<meta http-equiv="Content-Security-Policy">` – nur `'self'` für Script/Style/Font/Img |
| Referrer-Policy | `<meta name="referrer" content="strict-origin-when-cross-origin">` |
| Keine Inline-Styles/-Scripts | Styles in `css/styles.css`, Script nur `js/main.js` |
| Kein Tracking / keine Drittanbieter | lokale Fonts und Bilder |
| Kontakt ohne Formular | nur `tel:` / `mailto:` |

Hinweis: Manche Header (z. B. `frame-ancestors`, HSTS, `X-Frame-Options`) wirken **nur als HTTP-Response-Header**, nicht zuverlässig über Meta-Tags. Deshalb zusätzlich Server-Configs.

## Server-Header-Dateien im Repo

| Datei | Einsatz |
|---|---|
| [`.htaccess`](.htaccess) | Apache / viele Shared-Hostings |
| [`_headers`](_headers) | Netlify, Cloudflare Pages u. ä. |

Enthalten u. a.: HSTS, `X-Content-Type-Options`, `X-Frame-Options`, CSP, Referrer-Policy, Permissions-Policy, COOP/CORP/COEP.

## Nginx (Beispiel)

Wenn der Server Nginx nutzt, in den `server { ... }`-Block der Site:

```nginx
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-Frame-Options "DENY" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Permissions-Policy "accelerometer=(), camera=(), geolocation=(), gyroscope=(), magnetometer=(), microphone=(), payment=(), usb=()" always;
add_header Cross-Origin-Opener-Policy "same-origin" always;
add_header Cross-Origin-Resource-Policy "same-origin" always;
add_header Cross-Origin-Embedder-Policy "credentialless" always;
add_header Content-Security-Policy "default-src 'self'; base-uri 'self'; object-src 'none'; script-src 'self'; style-src 'self'; img-src 'self' data:; font-src 'self'; connect-src 'self'; form-action 'self'; frame-ancestors 'none'; upgrade-insecure-requests" always;

root /var/www/re-vier/theweb;
index index.html;
location / {
  try_files $uri $uri/ =404;
}
```

Danach: `nginx -t && systemctl reload nginx`

## Server aktualisieren (Deploy)

1. **Neuen Stand holen** (auf dem Server oder lokal zum Upload):

```bash
git clone https://github.com/WPreier/Re-Vier.git
# oder in bestehendem Clone:
git fetch --tags
git checkout v1.1.1
# bzw. git pull origin main
```

2. **Document-Root** auf den Ordner `theweb/` zeigen lassen (nicht das Repo-Root).

3. **Je nach Webserver:**
   - **Apache:** `.htaccess` im Document-Root (`theweb/`) mitdeployen; `AllowOverride` / `mod_headers` aktiv.
   - **Nginx:** Snippet oben in die Site-Config; kein `.htaccess`.
   - **Netlify / Cloudflare Pages:** `_headers` liegt bereits in `theweb/`.
   - **GitHub Pages:** Limited Header-Support – Meta-CSP greift trotzdem; für volle Header besser Nginx/Apache/CDN davor.

4. **TLS/HTTPS** aktivieren (Zertifikat, z. B. Let’s Encrypt). HSTS erst einschalten, wenn HTTPS zuverlässig läuft. Die Rewrite-Regel in `.htaccess` bei Bedarf entkommentieren.

5. **Prüfen:** Browser-DevTools → Network → Response Headers; oder https://securityheaders.com

## Was absichtlich nicht in HTML-Meta steht

- `Strict-Transport-Security` – nur über HTTPS-Response sinnvoll  
- `Permissions-Policy`, COOP/CORP/COEP – Meta nicht zuverlässig  
- `X-Frame-Options` / CSP `frame-ancestors` – Clickjacking-Schutz nur per HTTP-Header  

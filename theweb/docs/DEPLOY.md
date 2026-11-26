# REVIER installieren und aktualisieren

Anleitung für einen **neuen Server** und für spätere **Updates**.  
Die auszuliefernde Website liegt im Ordner **`theweb/`** (nicht im Repo-Root).

- Repository: https://github.com/WPreier/Re-Vier  
- Site-Root: `theweb/`  
- Security-Details: [`../SECURITY.md`](../SECURITY.md)  
- Version: Datei [`../VERSION`](../VERSION) bzw. Git-Tag `vX.Y.Z`

---

## Voraussetzungen

- Linux-Server (oder vergleichbar) mit Webserver **Nginx** oder **Apache**
- `git` installiert
- Domain (z. B. `www.re-vier.at`) zeigt per DNS auf die Server-IP
- Für Produktion: TLS-Zertifikat (empfohlen: Let’s Encrypt / Certbot)

Keine Build-Tools nötig (kein Node/npm auf dem Server erforderlich).

---

## Neuinstallation

### 1. Code holen

```bash
sudo mkdir -p /var/www
sudo git clone https://github.com/WPreier/Re-Vier.git /var/www/Re-Vier
cd /var/www/Re-Vier
git checkout main
# optional feste Version:
# git checkout v1.1.1
```

Document-Root für den Webserver:

```text
/var/www/Re-Vier/theweb
```

Rechte (Beispiel, User `www-data` unter Debian/Ubuntu):

```bash
sudo chown -R www-data:www-data /var/www/Re-Vier
# oder nur lesbar für den Webserver-User belassen, je nach Policy
```

### 2a. Nginx einrichten

Beispiel-Config (ohne TLS zuerst, Port 80):

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name www.re-vier.at re-vier.at;

    root /var/www/Re-Vier/theweb;
    index index.html;

    # Security-Header (siehe auch SECURITY.md)
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-Frame-Options "DENY" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    add_header Permissions-Policy "accelerometer=(), camera=(), geolocation=(), gyroscope=(), magnetometer=(), microphone=(), payment=(), usb=()" always;
    add_header Cross-Origin-Opener-Policy "same-origin" always;
    add_header Cross-Origin-Resource-Policy "same-origin" always;
    add_header Cross-Origin-Embedder-Policy "credentialless" always;
    add_header Content-Security-Policy "default-src 'self'; base-uri 'self'; object-src 'none'; script-src 'self'; style-src 'self'; img-src 'self' data:; font-src 'self'; connect-src 'self'; form-action 'self'; frame-ancestors 'none'; upgrade-insecure-requests" always;

    location / {
        try_files $uri $uri/ =404;
    }

    # Keine Directory-Listings
    autoindex off;
}
```

Aktivieren und testen (Pfad der Site-Datei anpassen):

```bash
sudo ln -s /etc/nginx/sites-available/re-vier /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

TLS mit Certbot (nach erfolgreichem HTTP):

```bash
sudo apt install certbot python3-certbot-nginx   # Debian/Ubuntu
sudo certbot --nginx -d www.re-vier.at -d re-vier.at
```

**Hinweis:** HSTS erst nutzen, wenn HTTPS zuverlässig funktioniert. Bei Certbot werden HTTPS-Blöcke typischerweise ergänzt; die `add_header`-Zeilen ggf. auch im `listen 443 ssl`-Server übernehmen.

### 2b. Apache einrichten

```apache
<VirtualHost *:80>
    ServerName www.re-vier.at
    ServerAlias re-vier.at
    DocumentRoot /var/www/Re-Vier/theweb

    <Directory /var/www/Re-Vier/theweb>
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/re-vier-error.log
    CustomLog ${APACHE_LOG_DIR}/re-vier-access.log combined
</VirtualHost>
```

Module und Site:

```bash
sudo a2enmod headers rewrite ssl
sudo a2ensite re-vier.conf
sudo apache2ctl configtest
sudo systemctl reload apache2
```

Die Datei **`theweb/.htaccess`** liefert die Security-Header, sofern `AllowOverride All` und `mod_headers` aktiv sind.

TLS:

```bash
sudo apt install certbot python3-certbot-apache
sudo certbot --apache -d www.re-vier.at -d re-vier.at
```

Nach stabilem HTTPS in `.htaccess` optional den HTTPS-Redirect entkommentieren.

### 3. Erste Prüfung

- Im Browser: `https://www.re-vier.at/` (oder IP/Hostname)
- Seiten: Start, Service, Team, Impressum
- Response-Header prüfen (DevTools → Network) oder https://securityheaders.com
- Version: in HTML-Quelle `<meta name="version" …>` bzw. Datei `theweb/VERSION`

---

## Update (bestehende Installation)

Auf dem Server im Clone:

```bash
cd /var/www/Re-Vier
sudo git fetch --tags origin
sudo git status
```

### Variante A – immer neueste `main`

```bash
sudo git pull origin main
```

### Variante B – feste Release-Version (empfohlen)

```bash
sudo git fetch --tags origin
sudo git checkout v1.1.1
# später z. B.:
# sudo git checkout v1.1.2
```

Aktuelle Tags: https://github.com/WPreier/Re-Vier/tags  
Versionsstand lokal: `cat theweb/VERSION`

### Nach dem Update

- **Nginx:** meist nichts weiter nötig (nur Dateien). Bei Config-Änderung: `sudo nginx -t && sudo systemctl reload nginx`
- **Apache:** meist nichts weiter; bei `.htaccess`-Änderung reicht oft Reload nicht mal – Datei gilt sofort. Bei VHost-Änderung: `sudo systemctl reload apache2`
- Browser-Hard-Reload / Cache prüfen
- Kurz smoke-testen: Startseite, eine Unterseite, Kontakt-Links

### Bei lokalen Änderungen auf dem Server (Konflikt)

```bash
cd /var/www/Re-Vier
sudo git stash          # nur falls nötig
sudo git pull origin main
# oder checkout Tag
```

Produktiv idealerweise **keine** manuellen Edits im Deploy-Verzeichnis – Änderungen im Repo, dann pull/checkout.

---

## Alternative: Dateien ohne Git kopieren

Wenn auf dem Server kein Git erlaubt ist:

1. Lokal oder CI: Release/ZIP von GitHub laden oder `theweb/` als Archiv packen  
2. Inhalt von **`theweb/`** nach Document-Root hochladen (FTP/SFTP/rsync)  
3. Bei Apache `.htaccess` mitkopieren; bei Nginx Header in der Server-Config pflegen  

Beispiel rsync:

```bash
rsync -av --delete ./theweb/ user@server:/var/www/Re-Vier/theweb/
```

---

## GitHub Pages (optional, ohne eigenen Server)

1. Repo → Settings → Pages  
2. Branch `main`, Ordner **`/theweb`**  
3. Speichern → URL z. B. `https://wpreier.github.io/Re-Vier/`

Update = Push auf `main` (bzw. Tag + Merge). Volle Security-Header sind auf GitHub Pages eingeschränkt; Meta-CSP in den HTML-Dateien greift trotzdem. Für Domain `re-vier.at` Custom Domain + DNS laut GitHub-Doku.

---

## Checkliste Neuinstallation

1. [ ] DNS A/AAAA auf Server  
2. [ ] Repo nach `/var/www/Re-Vier` geklont  
3. [ ] Document-Root = `…/theweb`  
4. [ ] Nginx- oder Apache-VHost aktiv  
5. [ ] Security-Header (Nginx-Config oder `.htaccess`)  
6. [ ] HTTPS (Certbot)  
7. [ ] Seiten und Header geprüft  
8. [ ] Backup/Monitoring nach Bedarf  

## Checkliste Update

1. [ ] `git fetch` / Tag oder `pull`  
2. [ ] `theweb/VERSION` prüfen  
3. [ ] Webserver reload nur wenn Config geändert  
4. [ ] Smoke-Test im Browser  

---

## Hilfe / Dateien im Repo

| Datei | Inhalt |
|---|---|
| [`DEPLOY.md`](DEPLOY.md) (diese Datei) | Install & Update |
| [`../SECURITY.md`](../SECURITY.md) | Header, CSP, Security-Maßnahmen |
| [`../README.md`](../README.md) | Überblick Site |
| [`CONTENT.md`](CONTENT.md) | Textquellen / Mapping |
| [`../CHANGELOG.md`](../CHANGELOG.md) | Versionshistorie |

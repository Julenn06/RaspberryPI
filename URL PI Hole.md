# Listas de Bloqueo para Pi-hole

Esta es una colección curada de listas de bloqueo (blocklists) recomendadas para Pi-hole. Estas listas ayudan a bloquear anuncios, trackers, malware, phishing y contenido malicioso en toda tu red.

---

## Tabla de Contenidos

1. [Cómo Agregar las Listas](#cómo-agregar-las-listas)
2. [Categorías de Listas](#categorías-de-listas)
3. [Lista Completa de URLs](#lista-completa-de-urls)

---

## Cómo Agregar las Listas

### Método 1: Interfaz Web

1. Accede a tu panel de Pi-hole: `http://[Tu_IP]/admin`
2. Ve a **Group Management** → **Adlists**
3. Copia y pega las URLs una por una en el campo "Address"
4. Haz clic en **Add**
5. Una vez agregadas todas las listas, ve a **Tools** → **Update Gravity**
6. Haz clic en **Update** para aplicar los cambios

### Método 2: Línea de Comandos

```bash
sqlite3 /etc/pihole/gravity.db "INSERT INTO adlist (address, enabled, comment) VALUES ('URL_AQUI', 1, 'Descripción');"
pihole -g
```

---

## Categorías de Listas

### Publicidad y Tracking
Bloquean anuncios publicitarios y rastreadores que recopilan información sobre tu navegación.

### Malware y Phishing
Protegen contra sitios web maliciosos, intentos de phishing y distribución de malware.

### Privacidad
Bloquean servicios de seguimiento y telemetría de empresas de tecnología.

### Contenido Específico
- **Smart TV**: Bloquea telemetría y anuncios de televisores inteligentes
- **Redes Sociales**: Bloquea rastreadores de Facebook y otras plataformas
- **Criptomonedas**: Previene minería de criptomonedas sin consentimiento
- **Contenido Adulto**: Filtra contenido no apto para menores

---

## Lista Completa de URLs

### Listas Principales y Combinadas

```text
# Steven Black - Lista unificada de hosts
https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts

# Steven Black - Extensión con noticias falsas, apuestas y contenido adulto
https://raw.githubusercontent.com/StevenBlack/hosts/master/alternates/fakenews-gambling-porn/hosts

# Steven Black - Extensión con noticias falsas
https://raw.githubusercontent.com/StevenBlack/hosts/master/alternates/fakenews/hosts

# Hagezi DNS Blocklists - Pro
https://raw.githubusercontent.com/hagezi/dns-blocklists/main/domains/pro.txt
```

### Tracking y Privacidad

```text
# Disconnect - Simple Tracking
https://s3.amazonaws.com/lists.disconnect.me/simple_tracking.txt

# EasyList - EasyPrivacy
https://easylist.to/easylist/easyprivacy.txt

# Firebog - Easyprivacy
https://v.firebog.net/hosts/Easyprivacy.txt

# Hostfiles - First-party Trackers
https://hostfiles.frogeye.fr/firstparty-trackers-hosts.txt
```

### Publicidad

```text
# Disconnect - Simple Ad
https://s3.amazonaws.com/lists.disconnect.me/simple_ad.txt

# AdAway
https://adaway.org/hosts.txt

# EasyList
https://easylist.to/easylist/easylist.txt

# Firebog - AdguardDNS
https://v.firebog.net/hosts/AdguardDNS.txt

# Firebog - Easylist
https://v.firebog.net/hosts/Easylist.txt

# Firebog - Prigent Ads
https://v.firebog.net/hosts/Prigent-Ads.txt

# Firebog - Admiral
https://v.firebog.net/hosts/Admiral.txt

# Firebog - W3KBL
https://v.firebog.net/hosts/static/w3kbl.txt

# Anudeep ND - Adservers
https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt

# Peter Lowe - Adservers
https://pgl.yoyo.org/adservers/serverlist.php?hostformat=hosts&showintro=0&mimetype=plaintext

# FadeMind - UncheckyAds
https://raw.githubusercontent.com/FadeMind/hosts.extras/master/UncheckyAds/hosts

# FadeMind - 2o7Net
https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.2o7Net/hosts

# BigDargon - HostsVN
https://raw.githubusercontent.com/bigdargon/hostsVN/master/hosts

# AdAway Official
https://raw.githubusercontent.com/AdAway/adaway.github.io/master/hosts.txt
```

### Malware y Seguridad

```text
# NoTracking - Hosts Blocklists
https://raw.githubusercontent.com/notracking/hosts-blocklists/master/hostnames.txt

# URLhaus - Malware Distribution
https://urlhaus.abuse.ch/downloads/hostfile/

# Firebog - Prigent Malware
https://v.firebog.net/hosts/Prigent-Malware.txt

# Firebog - Prigent Crypto
https://v.firebog.net/hosts/Prigent-Crypto.txt

# Firebog - RPiList Malware
https://v.firebog.net/hosts/RPiList-Malware.txt

# DandelionSprout - Anti-Malware
https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareHosts.txt

# FadeMind - Risk Hosts
https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Risk/hosts

# NoTrack - Malware
https://gitlab.com/quidsup/notrack-blocklists/raw/master/notrack-malware.txt

# Spam404 - Main Blacklist
https://raw.githubusercontent.com/Spam404/lists/master/main-blacklist.txt

# Cyberhost - Malware
https://lists.cyberhost.uk/malware.txt

# Malware Filter - Phishing
https://malware-filter.gitlab.io/malware-filter/phishing-filter-hosts.txt
```

### Phishing y Scams

```text
# Phishing Army - Extended Blocklist
https://phishing.army/download/phishing_army_blocklist_extended.txt

# Firebog - RPiList Phishing
https://v.firebog.net/hosts/RPiList-Phishing.txt

# Jarelllama - Scam Blocklist
https://raw.githubusercontent.com/jarelllama/Scam-Blocklist/main/lists/wildcard_domains/scams.txt
```

### Privacidad y Telemetría

```text
# Crazy-max - Windows Spy Blocker
https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt

# Polish Filters Team - KADhosts
https://raw.githubusercontent.com/PolishFiltersTeam/KADhosts/master/KADhosts.txt

# Matomo - Referrer Spam Blacklist
https://raw.githubusercontent.com/matomo-org/referrer-spam-blacklist/master/spammers.txt

# AssoEchap - Stalkerware Indicators
https://raw.githubusercontent.com/AssoEchap/stalkerware-indicators/master/generated/hosts
```

### Smart TV y Dispositivos

```text
# Perflyst - SmartTV Blocklist
https://raw.githubusercontent.com/Perflyst/PiHoleBlocklist/master/SmartTV.txt

# Perflyst - Amazon FireTV
https://raw.githubusercontent.com/Perflyst/PiHoleBlocklist/master/AmazonFireTV.txt
```

### Redes Sociales

```text
# Anudeep ND - Facebook
https://raw.githubusercontent.com/anudeepND/blacklist/master/facebook.txt

# JmDugan - Facebook All
https://raw.githubusercontent.com/jmdugan/blocklists/master/corporations/facebook/all

# BlockListProject - Facebook
https://raw.githubusercontent.com/blocklistproject/Lists/master/facebook.txt

# BlockListProject - TikTok
https://raw.githubusercontent.com/blocklistproject/Lists/master/tiktok.txt
```

### Criptomonedas (Cryptojacking)

```text
# Hoshsadiq - NoCoin List
https://raw.githubusercontent.com/hoshsadiq/adblock-nocoin-list/master/hosts.txt

# BlockListProject - Crypto
https://raw.githubusercontent.com/blocklistproject/Lists/master/crypto.txt
```

### Contenido Adulto

```text
# Firebog - Prigent Adult
https://v.firebog.net/hosts/Prigent-Adult.txt

# ChadMayfield - Porn Top1M
https://raw.githubusercontent.com/chadmayfield/my-pihole-blocklists/master/lists/pi_blocklist_porn_top1m.list
```

### Listas Complementarias

```text
# FadeMind - Spam Hosts
https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Spam/hosts

# MVPS Hosts
https://winhelp2002.mvps.org/hosts.txt

# SomeoneWhoCares - Hosts Zero
https://someonewhocares.org/hosts/zero/hosts

# SomeoneWhoCares - Hosts
https://someonewhocares.org/hosts/hosts

# Mitchell Krogza - Badd Boyz Hosts
https://raw.githubusercontent.com/mitchellkrogza/Badd-Boyz-Hosts/master/hosts

# Hectorm Mirror - AdAway
https://raw.githubusercontent.com/hectorm/hmirror/master/data/adaway.org/list.txt

# Hectorm Mirror - MVPS
https://raw.githubusercontent.com/hectorm/hmirror/master/data/winhelp2002.mvps.org/list.txt

# RooneyMcNibNug - SNAFU
https://raw.githubusercontent.com/RooneyMcNibNug/pihole-stuff/master/SNAFU.txt
```

### BlockListProject - Colección Completa

```text
https://raw.githubusercontent.com/blocklistproject/Lists/master/ads.txt
https://raw.githubusercontent.com/blocklistproject/Lists/master/malware.txt
https://raw.githubusercontent.com/blocklistproject/Lists/master/phishing.txt
https://raw.githubusercontent.com/blocklistproject/Lists/master/scam.txt
https://raw.githubusercontent.com/blocklistproject/Lists/master/tracking.txt
https://raw.githubusercontent.com/blocklistproject/Lists/master/ransomware.txt
https://raw.githubusercontent.com/blocklistproject/Lists/master/abuse.txt
https://raw.githubusercontent.com/blocklistproject/Lists/master/fraud.txt
```

---

## Recomendaciones de Uso

### Configuración Básica (Recomendada para principiantes)
- Steven Black - Lista unificada
- AdGuard DNS
- Hagezi DNS Blocklists - Pro
- BlockListProject - Ads, Malware, Phishing, Tracking

### Configuración Avanzada (Mayor protección)
Todas las listas anteriores más:
- Listas de privacidad específicas
- Bloqueo de Smart TV
- Bloqueo de redes sociales (si es necesario)
- Protección contra cryptojacking

### Configuración Familiar (Protección adicional)
Configuración avanzada más:
- Listas de contenido adulto
- Bloqueo de noticias falsas
- Listas de protección infantil

---

## Notas Importantes

- **Rendimiento**: Agregar muchas listas puede aumentar el uso de memoria. Para dispositivos con recursos limitados, comienza con las listas básicas.
- **Falsos Positivos**: Algunas listas pueden bloquear sitios legítimos. Usa la función de "Whitelist" de Pi-hole para desbloquear dominios específicos.
- **Actualización**: Las listas se actualizan automáticamente una vez por semana. Puedes cambiar la frecuencia en la configuración de Pi-hole.
- **Testing**: Después de agregar nuevas listas, prueba la navegación en tus dispositivos para detectar posibles problemas.

---

## Mantenimiento

### Actualizar las Listas Manualmente

**Interfaz Web:**
- Tools → Update Gravity → Update

**Línea de Comandos:**
```bash
pihole -g
```

### Ver Estadísticas

```bash
pihole -c
```

### Verificar Dominios Bloqueados

```bash
pihole -q ejemplo.com
```

---

## Recursos Adicionales

- [Firebog - The Big Blocklist Collection](https://firebog.net/)
- [BlockListProject](https://blocklist.site/)
- [Pi-hole Discourse Community](https://discourse.pi-hole.net/)
- [r/pihole en Reddit](https://www.reddit.com/r/pihole/)

---

**Última actualización:** Diciembre 2025  
**Total de listas:** 65+ listas de bloqueo  
**Dominios bloqueados (aprox.):** 5-10 millones dependiendo de la configuración
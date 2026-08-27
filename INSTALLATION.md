# UniFi Device Card – lokale Installation

Diese ZIP enthält die gebaute Home-Assistant-Frontendkarte aus dem aktuellen lokalen Entwicklungsstand.

1. `unifi-device-card.js` nach `/config/www/unifi-device-card.js` kopieren.
2. In Home Assistant **Einstellungen → Dashboards → Ressourcen** öffnen.
3. `/local/unifi-device-card.js` als **JavaScript-Modul** hinzufügen.
4. Home Assistant bzw. den Browser-Frontend-Cache neu laden.

Kartentyp: `custom:unifi-device-card`

Für eine reguläre HACS-Installation stattdessen das Repository
`https://github.com/bluenazgul/unifi-device-card` als benutzerdefiniertes
Dashboard-Repository hinzufügen. HACS importiert keine lokale ZIP-Datei.

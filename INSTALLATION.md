# Installation über HACS

Dieses Repository enthält eine Home-Assistant-Dashboardkarte. Sie wird in HACS
als **Dashboard** hinzugefügt, nicht als Integration.

1. HACS in Home Assistant öffnen.
2. **Dashboard** öffnen.
3. Rechts oben **⋮ → Benutzerdefinierte Repositories** auswählen.
4. Als Repository eintragen:
   `https://github.com/RAFd3v-HA/HACS-Unifi-Card`
5. Kategorie **Dashboard** auswählen und hinzufügen.
6. Nach **UniFi Device Card** suchen und **Herunterladen** wählen.
7. Home Assistant beziehungsweise den Browser-Cache neu laden.

HACS installiert `unifi-device-card.js` nach
`/config/www/community/HACS-Unifi-Card/`. Die Ressource ist anschließend unter
`/hacsfiles/HACS-Unifi-Card/unifi-device-card.js` verfügbar und wird von HACS
normalerweise automatisch registriert.

Kartentyp:

```yaml
type: custom:unifi-device-card
device_id: DEINE_DEVICE_ID
```

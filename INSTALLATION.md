# Installation über HACS

Dieses Repository enthält die **UniFi Device Card**. Es wird in HACS als
**Dashboard** hinzugefügt, nicht als Integration.

1. HACS in Home Assistant öffnen.
2. **Dashboard** öffnen.
3. Rechts oben **⋮ → Benutzerdefinierte Repositories** auswählen.
4. Als Repository eintragen:
   `https://github.com/RAFd3v-HA/HACS-Unifi-Card`
5. Kategorie **Dashboard** auswählen und hinzufügen.
6. Nach **UniFi Device Card** suchen und **Herunterladen** wählen.
7. Browser beziehungsweise Frontend neu laden.

HACS installiert `unifi-device-card.js` nach
`/config/www/community/HACS-Unifi-Card/`. Die Ressource ist anschließend unter
`/hacsfiles/HACS-Unifi-Card/unifi-device-card.js` verfügbar und wird von HACS
normalerweise automatisch registriert.

Kartentyp:

```yaml
type: custom:unifi-device-card
device_id: DEINE_DEVICE_ID
```

## Optionales Backend

Für automatische Client-zu-Port-Zuordnung, Client-VLANs, zuverlässige
Portzustände, WLAN-Bänder und Mesh-RSSI installiere zusätzlich das separate
Repository `https://github.com/RAFd3v-HA/HACS-Unifi-Card-Backend` in HACS als
**Integration**. Danach Home Assistant neu starten und unter
**Einstellungen → Geräte & Dienste → Integration hinzufügen** einmal
**UniFi Device Card Backend** hinzufügen.

Das Backend verlangt keine weiteren UniFi-Zugangsdaten und die Karte bleibt
auch ohne Backend vollständig nutzbar.

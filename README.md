# UniFi Device Card

Eine eigenständige Lovelace-Karte für Home Assistant, die UniFi-Switches,
Gateways, Access Points und Bridges als interaktive Geräteansicht darstellt.
Sie verbindet realistische Frontpanels mit Live-Telemetrie, Portdetails,
WLAN-Statistiken und optionalen Steuerfunktionen.

Die Karte arbeitet grundsätzlich mit den Entitäten der offiziellen
UniFi-Network-Integration von Home Assistant. Ein optionales Companion-Backend
ergänzt Live-Topologie und kontrollierte Direktaktionen. Die Karte bleibt auch
ohne dieses Backend benutzbar; backendabhängige Zusatzfunktionen werden dann
automatisch ausgeblendet.

## Inhaltsverzeichnis

- [Funktionsumfang](#funktionsumfang)
- [Datenquellen und Backend](#datenquellen-und-backend)
- [Voraussetzungen](#voraussetzungen)
- [Installation](#installation)
- [Karte hinzufügen](#karte-hinzufügen)
- [Konfiguration](#konfiguration)
- [Unterstützte Geräte](#unterstützte-geräte)
- [Erkennung und Darstellung](#erkennung-und-darstellung)
- [Sicherheit und Datenschutz](#sicherheit-und-datenschutz)
- [Fehlerbehebung](#fehlerbehebung)

## Funktionsumfang

### Premium-UI und Darstellung

- Responsive, berührungsfreundliche Oberfläche für Desktop, Tablet und Mobilgeräte
- Automatische Anpassung an Home-Assistant-Sections und schmale Kartenbreiten
- Darstellungsmodi `auto`, `light` und `dark`
- Modellabhängige weiße, silberne und dunkle Hardware-Oberflächen
- Einheitlicher Hardware-Frame für Produktansicht und interaktives Frontpanel
- Saubere Abstände, gemeinsame Rundungen und fließender Übergang zwischen Bild und Ports
- Konfigurierbare Farben für Karte, Titel, Telemetrie, Labels, Werte, Ports, APs und Buttons
- Einstellbare Hintergrundtransparenz
- Theme-basierte, integrierte oder vollständig eigene Button-Farben
- Eigene Größensteuerung für Ports und Access Points
- Tastaturbedienbare Ports und Steuerelemente mit sichtbaren Fokuszuständen
- Mehrsprachige Oberfläche in Deutsch, Englisch, Niederländisch, Französisch,
  Spanisch, Italienisch, Schwedisch, Dänisch, Norwegisch, Finnisch, Polnisch
  und Tschechisch

### Geräte-Header und Telemetrie

- Anzeigename sowie Modell- und Firmwarezeile
- Optional ausblendbarer Gerätename
- CPU-, Speicher- und Temperaturwerte als kompakte Telemetrie-Pills
- Optional ausblendbare Header-Telemetrie
- Statuschip für verbundene und bekannte Ports
- Unterscheidung zwischen vollständigem, teilweisem und unbekanntem Portstatus
- Kennzeichnung und Erläuterung unbekannter Portzustände im Status-Tooltip
- Klick auf Modell/Firmware öffnet die zugehörige Home-Assistant-Geräteseite
- Reboot-Schaltfläche, wenn eine passende Entität vorhanden ist
- Geräte-LED-Schalter für Switches und Access Points, wenn die offizielle
  UniFi-Integration eine passende `light.*`-Entität bereitstellt

### Switches und Gateways

- Modellabhängige RJ45-, SFP-, SFP+-, SFP28-, WAN- und Uplink-Anordnung
- Modellabhängige ein- bis vierreihige Layouts sowie Odd/Even-Portanordnung
- Spezielle Frontpanels für Desktop-, Rack-, Gateway-, Aggregation- und Ultra-Geräte
- Automatischer Fallback anhand der erkannten Portanzahl für unbekannte Switches
- Optionale 180-Grad-Drehung des Frontpanels
- Einstellbare Ports pro Reihe und Portgröße
- Manuelle oder automatische WAN-/WAN2-Zuordnung
- Manuell wählbare Spezialports
- Optionale eigene Portnamen für Tooltip und Detailüberschrift
- Visuelle Link-LEDs mit Geschwindigkeits- und PoE-Farben
- Getrennt konfigurierbares, rein visuelles Blinken für RJ45- und SFP-Links

### Portstatus und Portdetails

Direkt unter jedem Port können – sofern vorhanden – kompakt dargestellt werden:

- Linkstatus
- Linkgeschwindigkeit wie `100M`, `1G`, `2.5G` oder `10G`
- VLAN beziehungsweise Client-VLAN
- Name des verbundenen Clients
- PoE-Zustand

Ein Klick auf den Port öffnet die Detailansicht mit:

- Verbindungsstatus
- ausgehandelter Geschwindigkeit
- Port-VLAN oder Client-VLAN
- einem oder mehreren verbundenen Clients
- Clientname, IP-Adresse und VLAN
- PoE-Fähigkeit und PoE-Zustand
- aktueller PoE-Leistungsaufnahme
- RX-/TX-Telemetrie
- Port aktivieren oder deaktivieren
- PoE ein- oder ausschalten
- PoE-Port neu starten beziehungsweise Power Cycle

Die Clientinformation nutzt dabei die gesamte Breite der Detailansicht.
PoE-Zustand, PoE-Leistung und Power Cycle stehen auf ausreichend breiten Karten
gemeinsam in einer Zeile und werden auf sehr schmalen Karten automatisch
untereinander angeordnet.

Das Abschalten eines aktiven Ports erfordert eine Bestätigung. Bei
`dynamic_port_details: true` startet die Karte ohne Detailbereich; ein erneuter
Klick auf den ausgewählten Port blendet ihn wieder aus.

### Intelligente Linkerkennung

Die Karte kombiniert mehrere Signale, weil nicht jedes UniFi-Modell dieselben
Home-Assistant-Entitäten liefert:

- direkte Link-Entität
- Linkgeschwindigkeit
- erkannter Client am Port
- PoE-Leistungsaufnahme
- RX-/TX-Aktivität
- Live-Portstatus aus dem optionalen Backend
- eindeutige Uplink-Zuordnung eines verwalteten UniFi-Geräts

Ein positiver Geschwindigkeitswert wird nicht blind als aktiver Link gewertet.
Insbesondere vermeidet ein Ghost-Link-Schutz falsche 10-Mbit/s-Verbindungen.
Echte 10-Mbit/s-Ports können gezielt über `trust_link_speed_ports` freigegeben
werden. Bei einem bestätigten `no_link` wird keine alte ausgehandelte
Geschwindigkeit angezeigt.

### VLANs und Clients

- Native VLAN-, PVID- und VLAN-Attribute werden automatisch erkannt
- Client-VLAN wird vom Portprofil getrennt behandelt
- Kein doppeltes VLAN-Feld, wenn das VLAN bereits in der Clientzeile steht
- Automatische Zuordnung von kabelgebundenen Clients zum Switchport
- Anzeige mehrerer Clients, wenn der Controller diese eindeutig einem Port zuordnet
- Erkennung verwalteter UniFi-Geräte über deren kabelgebundenen Uplink
- Konservativer MAC-Table-Fallback für genau einen eindeutigen Client an einem
  aktiven Nicht-Uplink-Port
- Clients können dadurch auch ohne eigene Home-Assistant-Entität erscheinen
- Unsichere Mehrfach-MAC- oder Uplink-Zuordnungen werden bewusst nicht geraten

### PoE-Steuerung

- PoE-Fähigkeit wird aus Modelllayout, Home-Assistant-Entitäten und – falls
  vorhanden – Controllerdaten zusammengeführt
- PoE-Anzeige funktioniert auch bei Ports ohne aktiven Netzwerklink
- Leistungsaufnahme kann auch ohne schaltbare PoE-Entität angezeigt werden
- PoE-Toggle über eine passende Home-Assistant-Switch-Entität
- Power Cycle bevorzugt eine verwendbare offizielle Button-Entität
- Ist diese Entität nicht vorhanden oder nicht verfügbar, kann das optionale
  Backend kontrolliert auf die Controllerfunktion zurückfallen
- Power Cycle wird bei einem bestätigten, PoE-fähigen und aktivierten Port
  angeboten; Berechtigung, Uplink-Schutz und Live-Zustand werden beim Klick
  nochmals serverseitig geprüft
- Eine Ablehnung zeigt den konkreten Grund direkt unter den Portaktionen
- Direkte Power-Cycle- und Etherlighting-Aktionen sind gegen Doppelklicks
  geschützt und pro Switch serialisiert
- Unsichere Power-Cycle-Antworten werden nicht automatisch wiederholt

### Access Points und WLAN-Geräte

- Eigene AP-Kartenansicht mit Status, Uptime, Clients und optionalem Reboot
- Normales und kompaktes AP-Layout
- Einstellbare AP-Größe
- Modellabhängige Geometrien für runde APs, Wall/In-Wall, Mesh-Säulen,
  Outdoor-Geräte, Extender, Bridges und 5G Backup
- Kombinierte AP-/Switch-Ansicht für kompatible In-Wall-Geräte und Dream Wall
- Integrierte Ports können separat ein- oder ausgeblendet werden
- WLAN-Client-Gesamtzahl
- Gruppierung der Clients nach 2,4 GHz, 5 GHz und 6 GHz
- Clientnamen direkt innerhalb des jeweiligen Frequenzbands
- Eigene Gruppe für Clients ohne zuverlässig bestimmbares Band
- Keine Bandzuordnung nur anhand mehrdeutiger Kanalnummern

### Mesh-Signal

- Mesh-Signal wird nur angezeigt, wenn UniFi einen drahtlosen Uplink bestätigt
- Kabelgebundene APs werden nicht fälschlich als Mesh-Geräte dargestellt
- Automatische Suche nach RSSI-/Signal-Entitäten am Gerät
- Optional auswählbare Entität mit `mesh_signal_entity`
- Unterstützung für negative dBm-Werte, positive RSSI-Beträge und Prozentwerte
- Farbcodierte Skala von schwach bis ausgezeichnet
- Anzeige des Uplink-Geräts, wenn diese Information vorhanden ist

### Produktansichten

- Produktbilder sind standardmäßig deaktiviert und können pro Karte aktiviert werden
- Exakte Zuordnung erfolgt über die normalisierte Modellkennung, nicht über den Gerätenamen
- Eigene Bild-URL über HTTPS, `/local/`, `/api/` oder Data-URL möglich
- Cloud Gateway Fiber und Express 7 besitzen eine umschaltbare Vorder-/Rückansicht
- Ultra 60W und Ultra 210W verwenden bewusst dieselbe Gehäuseansicht, weil sich
  die Gerätehardware nur durch Netzteil beziehungsweise PoE-Budget unterscheidet
- Das Live-Frontpanel bleibt unabhängig vom Produktbild vollständig interaktiv
- Bei Nutzung der integrierten Produktansichten werden Bilder erst nach
  Aktivierung von der Hersteller-CDN geladen

Exakte Produktansichten sind derzeit für folgende Modellgruppen hinterlegt:

- USW Flex 2.5G 8 PoE
- USW Flex
- USW Flex Mini
- U7 Pro Wall
- USW Aggregation
- UK Ultra
- UAP AC Pro
- Device Bridge Switch
- USW 16 PoE
- USW Pro 48
- USW Pro Max 16 PoE
- U7 Pro
- Cloud Gateway Fiber
- USW Ultra 60W
- USW Ultra 210W
- U6 Mesh
- UniFi Express
- UniFi Express 7

### Etherlighting

Kompatible Switches können über das optionale Backend zusätzliche
Etherlighting-Einstellungen anzeigen:

- LED-/Etherlighting-Modus
- Zuordnung nach Geschwindigkeit oder Netzwerk
- Verhalten wie dauerhaft oder atmend
- Helligkeit

Die Steuerelemente erscheinen nur, wenn der Controller die native Fähigkeit
für das konkrete Gerät meldet. Änderungen werden pro Gerät serialisiert und
anschließend durch erneutes Lesen bestätigt. Nicht unterstützte Geräte behalten
die normale Kartenansicht ohne leere Bedienelemente.

### Visueller Karteneditor

Die Karte besitzt einen umfangreichen Home-Assistant-Editor:

- Auswahl des UniFi-Geräts
- Anzeigename und Header-Telemetrie
- Produktbild und eigene Bild-URL
- Panel- und Detailverhalten
- Dark-/Light-/Auto-Modus
- Portgröße, Portanordnung, Spezialports, WAN und WAN2
- AP-Layout, AP-Größe und integrierte Ports
- Mesh-Signal-Entität
- LED-Blinkanimation
- Farb- und Button-Editor mit Vorschau und Einzel-Reset
- Anzeige fehlender oder deaktivierter Entitätstypen
- Explizite Aktion zum Aktivieren deaktivierter Link-Speed-Sensoren
- Status von Companion-Backend und ausgewählter UniFi-Datenquelle
- Direkter Sprung zur Backend-Konfiguration

Der Editor verändert keine Dashboard-Karten außerhalb der gerade bearbeiteten
Karte. Das Aktivieren deaktivierter Sensoren erfolgt nur nach einem bewussten
Klick und lädt anschließend ausschließlich den betroffenen UniFi-Eintrag neu.
Einige Expertenoptionen bleiben bewusst YAML-basiert, darunter `rotate180`,
`port_name`, manuelle `port_<n>`-Zuordnungen, `log_level` und `debug`.

## Datenquellen und Backend

### Entity-basierter Standardbetrieb

Ohne Backend liest die Karte die registrierten Home-Assistant-Geräte,
Entitätsmetadaten und Zustände der UniFi-Network-Integration. Sie erkennt dabei
auch umbenannte und lokalisierte Entitäten anhand stabiler IDs, Translation Keys,
Gerätezuordnung und Attribute.

Dieser Modus liefert – abhängig von Modell, Firmware und aktivierten Entitäten –
unter anderem:

- Geräteidentität, Firmware und Online-Status
- CPU, Speicher und Temperatur
- Portstatus und Geschwindigkeit
- RX/TX
- PoE-Schalter, PoE-Leistung und Power-Cycle-Buttons
- Uptime und Clientanzahl
- LED- und Reboot-Entitäten
- WLAN-Client-Tracker

### Optionales Companion-Backend

Das Companion-Backend stellt authentifizierte Home-Assistant-WebSocket-Befehle
bereit. Es ergänzt die Karte um Live-Topologie und Aktionen, ohne Zugangsdaten
an den Browser zu übertragen.

Bei der Einrichtung stehen zwei Quellen zur Wahl:

1. **Offizielle UniFi-Integration verwenden – empfohlen**

   Das Backend nutzt die bereits von Home Assistant geladene Controller-Sitzung.
   Es werden keine zusätzlichen Zugangsdaten gespeichert und keine zweite
   Sitzung aufgebaut.

2. **Separaten direkten Login verwenden – optionaler Fallback**

   Das Backend validiert Host, Port, Benutzer, Passwort und Standort. Diese
   Daten bleiben ausschließlich im Home-Assistant-Config-Entry. Die eigene
   Sitzung existiert nur, solange dieser Modus ausgewählt ist.

Die gewählte Quelle ist strikt: Ein explizit gewählter direkter Login fällt
nicht still auf eine andere Controller-Sitzung zurück. Vorhandene ältere
Backend-Einträge werden automatisch auf die empfohlene offizielle Quelle
migriert.

Das Backend liefert:

- Live-Porttabelle und Controller-Portnamen
- Linkstatus, Geschwindigkeit und VLAN
- PoE-Fähigkeit, Modus, Zustand und Leistung
- Clientname, IP, MAC, VLAN und Linkrate
- verwaltete UniFi-Uplink-Geräte
- konservative MAC-Table-Clients ohne Home-Assistant-Entität
- WLAN-Zuordnung und Frequenzband
- Mesh-Uplink und RSSI
- Etherlighting-Fähigkeit und -Zustand
- kontrollierten PoE-Power-Cycle

Datenschutzfreundliche Diagnosen enthalten nur Status, Fähigkeiten und
Objektanzahlen. Zugangsdaten, Host, Clientnamen, IP-Adressen und MAC-Adressen
werden nicht ausgegeben.

## Voraussetzungen

- Home Assistant mit eingerichteter offizieller UniFi-Network-Integration
- Das gewünschte UniFi-Gerät erscheint unter **Einstellungen → Geräte & Dienste**
- Für möglichst vollständige Portdaten sollten passende UniFi-Entitäten aktiviert sein
- Optional: UniFi Device Card Backend für Live-Topologie, Clients ohne
  Home-Assistant-Entität, Etherlighting und den PoE-Fallback

## Installation

### HACS

1. HACS öffnen.
2. **Dashboard** auswählen; in älteren HACS-Versionen heißt dieser Bereich
   **Frontend**.
3. Nach **UniFi Device Card** suchen und die Karte installieren.
4. Browser beziehungsweise Frontend neu laden.

Falls die Karte als benutzerdefinierte Quelle installiert wird:

1. Im HACS-Menü **Benutzerdefinierte Repositories** öffnen.
2. Die URL dieses Repositories eintragen.
3. Kategorie **Dashboard** wählen.
4. **UniFi Device Card** installieren.

HACS registriert die Datei `unifi-device-card.js` als Frontend-Ressource.

### Manuell

1. `unifi-device-card.js` nach `/config/www/unifi-device-card.js` kopieren.
2. Unter **Einstellungen → Dashboards → Ressourcen** hinzufügen:
   - URL: `/local/unifi-device-card.js`
   - Typ: `JavaScript-Modul`
3. Browser vollständig neu laden.

### Optionales Backend

Das Backend ist nicht Bestandteil des Frontend-HACS-Pakets. Es wird als
separates Home-Assistant-Integrationspaket bereitgestellt. Im entpackten
Backend-Paket muss der vollständige Ordner
`custom_components/unifi_device_card` vorhanden sein. Bei manueller
Installation wird dieser Ordner unverändert nach
`/config/custom_components/unifi_device_card` kopiert, sodass dort unter
anderem `manifest.json`, `config_flow.py` und `websocket_api.py` liegen.
Für den manuellen Upload ist das bereitgestellte Archiv
`HACS-Unifi-Card-Backend-repository-upload.zip` vorgesehen.

Danach:

1. Home Assistant neu starten.
2. **Einstellungen → Geräte & Dienste → Integration hinzufügen** öffnen.
3. **UniFi Device Card Backend** auswählen.
4. Empfohlen: vorhandene offizielle UniFi-Integration als Quelle wählen.
5. Optional: direkten Login nur dann wählen, wenn die offizielle Quelle die
   benötigten Daten nicht bereitstellt.

Über **Konfigurieren** werden Status und Diagnoseeinstellung angezeigt. Über
**Neu konfigurieren** kann die Quelle gewechselt oder ein direkter Login
erneuert werden.

## Karte hinzufügen

### Visueller Editor

1. Dashboard bearbeiten.
2. **Karte hinzufügen** wählen.
3. Nach **UniFi Device Card** suchen.
4. UniFi-Gerät auswählen.
5. Darstellung und Funktionen im Editor konfigurieren.

### Minimale YAML-Konfiguration

```yaml
type: custom:unifi-device-card
device_id: DEINE_HOME_ASSISTANT_DEVICE_ID
```

### Vollständiges Beispiel

```yaml
type: custom:unifi-device-card
device_id: DEINE_HOME_ASSISTANT_DEVICE_ID
name: Netzwerk-Switch
show_name: true
show_telemetry: true
appearance_mode: auto

show_product_image: true
product_image_url: /local/unifi/mein-switch.png
show_panel: true
dynamic_port_details: true

background_color: "#17191d"
background_opacity: 100
title_color: "#f5f7fb"
telemetry_color: "#d1d5db"
label_color: "#9ca3af"
value_color: "#f3f4f6"
meta_color: "#94a3b8"
port_label_color: "#9ca3af"
special_port_label_color: "#60a5fa"

button_theme_style: true
button_default_color: true
button_color: "#0090d9"
button_text_color: "#ffffff"
button_secondary_color: "#262b34"
button_secondary_text_color: "#e2e8f0"
button_border_color: "#3b4350"

port_size: 36
ports_per_row: 8
force_sequential_ports: false
rotate180: false
port_led_blink: false
port_led_blink_rj45: true
port_led_blink_sfp: true
port_led_blink_speed_rj45: 0.2
port_led_blink_speed_sfp: 0.2

edit_special_ports: false
special_ports: [1, 2, 9]
wan_port: auto
wan2_port: none
trust_link_speed_ports: [3, 7]

port_name:
  1: Uplink
  3: Access Point

device_layout: combined
integrated_ports: true
ap_compact_view: false
ap_compact_show_header_telemetry: false
ap_scale: 100
mesh_signal_entity: sensor.mein_ap_mesh_signal

ap_led_color: "#2563eb"
ap_color: "#e5e7eb"
ap_ring_color: "#cbd5e1"
ap_inner_color: "#f8fafc"

log_level: warn
debug: false
```

## Konfiguration

### Allgemein und Darstellung

| Schlüssel | Typ | Standard | Beschreibung |
|---|---|---:|---|
| `device_id` | String | erforderlich | Home-Assistant-Geräte-ID des UniFi-Geräts. |
| `name` | String | Gerätename | Eigener Titel im Kartenkopf. |
| `show_name` | Boolean | `true` | Titelzeile ein- oder ausblenden. |
| `show_telemetry` | Boolean | `true` | CPU-, Speicher- und Temperaturwerte im Header anzeigen. |
| `appearance_mode` | String | `auto` | `auto`, `light` oder `dark`. |
| `show_product_image` | Boolean | `false` | Zugeordnete Produktansicht laden. |
| `product_image_url` | String | automatisch | Eigene HTTPS-, `/local/`-, `/api/`- oder Data-URL. |
| `show_panel` | Boolean | `true` | Gerätespezifischen Panel-Hintergrund anzeigen; Ports und Interaktion bleiben auch bei `false` sichtbar. |
| `dynamic_port_details` | Boolean | `false` | Portdetails nur nach Auswahl anzeigen. |
| `background_color` | CSS-Farbe | Theme | Kartenhintergrund. |
| `background_opacity` | Zahl | `100` | Deckkraft von `0` bis `100`. |
| `title_color` | CSS-Farbe | Theme | Titelfarbe. |
| `telemetry_color` | CSS-Farbe | Theme | Farbe der Header-Telemetrie. |
| `label_color` | CSS-Farbe | Theme | Farbe allgemeiner Labels. |
| `value_color` | CSS-Farbe | Theme | Farbe allgemeiner Werte. |
| `meta_color` | CSS-Farbe | Theme | Farbe der Modell-/Firmwarezeile. |
| `port_label_color` | CSS-Farbe | Theme | Farbe normaler Portnummern. |
| `special_port_label_color` | CSS-Farbe | Theme | Farbe von WAN-/Spezialport-Labels. |

### Buttons und Farben

| Schlüssel | Typ | Standard | Beschreibung |
|---|---|---:|---|
| `button_theme_style` | Boolean | `true` | Button-Farben aus dem Home-Assistant-Theme verwenden. |
| `button_default_color` | Boolean | `true` | Integrierte Standardfarben nutzen, wenn Theme-Stil aus ist. |
| `button_color` | CSS-Farbe | Standard/Theme | Primärer Button-Hintergrund. |
| `button_text_color` | CSS-Farbe | Standard/Theme | Primäre Text-/Iconfarbe. |
| `button_secondary_color` | CSS-Farbe | Standard | Sekundärer Button-Hintergrund. |
| `button_secondary_text_color` | CSS-Farbe | Standard | Sekundäre Text-/Iconfarbe. |
| `button_border_color` | CSS-Farbe | Standard | Rahmenfarbe sekundärer Buttons. |
| `ap_led_color` | CSS-Farbe | Modell | AP-LED-Fallback ohne RGB-Entität. |
| `ap_color` | CSS-Farbe | Modell | AP-Gehäusefarbe. |
| `ap_ring_color` | CSS-Farbe | Modell | AP-Außenring. |
| `ap_inner_color` | CSS-Farbe | Modell | AP-Innenfläche. |

### Ports und Gateways

| Schlüssel | Typ | Standard | Beschreibung |
|---|---|---:|---|
| `port_size` | Zahl | `36` | Portgröße in Pixeln, Bereich `24` bis `52`. |
| `ports_per_row` | Zahl | Modell | Optionale Anzahl Ports pro Reihe. |
| `force_sequential_ports` | Boolean | `false` | Natürliche Reihenfolge statt Odd/Even-Layout. |
| `rotate180` | Boolean | `false` | Frontpanel um 180 Grad drehen. |
| `edit_special_ports` | Boolean | `false` | Spezialport- und WAN-Auswahl aktivieren. |
| `special_ports` | Zahlenliste | Modell | Ports, die als Spezialslots dargestellt werden. |
| `wan_port` | String | `auto` | WAN-Zuordnung: `auto`, Slot-Key oder `port_<n>`. |
| `wan2_port` | String | `auto` | WAN2-Zuordnung: `auto`, `none`, Slot-Key oder `port_<n>`. |
| `trust_link_speed_ports` | Zahlenliste | `[]` | Echte 10-Mbit/s-Links gezielt freigeben. |
| `port_name` | Objekt | `{}` | Eigene Namen je Portnummer. |
| `port_<n>` | Entity-ID | automatisch | Manueller Link-Speed-Fallback für einen physischen Port. |

Wenn `wan_port` oder `wan2_port` in YAML gesetzt wird, behandelt der Editor
`edit_special_ports` automatisch als aktiviert.

Beispiel für eine manuelle Portzuordnung:

```yaml
port_5: sensor.mein_switch_port_5_link_speed
```

Portnummer im Schlüssel und Entity müssen zum selben physischen Port gehören.

### Port-LED-Animation

| Schlüssel | Typ | Standard | Beschreibung |
|---|---|---:|---|
| `port_led_blink` | Boolean | `false` | Rein visuelles Link-LED-Blinken aktivieren. |
| `port_led_blink_rj45` | Boolean | `true` | RJ45-Link-LEDs animieren. |
| `port_led_blink_sfp` | Boolean | `true` | SFP-Link-LEDs animieren. |
| `port_led_blink_speed_rj45` | Zahl | `1` / Editor `0.2` | RJ45-Intervall in Sekunden, `0.1` bis `1`; der Editor startet mit 5 Blinkimpulsen pro Sekunde. |
| `port_led_blink_speed_sfp` | Zahl | `1` / Editor `0.2` | SFP-Intervall in Sekunden, `0.1` bis `1`; der Editor startet mit 5 Blinkimpulsen pro Sekunde. |
| `port_led_blink_speed` | Zahl | `1` | Weiterhin unterstützter gemeinsamer Legacy-Fallback. |

### Access Points und integrierte Geräte

| Schlüssel | Typ | Standard | Beschreibung |
|---|---|---:|---|
| `device_layout` | String | `combined` | `combined`, `network` oder `ap`. |
| `integrated_ports` | Boolean | `true` | Integrierte Ports kompatibler In-Wall-APs anzeigen. |
| `ap_compact_view` | Boolean | `false` | Kompaktes AP-Layout verwenden. |
| `ap_compact_show_header_telemetry` | Boolean | `false` | Header-Telemetrie auch kompakt anzeigen. |
| `ap_scale` | Zahl | `100` | AP-Größe in Prozent, Bereich `25` bis `140`. |
| `mesh_signal_entity` | Entity-ID | automatisch | Optionaler dBm- oder Prozent-Sensor für Mesh-Signal. |

### Diagnose

| Schlüssel | Typ | Standard | Beschreibung |
|---|---|---:|---|
| `log_level` | String | `warn` | `error`, `warn`, `info`, `debug` oder `trace`. |
| `debug` | Boolean | `false` | Kurzform für `log_level: debug`, sofern kein Level gesetzt ist. |

## Unterstützte Geräte

Die Karte enthält explizite Layouts für zahlreiche UniFi-Network-Geräte und
verwendet für unbekannte Modelle sichere Familien-Fallbacks.

### Switches und Aggregation

- UniFi Switch Compact 8, Switch 8, Switch 8 60W und Switch 8 150W
- UniFi Switch 16 PoE 150W
- USW Flex Mini, Flex, Flex Mini 2.5G, Flex 2.5G 8 und Flex 2.5G 8 PoE
- USW Lite 8 PoE und Lite 16 PoE
- USW 16/24/48 sowie PoE-Varianten
- US-24-250W
- USW Pro 24/48 sowie PoE-Varianten
- USW Pro Max 16/24/48 sowie PoE-Varianten
- USW Enterprise 8/24/48 PoE und Enterprise XG 24
- US-16-XG, USW Flex XG und US XG 6 PoE
- USW Mission Critical und USW Industrial
- USW Aggregation und USW Pro Aggregation
- USW Ultra, Ultra 60W und Ultra 210W
- USW Pro XG 8/10/24/48 und PoE-Varianten
- USW Pro HD 24 und 24 PoE
- Enterprise Campus 24/48 PoE, 24S/48S PoE und Campus Aggregation

### Gateways und integrierte Geräte

- Enterprise Fortress Gateway
- Dream Machine, Dream Machine Pro, SE, Pro Max und Beast
- Dream Router, Dream Router 7 und Dream Router 5G Max
- Dream Wall
- Cloud Gateway Ultra, Max, Fiber und Industrial
- UniFi Express und Express 7
- UXG Lite, UXG Max und UXG Pro
- UniFi Travel Router
- UniFi Security Gateway, USG Pro 4 und USG XG 8

### Access-Point- und Bridge-Designs

| Designfamilie | Beispiele | Darstellung |
|---|---|---|
| Rund | UAP-, U6-, U7-, E7- und Enterprise-APs | Skalierbare runde AP-Fläche und sicherer Fallback |
| Wall/In-Wall | UAP In-Wall, U6 In-Wall, U7 Pro Wall, U7 In-Wall | Rechteckiges Gehäuse, optional mit integrierten Ports |
| Mesh-Säule | FlexHD, U6 Mesh, U7 Mesh | Hohes abgerundetes Gehäuse mit LED-Ring |
| AC Mesh | UAP AC Mesh | Schmales Gehäuse mit zwei Antennen |
| Outdoor-Panel | UAP AC Mesh Pro, UK Ultra | Wetterfestes Panel |
| U6 Mesh Pro | U6 Mesh Pro | Schmales rechteckiges Gehäuse mit Front-LED |
| Extender | BeaconHD, U6 Extender | Wandstecker-Gehäuse |
| U7 Outdoor | U7 Outdoor, U7 Pro Outdoor | Eigenes Outdoor-Gehäuse mit unterer LED |
| Building Bridge | UBB, UBB XG | Runde Bridge mit LED-Rand |
| Device Bridge | UDB, UDB IoT, UDB Pro, UDB Pro Sector | Modellabhängige Bridge-Geometrie |
| Richtfunk-Bridge | U-AirWire, Device Bridge Switch | Skalierbares gerichtetes Bridge-Gehäuse |
| E7 | E7, U7 Enterprise, E7 Campus, E7 Audience | Abgerundete Enterprise-Gehäuse mit LED-Rand |
| BaseStation | WiFi BaseStation XG | Breites Gehäuse mit unteren Anschlüssen |
| 5G Backup | UniFi 5G Backup | Eigenes Gerätedisplay mit Signal, Uptime, CPU und RAM |

Unbekannte Geräte aus bekannten AP-Familien verwenden das runde AP-Design.
Unbekannte Switches verwenden eine aus Portanzahl und Entitäten abgeleitete
silberne Frontpanel-Darstellung.

### Abgrenzung

Die Karte ist auf Geräte der UniFi-Network-Domäne ausgerichtet. Protect-,
Access-, Power- oder Drive-Geräte werden nicht als erfundene Network-Karten
dargestellt, wenn Home Assistant dafür keine passende Network-Geräte- und
Portstruktur liefert.

## Erkennung und Darstellung

- Geräte werden über die Home-Assistant-Geräteregistry ermittelt
- Modellkennungen werden normalisiert und auf bekannte Aliase abgebildet
- Entitäten werden über Device-ID, Config-Entry, Unique-ID, Translation Key,
  Entity-ID, Namen und Attribute zugeordnet
- Umbenannte Entitäten bleiben nach Möglichkeit verwendbar
- Spezialports bleiben getrennt von normalen nummerierten Ports
- Layoutinformationen werden mit live entdeckten Ports zusammengeführt
- Modellwissen bestimmt Geometrie und mögliche PoE-Ports, nicht den aktiven Link
- Backenddaten können einen veralteten Entityzustand korrigieren, überschreiben
  aber keine stärkeren aktuellen Belege durch bloße Vermutungen
- Fehlende optionale Daten erzeugen keine lauten Fehler und keine leeren Controls

In Sections-Ansichten fordern Switches und Gateways automatisch die für ihr
Frontpanel sinnvolle Breite an. AP-Karten bleiben kompakter. Eigene
`grid_options` des Dashboards haben Vorrang.

## Sicherheit und Datenschutz

- Die Lovelace-Karte speichert keine UniFi-Zugangsdaten
- Der Browser verbindet sich nie direkt mit dem UniFi-Controller
- Backend-Kommunikation läuft über authentifizierte Home-Assistant-WebSockets
- Der empfohlene Backend-Modus nutzt die bestehende Home-Assistant-Sitzung
- Direkte Zugangsdaten bleiben ausschließlich im Backend-Config-Entry
- Topologieabfragen sind auf das angefragte Infrastrukturgerät begrenzt
- PoE-Power-Cycle und Etherlighting verlangen einen Home-Assistant-Administrator
- Schreibaktionen verlangen zusätzlich ein UniFi-Konto mit Administratorrechten
- Controllerfehler, interne Payloads und Zugangsdaten werden nicht an den Browser durchgereicht
- Diagnosen enthalten keine Clientnamen, IP-Adressen, MAC-Adressen oder Passwörter
- SSH, `ubus` und direkte Dateisystemeingriffe auf UniFi-Geräten werden nicht verwendet

Produktbilder werden nur bei aktivierter Produktansicht geladen. Eine eigene
lokale Bilddatei kann das externe Laden vollständig ersetzen.

## Fehlerbehebung

### Karte erscheint nicht

- Browser vollständig neu laden
- Unter **Einstellungen → Dashboards → Ressourcen** prüfen, ob die JavaScript-Datei registriert ist
- Bei HACS den Ressourcenpfad der installierten Karte prüfen
- Bei manueller Installation `/local/unifi-device-card.js` verwenden
- Browserkonsole auf Meldungen mit dem Präfix `UNIFI-DEVICE-CARD` prüfen

### Gerät fehlt im Editor

- Prüfen, ob das Gerät unter **Einstellungen → Geräte & Dienste** vorhanden ist
- Offizielle UniFi-Network-Integration neu laden
- Prüfen, ob das Gerät wirklich zur Network-Domäne gehört
- Mit `log_level: debug` oder `trace` die Erkennung in der Browserkonsole prüfen

### Ports sind aktiv, werden aber als offline angezeigt

Für das Gerät insbesondere folgende Entitäten prüfen und bei Bedarf aktivieren:

- Port-Schalter
- Link-Speed-Sensoren
- RX-/TX-Sensoren
- PoE-Schalter
- PoE-Leistungssensoren
- Power-Cycle-Buttons

Viele Link-Speed-Sensoren sind in Home Assistant standardmäßig deaktiviert. Der
visuelle Editor kann erkannte deaktivierte Link-Speed-Sensoren nach einem
bewussten Klick in begrenzten Gruppen aktivieren.

### Port zeigt `no_link` und trotzdem eine Geschwindigkeit

Die aktuelle Version blendet eine alte ausgehandelte Geschwindigkeit bei
bestätigtem `no_link` aus. Nach einem Update Browsercache leeren und die
Frontend-Ressource neu laden.

### Client oder VLAN fehlt

- Prüfen, ob der Client in UniFi aktuell am erwarteten Switchport verbunden ist
- Optionales Backend und gewählte Datenquelle im Karteneditor prüfen
- Backend-Integration neu laden
- Bei direktem Login Standort und Berechtigungen prüfen
- Mehrere MAC-Adressen an einem Port werden nicht automatisch als eindeutiger
  Direktclient geraten

### PoE-Power-Cycle fehlt

Power Cycle erscheint nur, wenn:

- eine verwendbare Home-Assistant-Button-Entität vorhanden ist, oder
- das Backend beziehungsweise seine Live-Portdaten den Port als PoE-fähig und
  PoE-aktiv bestätigen

Für den Backend-Fallback müssen Home-Assistant- und UniFi-Benutzer
Administratorrechte besitzen. Uplinks, deaktivierte Geräte und nicht
PoE-fähige Ports werden abgelehnt. Der Button bleibt bei einem erkannten
aktiven PoE-Port sichtbar; eine fehlende Berechtigung oder nicht verfügbare
Controllerverbindung wird als konkrete Fehlermeldung angezeigt.

### Mesh-Signal fehlt

- Prüfen, ob der AP tatsächlich drahtlos uplinkt
- RSSI-/Signal-Sensor aktivieren oder über `mesh_signal_entity` auswählen
- Bei kabelgebundenem Uplink bleibt die Anzeige absichtlich verborgen

### Produktbild fehlt

- `show_product_image: true` setzen
- Browserzugriff auf die Hersteller-CDN prüfen
- Alternativ eine lokale Datei über `product_image_url: /local/...` verwenden
- Bei unbekannter Modellkennung wird kein unpassendes Bild geraten

### Eigene Portzuordnung funktioniert nicht

1. Schlüssel als `port_<nummer>` schreiben.
2. Eine vorhandene Link-Speed-Entity zuweisen.
3. Prüfen, ob Schlüssel und Entity denselben physischen Port beschreiben.
4. Dashboard speichern und Frontend neu laden.

Beispiel:

```yaml
port_5: sensor.mein_switch_port_5_link_speed
```

### Hintergrundfarbe wirkt nicht

- Gültige CSS-Farbe verwenden, beispielsweise `#1f2937`, `red` oder eine CSS-Variable
- `background_opacity` prüfen
- Browsercache nach einem Update leeren

## Hinweise

- Alle Funktionen besitzen Fallbacks für fehlende optionale Entitäten
- Die tatsächliche Datenmenge hängt von UniFi-Modell, Firmware,
  Home-Assistant-Version und aktivierten Entitäten ab
- Produktnamen und Produktdarstellungen gehören dem jeweiligen Hersteller
- Dieses Projekt ist nicht mit dem Hersteller verbunden oder von ihm unterstützt
- Änderungen und neue Funktionen werden in `CHANGELOG.md` dokumentiert

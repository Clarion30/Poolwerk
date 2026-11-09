# Poolwerk

![Poolwerk-Logo](bilder/poolwerk-logo.png)

Poolwerk ist die Steuerung, die ich für meine eigene Poolanlage gebaut habe. Filterung, Wasserpflege und Wasserwege lassen sich damit von einer Stelle aus bedienen und überwachen. Vorher lief meine Anlage mit einer ProCon.IP; beim Aufbau meines Webportals sind einige Ähnlichkeiten wiederzuerkennen.

**Poolwerk deckt weit mehr als eine reine Dosier- oder Filtersteuerung ab.** Wasserpflege, Pumpen, Ventile, Wärmepumpe, Gegenstromanlage, Überwachung, Diagnose und Home Assistant sind in einer gemeinsamen Steuerung zusammengeführt. Dabei lässt sich der Funktionsumfang an die jeweilige Anlage anpassen.

## Für unterschiedliche Poolanlagen

Poolwerk lässt sich an die vorhandene Pooltechnik anpassen: **Flüssigchlor-Dosierung oder Salzelektrolyse, pH-Minus und pH-Plus** können je nach Ausstattung genutzt werden. Die einzelnen Funktionen lassen sich unabhängig aktivieren oder deaktivieren.

Auch **ohne Belimo-Antriebe oder andere motorisierte Ventile** ist ein Betrieb möglich. Ventilansteuerung und Endlagenrückmeldung können passend zur Anlage eingerichtet oder abgeschaltet werden.

## Was Poolwerk übernimmt

- **Filterpumpe und Wasserwege:** Wochenplan mit Drehzahlstufen, Überwachung von Durchfluss und Filterdruck sowie Rückmeldung der Ventilstellungen.
- **Wasserpflege:** Messung von pH, Redox, freiem Chlor und Salz sowie die jeweils aktivierte Dosierung oder Elektrolyse.
- **Weitere Pooltechnik:** Einbindung von Wärmepumpe, PV-Freigabe, Gegenstromanlage und Wassermeldern.
- **Auswertung:** Messverläufe, Laufzeiten und Diagnose helfen dabei, den Betrieb und mögliche Störungen nachzuvollziehen.

## Hardware und technische Ausstattung

### ESP32-S3 als Zentrale

Im aktuellen Aufbau arbeitet ein **ESP32-S3-DevKitC-1-N32R16V**. Die Firmware basiert auf **ESPHome mit ESP-IDF** und unterstützt zwei Boardprofile:

| Boardprofil | Flash-Speicher | PSRAM |
| --- | ---: | ---: |
| ESP32-S3 N32R16V – aktueller Aufbau | 32 MB | 16 MB |
| ESP32-S3 N16R8 – ebenfalls unterstützt | 16 MB | 8 MB |

Der ESP32 übernimmt die Messwerterfassung, Steuerungsabläufe, Freigaben, Displays und das Webportal. Der zusätzliche Speicher wird unter anderem für die Oberfläche, Anzeigen und Laufzeitdaten genutzt. Die Auswahl des Boardprofils erfolgt in der Firmwarekonfiguration.

### Sonden und Messmodule

| Messung | Verwendete Technik | Aufgabe in Poolwerk |
| --- | --- | --- |
| pH | pH-Elektrode mit **Atlas Scientific EZO-pH** | Erfassung des pH-Werts als Grundlage für Anzeige und pH-Regelung |
| Redox | Redox-Elektrode mit **Atlas Scientific EZO-ORP** | Messung des Redoxpotenzials und Ermittlung des automatischen Chlorbedarfs |
| Leitfähigkeit / Salz | **Apera 2301-C**, Zellkonstante K = 1,0, mit **Atlas Scientific EZO-EC** | Messung der Leitfähigkeit; daraus berechnet Poolwerk die Salzanzeige |
| Freies Chlor | **ProCon.Cl-Sonde mit Potentiostat**, ausgewertet über einen separaten **ADS1115** | Erfassung des Chlorsignals und zusätzliche Begrenzung der Chlorzugabe |
| Temperaturen | **DS18B20** | Messung beispielsweise an Messzelle, Skimmer und weiteren Anlagenstellen |
| Hauptdurchfluss | **TUF-2000M** über **RS485 / Modbus** | Anzeige des Volumenstroms und Überwachung des Wasserflusses |
| Messzellendurchfluss | Impulsgeber **Y201** | Erfassung des Durchflusses durch die Messzelle |
| Filterdruck | Analoger Drucksensor am **ADS1115** | Überwachung des Filterdrucks |
| Elektrolysestrom | **ACS712**, im eigenen Aufbau die 20-A-Ausführung | Überwachung des Zellstroms; die Software unterstützt die Auswahl zwischen 20-A- und 30-A-Sensor |

Die Leitfähigkeitssonde besitzt keinen eigenen Temperaturfühler. Für die Temperaturkompensation verwendet Poolwerk die zugeordnete Messzellentemperatur. pH, Redox und Leitfähigkeit werden über die Atlas-Messmodule ausgelesen; Kalibrierung und Diagnose sind in die Bedienoberfläche integriert.

### Erweiterungen und Ein-/Ausgänge

Vier **MCP23017** erweitern den ESP32 um insgesamt **64 digitale Ein-/Ausgänge**. Im Aufbau sind davon **24 Relaiskanäle** und **16 Endschaltereingänge** vorgesehen. Weitere Anschlüsse dienen unter anderem dem Encoder-Taster, Wasserstandssensoren, Wasseralarmen, Optokopplern und zusätzlichen Steuereingängen; ein Teil bleibt als Reserve.

Zwei **ADS1115-Messmodule** erfassen die analogen Signale. Ein Modul verarbeitet unter anderem Filterdruck, Elektrolysestrom, Batteriespannung der Echtzeituhr und das Bypass-Signal. Das zweite ist der galvanisch getrennten Chlormessung zugeordnet. Die Anzahl der vorgesehenen Kanäle bedeutet dabei nicht, dass jede angeschlossene Anlage alle Funktionen benötigt.

### Galvanische Trennung

Die Messkreise für **pH, Redox, Leitfähigkeit und Chlor** sind galvanisch getrennt aufgebaut. Auch die Steuerungs- und Lastseite sind getrennt; ESP32 und Antriebe haben eigene Stromversorgungen.

Bei der Chlormessung kommen ein **isolierter TRACO-DC/DC-Wandler**, eine separate Platine mit **ADS1115** und eine **ISO1640-I²C-Trennung auf einem Pololu-5392-Modul** zum Einsatz. Mit diesem Aufbau konnte ich den zuvor beobachteten unerwünschten Abfall der Chloranzeige bei Anlagenstillstand beheben.

### Antriebe, Bedienung und Kommunikation

- **Belimo-Antriebe mit Endschaltern** übernehmen in meiner Anlage die Ventile. Poolwerk wertet die Rückmeldungen aus und überwacht, ob die angeforderte Stellung erreicht wird.
- Für die Salzelektrolyse verwende ich eine **Zodiac LM2-40**. Die Steuerung übernimmt Freigabe, Stromüberwachung und automatische Umpolung. Alternativ ist Flüssigchlor-Dosierung vorgesehen.
- Zwei **TFT-Displays** und ein **EC11-Drehencoder mit Taster** ermöglichen die Bedienung am Gerät.
- Ein **W5500-Ethernetmodul** verbindet die Steuerung per LAN; zusätzlich steht WLAN zur Verfügung.
- Eine **DS3231-Echtzeituhr** und eine **microSD-Karte** ergänzen Zeitbasis und lokale Aufzeichnung. Die PDF-Anleitung liegt direkt im ESP-Flash.
- **ESPHome / Home Assistant**, MQTT und das eigene Webportal verbinden Messwerte, Bedienung und übergeordnete Hausautomation.

## Webportal

Die Übersicht zeigt Wasserwerte, Temperaturen und Betriebszustände. Darunter lassen sich die weiteren Anlagenteile bedienen und die Rückmeldungen der Wasserwege ansehen.

![Poolwerk-Webportal mit Messwerten und Anlagenbedienung](bilder/webportal.png)

## Home Assistant

Poolwerk ist über **ESPHome in Home Assistant eingebunden**. Mein Dashboard zeigt Wasserqualität, Temperaturen, Betriebszustände, Durchfluss, Filterdruck und Störungsmeldungen. Über „Poolwerk öffnen“ erreiche ich das eigene Webportal.

<img src="bilder/home-assistant.png" alt="Poolwerk-Dashboard in Home Assistant mit Messwerten und Störungsanzeige" width="650">

## Bedienung direkt am Gerät

Zwei Displays und ein Drehknopf ermöglichen die Bedienung vor Ort. Das Menü führt zu Wasserwerten, Filterpumpe, Kalibrierung und Systemmeldungen; Störungen werden mit ihrer Ursache angezeigt.

<img src="bilder/gehaeusefront-aktuell.jpg" alt="Poolwerk-Gehäuse mit zwei Displays und Drehknopf" width="650">

<details>
<summary>Bedienmenü und Störungsanzeige ansehen</summary>

| Bedienung mit Drehknopf | Beispiel einer Störungsmeldung |
| --- | --- |
| ![Bedienmenü am Gerät](bilder/display-bedienmenue.jpg) | ![Display mit Meldung zur Redox-Sicherheitsgrenze](bilder/display-stoerungsmeldung.jpg) |

</details>

## Anlage im Technikraum

In meiner Anlage übernehmen Belimo-Antriebe die motorisierten Ventile. Angebaute Endschalter melden die Endlagen zurück, damit Poolwerk die erreichte Stellung erkennen kann.

![Übersicht der Verrohrung, Ventile und Messzelle](bilder/technikraum-uebersicht.jpg)

<details>
<summary>Weitere Fotos von Wasserwegen, Pumpe und Endschaltern</summary>

| Wasserweg im Technikraum | Ventilantrieb mit Endschaltern |
| --- | --- |
| ![Wasserweg im Technikraum](bilder/wasserweg.jpg) | ![Ventilantrieb mit Endschaltern](bilder/ventilantrieb.jpg) |

| Verrohrung und Elektrolyse | Pumpe mit motorisierten Ventilen |
| --- | --- |
| ![Verrohrung und Elektrolyseeinheit](bilder/verrohrung-elektrolyse.jpg) | ![Pumpe und Ventile](bilder/pumpe-ventile.jpg) |

| Belimo mit rotem Ventilgriff | Endschalter im Detail |
| --- | --- |
| ![Belimo-Antrieb mit rotem Ventilgriff](bilder/belimo-endschalter.jpg) | ![Belimo-Antrieb und Endschalter an der Pumpe](bilder/belimo-pumpe-detail.jpg) |

</details>

## Die Steuerung als Prototyp

Poolwerk ist ein **Prototyp im laufenden Aufbau**, der bereits an meinem Pool arbeitet. Die offene Ansicht zeigt den tatsächlichen Entwicklungsstand mit Steuerungsmodulen, Relais und Verdrahtung.

![Offene Poolwerk-Steuerung im Prototypenaufbau](bilder/steuerung-prototyp-offen.jpg)

<details>
<summary>Einbau der Steuerung im Technikraum</summary>

![Poolwerk-Steuerung neben der Elektroverteilung](bilder/steuerung-technikraum.jpg)

</details>

## Einstellungen und Diagnose

Die Einstellungen sind nach Anlagenbereichen gegliedert. Dort lassen sich unter anderem Betriebsarten, vorhandene Komponenten, Sollwerte und Grenzen anpassen. Die Screenshots zeigen die Einstellungen meiner Anlage.

<details>
<summary>Filterpumpe, Wärmepumpe sowie pH und Redox ansehen</summary>

**Filterpumpe:** Zuordnung der Pumpenstufen und Ventile; weitere Reiter führen zu Betrieb, Wochenprogramm, Messwerten und Diagnose.

![Einstellungen der Filterpumpe](bilder/einstellungen-filterpumpe.png)

**Wärmepumpe:** Betriebsart, Schaltausgang, MQTT-Anbindung und zugehörige Ventile lassen sich passend zur Ausstattung einstellen.

![Einstellungen der Wärmepumpe](bilder/einstellungen-waermepumpe.png)

**pH und Redox:** Sollwerte, Anzeigebereiche und Regelgrenzen sind getrennt einstellbar. Kalibrierung und Messwerte haben eigene Reiter.

![Einstellungen für pH und Redox](bilder/einstellungen-ph-redox.png)

</details>

<details>
<summary>Systemdiagnose ansehen</summary>

Die Systemansicht zeigt CPU-Auslastung, Temperatur, freien Speicher und Laufzeit. Zähler zu LAN-Unterbrechungen und Wiederherstellungen unterstützen die Fehlersuche.

![Systemdiagnose mit CPU-, Speicher- und Netzwerkdaten](bilder/systemdiagnose.png)

</details>

## Messverläufe

Der Live-Mitschreiber zeigt ungeglättete Messpunkte und ihre Spanne. So lässt sich beispielsweise verfolgen, wie stark der pH-Wert über einige Minuten schwankt.

![Ungeglätteter pH-Verlauf](bilder/ph-verlauf.png)

## Einblick in die Anleitung

[**PDF-Leseprobe öffnen: erste vier Seiten der Anleitung 096-345**](dokumentation/Poolwerk_096-345_Anleitung_Leseprobe_Seiten_1-4.pdf)

Der Auszug enthält Titelseite, Inhaltsverzeichnis, aktuelle Anschlüsse und Installation. Das Inhaltsverzeichnis bezieht sich auf die vollständige Anleitung. Auf der Steuerung liegt die passende vollständige Ausgabe unter **Einstellungen → System → Anleitung herunterladen (PDF)**.

---

Dies ist ein Ausschnitt aus dem Projekt. Die automatische Frischwassernachfüllung ist als Erweiterung vorbereitet. Das Repository dient der Vorstellung; Firmware und vollständige Projektquellen werden hier nicht bereitgestellt.

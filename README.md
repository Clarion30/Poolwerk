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

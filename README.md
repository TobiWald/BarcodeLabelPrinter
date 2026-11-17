# BarcodeLabelPrinter

Kleine Web-App, die Barcode-Etiketten direkt aus dem Browser auf einen **Phomemo Q31** druckt. Keine App, kein Konto, nichts wird an einen Server geschickt.

## So funktioniert's

1. Seite in **Chrome auf Android** (oder Chrome/Edge am Computer) öffnen.
2. Produktnamen eingeben, **Weiter** tippen.
3. Anzahl wählen, **In die Liste**.
4. Das für alle Produkte wiederholen.
5. **Alles drucken**: beim ersten Mal den Drucker in der Bluetooth-Auswahl antippen, dann laufen alle Etiketten nacheinander durch.

Den Q31 **nicht** vorher in den Bluetooth-Einstellungen des Handys koppeln. Die Verbindung läuft direkt über die Seite.

## Barcode

- Format **EAN-13** mit Präfix `20` (GS1-Bereich für interne Artikelnummern), liest jeder Barcodescanner.
- Der Code wird aus dem Produktnamen berechnet. Gleicher Name ergibt immer denselben Code, Groß-/Kleinschreibung und doppelte Leerzeichen spielen keine Rolle. So passt ein Nachdruck immer zum ersten Etikett.
- Nur Ziffern, dadurch keine Probleme mit deutschem Tastaturlayout bei Scannern, die als Tastatur arbeiten.

## Einstellungen

Über das Regler-Symbol oben rechts:

- **Etikettengröße**: 40×12 (Standard), 30×12, 40×14, 30×14, 40×15, 30×15, 50×15 mm
- **Druckstärke**: 1 bis 8 (Standard 6). Bei blassem Druck höher stellen.
- **Testetikett drucken**: einmal zum Ausprobieren.

Die Druckliste und die Einstellungen bleiben im Browser gespeichert.

## Technik

- Eine einzige `index.html`, ohne Bibliotheken. Barcode, Rasterung und Druckprotokoll sind selbst geschrieben.
- Web Bluetooth, Service `0xFF00`, Schreibkanal `0xFF02`.
- Druckprotokoll der Phomemo D30-Familie (der Q31 ist baugleich): `ESC 7` Heizzeit, `1F 11 0A` Etiketten mit Lücke, `ESC @` + `GS v 0` Rasterbild (90° gedreht, 8 Punkte/mm), `ESC d 0` bis zur nächsten Lücke.
- Protokoll abgeleitet aus [Phomymo](https://github.com/transcriptionstream/phomymo) (MIT) und [phomemo_d30](https://github.com/polskafan/phomemo_d30).
- Läuft offline (Service Worker) und lässt sich über „Zum Startbildschirm hinzufügen“ wie eine App installieren.

## Hosting

GitHub Pages: *Settings → Pages → Branch `main` / root*. Web Bluetooth braucht https, das liefert GitHub Pages automatisch.

## Browser

| Gerät | Geht? |
|---|---|
| Android + Chrome | ja |
| Windows/Mac/Linux + Chrome oder Edge | ja |
| iPhone/iPad (Safari, Chrome) | nein, Apple erlaubt kein Web Bluetooth. Ausweg: Browser „Bluefy“ |
| Firefox | nein |

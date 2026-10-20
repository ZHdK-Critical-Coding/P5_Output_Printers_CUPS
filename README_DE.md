# P5.js: CUPS Printer

[English](README.md)

Zeichnet eine generative Grafik im A4-Format und druckt sie bei jedem
Klick auf einem normalen Büro- oder Tintenstrahldrucker: Die Zeichenfläche
geht als PNG an den Printer Server, der sie auf eine Seite setzt und an den
Drucker weitergibt. Ein Ausgangspunkt, um Plakate, Flyer oder Serien aus
einem Sketch zu drucken.

## Installation

Bibliotheken (in `libraries/`): p5.js 1.10.0.

Lizenz: MIT

## Server

Braucht die macOS-App **Printer Server** aus
[Servers_Printer](https://github.com/ZHdK-Critical-Coding/Servers_Printer)
(Download unter *Releases*). Sie läuft auf dem Mac, auf dem der Drucker
eingerichtet ist (*Systemeinstellungen → Drucker*), und nimmt Bilder auf
Port 8070 entgegen. In der App den Modus des Druckers auf **CUPS image**
stellen und ihn anhaken. Sie antwortet auf jedes Bild mit `ok`, sobald der
Auftrag beim Drucker ist, mit `busy`, solange der vorherige Auftrag noch
übergeben wird, oder mit `error: …`.

Läuft der Server auf demselben Mac, funktioniert die Standardadresse in
`sketch.js` (`ws://localhost:8070`). Sonst die in der App angezeigte
Adresse in `printerConnection` eintragen. Ist Port 8070 belegt, nimmt die
App den nächsten freien Port und zeigt ihn an.

## Ausführen

1. Printer Server starten und einen Drucker im Modus **CUPS image**
   anhaken (siehe Server).
2. Den Ordner in Visual Studio Code öffnen und Live Server starten (in der
   Statusleiste auf **Go Live** klicken).
3. Auf die Zeichenfläche klicken: Die Grafik wird gedruckt und eine neue
   gezeichnet.

Der Verbindungsstatus steht unter der Zeichenfläche.

## Coding-Hilfe

- **`sketch.js` → `printerConnection`**: WebSocket-Adresse des Printer
  Servers.
- **`sketch.js` → `pageOptions`**: Seitenoptionen, einmal nach dem
  Verbinden gesendet. `media` (`'A3'`, `'A4'`, `'A5'`, `'A6'`, `'Letter'`,
  `'Legal'`), `orientation` (`'auto'` wie das Bild, `'portrait'`,
  `'landscape'`), `fit` (Seite innerhalb des Rands füllen, oder mit
  `false` in der Grösse aus `dpi` drucken), `margin` in mm, `copies`,
  `dither` (schwarz-weisse Punkte wie beim Nadeldrucker). Alle Felder sind
  optional.
- **`communicateWithPrinter()`**: sendet Daten und gibt ein Promise zurück,
  das erfüllt wird, wenn der Server `ok` antwortet. Jede andere Antwort
  oder eine geschlossene Verbindung lehnt es ab.
- **`printCanvas()`**: wandelt die Zeichenfläche mit `canvas.toBlob()` in
  ein PNG um und sendet es als Binärdaten.
- **`setup()`**: erstellt die Zeichenfläche im A4-Format (420 × 594) mit
  `pixelDensity(3)`, damit das PNG 1260 × 1782 Pixel hat und scharf
  gedruckt wird. Öffnet den WebSocket und sendet `pageOptions`, sobald er
  verbunden ist.
- **`draw()`**: zeichnet die Grafik aus der Zufallszahl `seed`, damit sie
  gleich bleibt, bis sie gedruckt wird. Diesen Teil durch eine eigene
  Zeichnung ersetzen.
- **`mousePressed()`**: druckt bei einem Klick in die Zeichenfläche und
  erhöht danach `seed` für die nächste Grafik.

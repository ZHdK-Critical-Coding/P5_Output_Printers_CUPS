# P5.js: CUPS Printer

[Deutsch](README_DE.md)

Draws a generative graphic in A4 proportions and prints it on a normal
office or inkjet printer with every click: the canvas is sent as a PNG to
the Printer Server, which places it on a page and passes it to the printer.
A starting point for printing posters, flyers or series from a sketch.

## Installation

Libraries (in `libraries/`): p5.js 1.10.0.

License: MIT

## Server

Needs the macOS app **Printer Server** from
[Servers_Printer](https://github.com/ZHdK-Critical-Coding/Servers_Printer)
(download it under *Releases*). It runs on the Mac the printer is set up on
(*System Settings → Printers*) and accepts images on port 8070. In the app,
set the printer's mode to **CUPS image** and check it. It answers every
image with `ok` once the job is handed to the printer, `busy` while the
previous job is still being handed over, or `error: …`.

If the server runs on the same Mac, the default address in `sketch.js` works
(`ws://localhost:8070`). Otherwise put the address shown in the app into
`printerConnection`. If port 8070 is taken, the app uses the next free port
and shows it.

## How to Run

1. Start Printer Server and check a printer in **CUPS image** mode (see
   Server).
2. Open the folder in Visual Studio Code and start Live Server (click
   **Go Live** in the status bar).
3. Click on the canvas: the graphic is printed and a new one is drawn.

The connection status is shown below the canvas.

## Coding Help

- **`sketch.js` → `printerConnection`**: WebSocket address of the Printer
  Server.
- **`sketch.js` → `pageOptions`**: page options, sent once after
  connecting. `media` (`'A3'`, `'A4'`, `'A5'`, `'A6'`, `'Letter'`,
  `'Legal'`), `orientation` (`'auto'` follows the image, `'portrait'`,
  `'landscape'`), `fit` (fill the page inside the margin, or with `false`
  print at `dpi`), `margin` in mm, `copies`, `dither` (black and white dots
  like on the line printer). All fields are optional.
- **`communicateWithPrinter()`**: sends data and returns a Promise that
  resolves when the server answers `ok`. Any other answer or a closed
  connection rejects it.
- **`printCanvas()`**: converts the canvas to a PNG with `canvas.toBlob()`
  and sends it as binary data.
- **`setup()`**: creates the canvas in A4 proportions (420 × 594) with
  `pixelDensity(3)`, so the PNG has 1260 × 1782 pixels and prints sharply.
  Opens the WebSocket and sends `pageOptions` when it is connected.
- **`draw()`**: draws the graphic from the random seed `seed`, so it stays
  the same until it is printed. Replace this part with your own drawing.
- **`mousePressed()`**: prints on a click inside the canvas and then
  increases `seed` for the next graphic.

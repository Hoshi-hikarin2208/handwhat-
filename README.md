# Hand Studio

Hand Studio is a browser-based hand-gesture studio. The app is a single HTML file with no build step or application server.

## Features

- **Sign Typer:** recognize supported hand signs, type letters, and run a guided I-love-you gesture sequence.
- **Air Draw:** draw freehand, make boxes, draw with two pointing fingers, choose colors, and adjust brush and eraser sizes.
- **Portal:** choose from 33 live filters, outline the effect with 4, 6, or 8 fingertips, and save snapshots.
- **Puzzle Cam:** frame a subject with four fingertips, choose from 22 shapes or Freestyle, then fill a 3 x 3 collage with timed photos.
- **Face and pose cues:** optional face mesh, expression estimates, and hand/head cues. These are rough visual signals, not reliable emotion readings or full-body tracking.
- **Display controls:** mirror the view, cycle Auto/Dark/Light themes, and pause or resume the camera.

### Keyboard shortcuts

| Key | Action |
| --- | --- |
| `M` | Toggle mirroring |
| `T` | Cycle Auto, Dark, and Light themes |
| `P` | Start, pause, or resume the camera |
| `1`-`9` | Select one of the first nine Portal filters |

Shortcuts are ignored while typing in text fields.

## Run locally

Use a recent Chrome or Edge browser. Camera access generally requires a secure context, so serve the folder from localhost rather than opening the file directly.

From the project directory, run:

```powershell
py -m http.server 8000 --bind 127.0.0.1
```

Then open <http://localhost:8000/hand-studio.html>, select **Start camera**, and grant camera permission. Stop the server with `Ctrl+C`.

The app has no build or package-install step. It does require an internet connection to load the pinned MediaPipe scripts and model assets from jsDelivr.

## Privacy

- The camera starts only after you select **Start camera**. The app requests video only; it does not request microphone access.
- Camera frames are processed in the browser by MediaPipe. This app has no backend and does not upload camera frames.
- MediaPipe libraries and model assets are downloaded from jsDelivr. Saving a snapshot or collage creates a local browser download.
- Pause the camera or close the page to stop camera access.

## Security notes

The HTML includes a restrictive Content Security Policy, hash-pinned inline script and style blocks, and Subresource Integrity checks for the two top-level MediaPipe scripts. MediaPipe dynamically loads helper scripts and model assets; the policy permits only the pinned package versions and the helper script paths those versions use.

The legacy Face Mesh runtime requires `unsafe-eval`. The policy keeps script elements and inline event attributes separately restricted, but this compatibility exception still increases risk for code from the permitted MediaPipe helpers. Removing it would require replacing or disabling Face Mesh.

The policy is embedded in HTML. Some protections, including `frame-ancestors` and camera/microphone Permissions Policy, require HTTP response headers and cannot be reliably set by a standalone HTML file. For deployment, use HTTPS and configure appropriate server headers without overriding the app's required CSP sources.

When editing `hand-studio.html`, changes to the inline `<script>` or `<style>` blocks require updating their CSP hashes. Changing a MediaPipe version also requires recalculating the corresponding SRI hash and reviewing the allowed helper paths.

## License

Hand Studio is licensed under the Apache License, Version 2.0. See [LICENSE](./LICENSE). Third-party dependencies and assets remain subject to their own licenses.

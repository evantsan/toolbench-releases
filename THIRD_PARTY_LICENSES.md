# Third-party components

Toolbench's own code is covered by [`LICENSE`](LICENSE). The installers include the components below, each under its own license, and these licenses take priority over Toolbench's for those components. Their full license texts are in the app, under **Settings → About → Open-source licenses**.

- **Loading:** the LGPL components are separate WebAssembly/JS files, loaded on demand in their own Web Worker. They aren't compiled or linked into Toolbench's code. Nothing GPL-licensed ships, apart from pdf.js's fallback fonts (data files, with the font exception).
- **Audit:** last done 2026-11-15.

## Copyleft components

### FFmpeg (audio/video fallback): LGPL-2.1-or-later as built

- **Build:** Toolbench's own LGPL-only build of ffmpeg.wasm (single-threaded), served from `/vendor/ffmpeg/` and downloaded only when a job needs it (GIF, animated WebP, MP3/Ogg/FLAC encoding, containers WebCodecs can't read, browsers without WebCodecs). Most audio/video work runs on the browser's own codecs (WebCodecs) instead.
- **Version:** FFmpeg n5.1.4, built from the ffmpeg.wasm `v12.15` build scripts (MIT) with x264, x265, libpostproc and unused libraries removed. Build files, configure line and checksums: [`source/ffmpeg-lgpl/`](source/ffmpeg-lgpl/README.md).
- **Build configuration** (read from the shipped `ffmpeg-core.wasm`): `--disable-gpl --disable-nonfree --disable-version3 --disable-postproc --enable-libvpx --enable-libmp3lame --enable-libvorbis --enable-libopus --enable-zlib --enable-libwebp`. Toolbench's tests and the build workflow fail if GPL code appears in the binary.

| Library linked into the core | License           | Used by Toolbench?                        |
| ---------------------------- | ----------------- | ----------------------------------------- |
| FFmpeg (libav\*)             | LGPL-2.1-or-later | yes                                       |
| LAME (libmp3lame)            | LGPL-2.0-or-later | yes: MP3 output                           |
| libvpx                       | BSD-3-Clause      | yes: WebM (VP9) without WebCodecs         |
| libvorbis, libogg            | BSD-3-Clause      | yes: Ogg output                           |
| Opus                         | BSD-3-Clause      | yes: Opus audio in WebM without WebCodecs |
| libwebp                      | BSD-3-Clause      | yes: animated WebP                        |
| zlib                         | Zlib              | yes                                       |

- **Source:** FFmpeg <https://github.com/FFmpeg/FFmpeg/tree/n5.1.4>; build scripts <https://github.com/ffmpegwasm/ffmpeg.wasm/tree/v12.15> plus the changes in [`source/ffmpeg-lgpl/build/`](source/ffmpeg-lgpl/build/).
- **LGPL notes:** the core is a separate, replaceable file loaded in its own worker, so a user can substitute a modified build. Its license texts (LGPL-2.1, LAME's LGPL-2.0, and the BSD/Zlib notices of the bundled libraries) are on the app's Open-source licenses page.

### Mediabunny (audio/video containers): MPL-2.0

- **Package:** `mediabunny` 1.63.0, bundled unmodified into the media worker (`media-wc.worker`).
- **MPL notes:** MPL-2.0 is file-level copyleft. Its files stay under the MPL (and any changes to them would have to be published), while the rest of Toolbench is unaffected. Toolbench doesn't modify them. Source: <https://github.com/Vanilagy/mediabunny>.

### libheif (HEIC/HEIF decoding): LGPL-3.0

- **Package:** `libheif-js` 1.23.5 (`libheif.wasm`, loaded lazily by the image worker).
- **Bundled decoders:**
  - libde265 (HEVC): LGPL-3.0
  - dav1d (AV1): BSD-2-Clause
- **Source:** <https://github.com/strukturag/libheif>, <https://github.com/strukturag/libde265>, <https://github.com/catdad-experiments/libheif-js>.
- **LGPL notes:** the library is a separate, replaceable file, so a user can substitute a modified build. Its license text and a source link are shown in the app's Open-source licenses page.

### Liberation Sans (fallback fonts used by pdf.js): GPL-2.0 with the Liberation font exception

- Served from `/vendor/pdfjs/standard_fonts/` and used only to draw standard PDF fonts. The full terms are in `LICENSE_LIBERATION` next to the fonts.

## Permissive components

| Component                                                                                                             | Used for                    | License                                                                           |
| --------------------------------------------------------------------------------------------------------------------- | --------------------------- | --------------------------------------------------------------------------------- |
| `@ffmpeg/ffmpeg` 0.12.15, `@ffmpeg/util` 0.12.2                                                                       | ffmpeg.wasm JS wrapper      | MIT                                                                               |
| [pdf.js](https://mozilla.github.io/pdf.js/) (`pdfjs-dist` 6.5.438)                                                    | PDF rendering               | Apache-2.0                                                                        |
| pdf.js wasm decoders: OpenJPEG, PDFium JBIG2, qcms, QuickJS                                                           | JPEG 2000, JBIG2, color, JS | BSD-2-Clause, BSD-3-Clause, MIT, MIT                                              |
| Foxit standard fonts (via pdf.js)                                                                                     | PDF standard fonts          | BSD-3-Clause-style (`LICENSE_FOXIT`)                                              |
| [pdf-lib](https://pdf-lib.js.org) 1.17.1                                                                              | PDF editing                 | MIT                                                                               |
| `@jsquash/*` (Squoosh codecs): MozJPEG, libpng/OxiPNG, libwebp, libavif + libaom, resize, hqx, magic-kernel           | Image codecs                | Apache-2.0 wrappers; codecs IJG/BSD-3/Zlib, libpng, BSD-3, BSD-2, MIT, Apache-2.0 |
| [Tesseract](https://github.com/tesseract-ocr/tesseract) via `tesseract.js` 7, `tesseract.js-core` 7 (incl. Leptonica) | OCR                         | Apache-2.0 (Leptonica: BSD-2-Clause)                                              |
| `@tesseract.js-data/*` (tessdata_best_int / fast models)                                                              | OCR language models         | Apache-2.0 models; MIT packaging                                                  |
| [exifr](https://github.com/MikeKovarik/exifr) 7.1.3                                                                   | Metadata reading            | MIT                                                                               |
| [Papa Parse](https://www.papaparse.com) 5.7.1                                                                         | CSV                         | MIT                                                                               |
| [qrcode](https://github.com/soldair/node-qrcode) 1.5.4                                                                | QR generation               | MIT                                                                               |
| [jsQR](https://github.com/cozmo/jsQR) 1.4.0                                                                           | QR decoding                 | Apache-2.0                                                                        |
| [hash-wasm](https://github.com/Daninet/hash-wasm) 4.13.0                                                              | Streaming hashes            | MIT                                                                               |
| [jsdiff](https://github.com/kpdecker/jsdiff) (`diff` 9.0.0)                                                           | Text diff                   | BSD-3-Clause                                                                      |
| [fflate](https://github.com/101arrowz/fflate) 0.8.3                                                                   | Zip                         | MIT                                                                               |
| [Comlink](https://github.com/GoogleChromeLabs/comlink) 4.4.2                                                          | Worker RPC                  | Apache-2.0                                                                        |
| [idb](https://github.com/jakearchibald/idb) 8.0.4                                                                     | IndexedDB wrapper           | ISC                                                                               |
| [Svelte](https://svelte.dev) 5                                                                                        | UI                          | MIT                                                                               |

The permissive licenses require their copyright notices and license texts to be reproduced with the software. The app's Open-source licenses page carries them all, including the LGPL and library notices for the FFmpeg core (copied from the exact upstream sources).

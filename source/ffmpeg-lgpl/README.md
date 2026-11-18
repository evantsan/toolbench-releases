# LGPL-only ffmpeg.wasm core

The FFmpeg core that Toolbench ships (in the installer and the web app, at `/vendor/ffmpeg/`), built by
the GitHub Actions workflow [`build/ffmpeg-lgpl.yml`](build/ffmpeg-lgpl.yml) from the files in
[`build/`](build): upstream [ffmpeg.wasm v12.15](https://github.com/ffmpegwasm/ffmpeg.wasm/tree/v12.15)
(commit `71aa99d37c02a7b4c435275ca9ef50e612f6efa1`, MIT; the tag npm `@ffmpeg/core` 0.12.10 was published from) with x264, x265, libpostproc and unused
libraries removed, plus upstream's later 5 MB stack (commit `b409e36`; libvpx and libopus overflow the 64 KB default). FFmpeg n5.1.4, emscripten 3.1.40, `-O3 -msimd128`, single-threaded.

**License:** LGPL-2.1-or-later as a whole (FFmpeg LGPL-2.1+, LAME LGPL-2.0+; libvpx, libvorbis,
libogg, libopus, libwebp BSD; zlib Zlib). Loaded as a separate, replaceable file in its own worker.

|                    |                                                                              |
| ------------------ | ---------------------------------------------------------------------------- |
| Variant            | `st` (only this one ships; the workflow can build `mt`)                      |
| Built              | 2026-11-15                                                                   |
| `ffmpeg-core.js`   | `7195ee0b56f1221097fa72da605a429857aabc79e4e802aaf10d49361d65cf91`           |
| `ffmpeg-core.wasm` | `cfd9f1f69136f4cb702c6178eca47347c3bde8494601fe4f131b43ed4a0bfbf0` (23.0 MB) |

Configure line, as read back from the wasm:

```
--target-os=none --arch=x86_32 --enable-cross-compile --disable-asm --disable-stripping --disable-programs --disable-doc --disable-debug --disable-runtime-cpudetect --disable-autodetect --nm=emnm --ar=emar --ranlib=emranlib --cc=emcc --cxx=em++ --objcc=emcc --dep-cc=emcc --extra-cflags='-I/opt/include -O3 -msimd128' --extra-cxxflags='-I/opt/include -O3 -msimd128' --disable-pthreads --disable-w32threads --disable-os2threads --disable-gpl --disable-nonfree --disable-version3 --disable-postproc --enable-libvpx --enable-libmp3lame --enable-libvorbis --enable-libopus --enable-zlib --enable-libwebp
```

**Rebuilding or replacing it.** Run `build/ffmpeg-lgpl.yml` in your own GitHub repository (or run the
`Dockerfile` locally with Docker), which produces `ffmpeg-core.js` and `ffmpeg-core.wasm`. The web app loads
these two files from `/vendor/ffmpeg/`; the Windows app carries the same two files inside `Toolbench.exe`.
FFmpeg's own source is at <https://github.com/FFmpeg/FFmpeg/tree/n5.1.4>.

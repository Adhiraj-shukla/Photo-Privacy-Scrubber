# Photo Privacy Scrubber

A browser-based tool that shows what hidden data your photos carry (GPS location, device, timestamps) and lets you remove it before sharing. Everything runs on your device. **No photo is ever uploaded to a server.**

**Live demo:** https://Adhiraj-shukla.github.io/Photo-Privacy-Scrubber/

## Why this exists

Photos taken on phones and cameras often embed EXIF metadata. That can include the exact coordinates where the photo was taken, the phone model, and the date and time. Sharing the original file can leak your home, school, or daily routine without you realising. This tool makes that data visible and removes it.

## Features

- Drag and drop, file picker, or paste (Ctrl+V) for one or many photos
- Metadata viewer grouped by location, device, time, camera settings, and image info
- Privacy summary for each photo, such as "Reveals: exact location, device, when it was taken"
- Map link for photos that contain GPS coordinates (OpenStreetMap)
- One-click metadata removal
- Selective cleaning: optionally keep the date and time, or camera settings (JPEG only)
- Verification step that re-reads the cleaned file and reports what is left
- File size comparison before and after cleaning
- Batch cleaning with a single zip download
- Responsive layout with automatic dark mode

## How it works

| Step | Technique |
|------|-----------|
| Reading metadata | [exifr](https://github.com/MikeKovarik/exifr) parses EXIF, GPS, XMP, and IPTC in the browser |
| Full removal | The image is redrawn on an HTML `<canvas>` and exported with `toBlob()`. Canvas export does not carry metadata over. |
| Selective removal (JPEG) | [piexifjs](https://github.com/hMatoba/piexifjs) rewrites the EXIF block, keeping only the fields you choose, without re-encoding the image |
| Batch download | [JSZip](https://stuk.github.io/jszip/) bundles the cleaned photos |
| Verification | The cleaned file is parsed again with exifr |

All libraries load from CDNs, so the project is a single `index.html` with no build step.

## Run locally

1. Download or clone this repository.
2. Open `index.html` in a modern browser.

An internet connection is needed the first time to load the libraries and fonts from their CDNs.

## Deploy to GitHub Pages

1. Put `index.html` in the root of your repository.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, then select `main` and `/ (root)`.
4. Save. The site goes live at `https://YOUR-USERNAME.github.io/YOUR-REPO/` after a minute or two.

## Privacy

- Files are read and processed in memory by your browser.
- There is no backend, no analytics, and no network request that includes your photos.
- The only external requests are for the libraries and fonts used by the page.

## Known limitations

- **HEIC (iPhone default):** most browsers cannot decode HEIC, so cleaning fails with a message. Convert to JPEG first. Adding [heic2any](https://github.com/alexcorbi/heic2any) would fix this.
- **Quality:** full removal re-encodes JPEG and WebP at quality 0.95, so there is a small quality loss. Selective JPEG mode does not re-encode.
- **Selective mode and XMP:** some photos store extra metadata in XMP segments that selective mode may not remove. The verification message warns if location data is still present. Cleaning with nothing kept removes everything.
- **Very large images:** may be slow or hit canvas size limits on low-memory phones.
- Metadata is not the only privacy risk. Faces, street signs, and screens in the photo itself are not detected or blurred.

## Future work

- Lossless JPEG stripping for full removal without re-encoding
- HEIC support
- Face and text blur
- Installable offline PWA
- Before and after metadata comparison view

## Tech stack

HTML, CSS, and vanilla JavaScript. Libraries: exifr, piexifjs, JSZip.

## License

MIT. Add a `LICENSE` file to your repository if you want to use this license.

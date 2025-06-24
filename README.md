# Spotlight

A Jellyfin homepage enhancement that injects a configurable slideshow of user-permitted content onto the Jellyfin homepage.

This project was initiated by **BobHasNoSoul** ([BobHasNoSoul](https://github.com/BobHasNoSoul)) and I in [**November 2023**](https://forum.jellyfin.org/t-featured-content-bar).

Since then, several implementations have been released. The most streamlined install, and what I would currently reccommend is 

\*\*MakD's \*\*[**Jellyfin-Media-Bar**](https://github.com/MakD/Jellyfin-Media-Bar) 

This Spotlight repo is a bit more fiddily in installation..

---

## ✨ Features

- **Movies & Series**: Showcase backdrops, logos, and metadata for both movies and TV series.
- **Trailer Playback**: Fetch and play trailers from YouTube (optional) with a number of adjustments to our nested iframe.
- **Configurable Interval**: Set how long static slides remain visible.
- **Custom Lists**: Supply your own list of item IDs to show via `list.txt`.
- 
---

## 🚀 Installation

1. **Download the Assets**

   - [`spotlight.html`](./spotlight.html)
   - [`spotlight.css`](./spotlight.css)

2. \*\*Configure \*\*\`\`\
   At the top of the file, edit the `config` block:

   ```js
   const config = {
     mediaTypes: 'movie,series',    // 'movie', 'series', or 'movie,series'
     useTrailers: true,             // true to enable trailers, false for static slides
     shuffleInterval: 30,           // seconds per non-trailer slide
     token: 'YOUR_JELLYFIN_API_KEY' // your Jellyfin API key
   };
   ```

3. **Deploy to Jellyfin Web UI**

   ```bash
   cd /path/to/jellyfin-web/ui/
   mkdir -p spotlight
   cp /path/to/spotlight.html /path/to/spotlight.css spotlight/
   ```

4. \*\*Inject into \*\*\`\`

   - Locate the chunk file in `/path/to/jellyfin-web/`.
   - Find the line ending with `data-backdroptype="movie,series,book">`.
   - Immediately after, insert:
     ```html
     <style>
       .featurediframe {
         width: 99.5vw;
         height: 63vh;
         display: block;
         border: 0;
         margin: -65px auto -50px auto;
       }
     </style>
     <iframe
       class="featurediframe"
       src="/web/ui/spotlight/spotlight.html"
       tabindex="0">
     </iframe>
     ```

5. **Clear Cache & Refresh**\
   Press `Ctrl + Shift + R` (or `Cmd + Shift + R` on macOS) until the slideshow appears on your Jellyfin homepage.

---

## 📋 Custom Lists (`list.txt`)

Place a `list.txt` inside the `spotlight` folder to override random selection:

```
Happy Fathers Day!               This first line is picked up as the list title (but not currently used)
4a05a2baa566acb6ea1de8edb75a56d6 This right-side text is ignored by the script so you can label these items here
8c4a181803702a61cc48072bd5113fb6 Copy these item IDs out of the Media item page address
496528765c9937932301a1590752a7f4 If a user doesnt have access to an item it will skip
c3c48cb8a80c0b5172e8470966a10381 The Shining
5f5c9047099065f639213d29471a5b19 There Will be Blood
1e178c7db16ff20bdad078a2b94780b7 Home Alone
```

- **Line 1**: List title (for reference; not displayed).
- **Lines 2+**: Jellyfin item IDs (trailing comments/text ignored).

---

## 📄 License

Distributed under the [MIT License](LICENSE).


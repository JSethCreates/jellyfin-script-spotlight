# Spotlight - Trailers on the Jellyfin Homepage

**Spotlight** helps you rediscover your media by playing trailers fetched from your libraries' metadata — creating a living, evolving homepage slideshow that highlights your collection.

![2](https://raw.githubusercontent.com/JSethCreates/jellyfin-script-spotlight/refs/heads/main/screenshots/2.PNG)

---

### How Spotlight works

- **Dynamic discoveries**  
  Spotlight searches your library for a random movie or series (configurable in the HTML file). If the item has a background image and a logo, it creates a slide.  
  When a movie or series has a trailer linked in its metadata, Spotlight fetches and embeds that YouTube trailer directly on the slide.
  It can also read from a static list of media in list.txt (more below)

- **Trailer aspect ratio detection**  
  Spotlight uses noembed to determine each trailer’s true aspect ratio. The container is dynamically resized to minimize letterboxing and pillarboxing, showing as much video area in teh frame as possible. (Hard-coded black bars on the yt video itself cannot be clipped.)

- **Cinematic YouTube playback**  
  Trailers are embedded using YouTube’s minimal player configuration:
  - Auto-plays without overlays.
  - Suppresses most UI elements for a cleaner presentation.
  - Starts at moderate volume (50% by default).

- **Automatic transitions**  
  After a trailer finishes, Spotlight automatically loads the next slide. If no trailer is available, a static slide is shown (default duration: 10 seconds, configurable in HTML).

- **Detailed metadata overlays**  
  Each slide includes:
  - **Year** of release.
  - **Duration** (minutes for movies, seasons for series).
  - **Ratings** (community and critic, where available).
  - **Logo** (if available) and **backdrop** image.
  - **Plot summary** at the bottom.

- **Access-aware randomization or custom lists**  
  Media items can be pre-defined in a `list.txt` file. If absent, Spotlight selects random items from the user’s accessible library (respecting profile restrictions, ie. it cannot serve content a user is unable to access).

![1](https://raw.githubusercontent.com/JSethCreates/jellyfin-script-spotlight/refs/heads/main/screenshots/1.PNG)

---

## 🛠️ Windows Installation

1. Create a `UI` folder inside your `\Server\jellyfin-web\` directory.  
2. Place `spotlight.html` and `spotlight.css` inside that folder.

3. In your `\Server\jellyfin-web\` directory, find the file named `home-html.~random-characters~.chunk.js` and edit it.
   Paste the following snippet after  
   **...movie,series,book">**:

```html
<style>
  .spotlightiframe {
    width: 99.5vw;
    height: 63vh;
    display: block;
    border: 0px solid #000;
    margin: -65px auto -50px auto;
  }
</style>
<iframe class="spotlightiframe" src="/web/UI/spotlight.html" tabindex="0"></iframe>
```

4. Save your modefied chunk.js file, hard-reload your Jellyfin homepage (Ctrl + Shift + R) and enjoy!

---

## Notes

- **Trailers are not loaded if the device width is below 700px.**

- The script goes to great lengths to maximize video size, but if YouTube videos have hard-coded letterbox or pillarbox bars, they cannot be removed.

- The script ensures the YouTube player stops when navigating away from the page. However, due to Jellyfin’s SPA architecture, using the browser "back" button may not fully reload the page, and slides may appear without video. Using Jellyfin’s Home button or refreshing the homepage restores trailer playback.

- Certain youtube videos are not allowed to be played off-site, the fail is noted in console, and those slides will revert to image only

- Several variables can be adjusted around line 100 in `spotlight.html`:
  - `moviesSeriesBoth = 3` — (1 = movies only, 2 = series only, 3 = both)
  - `shuffleInterval = 10000` — time (ms) between static slide changes without video
  - `useTrailers = true` — set to `false` to disable trailers

- You can change the hardcoded fallbackColors to adjust the colors of the buttons, or the frame will inherit colors from your CSS if it is MD3 compatible eg. [GNAT](https://github.com/JSethCreates/jellyfin-theme-sethstyle)

- My ultimate goal is to support local trailers stored on your server, eliminating the need for YouTube. While planned, this feature is **not included in this release**.

- For a static image bar with more thorough Linux integration instructions, consider [MakD’s Jellyfin-Media-Bar](https://github.com/MakD/Jellyfin-Media-Bar).


## 🗒️ Using a `list.txt` file

If you'd like to manually control which media items Spotlight displays, you can provide a `list.txt` file.

- Create a text file named `list.txt` and place it in the same `UI` folder as `spotlight.html` and `spotlight.css`.
- Each line of the file should contain a single Jellyfin item ID.  
- Spotlight will exclusively select items listed in this file, in random order.  
- If `list.txt` is absent or empty, Spotlight will automatically fall back to picking random items from the user's accessible library.

### How to find item IDs

To find a media item's ID:

1. Navigate to the item in your Jellyfin web interface.
2. Copy the URL from your browser — the item ID is the long string after `/details/`.  
   Example:  
   ```
   http://localhost:8096/web/index.html#/details?id=66c39f275a415478171ccd911de550c9&serverId=d0bc182015b64c0ebe491b72839266c7
   ```
   Here, the ID is `66c39f275a415478171ccd911de550c9`.

### Example `list.txt`

```
Happy Fathers Day!               This first line is picked as the list title (but not currently used)
4a05a2baa566acb6ea1de8edb75a56d6 This right-side text is ignored by the script so you can label items
8c4a181803702a61cc48072bd5113fb6 Copy these item IDs out of the Media item page address
496528765c9937932301a1590752a7f4 If a user doesnt have access to an item it will skip
c3c48cb8a80c0b5172e8470966a10381 The Shining
5f5c9047099065f639213d29471a5b19 There Will be Blood
1e178c7db16ff20bdad078a2b94780b7 World's Greatest Dad
```

When `list.txt` is present, only these items will be included in the Spotlight slideshow (with trailers and metadata, if available).


![More Demo] (https://i.imgur.com/sxyrRdX.mp4)

---

## 📄 License

Distributed under the [MIT License](LICENSE).

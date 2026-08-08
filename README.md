# Vishnu & Anitha — Wedding Invitation

## Why your songs weren't working

Your original file pointed to `song1.mp3`, `song2.mp3`, `song3.mp3` — but those
audio files were never actually part of the project, they only existed on your
own computer. When you drag-and-dropped just `index.html` into Netlify, there
was nothing for those filenames to point to, so the browser silently failed
(that's also why the old code had `.catch(err => console.log(...))` — it was
swallowing the error instead of showing you what went wrong).

**The fix:** audio files need to live *inside* the project folder, next to
`index.html`, and you upload the whole folder — not just the HTML file.

## Folder structure (already set up for you)

```
wedding-site/
├── index.html
└── assets/
    ├── audio/          ← put your 3 mp3 files here
    │   ├── song1.mp3
    │   ├── song2.mp3
    │   └── song3.mp3
    └── images/         ← optional: for a couple photo / OG preview image
```

## Steps to add your songs

1. Get your 3 song files (mp3 format, ideally under ~5 MB each so the page
   loads fast on mobile data).
2. Rename them exactly: `song1.mp3`, `song2.mp3`, `song3.mp3` — **filenames
   are case-sensitive on Netlify**, so `Song1.mp3` will NOT match `song1.mp3`.
   (Or keep your own filenames and just edit the `playlist` array near the
   bottom of `index.html` to match.)
3. Drop them into `wedding-site/assets/audio/`.
4. Deploy the **entire `wedding-site` folder** (not just `index.html`) to
   Netlify — see below.

If a song file is missing or broken, the site now skips it automatically
instead of breaking, and hides the music player if none of the songs load —
so the page never looks broken to guests.

## Deploying to Netlify

**Easiest way (drag & drop):**
1. Go to https://app.netlify.com/drop
2. Drag the whole `wedding-site` folder (the one containing `index.html` and
   `assets/`) into the browser window.
3. Netlify gives you a live URL immediately. You can rename it under
   Site settings → Change site name.

**Also fine:** zip the folder first and upload the zip — Netlify unzips it
automatically and keeps the folder structure.

## Other things fixed in this version

- The old file was truncated mid-script — several closing braces, the
  "Next Song" button, and the RSVP button had no working code at all. All of
  that is now complete.
- RSVP form now actually builds a WhatsApp message and opens `wa.me` with the
  guest's name and message pre-filled.
- Countdown now shows "They are married! 🎉" once the date passes, instead of
  freezing at `00:00:00:00`.
- Music player no longer looks broken if a song fails to load — it retries
  the next track, and hides itself if no songs are found at all.
- Added meta description, Open Graph tags, and a favicon so the link looks
  good when shared on WhatsApp.
- Accessibility: labeled form fields, visible focus states, `aria-hidden` on
  decorative icons, skip-to-content link, and reduced-motion support.
- Fixed the background image freezing/jumping on iPhones (fixed backgrounds
  don't render properly in mobile Safari).
- Phone numbers are now tap-to-call with the +91 country code.

## Responsiveness pass (latest update)

- Added a real `assets/images/og-cover.jpg` — the old link pointed to a file
  that never existed, so the preview card was broken every time the link was
  shared on WhatsApp/social media.
- Fixed the music player button and the entrance screen so they no longer sit
  under the iPhone Dynamic Island / notch or Android camera cutouts.
- Fixed rotated/landscape phones (short screen height): the hero used to
  force a full-height block with full padding, which pushed the countdown
  and scroll arrow off-screen. It's now compact in that orientation.
- The hero title now scales up properly on laptop/desktop monitors instead
  of staying tablet-sized once the screen gets past ~768px.
- Re-checked every section at phone (320–430px), tablet (768–1024px), and
  desktop (1440–1920px) widths for horizontal overflow — none found.

## Swapping the demo couple photo

`assets/images/couple-photo.jpg` (the banner in "The Happy Couple" section)
and `assets/images/og-cover.jpg` (the WhatsApp/social preview image) are
currently a demo photo. To use your real photo:

1. Get a nice landscape/wide photo of the two of you (the current one is
   ~761x460px - similar proportions work best so nobody's face gets cropped
   oddly).
2. Replace `assets/images/couple-photo.jpg` with it, keeping the exact same
   filename.
3. For the share-preview image (`og-cover.jpg`), either swap it for a similar
   1200x630px design, or just tell me and I'll regenerate the gold-framed
   version with your real photo.

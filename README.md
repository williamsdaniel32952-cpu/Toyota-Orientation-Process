# TRIGO Employee Orientation — Deployment Package

This is the orientation web app, set up so videos and images load directly
from this folder on every device — phones, tablets, PCs — without any
admin upload step.

## How it works

The app is a single `index.html` file. It looks for media at predictable
paths in the `assets/` folder:

```
assets/images/moduleN/slideN.jpg          (one image per slide)
assets/videos/moduleN/moduleN-intro.mp4   (one intro video per module)
```

Drop a file in the right spot and the app shows it. If a file is missing,
the app silently hides that spot — no error message shown to employees.

## Folder layout

```
employee-orientation/
├── index.html                ← the whole app
├── README.md                 ← this file
└── assets/
    ├── images/
    │   ├── module1/          ← slide images for Module 1
    │   ├── module2/
    │   ├── ...
    │   └── module10/
    └── videos/
        ├── module1/          ← intro video for Module 1
        ├── module2/
        ├── ...
        └── module10/
```

## Module-number reference

| # | Module                                    |
|---|-------------------------------------------|
| 1 | New Hire Information                      |
| 2 | Shift Times & Schedule                    |
| 3 | Cell Phone & Electronics Policy           |
| 4 | Attendance Policy                         |
| 5 | Dress Code & Personal Conduct             |
| 6 | Safety Procedures                         |
| 7 | EPIC Inspector Processes                  |
| 8 | Example Materials & Forms                 |
| 9 | Handbook Highlights — Slide Deck          |
| 10 | Health & Safety Awareness Training       |

## Adding an intro video to a module

1. Have the video file ready — must be `.mp4`, H.264, ideally 720p or lower.
2. Rename it exactly: `moduleN-intro.mp4` (replace N with the module number).
3. Drop it into `assets/videos/moduleN/`.
4. Commit and push. That's it.

Example: the intro for Module 8 goes at:
```
assets/videos/module8/module8-intro.mp4
```

## Adding a supporting image to a slide

1. Save the image as `.jpg`.
2. Name it `slideN.jpg` where N is the 1-based slide number inside that
   module (slide 1, slide 2, etc).
3. Drop it into `assets/images/moduleN/`.
4. Commit and push.

Example: an image for the third slide of Module 8 goes at:
```
assets/images/module8/slide3.jpg
```

## Deployment to GitHub Pages

1. Create a new public GitHub repository (any name).
2. Upload the entire contents of this folder to the repository root.
3. In the repo, go to **Settings → Pages**.
4. Under "Build and deployment", set **Source** to "Deploy from a branch",
   pick the `main` branch and `/ (root)` folder, then **Save**.
5. Wait about a minute. GitHub will show you the public URL at the top of
   the Pages settings (something like
   `https://yourname.github.io/repo-name/`).
6. Open that URL in any browser to use the app.

## Deployment elsewhere (any web server)

The app is fully static — no backend, no build step, no Node/npm. Just
host the files. Works on:

- Any static web host (Netlify, Cloudflare Pages, AWS S3, your company's
  IIS or Apache server)
- A network share mapped as a web folder
- A local folder opened directly in a browser (some features like
  videos may not play from `file://` due to browser security — best to
  host via a real web server)

The only requirement is that the relative folder structure under
`assets/` is preserved exactly.

## What does NOT need to be done

- **No admin upload.** Don't use the admin panel's "Add Video" or
  "Add Image" buttons for production media — those store files only in
  one person's browser. Drop files into the `assets/` folder instead.
- **No content-data.json file is required.** All module content is built
  into `index.html`. The admin features still work for editing slide
  titles, adding quiz questions, etc., on a per-device basis — but for
  rolling out to everyone, the canonical content is what's in the code.
- **No re-encoding required** as long as the video is `.mp4` (H.264).
  720p is recommended for fast loading on phones. AVI / MKV won't play
  in browsers — convert with VLC or HandBrake first.

## Updating later

To swap a video: delete the old one and upload a new one with the same
filename. Commit. The cached version may need a hard refresh on phones
(pull down to refresh in Safari/Chrome) to pick up the new file.

## Troubleshooting

**A video doesn't play on phone:**
1. Check the file exists in the repo at exactly the expected path
   (case-sensitive — `Module8` is not the same as `module8`).
2. Check the filename matches exactly: `module8-intro.mp4` with a lowercase
   "m" and a hyphen.
3. Confirm the file is actually `.mp4` H.264. If you renamed an `.avi`
   to `.mp4`, the browser still won't play it. Convert properly with
   VLC: Media → Convert/Save → set Profile to "Video — H.264 + MP3 (MP4)".

**An image doesn't show on phone:**
- Same case-sensitive filename check. `Slide1.jpg` ≠ `slide1.jpg`.
- Confirm the file is genuinely `.jpg`. PNG renamed to JPG works in some
  browsers and not others. Re-export from the original.

**Employee progress disappears between sessions:**
- This is expected if they clear browser data or use a different device.
  Progress is tracked per-device in browser localStorage. For
  permanent records, export employee data from the admin panel.

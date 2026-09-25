# Image Occlusion for Anki (AnkiDroid)

Cover parts of an image, tap a box to reveal it — all in a **single** Anki card. Unlike Anki's built-in Image Occlusion, this doesn't split into one card per masked region.

## What's included

- `occlusion_front_template.html` — Front Template
- `occlusion_back_template.html` — Back Template
- `occlusion_styling.css` — Styling
- [box-picker](https://pratikkarbhal.github.io/Anki-multiple-image-occulsion/) — standalone tool to draw boxes visually and generate the coordinate data for you

## Setup

Works directly on the standard **Basic** note type. Your existing `Front` field holds the images exactly as you already add them — you're just adding one new field for the box data.

1. Anki Desktop → **Tools → Manage Note Types** → **Basic** → **Fields...** → **Add field** → name it exactly `Occlusions`
2. Same note type → **Cards...**
3. Paste `occlusion_front_template.html` into **Front Template**
4. Paste `occlusion_back_template.html` into **Back Template**
5. Paste `occlusion_styling.css` into **Styling**
6. Save, then sync AnkiDroid

**Fields used:** `Front` (your images) and `Occlusions` (box coordinates — format below).

If your images live in a field named something other than `Front`, find-and-replace `{{Front}}` in both template files before pasting.

## Getting coordinates: the Box Picker tool

Drawing coordinates by hand is painful, so this tool does it visually instead.

1. Open [`box-picker.html`](https://pratikkarbhal.github.io/Anki-multiple-image-occulsion/) in any browser — phone or desktop, no install needed
2. Tap **+ Add Page(s)**, select your images in the **same order** they appear on the Anki card
3. All pages stack, scrollable, like a document — drag directly on any image to draw a box, no typing required
4. Tap an existing box to delete it. Use ▲▼🗑 in a page's header to reorder or remove a page
5. Tap **Generate Code**, then **Copy to Clipboard**
6. Paste the result into the `Occlusions` field for that note

Boxes are "blank" by design — tapping one on the Front just makes it disappear, revealing whatever's already on the image underneath. Works best when the image already has the answer printed at that spot.

## Occlusions field format

```
imgIndex:x,y,w,h; x,y,w,h || imgIndex:x,y,w,h
```

- `x, y, w, h` are **percentages** (0–100) of that image's own size, not pixels — stays correct at any screen size
- Multiple boxes on one image: separate with `;`
- Multiple images: separate their blocks with `||`
- An optional `=Label` can follow any box's coordinates if you want typed text instead of a blank reveal — the Box Picker tool just doesn't generate this by default

Example — one box on image 1, two boxes on image 2:

```
1:10,15,25,10 || 2:5,5,30,10; 40,40,15,15
```

## Limitations

- Template editing needs Anki Desktop — AnkiDroid alone can't reliably edit templates, so make changes on desktop and sync
- Box/answer data sits in the card's HTML, so it's inspectable via WebView dev tools — fine for personal study, not tamper-proof
- Field names are case-sensitive; `{{Front}}` in the templates must exactly match your actual field name


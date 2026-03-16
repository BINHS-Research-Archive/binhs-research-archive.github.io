# BINHS Research Archive — School Research Library

A lightweight, static research paper library hosted on GitHub Pages for completed research papers of the Senior High School students of Bulihan Integrated National High School. Papers are stored in Google Drive and the site auto-syncs from a public folder — no backend, no database, no manual catalog updates needed.

---

## Features

- 🔍 Full-text search across title, authors, abstract, section, keywords, and more
- 🎨 Dark and light mode with green accent theme
- 🏷️ Filter by Strand, Grade, and School Year
- 📄 View and download papers directly from Google Drive
- ⚡ Local caching for fast repeat visits with background refresh
- 📱 Fully mobile-responsive with bottom-sheet drawer on mobile
- 🔄 Force refresh button to manually sync new papers
- 💛 Minecraft-style splash text loaded from Google Drive
- 🖼️ School logo auto-loaded from Google Drive

---

## How It Works

1. A teacher uploads a PDF and a matching TXT metadata file into the correct Strand → School Year subfolder in Google Drive
2. The site recursively scans the folder structure via the Google Drive API on load
3. Strand and School Year are read directly from the folder names — no need to fill them in the TXT file
4. Papers are parsed, displayed as cards, and cached locally for 30 minutes
5. No rebuild or redeployment needed when new papers, strands, or school years are added

---

## Drive Folder Structure

The root folder contains strand subfolders, each containing school year subfolders. Files go inside the year folders.

```
📁 Papers/                        ← root folder (ID goes in config)
├── 📄 school-logo.png            ← auto-loads into header and footer
├── 📄 SplashText.txt             ← random splash quotes (one per line)
│
├── 📁 ABM/
│   ├── 📁 S.Y 23-24/
│   │   ├── ResearchTitle-1001.pdf
│   │   └── ResearchTitle-1001.txt
│   ├── 📁 S.Y 24-25/
│   └── 📁 S.Y 25-26/
│
├── 📁 AD/
│   └── 📁 S.Y 24-25/
│
├── 📁 EIM/
│   └── 📁 S.Y 25-26/
│
├── 📁 HE/
├── 📁 HUMSS/
├── 📁 SMAW/
└── 📁 STEM/
```

**Fully dynamic** — add a new strand folder or a new year folder in Drive and the site picks it up automatically on next sync. No code changes needed.

---

## File Naming Convention

Each paper requires two files with a matching 4-digit code suffix:

```
ResearchTitle-1234.pdf
ResearchTitle-1234.txt
```

The `-XXXX` suffix links the PDF to its metadata. The title portion of the filename does not matter — only the 4-digit code is used for matching.

---

## Metadata Format (TXT file)

```
Title: 
Authors: 
Date Published: 
Grade: 
Section: 
Research Type: 
Format and Design: 
Subject Area: 
Adviser: 
Keywords: 
Award: 
Abstract: 
```

> **Note:** `Strand` and `School Year` are automatically read from the folder names and do not need to be included in the TXT file. They can still be added as a fallback for files placed outside the folder structure.

### Field Reference

| Field             | Format                                | Example                                    |
|-------------------|---------------------------------------|--------------------------------------------|
| Title             | Full title                            | Effects of Vermicompost on Ampalaya Growth |
| Authors           | Comma-separated                       | Ana Reyes, Carlo Santos, Mia Cruz          |
| Date Published    | YYYY-MM-DD                            | 2024-02-10                                 |
| Grade             | Number only                           | 12                                         |
| Section           | Name only, no prefix                  | Marie Curie                                |
| Research Type     | Free text                             | Quantitative                               |
| Format and Design | Free text                             | True Experimental Design                   |
| Subject Area      | Free text                             | Biology                                    |
| Adviser           | Include title                         | Ms. Maria Santos                           |
| Keywords          | Comma-separated, 3–6 terms            | vermicompost, ampalaya, plant growth       |
| Award             | Leave blank if none                   | Best in Research — Regional Level          |
| Abstract          | Paragraph text, line breaks preserved | (see notes below)                          |

### Strand Values

The strand is taken from the folder name automatically. Current strands:

`ABM` · `AD` · `EIM` · `HE` · `HUMSS` · `SMAW` · `STEM`

Each strand has its own color coding in the library. Any new strand folder added to Drive gets a color assigned automatically — no code changes needed.

### Abstract Notes

- Everything after `Abstract:` is captured as the abstract
- Line breaks and blank lines are preserved exactly as written
- Blank lines become paragraph breaks in the library
- Do **not** use `<br>` or `<p>` tags — plain text only

### Optional Fields

All fields except **Title** and **Abstract** are optional. If a field is left blank or the line is removed entirely, that tag simply won't appear on the card or drawer. No broken layout, no placeholder text.

### Full Example

```
Title: Effects of Vermicompost on Ampalaya Growth in Urban Settings
Authors: Ana Reyes, Carlo Santos, Mia Cruz
Date Published: 2024-02-10
Grade: 12
Section: Archimedes
Research Type: Quantitative
Format and Design: True Experimental Design
Subject Area: Biology
Adviser: Ms. Maria Santos
Keywords: vermicompost, ampalaya, urban gardening, organic fertilizer, plant growth
Award: Best in Research — Regional Level
Abstract: This study examined the effectiveness of vermicompost as an organic fertilizer
for Momordica charantia (ampalaya) in urban container gardens.

Results showed a 34% increase in leaf biomass and a 28% improvement in fruit yield
compared to commercial fertilizer controls over an 8-week growth period.

The findings suggest that vermicompost is a viable, low-cost alternative to synthetic
fertilizers for urban agriculture applications.
```

This file would be placed at: `Papers/STEM/S.Y 24-25/EffectsOfVermicompost-1234.txt`

---

## Drive Assets

Two optional files can be placed in the **root of the Drive folder** (not inside any subfolder):

### `school-logo.png`

Automatically loads into the site header and footer. Supported formats: `png`, `jpg`, `jpeg`, `webp`. Name the file exactly as shown.

### `SplashText.txt`

Supplies random Minecraft-style splash quotes shown in the hero section. One quote per line. Refreshes on every page load and every time a research paper drawer is closed.

```
Also try STEM!
Have you cited your sources?
S.Y 25-26 edition!
Research is formalized curiosity!
BINHS represents!
```

If the file is missing or empty, the site falls back to a set of built-in default quotes.

---

## Setup

### 1. Google Drive (dedicated account recommended)

1. Create a new Google account solely for library storage
2. Create a root folder (e.g. `Papers`)
3. Right-click the folder → **Share** → **Anyone with the link can view**
4. Create strand subfolders (e.g. `STEM`, `ABM`) inside the root folder
5. Create school year subfolders (e.g. `S.Y 25-26`) inside each strand folder
6. Upload PDF + TXT pairs into the year folders — files inherit sharing settings automatically
7. Copy the **Folder ID** from the root folder URL:
   ```
   https://drive.google.com/drive/folders/THIS_IS_YOUR_FOLDER_ID
   ```

### 2. Google Cloud Console

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a new project
3. Enable the **Google Drive API**
4. Go to **Credentials** → **Create Credentials** → **API Key**
5. Click the key to edit it:
   - **Application restrictions** → HTTP referrers → add `https://binhs-research-archive.github.io/*`
   - **API restrictions** → Restrict key → select **Google Drive API**
6. Save and copy the API key

### 3. Configure the Site

Open `index.html` and find these two lines near the top of the `<script>` tag:

```js
const DRIVE_FOLDER_ID = 'YOUR_FOLDER_ID_HERE';
const GOOGLE_API_KEY  = 'YOUR_API_KEY_HERE';
```

Replace both values with your actual Folder ID and API Key.

### 4. Deploy to GitHub Pages

1. Push `index.html`, `LICENSE`, and `README.md` to your repository
2. Go to **Settings** → **Pages**
3. Set source to **Deploy from a branch** → `main` → `/ (root)`
4. Your site will be live at `https://binhs-research-archive.github.io/`

---

## Caching

The library caches paper data in the browser's `localStorage` for **30 minutes**.

| Scenario                   | Behavior                                            |
|----------------------------|-----------------------------------------------------|
| First visit                | Fetches from Drive, saves to cache                  |
| Return visit within 30 min | Loads instantly from cache                          |
| Return visit after 30 min  | Loads from cache instantly, refreshes in background |
| Sync failure with cache    | Serves stale cache with a warning banner            |
| Sync failure without cache | Shows error state                                   |
| 🔄 button clicked          | Clears cache and force-fetches from Drive           |

---

## License

### Website Code

The source code of this project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

### Research Papers

All research papers hosted through this library are the intellectual property of their respective student authors.

Unless otherwise noted, papers are shared under **Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)**:

- ✅ You may read, share, and adapt the papers
- ✅ You must credit the original authors
- ❌ You may not use them for commercial purposes

> Research papers © their respective authors. Licensed under [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/) unless otherwise noted.

---

## Built With

- Vanilla HTML, CSS, and JavaScript — no frameworks, no build tools
- [Google Drive API v3](https://developers.google.com/drive/api/v3/about-sdk)
- [Plus Jakarta Sans](https://fonts.google.com/specimen/Plus+Jakarta+Sans) + [Playfair Display](https://fonts.google.com/specimen/Playfair+Display) via Google Fonts
- Hosted on [GitHub Pages](https://pages.github.com)
- `localStorage` for client-side caching

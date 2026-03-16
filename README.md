# BINHS Research Archive — School Research Library

A lightweight, static research paper library hosted on GitHub Pages for completed research papers of the Senior High School students of Bulihan Integrated National High School. Papers are stored in Google Drive and the site auto-syncs from a public folder — no backend, no database, no manual catalog updates needed.

---

## Features

- 🔍 Full-text search across title, authors, abstract, section, keywords, and more
- 🎨 Dark and light mode with green accent theme
- 🏷️ Filter by Strand and Grade level
- 📄 View and download papers directly from Google Drive
- ⚡ Local caching for fast repeat visits with background refresh
- 📱 Fully mobile-responsive with bottom-sheet drawer on mobile
- 🔄 Force refresh button to manually sync new papers

---

## How It Works

1. A teacher uploads a PDF and a matching TXT metadata file to a shared Google Drive folder
2. The site scans the folder via the Google Drive API on load
3. Papers are parsed, displayed as cards, and cached locally for 30 minutes
4. No rebuild or redeployment needed when new papers are added

---

## File Naming Convention

Each paper requires two files in the Drive folder with a matching 4-digit code:

```
ResearchTitle-1234.pdf
ResearchTitle-1234.txt
```

The `-XXXX` suffix links the PDF to its metadata. The title portion of the filename does not matter — only the code suffix is used for matching.

---

## Metadata Format (TXT file)

```
Title: 
Authors: 
Date Published: 
Grade: 
Strand: 
Section: 
Research Type: 
Format and Design: 
Subject Area: 
School Year: 
Adviser: 
Keywords: 
Award: 
Abstract: 
```

### Field Reference

| Field             | Format                                | Example |
|-------------------|---------------------------------------|--------------------------------------------|
| Title             | Full title                            | Effects of Vermicompost on Ampalaya Growth |
| Authors           | Comma-separated                       | Ana Reyes, Carlo Santos, Mia Cruz          |
| Date Published    | YYYY-MM-DD                            | 2024-02-10                                 |
| Grade             | Number only                           | 12                                         |
| Strand            | See valid values below                | STEM                                       |
| Section           | Name only, no prefix                  | Marie Curie                                |
| Research Type     | Free text                             | Quantitative                               |
| Format and Design | Free text                             | True Experimental Design                   |
| Subject Area      | Free text                             | Biology                                    |
| School Year       | SY format                             | 2023-2024                                  |
| Adviser           | Include title                         | Ms. Maria Santos                           |
| Keywords          | Comma-separated, 3–6 terms            | vermicompost, ampalaya, plant growth       |
| Award             | Leave blank if none                   | Best in Research — Regional Level          |
| Abstract          | Paragraph text, line breaks preserved | (see below)                                |

### Valid Strand Values

`STEM` · `HUMSS` · `ABM` · `GAS` · `TVL` · `SPORTS` · `ARTS`

Each strand has its own color coding in the library.

### Abstract Notes

- Everything after `Abstract:` is captured as the abstract
- Line breaks and blank lines are preserved exactly as written
- Blank lines become paragraph breaks in the library
- Do **not** use `<br>` or `<p>` tags — plain text only

### Optional Fields

All fields except **Title** and **Abstract** are optional. If a field is left blank or the line is removed entirely, that tag simply won't appear on the card. No broken layout or placeholder text.

---

## Setup

### 1. Google Drive (dedicated account recommended)

1. Create a new Google account solely for library storage
2. Create a folder in Drive (e.g. `Papers`)
3. Right-click the folder → **Share** → **Anyone with the link can view**
4. Upload PDF + TXT pairs into the folder — files inherit the folder's sharing settings
5. Copy the **Folder ID** from the URL:
   ```
   https://drive.google.com/drive/folders/THIS_IS_YOUR_FOLDER_ID
   ```

### 2. Google Cloud Console

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a new project
3. Enable the **Google Drive API**
4. Go to **Credentials** → **Create Credentials** → **API Key**
5. Click the key to edit it:
   - **Application restrictions** → HTTP referrers → add `https://yourusername.github.io/*`
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
4. Your site will be live at `https://yourusername.github.io/your-repo-name`

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
| 🔄 button clicked          |Clears cache and force-fetches from Drive            |

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

> **📌 Two guides, two jobs.** For what FFT does today and the rules behind it, see [FFT — How It Works](https://claude.ai/artifact/W7u5DM3w3YRWX7CRZ6AoDT) — always current, updated with every release. This page is the **step-by-step tutorial with screenshots**. It matches FFT **v2.2.0** (October 2026).

## Introduction
File Formatting Tool (FFT) is a Microsoft Word add-in. It formats regulatory documents for health authority submissions faster, more consistently and with fewer mistakes.

FFT is made for documents that:
- Follow the eCTD structure
- Contain many tables, figures and cross-references
- Are revised and re-formatted often

Three advantages:
- Works directly inside Word
- Works on any Word document
- Keyboard-driven: one key per paragraph

FFT supports three regions. Pick the region first, and the page size, fonts, captions and document list follow:
- **US** — eCTD Modules 1–3, USPI, General / SOP
- **EU** — SmPC, General / SOP
- **JP** — NDA Module 2, Module 1 (1.6 translated labels), General

Once FFT is opened in Word, it appears as a task pane on the right side of the document.

## Sign In (since v2.0)
![48](/fft_user_guide/images/48.png)

FFT asks you to sign in the first time on a computer.

1. Click **Create account**, enter your work e-mail and a password (8 characters or more), click **Create**.
2. A 6-digit code arrives by e-mail from fft@binltools.com (check junk the first time). Read it at your own pace — the pane waits.
3. Type the code and click **Confirm**.

- TopAlliance colleagues and pre-registered partner users are activated at once. Anyone else sees **Awaiting approval** until TopAlliance activates the account; the pane updates by itself.
- You stay signed in on that computer. The top of the pane shows your e-mail and organization, and the days left when the licence has 30 days or fewer.
- **Sign out**: Settings → Sign out. **Forgot password**: on the sign-in screen, enter your e-mail, type the code from the e-mail, choose a new password.
- Licences belong to an organization (TopAlliance, a partner company). When a licence expires the pane says so and Step 1 stops loading styles; contact TopAlliance to renew.

Two web pages use the same account:
- **[binltools.com/fft/finalize.html](https://binltools.com/fft/finalize.html)** runs the JP finishing script and the label scripts on uploaded files, one or several at a time. No Python needed; each result downloads by itself; files are processed in memory and not stored.
- **[binltools.com/fft/admin.html](https://binltools.com/fft/admin.html)** is the administrator's page: accounts, organizations, seats and announcements.

## The Pane
Before formatting, get to know the pane.

### Announcements
![11](/fft_user_guide/images/11.png)

Version updates, known issues and new features are posted here. Click the bell at the top left to open the board.

A **red dot** on the bell means there is an announcement you have not opened yet on this computer. It clears when you open the board.

### Shortcut Pill
![12](/fft_user_guide/images/12.png) ![13](/fft_user_guide/images/13.png) ![44](/fft_user_guide/images/44.png)

The pill at the top of the pane shows whether the style keys are active. It has three states:

| Pill | Meaning |
|---|---|
| Grey **Shortcut Off** | Keys are off. Click the pill to turn them on. |
| Green **Shortcut On** | Keys are on. Click a paragraph, press a key. |
| Amber **Shortcut Paused** | You are typing in the document. Keys are inactive there. Click back into the pane and the pill turns green again. |

- Only clicking the pill changes On / Off. Your choice is remembered on this computer.
- On a new computer the pill starts **Off**. Click it once.
- Every key also exists as a button, so the pill is never required.

### Settings
![14](/fft_user_guide/images/14.png)

Click the gear icon in the top-right corner of the pane.

- **About** — version number, **Sign out** and a **Documentation** button (opens the guide list on binltools.com: install guide, this user guide, FAQ).
- **Notifications** — how long messages stay on screen (seconds), and whether to show success / warning / error messages.
- **Shortcuts** — click a key box, press the new key, then **Save**. **Reset to Defaults** restores the original keys. See 2.1 for the default keys.

💡 Tips:
- Heading keys **1–6** are fixed and cannot be changed.

### Version and Build Stamp
The pane header shows `Ver. 2.2.0 · <build stamp>`. If the stamp is older than the latest announcement, Word is running a cached copy of the pane. See FAQ #1 (close Word, clear the add-in cache).

### Running Bar and Result Messages
Every button shows a blue **running…** bar while it works and ends with a result message — even when there was nothing to do (for example "no highlighted text found"). Green success messages disappear after 3 seconds. Detailed results (for example the list of words 2.5 changed) stay under the button.

![36](/fft_user_guide/images/36.png) ![49](/fft_user_guide/images/49.png)

### Three Tabs: Configure, Format, Finalize
![15](/fft_user_guide/images/15.png)

The tab bar at the bottom of the pane has three tabs, worked left to right. **Format** and **Finalize** unlock after Step 1 is submitted.

## Before You Start
- **Remove tracked changes and comments** from the document: File → Info → Check for Issues → Inspect Document → Remove All (Comments, Revisions and Versions).
- **Translated documents: translate first, format last.** Translation tools break Word fields. Run FFT on the final translated text.
- **Tag the document title as Heading 1 first** in a new document. The heading numbers hang off it.
- **Changing the region** of an already formatted document needs a fresh document, because styles already in the document win over newly loaded ones. **Changing the module** does not — just re-run Step 1.

## Step 1 Configure
Step 1 tells FFT what the document is. FFT then loads the styles, sets the page and margins, and writes the header and footer.

![16](/fft_user_guide/images/16.png)

### 1.0 Select Region
Choose where the document will be submitted: **US**, **EU** or **JP**. The region is remembered on this computer.

### 1.1 Choose Module Category
The list depends on the region:

| Region | Categories |
|---|---|
| US | General · Module 1 · Module 1 Regional · Module 2 · Module 3 |
| EU | General · EU Regional |
| JP | General · Module 1 · Module 2 |

"General" means non-CTD documents.

### 1.2 Choose Specific Module
Pick the exact document.

| Category | Documents |
|---|---|
| General (US / EU) | regular_1, regular_1.0, SOP |
| General (JP) | regular_1, regular_1.0 (A4, 25 mm, Japanese styles; no SOP) |
| US Module 1 | 1.6.1, 1.6.2, 1.9.4, 1.12.14, 1.20 |
| US Module 1 Regional | USPI |
| EU Regional | SmPC |
| US Module 2 | 2.2, 2.3.S, 2.3.P, 2.3.A, 2.4, 2.5, 2.6.1–2.6.7, 2.7.1–2.7.4 |
| US Module 3 | every 3.2.S.x, 3.2.P.x and 3.2.A.x document |
| JP Module 2 | 2.2, 2.3.S, 2.3.P, 2.3.A, 2.3.R, 2.4, 2.5, 2.6.1–2.6.7, 2.7.1–2.7.6 |
| JP Module 1 | 1.6 — translated foreign labels (see the JP chapter) |

- regular_1 numbers headings 1 / 1.1 / 1.1.1. regular_1.0 starts at 1.0.
- CTD modules number headings with the module prefix: 2.5 → 2.5.1 → 2.5.1.1.
- USPI and SmPC keep their fixed, typed section numbers (FDA PLR / EMA QRD). FFT never auto-numbers them.

### 1.3 Product and Company
Type the product name and the company name. They are printed in the page header and remembered on this computer. Company defaults to TopAlliance Biosciences. Leave Product empty and the header keeps its placeholder.

### Layout
Open the **Layout** section to control what Step 1 loads.

![17](/fft_user_guide/images/17.png)

#### Load Template
FFT styles (named "FFT …" in the Styles pane) only need to be loaded once per document.

- **ON** (first time): loads the FFT styles, header and footer for the selected module.
- **OFF** (reopening a formatted document): keeps what is there and lets you go straight to Step 2.

#### Load Header / Load Footer
![18](/fft_user_guide/images/18.png) ![19](/fft_user_guide/images/19.png)

- **ON**: FFT writes the predefined header / footer for the module.
- **OFF**: the document's existing header / footer is kept.

Turn these OFF when the document already has an approved header / footer.

#### Insert Section Skeleton (USPI and SmPC only)
![37](/fft_user_guide/images/37.png)

- **ON** for a new document: inserts the fixed section list (FDA PLR sections 1–17 / EMA QRD sections 1–10).
- **OFF** when reformatting an existing document.

#### Margins
Margins fill in automatically when you pick a module. You can still edit them before Submit.

| Document | Margins |
|---|---|
| US CTD modules | 1 / 0.67 / 1.1 / 0.9 in (top / bottom / left / right) |
| USPI | 1 in all round |
| SmPC | EMA QRD preset (0.79 in top/bottom, 0.98 in left/right) |
| JP | 25 mm all round, A4 page |
| JP 1.6 labels | Page, margins, header and footer stay as in the source |

### Submit
Click **Submit**. FFT loads the styles, sets the page and margins, and writes the header and footer. Track Changes is switched off while it loads (a template load is not review content).

💡 Tips:
- **Wrong module number?** Re-run Step 1 with the right module. Headings renumber in place.
- **Wrong region?** Start from a fresh document.
- **Wrong template loaded?** Delete the FFT styles (Styles → Manage Styles → Import/Export → select all FFT styles → Delete), then run Step 1 again.

## Step 2 Format
Step 2 formats the content. It opens after Step 1 is submitted.

Step 2 has five sections:
- 2.1 Styles — headings and paragraphs
- 2.2 Table Cells — tables, in one click or by rectangle
- 2.3 Normalize Symbols — full-width symbols → English (not for JP)
- 2.4 Clean Leftover CN Fonts — Chinese fonts on translated text (JP only)
- 2.5 Scientific Typography — italics, subscripts and (JP) house-style text fixes

**Work in this order: 2.1 → 2.2 → 2.3 / 2.4 → 2.5.** 2.5 restores an italic that covers a whole cell after 2.2, and Step 3 cross-references need 2.5's corrected text.

💡 Tips:
- A style key can be undone with Ctrl + Z.
- 2.3, 2.4 and 2.5 change the whole document. Their text edits land as **tracked changes** — review them in Track Changes rather than undoing.

### 2.1 Styles
![20](/fft_user_guide/images/20.png)

Each style has a button and a key (shown in grey on the button). Both do the same thing.

#### Available Styles

| Key | Style | Use for |
|---|---|---|
| X | Normal | Body text |
| C | Normal 2 | Body text, second form (JP: without first-line indent) |
| 1–5 | Heading 1–5 | Section headings, numbered automatically (2.5, 2.5.1, …). A number typed in the text that equals the automatic one is removed; a different one is highlighted cyan |
| 6 | NoNum Heading | Heading without a number; still appears in the TOC |
| T | Table Title | Table caption — inserts the prefix and auto-number |
| E | Table Note | Note line under a table |
| F | Figure Title | Figure caption — inserts the prefix and auto-number |
| B / H / U | Bullet / Sub Bullet / 3rd Bullet | Bullet lists (• → o → ▪) |
| V / G / Y | Numbering / Sub Numbering / 3rd Numbering | Numbered lists (US: 1. → a. → i. · JP: （1） → 1） → a)) |
| Shift + H | Highlight | Cyan highlight on the selected text — marks a spot to check in 3.1 |

The same key applies the correct regional style: **1** is FFT Heading 1 in a US document and FFT JP Heading 1 in a JP document.

Keys without a button:

| Key | Action |
|---|---|
| N | Navigate — select the paragraph under the cursor |
| → / ← | Next / previous paragraph |
| Backspace | Delete the selected paragraph |
| Shift + B / I / U | Bold / italic / underline the selected text |
| Shift + → / ← | Next / previous highlight (Step 3.1) |

All keys except 1–6 can be changed in Settings.

#### Method A — Keyboard (Recommended)
Format the whole document without touching the mouse.

Three principles:
- One key = one style.
- One paragraph at a time. Tables and figures are skipped.
- Keys work only while the pill is green **Shortcut On** and only on the selected paragraph.

![12](/fft_user_guide/images/12.png) ![13](/fft_user_guide/images/13.png)

#### 1. Place the cursor ####
Click inside the paragraph to format. Plain text only — not a table or figure.

![21](/fft_user_guide/images/21.png)

#### 2. Turn shortcuts on ####
Click the pill so it shows green **Shortcut On**. Click once inside the pane if the pill shows **Paused**.

![13](/fft_user_guide/images/13.png)

#### 3. Navigate to the paragraph ####
Press **N**. FFT selects the paragraph under the cursor.

![22](/fft_user_guide/images/22.png)

#### 4. Apply the style ####
Press the key of the style. The style is applied at once and the selection stays on the paragraph.

![23](/fft_user_guide/images/23.png)

#### 5. Move to the next paragraph ####
Press **→**. Repeat steps 4 and 5 to the end of the document.

#### Method B — Buttons
- Click inside a paragraph.
- Click the style button in the pane.

![24](/fft_user_guide/images/24.png)

#### Captions and Auto-Numbering
**T** and **F** insert the caption prefix and a live number in front of the title:

| Region / document | Table caption |
|---|---|
| US CTD module | Table 2.5-1 |
| USPI / SmPC | Table 1: |
| General / SOP | Table 1 |
| JP | 表 2.5-1. |

- Inserting a caption in the middle of the document renumbers the ones after it.
- Table captions go above the table, figure captions below the figure.
- Bold, indents, list numbers and font colour inherited from the surrounding text are removed when the key is pressed (a caption typed after a blue link no longer stays blue).
- **Typed prefix:** if the title already starts with its own label — "表 2.4-4：" or "Table 2.4-4:" — the typed prefix is removed for you, whatever number it carries (v2.1.12). The live number is the one FFT inserts; it moves when a caption is added before it. When the typed number was different, the toast says so ("Typed 図 2.6.2-1 removed — now numbered 2"): check the mentions of that figure in the text, 3.3 links them to the new number.
- JP: the caption paragraph is also freed of a Japanese font inherited from the heading above (表 in ＭＳ ゴシック), as long as Track Changes is off at that moment; with tracking on, 2.4 and the finishing script do it later.
- The lists of tables and figures (3.4) collect captions by their number field. A number typed by hand will not appear in the lists.

#### Style Not Applying?
- The paragraph still carries direct formatting from its source (common in translated text). Click in it and press **Ctrl + Space** in the document.
- If a style is missing from the document, FFT imports it when you press the key (brief "importing…" notice). No fresh document needed.
- Heading indents still wrong? See FAQ #2.

### 2.2 Table Cells
Tables are skipped by the paragraph walk. They are formatted here — a whole table in one click, or a block of cells by rectangle.

![45](/fft_user_guide/images/45.png)

**Table type** (above the font box, since v2.2.0) fills the boxes for you:
- **Standard table** — 10 pt; font and size stay editable.
- **略語一覧** (JP) — 10.5 pt, single spacing, 0 before / 0 after: the approved layout of the abbreviations table. The boxes are locked. Its alignment to the line grid cannot be set from the pane — the JP page-grid script does that (see the JP workflow).
- **Custom…** — font, size, **line spacing** (12 = single), **space before** and **after**. One table that needs 9 pt: pick Custom, type 9. An empty box leaves that setting to the FFT table style.

Set the **Font** (type "Ti" and pick Times New Roman from the list; the box previews the font) and **Size** (default 10 pt) next. The box sets the Latin font only; in JP documents Japanese text keeps ＭＳ 明朝 from the FFT JP table style.

#### Whole table (recommended)
1. Click inside any cell of the table.
2. Set **Header rows** (default 1; use 2 for a two-row header).
3. Click **Format whole table**.

What FFT does:
- Header rows → **Header** cells (bold, centred) and repeat on every page.
- Cells that hold only a value → **Numerals** (centred): "27.7", "1.75 ± 3.5", "23.7–42.43", "98%", "n=4", "NC".
- Cells the source already centres → **Numerals** (centred), so the layout follows the English.
- Everything else → **Text** (left): words, study numbers ("2352-13085"), values with words ("3 Male, 3 Female", "100 mg/kg, Q2W").
- Keeps what the source meant: indents that show hierarchy (Sex → Female / Male), typed leading spaces, bullets inside cells, and bold, italic, underline and colour — also when they cover the whole cell.
- Merged cells are handled from the table's own grid. Cells that Word refuses, nested tables and rows it cannot read are listed under the button, not guessed.
- Track Changes is switched on. Every cell's text is compared before and after; if anything differs the pane names the cell (2.2 never edits text — Ctrl + Z undoes a run).

The result list under the button shows the cells per type and the checks.

#### Manual: a block of cells
For tables the automatic roles do not fit (a label-and-value table, an odd layout):

![46](/fft_user_guide/images/46.png)

#### 1. Start of the block ####
Click in the top-left cell and click **Submit**.

![27](/fft_user_guide/images/27.png)

![28](/fft_user_guide/images/28.png)

#### 2. End of the block ####
Click in the bottom-right cell.

![29](/fft_user_guide/images/29.png)

#### 3. Apply ####
Click **Header**, **Text** or **Numerals**. The rectangle is taken in grid columns, so a merged cell inside it is formatted once. A Header block that starts at the first row also sets the repeat-on-every-page flag.

![31](/fft_user_guide/images/31.png)

![32](/fft_user_guide/images/32.png)

💡 Tips:
- Bold, italic, underline and colour are left as found in Text and Numerals cells (crucial values often are bold in the source); only Header forces bold. Use Word to clear formatting where you do not want it.
- JP: every cell is centred vertically; US / EU text cells sit at the top.
- Run 2.2 before 2.5.

#### 2.2.4 Table Style in Word
![33](/fft_user_guide/images/33.png)

- Click in the table → **Table Design** → choose a style. **Table Grid** is recommended for most tables.

#### 2.2.5 Table Layout in Word
- Alignment → Cell Margins: cell padding and spacing.

![34](/fft_user_guide/images/34.png)

### 2.3 Normalize Symbols
![38](/fft_user_guide/images/38.png)

One click converts full-width symbols in the whole document to English ones:
- （ ） → ( )
- ， → ,
- 。 → .
- ： → :

- Changes are tracked. Review them in Track Changes.
- Disabled for JP: Japanese full-width punctuation is correct and must stay.

### 2.4 Clean Leftover CN Fonts (JP only)
![39](/fft_user_guide/images/39.png)

Translated text often keeps Chinese fonts (DengXian, SimSun, YaHei, 游明朝…) from the source. One click removes them from body paragraphs and captions so the Japanese template fonts apply.

- Tables and figures are never touched.
- Changes are tracked. Paragraphs FFT cannot edit (inside tables, field results) are reported as skipped.
- Greyed out outside JP.

### 2.5 Scientific Typography
![40](/fft_user_guide/images/40.png)

One click fixes scientific typography in the whole document. **Run it after 2.2 and before 3.3.**

Every region:
- *in vivo*, *in vitro*, *ex vivo*, *in situ*, *in silico*, *de novo* → italic
- EC50, T1/2, Cmax, Vd, KD, AUC0-168h, MRT0-last, AUC0→∞ → subscripts
- Inside tables too
- A subscript the translator faked by shrinking the digits ("50" at 6.5 pt, not a real subscript) is first restored to the size of the text around it, so it no longer comes out tiny. Subscripts that already exist are left alone and counted separately ("Already sub/superscript, left alone").

JP adds the partner's house style:
- Citations → "Rosenberg et al. 2016, Topalian et al. 2012"
- Ranges with a unit in Japanese text → 1～75 mg/kg; every wave dash → ～
- Ranges in English text and table cells → en dash (68 pM – 6.8 µM); reference list keeps hyphens
- Missing space between number and unit → 0.68 nM (also in tables)
- Unit products → middle dot (μg·h/mL)
- ～ in the English reference list → hyphen; reference list blue; stray blue study numbers in tables back to black
- Legacy Symbol-font Greek letters (μ, α, γ, ∞) repaired

What you see afterwards:
- **Text edits are tracked changes.** Review them before accepting.
- **Italic, subscript and colour are not tracked** — Word does not record formatting made by an add-in. The list under the button names every word FFT italicised or subscripted.
- Anything FFT is unsure about is highlighted cyan for 3.1.

## Step 3 Finalize
Step 3 builds the document-wide structures. Work the four sections in order; the TOC is always last.

Before you start: Steps 1 and 2 are complete for the whole document.

### 3.1 Highlights
![35](/fft_user_guide/images/35.png)

Lists every highlighted spot in the document — FFT's cyan marks and any colour a reviewer added by hand.

- Click **Load Highlights**. The counter shows "n / N". The list appears in seconds, however long the document (v2.1.13: the document is read once, not paragraph by paragraph).
- Click an item to jump to it, or use **Previous / Next** (Shift + ← / Shift + → while shortcuts are on).
- Fix or clear each spot.

Run 3.1 **before 3.3** (clears the marks from 2.5) and **again after 3.3** (its unclear cases).

### 3.2 Bookmarks
![47](/fft_user_guide/images/47.png)

One click creates a bookmark for every entry in the reference list. Cross-references to literature use these bookmarks.

FFT:
- Finds the heading **References** / 参考文献 / 引用文献 / 文献 (any FFT Heading or NoNum Heading style). The list ends at the next heading.
- Bookmarks every entry as **FirstAuthor_Year** (for example Ying_2022). Duplicates get _2, _3.

#### Step by Step
1. Make sure the References heading uses an FFT Heading style.
2. Finalize → 3.2 → **Add Bookmarks**.
3. The pane reports how many bookmarks were created ("no reference list" if none was found).

💡 Tips:
- Clicking twice does not create duplicates. Ctrl + Z undoes it.
- The bookmarks appear in Word → Insert → Bookmark.
- Literature is linked by bookmarks; tables and figures are linked by their caption fields. 3.3 handles both.

### 3.3 Cross-references
![41](/fft_user_guide/images/41.png)

Three buttons.

#### Link Mentions
Turns typed mentions in the body text into live links:

| You wrote | Becomes |
|---|---|
| "Table 2.5-1", "表 2.4-1" | A reference to the caption. Renumbers if captions move. |
| "(Smith et al., 2020)", "（Bray et al., 2024）" | A link to the reference entry (run 3.2 first). Your text stays as typed. |

- One pair of parentheses can hold several citations; each is linked separately.
- Unclear cases (no matching caption, two entries for the same author and year) are highlighted cyan for 3.1.
- Reference entries never cited in the text are counted — a quick way to find a spelling mismatch.
- Body text only: never tables, captions, the TOC or the reference list. Changes are tracked.

Manual equivalents in Word: Insert → Cross-reference → Table / Figure → Only label and number; Insert → Link → Place in this document → the Author_Year bookmark.

#### Link Sections
Turns section mentions ("2.5.4", "5.3.5.3") into links, following the region's convention:

| Mention | US / EU | JP |
|---|---|---|
| A section of this document (e.g. 2.5.4 inside the 2.5) | Blue live link to the heading | Blue live link to the heading |
| A section of another document | Number turns blue; the link is made in docuBridge at publishing | M2 / M3: written [M2.7.4], brackets black, M + number blue. M4 / M5: stays black, written M4.2.1.1 |

- Needs a numeric module from Step 1. USPI, SmPC and SOP show a warning.
- Only mentions with three or more segments are linked ("2.5" alone could be a dose).
- Figure and table numbers are not section mentions: in "図 2.6.1-1" or "Table 2.6.1-3" the number belongs to Link Mentions, so it gets one link, to the caption (v2.1.11).

#### Update All Fields
Refreshes every field in one click: cross-references, caption numbers and the TOC.

💡 Tips:
- Numbered (Vancouver) reference lists: make the list a numbered list (**V**) and use Word → Insert → Cross-reference → Numbered item. Link Mentions is for author-date citations.
- If the TOC loses its FFT look after an update, delete the three lists and click **Add TOC** again (3.4).

### 3.4 Table of Contents
![42](/fft_user_guide/images/42.png)

One click builds the **Table of Contents**, **List of Tables** and **List of Figures** from the FFT headings and captions.

#### Step by Step
1. All headings and captions carry FFT styles.
2. Put the cursor where the lists should go.
3. Finalize → 3.4 → **Add TOC**.

- A list with nothing to list is skipped (the pane says so).
- JP: the 目次 / 表一覧 / 図一覧 titles appear in the 目次 at the same level as 略語一覧, and the 目次 runs margin to margin. On an older document, delete the 目次 and add it again to get this.
- JP: after Add TOC the pane reminds you of the finishing script (see the JP chapter).

💡 Tips:
- **Do not right-click → Update Field on the TOC.** Word rebuilds it in its default look. Delete the three lists and click **Add TOC** again instead.

Once Step 3 is complete the document is ready for final QC and submission export (eCTD / PDF).

## Region Quick Reference

| | US | EU | JP |
|---|---|---|---|
| Documents | CTD Modules 1–3, USPI, General / SOP | SmPC, General / SOP | CTD Module 2, Module 1 → 1.6 labels, General |
| Page | Letter | A4, EMA QRD margins | A4, 25 mm margins, 38 lines per page |
| Fonts | Times New Roman 12 pt | Times New Roman 11 pt (SmPC) | Times New Roman + ＭＳ 明朝 / ゴシック 10.5 pt |
| Captions | Table 2.5-1 (bold) | Table 1: | 表 2.5-1. (not bold) |
| Heading numbers | Auto; USPI typed (FDA PLR) | Typed (EMA QRD) | Auto; 1.6 labels typed |
| 2.3 Normalize Symbols | Yes | Yes | Disabled |
| 2.4 Clean CN Fonts | — | — | Yes |
| 2.5 Typography | Italics + subscripts | Italics + subscripts | + citations, ranges, units |
| After the pane | — | — | jp_finalize.py |

## JP Workflow End to End
![43](/fft_user_guide/images/43.png)

1. **Translate first, format last.** Run FFT on the final Japanese text.
2. **Step 1**: Region JP → Module 2 → the module (or General → regular_1). Type the product name. Submit. FFT sets A4, 25 mm margins and the "Version: Date:" header.
3. **Step 2**: 2.1 walk the paragraphs → 2.2 tables (run it or skip it — see **Tables: two ways** below) → 2.4 Clean CN Fonts → 2.5 Typography.
4. **Step 3**: 3.1 Highlights → 3.2 Bookmarks → 3.3 Link Mentions and Link Sections → 3.1 again → 3.4 目次.
5. **Page grid & tables script**: open **binltools.com/fft/finalize.html**, sign in with your FFT account, choose **JP page grid & tables**, drop the file — or all the modules of a round at once, the options apply to every file — and click **Run**. Each result downloads by itself (the browser asks once to allow several downloads) and the page scrolls to the log, one block per file. (Locally with Python: `python jp_finalize.py FILE.docx`.)

**Before or after the pane?** Both work. Since the October 2026 training the partner runs it **first**, on the translated file: tick *略語一覧 table* and *All other tables to 10 pt*, run, then open the result in Word and start at Step 1 — Step 1 prints "Line grid: 38 lines kept in all N section(s)" so you know the grid survived. Run the script **once more after the 目次** only if the caption fonts need the fix (row 2 below), with both table boxes **unticked**, so the 9 / 8 pt exceptions you made by hand stay.

It does what a Word add-in cannot. It writes `FILE_final.docx` next to the input (`--in-place` overwrites and keeps a .bak).

#### What the finishing script changes

| # | Change | Where | Default |
|---|---|---|---|
| 1 | Page grid: 38 lines per page (行数だけを指定する), written into every section. The source's own grid settings on mid-document section breaks are replaced. | Whole document | Always |
| 2 | Caption prefixes (表 2.4-1, 図 2.4-1): the 表 / 図 character loses a directly applied Japanese font so the caption style's ＭＳ 明朝 applies. | Captions only | Always (untick *Page grid only* to skip) |
| 3 | Greek letters and symbols (μ, α, ≥, →, ∞ …) are shown in Times New Roman instead of the Japanese font. | Body text only | Always (tick *Keep East Asian hints* to skip) |
| 4 | The same Greek/symbol font change inside tables. | Tables | **Only with *Also change tables*** |
| 5 | 略語一覧 table set to 10.5 pt, single spacing, rows aligned to the line grid; table text in the whole document is allowed to align to the grid. Found by its FFT heading, or — on a file that has not been through the pane — by the 略語…一覧 heading directly above it. | 略語一覧 table (grid setting: all tables) | **Only with *略語一覧 table*** |
| 6 | Every other table's text set to one size (default 10 pt): every cell, header rows and notes inside the table included. Only the size changes; fonts, spacing, alignment and row heights stay. Make the 9 / 8 pt exceptions afterwards, by hand or with 2.2 Custom. | All tables except 略語一覧 | **Only with *All other tables to N pt*** |
| 7 | Any other change to tables — font, spacing, alignment, row height. | — | **Never** |
| 8 | Built-in Normal style set to 10.5 pt. | Styles | Only with the 44 chars × 38 lines grid |

The script never adds, deletes or reorders text.

#### Tables: two ways

Decide once per document, before Step 2, and keep to the same column through the finishing script.

| | A · Tables stay as they are | B · Tables get the FFT format |
|---|---|---|
| **When** | The tables are already laid out the way the receiver wants — typically the same layout as the English source | The tables need formatting: newly translated, pasted from another source, or inconsistent with each other |
| **2.2 Table Cells** | Skip | **Format whole table** on every table with Table type **Standard**, the **略語一覧** preset on that one (manual mode for unusual ones) |
| **2.5 Typography** | Run. It still corrects text inside tables: subscripts, unit spaces, ranges, Greek letters | Run, after 2.2 |
| **Page grid & tables script** | both table boxes **unticked** — every table stays byte-for-byte as in the input | *略語一覧 table* **ticked**, and *All other tables to 10 pt* when the script runs before the pane (rows 4–6 above) |

- The two steps in a column belong together: the approved look for B needs both 2.2 and the ticks.
- Script first, then 2.2 on the 略語一覧 table? That first 2.2 pass resets the table's grid alignment — run the script once more at the end with only *略語一覧 table* ticked.
- Not sure which one applies? Ask whoever receives the file before you start.
- Applied a Word table style afterwards (Table Design)? It resets the cell alignment and the repeating header row — run 2.2 on that table again.

**Tell the partner what was done:** the log ends with a numbered **What changed** list for that file (counts included). Paste it into the delivery e-mail.

Optional, for EndNote reference lists:

```
python reformat_references.py FILE.docx --apply
```

Rewrites entries into "Authors. Title. Journal, Year, Vol(Issue): pages." and lists the hand-typed entries it left alone. Works before or after FFT.

### JP 1.6 — Translated Foreign Labels
Japanese translations of the EU SmPC, US PI or NMPA label are filed under JP Module 1 → 1.6. Their section numbers are FDA's / EMA's / NMPA's own, with gaps, so they stay typed:
- Keys 1–3 restyle the heading without numbering it; T / F apply the caption style only.
- Step 1 leaves page size, margins, header and footer as in the source.
- The bulk formatting of a whole label is done before the pane pass: **binltools.com/fft/finalize.html** → **Label format (M1.6)** (profile smpc / uspi / nmpa), then Step 1 → 2.5 → 3.1 in the pane, then **Label page → M2.4** on the same page. Contact RA before formatting a label.

## Good to Know
- **Sign-in**: once per computer. The pane shows your e-mail at the top; Settings → Sign out on a shared PC.
- **Track Changes**: Step 1 switches it off. 2.3, 2.4, 2.5 and 3.3 switch it on and leave it on.
- **Formatting is not tracked**: italics, subscripts and colours made by 2.5 do not show as revisions. Read the list under the button.
- **Text boxes** are not reached by 2.5. Fix them by hand.
- **Pane looks old after an update**: check the build stamp, then FAQ #1.
- **Style will not apply**: Ctrl + Space in the document, then the key again.
- **Cyan after a heading key**: the number you typed differs from the automatic one. Check the level, then clear the highlight. A caption key never leaves cyan: the typed label is removed and the toast reports a changed number.

## Version History
- **v2.2.0** (Oct 7, 2026) — JP page-grid script: new *All other tables to N pt* option and the 略語一覧 table is found on a file that has not been through the pane, so the script can run first; Step 1 reports the line grid. 2.2: **Table type** presets (Standard, 略語一覧, Custom with line spacing and space before/after). 2.5: a subscript faked by shrinking the digits is restored to the surrounding size first; KD added; existing subscripts no longer re-counted.
- **v2.1.17** (Oct 6, 2026) — Finalize page: drop several files in one run, one download each; a file that fails is listed and the others still finish.
- **v2.1.16** (Oct 6, 2026) — Header and footer right-hand text sits on the right margin of every section, landscape pages included (the JP header used to keep the portrait position on a landscape 2.6.3 page; 2.1.15 shipped the template change, 2.1.16 the pane change that let it take effect). Re-run Step 1 on an existing document to pick it up.
- **v2.1.14** (Oct 6, 2026) — Link Mentions reads the whole caption number: "図 2.6.2-13" used to be linked to figure 1 with a stray "3" left behind. Link Sections no longer links "2.5.4" inside "2.5.41".
- **v2.1.13** (Oct 6, 2026) — 3.1 Load Highlights reads the document once instead of asking Word for every paragraph; a long document with one highlight used to take a minute, now seconds.
- **v2.1.12** (Oct 6, 2026) — Caption keys remove a typed label whatever its number (a typed 図 2.6.2-1 under a live 2 used to stay cyan and could not be cleaned by a second key press); the toast reports a changed number.
- **v2.1.11** (Oct 6, 2026) — Link Sections no longer links the module number inside a figure or table mention ("図 2.6.1-1" linked to section 2.6.1 as well as the figure).
- **v2.1.10** (Oct 5, 2026) — Announcements: a red dot on the bell marks an unread announcement; the board re-reads every time it is opened.
- **v2.1.9** (Oct 5, 2026) — Settings: the Documentation button opens the guide list on binltools.com; Sign out and Documentation sit side by side.
- **v2.1.8** (Oct 5, 2026) — finalize page: the result downloads by itself and the page scrolls to the log; the hint under Format whole table matches the 2.1.7 rule; How It Works lists the install, finalize and admin links; the install guide has a screenshot for every step.
- **v2.1.7** (Sep 30, 2026) — Format whole table: alignment follows the source — only pure values and cells the source centres are centred; study numbers and short text stay left (the Sep 25 rule centred them).
- **v2.1.6** (Sep 30, 2026) — finishing script leaves tables untouched unless *Also change tables* is ticked, and ends with a "What changed" summary; the install guide's manifest download link works again.
- **v2.1.5** (Sep 29, 2026) — JP: the heading keys attach their list again (the 2.1.4 font clean-up now runs on caption keys only).
- **v2.1.4** (Sep 28, 2026) — JP: heading and caption keys free the paragraph of a Japanese font inherited from the paragraph above (Track Changes off); finishing script recognises 略語及び略号一覧.
- **v2.1.3** (Sep 28, 2026) — 2.2 keeps bold, italic, underline and colour that cover a whole cell; heading and caption keys remove a typed number that equals the automatic one, cyan when it differs.
- **v2.1.2** (Sep 28, 2026) — heading and caption keys reset the font colour to the style (no more blue captions after a link).
- **v2.1.1** (Sep 25, 2026) — 2.2 font box: type-ahead list with preview; JP hint (Latin font only).
- **v2.1.0** (Sep 25, 2026) — 2.2 rebuilt: **Format whole table** (header rows, first column and long text left, numbers centred, header repeat), merged cells read from the table grid, indents / bullets / bold kept, three-button manual mode, Track Changes on, text guard.
- **v2.0.1** (Sep 23, 2026) — sign-in code screen no longer resets while you read the e-mail.
- **v2.0.0** (Sep 23, 2026) — accounts and licences: sign in once per computer, organizations with seats and expiry, templates served to licensed accounts; finalize page (JP finishing script and label scripts without Python); administrator page.
- **v1.9.4** (Sep 18, 2026) — 2.5 counts only exact-text hits; a converted ～ no longer hides a remaining ~.
- **v1.9.3** (Sep 17, 2026) — 2.5 result under the button is a bulleted list, one line per change type.
- **v1.9.2** (Sep 17, 2026) — 2.5 lists every word it italicised or subscripted (formatting is not tracked).
- **v1.9.1** (Sep 17, 2026) — every button shows the running bar and ends with a result message, including "nothing to do".
- **v1.9.0** (Sep 17, 2026) — JP 1.6 translated labels (typed-number heading styles); 2.5 converts a repeated wave dash once.
- **v1.8.6** (Sep 16, 2026) — General is region-aware: JP regular_1 / regular_1.0 load the JP base; no SOP under JP.
- **v1.8.5** (Sep 11, 2026) — Link Sections for US and EU: same-document sections link, other documents turn blue for docuBridge.
- **v1.8.4** (Sep 11, 2026) — JP 目次 margin to margin; 略語一覧 table 10.5 pt in the finishing script; [M2.6.3.1] black brackets, blue number.
- **v1.8.3** (Sep 10, 2026) — running bar, self-clearing success messages, 3.1 lists every highlight colour; JP 目次 titles at the 略語一覧 level; English ranges → en dash, unit products → middle dot, µ fix.
- **v1.8.2** (Sep 9, 2026) — 2.5 runs in every region (italics + subscripts); JP number–unit spaces; 2.2 keeps italics; Greek letters in TNR via the finishing script.
- **v1.8.1** (Sep 8, 2026) — JP wave dashes unified; reformat_references.py.
- **v1.8.0** (Sep 3, 2026) — 2.5 ranges, Symbol-font repair, reference list blue; [M…] section references; jp_finalize.py; JP header 10 pt, cells centred.
- **v1.7.0 – v1.7.8** (Sep 1, 2026) — 2.5 Scientific Typography (JP); Link Sections (JP); Finalize reordered to 3.1 Highlights / 3.2 Bookmarks / 3.3 Cross-references / 3.4 TOC; fast Highlights; Previous / Next buttons in 3.1; JP finishing reminder; shorter pane texts.
- **v1.6.3 – v1.6.4** (Aug 27, 2026) — pill starts Off on a new computer and pauses while you type in the document; empty figure list skipped.
- **v1.6.2** (Aug 20, 2026) — pill is a button; Track Changes off during Step 1; several citations per parenthesis.
- **v1.6.0 – v1.6.1** (Aug 19, 2026) — Step 1.3 Product and Company; one base template per document family, module number written at Step 1; 2.4 never touches tables; build stamp in the header; Link Mentions and Update All Fields.
- **v1.5.1 – v1.5.3** (Aug 13–18, 2026) — 2.4 Clean Leftover CN Fonts moved from Finalize to Format; JP template fixes.
- **v1.5.0** (Aug 7, 2026) — Step 1.0 Region selector (US / EU / JP); keys U and Y; re-run Step 1 to renumber; self-healing styles; USPI / SmPC skeleton toggle.

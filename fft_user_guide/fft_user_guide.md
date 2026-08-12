> **📌 Two guides, two jobs.** For what FFT does today and the rules behind it, see [FFT — How It Works](https://claude.ai/code/artifact/ebcf931d-0423-48f6-aeb5-3bbee7563f82) — always current, updated with every release. This page is the **step-by-step tutorial with screenshots**; check "What's new" below for features added after the screenshots were taken.

## What's new since January 2026

- **Step 1.0 Region selector** — choose **US / EU / JP** before picking a document. The category and module lists, page size, fonts, and caption language all follow the region. JP covers the NDA Module 2 set (表/図 captions, 目次, A4).
- **New style keys** — <kbd>u</kbd> (3rd-level bullet ▪) and <kbd>y</kbd> (3rd-level numbering i.), with matching buttons in Step 2.
- **Re-run Step 1 to renumber** — loading a different module number on an already-formatted document now updates the heading numbers in place.
- **Self-healing styles** — if a style is missing from your document, FFT imports it automatically when you press the key (brief "importing…" notice). No fresh document needed.
- **Step 3.4 Clean Leftover CN Fonts** — strips Chinese fonts (DengXian, SimSun…) stuck on translated text so the template's fonts apply.
- **USPI/SmPC** — Step 1 offers a section-skeleton toggle: ON for a new document, OFF when reformatting an existing one.

## Introduction
File Formatting Tool (FFT) is a Microsoft Word add-in designed to improve efficiency, consistency, and accuracy when formatting documents for health authority submissions. 

FFT is helpful for regulatory documents that:
- Under eCTD structure
- Contain extensive tables, figures, and cross-references
- Frequent revisions and re-formatting

Three Advantages:
- Works directly inside Word
-  Applies to any Word documents
-  Keyboard-driven shortcuts

Once FFT is opened in Word, it appears as a task pane on the right side of the document.

## Icons
Before diving into the FFT, let's take a step into the pane discovery.

### Announcements 
![11](/fft_user_guide/images/11.png)
Updated information will be posted in the announcements section, including version updates, issues detected/solved, new feature releases, etc.

### Shortcuts Status 
![12](/fft_user_guide/images/12.png)
Visual check for the Shortcuts function. The details of the Shortcuts function will be covered in section 2.1.

### Settings 
![14](/fft_user_guide/images/14.png)
Designed to help users get oriented, access documentation, and get in touch with the shortcut function before applying any formatting. 
Click the Settings icon in the top-right corner of the FFT pane. 

From here, users can manage:
- About - check documentation (User Guide), version and contact developer
- Notification - enable or disable the notification for each action
- Shortcuts - customize shortcut keys based on preference or reset to default. See 2.1 for more details

💡 Tips: 
- Clear all the track changes and comments before formatting: File → Info → Check for Issues → Inspect Document → Remove All (Comments, Revisions, and Versions)

### Configure, Format and Generate
Configure, Format, and Generate are the three fundamental processes of FFT, which help a document go from disorderly to ready for submission.

![15](/fft_user_guide/images/15.png)


## Step 1 Configure
Step 1 Configure defines how FFT will prepare the document before formatting begins. 
This step allows users to load a predefined FFT template, control layout elements, and set page margins.

![16](/fft_user_guide/images/16.png)

### 1.1 Select Module
At the top of Step 1, select the appropriate module or category from the dropdown list (for example, Module 2.5 – Clinical Overview).
The predefined styles are automatically added to the document and are ready for use in the following steps.

### 1.2 Set Up Layout
Provided flexibility in the template load and document layout setup.

#### 1.2.1 Load Templates
The principle of FFT template configuration is to deploy the preset styles (named "FFT XXX" in the Styles) and field into a Word document, and it only needs to be deployed once. 
The document needs to be opened several times for editing, but users do not have to deploy the FFT styles each time. 
Therefore, the design of the Load Templates helps the users decide whether to reload the template.

![17](/fft_user_guide/images/17.png)

- ON (recommended for first use):
   Loads FFT-defined header, footer, and styles
   Initializes the document with the selected module template

- OFF (recommended when reopening documents):
   Keeps the existing FFT configuration
   Allows users to proceed directly to Step 2

💡 Tips: 
- If the document already contains an FFT template, turning this option OFF prevents unnecessary reloading
- If a wrong FFT template been deployed in the document, users need to delete all the FFT Styles manually in Styles. (Styles - Manage Styles - Import/Export - Select all the FFT Styles - Delete)

#### 1.2.2 Load Header / Load Footer
These options allow fine control over whether FFT should overwrite existing headers and footers.

![18](/fft_user_guide/images/18.png) ![19](/fft_user_guide/images/19.png)

- Load Header
   ON: FFT applies the predefined header from the selected module
   OFF: Keeps the document’s existing header unchanged

- Load Footer
   ON: FFT applies the predefined footer from the selected module
   OFF: Keeps the document’s existing footer unchanged

💡 Tips: 

Turn OFF these options if
- The document already contains an approved or validated header/footer
- Users are working on a late-stage document and want to preserve layout

#### 1.2.3 Page Margin Settings
Users can manually define page margins (in inches):
- Top Margin (default: 1)
- Bottom Margin (default: 0.67)
- Left Margin (default: 1.1)
- Right Margin (default: 0.9)

The specified margins will be applied when formatting begins.



## Step 2 Format
Step 2 Format is where FFT applies formatting to document content.
This step becomes available only after Step 1 Configure has been completed and submitted.

Step 2 contains three independent formatting sections:
- 2.1 Styles – headings and paragraphs
- 2.2 Fix Tables – table cell formatting
- 2.3 Fix Symbols – symbol normalization

All actions in Step 2 can be undone using Ctrl + Z.

### 2.1 Styles
Section 2.1 is used to apply predefined FFT styles to headings and paragraphs.

![20](/fft_user_guide/images/20.png)

Each style is mapped to:
- A button in the FFT pane
- A keyboard shortcut, shown in grey parentheses on the button

Both methods apply identical formatting.

#### 2.1.1 Available Styles
- Normal [X] – Body text, justified
- Normal 2 [C] – Body text, left-aligned
- Heading 1–5 [1–5] – Section headings
- NoNum Heading [6] – Heading that does not contain numbers at the front
- Table Title [T] – Table captions
- Table Note [E] – Table Note
- Figure Title [F] – Figure captions
- Bullet Point [B] – Bulleted lists
- Sub Bullet Point [H] – Sub Bulleted lists
- Numbering [V] – Numbered lists
- Sub Numbering [G] – Sub Numbered lists
- Checkpoint [ctrl + CC] – Blue Highlight for the selected text, refer to 3.2 Checks for review instruction

Shortcuts that do not appear as buttons:

- Bold [Shift + B] – Bold the selected text
- Italic [Shift + I] – Italic the selected text
- Underline [Shift + U] – underline the selected text

#### 2.1.2 Method A – Formatting Using Keyboard Shortcuts (Recommended)
The Keyboard-driven shortcuts is one of the most powerful features of FFT. 
It allows users to format paragraph entirely by keyboard, significantly improve efficiency and eliminate repetitive formatting tasks.

Three Key principles:
- Pre-defined Word Styles

   all shortcuts applied one key to one style, and aligned with health authority eCTD expectations

- Sequential Traversal

   one paragraph at a time, auto-skip all the table and figure, focus on plain-text only

- Shortcut Mode 
   active only when the floating window is in green with "Shortcut On" and apply only to the navigated paragraph

   ![12](/fft_user_guide/images/12.png) ![13](/fft_user_guide/images/13.png)

#### Step-by-Step Paragraph Formatting
#### 1. Place the Cursor ####
   Click anywhere inside the paragraph that needs to be formatted. The cursor must be inside plain text, not inside a table or figure.

   ![21](/fft_user_guide/images/21.png)

#### 2. Activate Shortcut Mode ####
   Move the cursor to the FFT pane and click once. The floating indicator will change from: “Shortcut Off” (grey) → “Shortcut On” (green) 
   This confirms that shortcuts are active.

   ![13](/fft_user_guide/images/13.png)

#### 3. Navigate to the current paragraph ####
   Press [N] on the keyboard. (Think about "Navigate"). FFT will select the paragraph containing the cursor and highlight it in grey.

   ![22](/fft_user_guide/images/22.png)

#### 4. Apply the Desired Style ####
   Press the shortcut key corresponding to the target style (as shown in grey parentheses on the style buttons). 
   The style is applied immediately, and the selection remains on the same paragraph.

   ![23](/fft_user_guide/images/23.png)

#### 5. Move to the Next paragraph ####

   Press [→] / customized key to move to the next paragraph. Repeat step 4 and 5 until the entire document is formatted.

#### Default Shortcuts Not Included in the Styles Pane
- [N] – Navigate to the currently selected location
- [←] - Move to the previous paragraph
- [→] - Move to the next paragraph

💡 Tips:
- FFT automatically skips tables and figures during shortcut navigation
- Shortcuts Keys can be customized in the Settings

#### 2.1.2 Method B – Formatting Using Style Buttons
Users may also format content using the mouse:
- Select a paragraph using the cursor
- Click the desired style button in the FFT pane

Example: Select a table caption and click Table Title to apply the predefined table title style.

   ![24](/fft_user_guide/images/24.png)

### 2.2 Fix Tables
Section 2.2 is designed specifically for table formatting. Tables are excluded from shortcut traversal because:
- Table structures vary significantly between documents
- Accurate formatting requires explicit user-defined scope

#### 2.2.1 Table Formatting Principle
Users define an area within a table by selecting:
- The top-left cell (area "a")
- The bottom-right cell (area "b")

Header in purple, Data in orange

   ![25](/fft_user_guide/images/25.png)

FFT will format the cells in the defined area by choosing the category of the cells (header, text or numeral).

   ![26](/fft_user_guide/images/26.png)

#### 2.2.2 Cell Style Options
Cells without indentation (more reliable)
- Header Cells
- Data Cells (Text)
- Data Cells (Numerals)

This option fully normalizes formatting, including indentation.

Cells with indentation
- Header Cells
- Data Cells (Text)
- Data Cells (Numerals)

This option preserves existing indentation and is recommended for hierarchical tables.

Example:

Use “Cells with indentation” when indentation conveys hierarchy or categorization.

#### 2.2.3 Step-by-Step Table Formatting
#### 1. Define the Start of the Area ####
Select the top-left cell of the target area.
   
![27](/fft_user_guide/images/27.png) 

Click Submit.

![28](/fft_user_guide/images/28.png)

#### 2. Define the End of the Area ####
Click inside the bottom-right cell of the target area.

![29](/fft_user_guide/images/29.png)
   
Set the desired Text Size (default: 10Pt).

![30](/fft_user_guide/images/30.png)

#### 3. Apply Style ####
Choose the appropriate cell type, then the defined area will auto-format.

![31](/fft_user_guide/images/31.png)

![32](/fft_user_guide/images/32.png)

#### 2.2.4 Pre-set Table Style in Microsoft Word

![33](/fft_user_guide/images/33.png)

Microsoft Word has several preset table styles to choose from. The following are the reference steps:

- Create or select a table in the document
- Select Table Design on the top right of the document
- Under Table Style, choose the appropriate format that meets the table design

Table Grid is recommended for common table.

#### 2.2.5 Set up Table Layout in Microsoft Word
In addition to table design, the layout can also be edited directly in Table Layout. The following are the common use options:

- Alignment → Cell Margins: customized cell margins and spacing between cells

![34](/fft_user_guide/images/34.png)

- Data → Repeat Header Rows

### 2.3 Normalize Symbols
Section 2.3 normalizes full-width (Chinese) symbols into ASCII (half-width) symbols. One click for the entire document.

Examples:
- （ ） → ( )
- ， → ,
- 。 → .
- ： → :

This action applies to the entire document and cannot be limited to a selected range.


## Step 3 Finalize
Step 3 Finalize is the final stage of FFT.
It is used to generate document-wide reference structures, including bookmarks and tables of contents.

Step 3 contains two sections:
- 3.1 Bookmarks – Auto-add bookmarks based on the references
- 3.2 Checks - jump menu for reviewing all the checks that are highlighted in blue
- 3.3 TOC – Auto-generate the Table of Contents, Table of Figures, and Table of Tables

### 3.1 Bookmarks
Section 3.1 automatically creates bookmarks for references with a single click.
This function is designed to simplify cross-referencing literature references within the document.

FFT will:
- Identify the “References” section (typically the last section of the document)
- Automatically add bookmarks for each reference entry
- Assign each bookmark a standardized name (first-author-name_year, such as Ying_2022)

These bookmarks will appear in Word → Insert → Bookmark, allowing users to easily locate and reference them.

#### Step-by-Step Adding Bookmarks
1. Confirm the Reference section is formatted with FFT Heading in Styles
2. Navigate to Step 3 Finalize
3. Under 3.1 Bookmarks, click Add Bookmarks
4. FFT scans the References section and creates bookmarks automatically.

#### Create a Cross-Reference in the Paragraph
1. Select the text in the paragraph where the reference should appear
2. Go to Word → Insert → Cross-reference
3. Choose Bookmark as the reference type
4. Select the appropriate name_year bookmark

#### Important Distinction: Bookmarks vs Figures/Tables
Bookmarks are used for literature references only.
Figures and tables are cross-referenced using Word fields, not bookmarks.

Figure and table fields are automatically created when users apply Figure Title and Table Title in Step 2 Format → 2.1 Styles.

To cross-reference figures or tables:
- Use Word → Insert → Cross-reference
- Select Figure or Table as the reference type

💡 Tips:
- FFT prevents duplicate creation if “Add Bookmarks” is clicked multiple times
- Adding Bookmarks can be undone with a single Ctrl + Z

### 3.2 Checks
Section 3.2 Checks provides a visual pane for the user to review all the text highlighted in blue in section 2.1. 

#### Step-by-Step reviewing the text with a blue highlight
- Click "Load Checks" for FFT to detect all the highlighted checks in the document
- Select the checks by clicking in the visual pane
- Move between different checks by using Shift + → or Shift + ←

![35](/fft_user_guide/images/35.png)

### 3.3 TOC (Table of Contents)
Section 3.3 generates the structural tables required for health authority submissions.
With a single click, FFT will generate Table of Contents, Table of Figures and Table of Tables based on the FFT headings.

#### Step-by-Step Adding TOC
- Confirm all the headings equipped with FFT Heading in Styles
- Navigate to Step 3 Finalize
- Under 3.3 TOC, click Add TOC
- FFT inserts all three tables at the cursor position.

💡 Tips:
- Do Not Update FFT TOC Manually

   If users edit headings after generating the TOC - right-click the TOC and choose Update Field, Word will apply its default TOC format and overwrite the FFT-defined structure.
   
   If This Happens, delete the entire TOC, Table of Figures, and Table of Tables, and click Add TOC again.

Once Step 3 is complete, the document is ready for cross-reference verification, final QC review and submission export (eCTD / PDF).
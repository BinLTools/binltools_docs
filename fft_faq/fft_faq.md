## FAQ


#### 1. No response / refresh / update of FFT. ####
   - Check the build stamp next to the version in the pane header. If it is older than the latest announcement, Word is running a cached copy of the pane.
   - Try to clean cache then restart the FFT.
     1. Win + R -> `%LOCALAPPDATA%`
     2. Direct to `\Microsoft\Office\16.0\Wef`
     3. Delete all the cache files in this folder.

     ![5](/fft_faq/images/5.png)
     
     4. Close all the Word documents (close Word completely) and restart.

#### 2. Heading indentation is still incorrect after selecting FFT Heading. ####
   ![8](/fft_faq/images/8.png)
   
   - Define the multi-level list as FFT Heading.
   1. Select the heading and number
   2. Multi-level list 
   
   ![10](/fft_faq/images/10.png)

   3. Confirm the list you selected starts with "FFT Heading"
   4. Define New Multilevel List -> Ok
   
   ![9](/fft_faq/images/9.png)

#### 3. Shortcut function is not working at all. ####
   - First check the pill at the top of the pane. Grey "Shortcut Off": click it once (it starts Off on a new computer). Amber "Shortcut Paused": click inside the pane. Keys work only while the pill is green.
   - If the pill is green and the keys still do nothing, go to Settings and redefine the shortcut key.
   1. Top right corner for the Settings icon
   2. Scroll down to Shortcuts
   3. Click "Reset to Defaults" / customize the keys
   4. Save and follow the 2.1 for the shortcut procedure

#### 4. A style key does nothing, or the paragraph keeps its old look. ####
   - The paragraph still carries direct formatting from its source (common in translated text). Click inside it and press `Ctrl + Space` in the document, then press the style key again.
   - If the pane shows a short "importing…" notice, the style was missing from the document and FFT loaded it. Press the key again if needed.

#### 5. 2.5 Scientific Typography changed formatting but Track Changes shows nothing. ####
   - Only text edits are tracked. Italics, subscripts and colours made by an add-in are not recorded as revisions by Word.
   - The list under the Scan and Fix button names every word FFT italicised or subscripted. Use it to review.

#### 6. The Table of Contents lost its FFT look. ####
   - Do not right-click → Update Field on the TOC; Word rebuilds it in its default look.
   - Delete the Table of Contents, List of Tables and List of Figures, then click Add TOC (3.4) again.

#### 7. The sign-in code did not arrive. ####
   - It comes from fft@binltools.com within a minute. Check the junk folder the first time; some company mail filters hold the first message from a new sender.
   - Click **Send again** on the code screen. If nothing arrives after a second try, tell TopAlliance RA the e-mail address you used.

#### 8. The pane says "Awaiting approval". ####
   - Your account exists but is not yet assigned to a licensed organization. TopAlliance activates it; the pane updates by itself (or click **Check again**).
   - TopAlliance colleagues are activated automatically — if you see this, you may have typed a different e-mail than your work address.

#### 9. Someone else's e-mail shows at the top of the pane. ####
   - The previous user stayed signed in on this computer. Settings → **Sign out**, then sign in with your own account.

#### 10. 2.2 reports "rows skipped" or a text-guard message. ####
   - "Rows skipped (shape could not be read)": the table has a merge Word and FFT cannot reconcile in that row. Format that block with the manual steps (a / b) instead.
   - "TEXT GUARD: content changed at row r, cell c": FFT found a difference between the cell text before and after. Press Ctrl + Z to undo the run and send the table to TopAlliance RA — 2.2 never edits text, so this should not happen.

#### 11. The pane says "Heading number typed as … but numbered …" after I pressed the key. ####
   - The number typed in the text differs from the number FFT assigned (for example you typed 2.4.3.2.3 but the heading level makes it 2.4.3.2.2). The text is left as it is — since v2.8.6 FFT never highlights anything; before that the paragraph turned cyan.
   - Fix the level (press the right heading key) and delete the typed number. When the typed number matches, FFT removes it by itself. A caption key always removes the typed label and reports a changed number in the toast.
   - Cyan left in a file by an older FFT: Home → Text Highlight Color → No Color.

#### 12. I pressed a heading key, the style applied, but there is no number. ####
   - The document's FFT heading styles have lost their list. It happened on a file open straight from the SharePoint site: co-authoring merged the document while FFT worked and dropped the list.
   - Since v2.6.8 the heading key rebuilds the list by itself and shows "Heading list rebuilt — N heading(s) numbered". Press the key again on any heading that still has no number.
   - Since v2.8.5 the key builds the list with Word's own list commands, so it also works on the files that used to fail (translated 2.7.x files from the GlobalSubmit template, which bring their own heading ladder — Step 1 keeps the template's bullet and number lists on them but drops the heading list, AutoSave or not). Step 1 tells you: "Heading list: linked" or "Heading list: Word dropped it during the import — the first heading key rebuilds it".
   - If the pane still says **"Heading list could not be rebuilt (…)"**: close the file in Word, open **binltools.com/fft/script.html → Heading list repair** (JP or US), type the module number you chose in Step 1 (e.g. `2.7.2`), drop the file and run. The result `…_numfix.docx` has the FFT Heading styles linked to the number ladder again; every heading already tagged shows its number, and the keys number from then on. Nothing else in the file changes.
   - By hand in Word, if you prefer: Home → Multilevel List → *Define New Multilevel List* → *More*, then for each level link it to the style (Level 1 → FFT JP Heading 1, number style "(none)", text `2.7.2`; Level 2 → FFT JP Heading 2, text `2.7.2.` + Level 2 number; and so on). It is slow and easy to get wrong; the script does the same in one step.
   - When the file has no ladder at all (Step 1 said "dropped"), the repair builds it from the template: say whether the source's heading 1 is the module title or the first section ("1 …" = 2.7.4.1, as the translated 2.7.x files are), and type the module title to insert if the file has no title line (e.g. 臨床的安全性). Every heading is put on the list; the typed numbers "1", "1.1" are removed.
   - A local copy with AutoSave off is still good practice (co-authoring saves have dropped other things), but it is not the cause of this one.

#### 13. A reviewer sees the Japanese text (TOC, body) in a Gothic font, not ＭＳ 明朝. ####
   - Word on a non-Japanese Windows stores ＭＳ 明朝 / ＭＳ ゴシック under their English names, "MS Mincho" / "MS Gothic", when the template is loaded and again on every save. Most Japanese Word installations map the names back; some do not and fall back to Yu Gothic.
   - Step 1 prints a "Font names" line when a file carries the English names.
   - Run the file through **FFT Script → JP → Page grid & tables** with *Japanese font names* ticked (it is on by default, since v2.7.0). The result carries ＭＳ 明朝 / ＭＳ ゴシック again. The PDF is never affected; only people reviewing the .docx are.

#### 14. My translated file already carries the US FFT styles ("FFT Heading 1", "Table 2.3.P-1"). Do I delete them before Step 1 under JP? ####
   - No. The two style sets have different names and coexist. Since v2.8.0 Step 1 under JP moves every paragraph on a US FFT style to its JP twin — headings, body text, bullets, table cells, notes — numbers the headings from the module and rebuilds every caption that has a live number as `表 2.3.P-1. Title`. The result line under Submit says how many headings and captions it took over.
   - Before v2.8.0 the JP headings on such a file counted "1", "1.1": the module number had gone onto the US heading list. Re-run Step 1 under the new version and the headings read "2.3.P", "2.3.P.1".
   - A caption whose text before the number is not a label word is left alone and listed under Submit: press `t` / `f` on it by hand.
   - Deleting the US styles by hand is never needed and can strip the tables the English run formatted.

#### 15. After Step 1 (JP) a figure shows only as a thin strip, or looks hidden behind the text. ####
Pictures that sit in the text flow are cut down to the line height when the paragraph uses an exact line spacing — every FFT JP body style does, for the 38-line grid. Since v2.8.7 Step 1 gives each paragraph that holds an inline picture single line spacing (result line "Figures: …"), and the FFT Script *Page grid & tables* step does the same. On an older file: re-run Step 1, or run the file through the FFT Script page. If one picture is still clipped, click in its paragraph and set Line spacing to Single (Paragraph → Line spacing) — the style stays. Floating pictures (Layout Options → Top and Bottom) are not affected.

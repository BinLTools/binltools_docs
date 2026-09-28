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

#### 11. A heading or caption turned cyan after I pressed the key. ####
   - The number typed in the text differs from the number FFT assigned (for example you typed 2.4.3.2.3 but the heading level makes it 2.4.3.2.2, or the caption order gives a different table number).
   - Fix the level or the order, delete the typed number, then remove the highlight (Home → Text Highlight Color → No Color). When the typed number matches, FFT removes it by itself.


## Introduction
File Formatting Tool (FFT) is a Microsoft Word add-in designed to improve efficiency, consistency, and accuracy when formatting documents for health authority submissions.

This section describes the one-time deployment process required to install FFT on a local machine. It is now a single step: sideloading the add-in manifest into Microsoft Word.

The deployment process is required only once per device. After deployment, users may proceed directly to the FFT User Guide for daily use.

> **Previously installed FFT from the office-network server?** That version
> required installing a certificate and connecting a VPN. Neither is needed any
> more. See "Removing an older installation" at the bottom first.

## Get Started
### 0. Prerequisites
1. A machine (iPad, desktop, laptop, VM...) 🙃
2. Microsoft Word (2016 or later) / Microsoft Word Web

### 1. Download the Manifest
1. Download the manifest: [manifest_fft.xml](https://binltools.com/fft/manifest_fft.xml)
   (right-click → "Save link as…"; if it opens in the browser instead, press `Ctrl+S`).
2. Save it in a folder you will keep — for example `C:\Users\<username>\FFT-manifest`
   on Windows.

### 2. Sideload the Manifest
1. **MacOS / iPadOS**:
   1. Use Finder to sideload the manifest file -> Open Finder and then enter `Cmd+Shift+G` to open the Go to folder dialog.
   2. Enter `/Users/<username>/Library/Containers/com.microsoft.Word/Data/Documents/wef`.
   3. If the wef folder doesn't exist on your computer, create it.
   4. Copy the manifest into that folder, then restart Word.


2. **Windows**: 
   *Reference: [this page](https://learn.microsoft.com/en-us/office/dev/add-ins/testing/create-a-network-shared-folder-catalog-for-task-pane-and-content-add-ins).*
   1. select the new folder with saved manifest.
   2. Right click -> Show more options -> Give access to -> Specific people
   3. Select the owner -> Share -> Done. 
   
      ![1](/fft_deployment/images/1.png)
   
   4. Right Click saved manifest -> Sharing -> Copy the text under "Network Path"
   
      ![2](/fft_deployment/images/2.png)

   5. Open Word Options -> Trust Center -> Trust Center Setting -> Trusted Add-in Catalogs
   
   6. Paste the text into "Catalog Url" -> Add catalog -> Check "Show in Menu" -> OK
   
      ![3](/fft_deployment/images/3.png)
   
   7. Restart the Word -> Open File -> Home -> Add-ins -> Advanced -> "Contoso Task Pane" -> Add -> Show Task Pane
   
      ![4](/fft_deployment/images/4.png)

### 3. Check It Worked
The task pane shows a version stamp at the top: `Ver. x.y.z · <build time>`.
If the pane is blank, close Word completely, clear the add-in cache
(`%LOCALAPPDATA%\Microsoft\Office\16.0\Wef` on Windows), and reopen Word.

## Removing an Older Installation
Only needed if you installed FFT from the old office-network server.

1. Close all Word windows.
2. Word Options -> Trust Center -> Trust Center Settings -> Trusted Add-in
   Catalogs: remove the old catalog entry. Delete the old manifest file.
3. Clear the add-in cache: `Win+R` -> paste
   `%LOCALAPPDATA%\Microsoft\Office\16.0\Wef` -> Enter -> delete the contents.
4. *(Optional)* The old server's certificate is no longer used. To remove it:
   `Win+R` -> `certmgr.msc` -> Trusted Root Certification Authorities ->
   Certificates -> delete the `ratools.topalliancebio.com` entry.
5. Follow step 1 above with the new manifest.

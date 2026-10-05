## Introduction
File Formatting Tool (FFT) is a Microsoft Word add-in that formats regulatory documents for health authority submissions.

Installing it is **one step**: add the FFT manifest to Word. No certificate, no VPN — FFT runs at `https://binltools.com/fft/` behind a normal public certificate. Then create your FFT account inside the pane (once per person).

The installation is required only once per device. After that, use the FFT User Guide for daily work.

> **Upgrading from the old office-network FFT?** The certificate and VPN are no longer needed. Do "Remove the old version" at the bottom first.

## Get Started
### 0. Prerequisites
1. A machine (desktop, laptop, VM, iPad…) 🙃
2. Microsoft Word 2016 or later (desktop). Word for the web works with the same manifest through your organization's add-in catalog.
3. A work e-mail address (for the FFT account).

### 1. Download the manifest
1. Right-click **<https://binltools.com/fft/manifest_fft.xml>** → **Save link as…** (if it opens in the browser instead, press Ctrl + S).

   ![1](/fft_deployment/images/1.png)

2. Save it in a new folder, e.g. `C:\Users\<your-username>\FFT-manifest`, or any other folder of your choice inside your user folder (Desktop, Documents, …).

   ![2](/fft_deployment/images/2.png)

### 2. Add FFT to Word
1. **Windows**
   1. Find the folder's network path: right-click the folder → **Properties → Sharing** → copy the **Network Path** (`\\<your-PC-name>\Users\<your-username>\FFT-manifest`).

      ![3](/fft_deployment/images/3.png)

      *(The Sharing tab says "Not Shared"? Do not use the "Share…" button — it often fails silently. Type the path by hand instead: `\\<your-PC-name>\Users\<your-username>\FFT-manifest`. Your PC name: Win + R → `cmd` → Enter → `hostname` → Enter.)*

      ![5](/fft_deployment/images/5.png)

   2. Word → **File → Options → Trust Center → Trust Center Settings…**

      ![6](/fft_deployment/images/6.png)

   3. **Trusted Add-in Catalogs** → paste the network path into **Catalog Url** → **Add catalog** → tick **Show in Menu** → OK → OK.

      ![16](/fft_deployment/images/16.png)

   4. Close all Word windows, reopen Word.
   5. **Insert → Add-ins** → **Advanced…** (bottom left of the panel) → **SHARED FOLDER** → **FFT** → **Add**.

      ![7](/fft_deployment/images/7.png)

      ![4](/fft_deployment/images/4.png)

   6. The **FFT** button appears on the Home ribbon. Click it: the pane opens and shows **Sign in**.

      ![8](/fft_deployment/images/8.png)

2. **macOS / iPadOS**
   1. Finder → Cmd + Shift + G → `/Users/<username>/Library/Containers/com.microsoft.Word/Data/Documents/wef` (create `wef` if it does not exist).
   2. Copy `manifest_fft.xml` into it.
   3. Restart Word → **Insert → Add-ins → My Add-ins** → **FFT**.

### 3. Create your FFT account (once per person)
1. Open the FFT pane. It shows **Sign in**.
2. Click **Create account**, enter your work e-mail and a password (8 characters or more), click **Create**.

   ![10](/fft_deployment/images/10.png)

3. A 6-digit code arrives from fft@binltools.com (check junk the first time). Type it in the pane and click **Confirm**.

   ![12](/fft_deployment/images/12.png)

4. TopAlliance colleagues and pre-registered partner users are activated at once. Other partner users see **Awaiting approval** until TopAlliance activates the account — the pane updates by itself.

You stay signed in on that computer. On a shared PC, sign out from Settings when you are done.

**Check it worked:** the top of the pane shows `Ver. x.y.z · <build time>` and, once signed in, your e-mail and organization.

![13](/fft_deployment/images/13.png)

## Remove the old version (only if you installed the office-network FFT)
1. Close all Word windows.
2. Word → Trusted Add-in Catalogs (step 2.1.3): remove the old catalog entry; delete the old manifest from your local FFT folder.
3. Clear Word's add-in cache: Win + R → `%LOCALAPPDATA%\Microsoft\Office\16.0\Wef` → Enter → delete the contents of the folder.

   ![15](/fft_deployment/images/15.png)

4. *(Optional)* The old server's certificate is no longer used: Win + R → `certmgr.msc` → Trusted Root Certification Authorities → Certificates → delete the `ratools.topalliancebio.com` entry.
5. Continue with step 1.

## Troubleshooting

| Symptom | Fix |
|---|---|
| Task pane is blank / "ADD-IN ERROR" | Clear Word's add-in cache (Remove the old version, item 3), then fully close and reopen Word |
| FFT not listed under Shared Folder | Catalog path wrong or "Show in Menu" unticked — redo step 2.1.1–2.1.4. Paste the path into Win + R: it must open a folder that contains the manifest |
| Trust Center settings are greyed out | Company-managed PC. Ask your Microsoft 365 admin to deploy the manifest centrally (Microsoft 365 admin center → Settings → Integrated apps → Upload custom apps), then continue with step 3 |
| Pane looks out of date | Check the build stamp next to the version; if older than the latest release, clear the Wef cache and restart Word |
| No sign-in code | See FAQ #7 |
| "Awaiting approval" | See FAQ #8 |

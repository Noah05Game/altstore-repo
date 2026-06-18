# 📦 AltStore / SideStore Repository

A self-hosted, GitHub Pages-compatible app repository for AltStore and SideStore. No server needed — just a GitHub account.

---

## 📋 What's Included

```
altstore-repo/
├── index.html              ← Public-facing app browser
├── admin.html              ← Visual editor for apps.json
├── apps.json               ← The source file AltStore/SideStore reads
├── apps/                   ← Your .ipa files go here
│   └── .gitkeep
├── icons/                  ← App icons and screenshots go here
│   └── .gitkeep
├── .github/
│   └── workflows/
│       └── deploy.yml      ← Auto-validates & deploys on push
└── README.md               ← This file
```

---

## 🚀 Setup Guide (First Time)

### Step 1 — Fork or create the repository

1. Go to [github.com](https://github.com) and log in
2. Create a **new repository** (e.g., `altstore-repo`)
3. Set visibility to **Public** (required for GitHub Pages free tier)
4. Upload all files from this project to the repository root

### Step 2 — Enable GitHub Pages

1. In your repository, go to **Settings** → **Pages**
2. Under **Source**, select **GitHub Actions**
3. Save the settings

### Step 3 — Set your repository URLs

Open `apps.json` and replace every instance of:
```
https://yourusername.github.io/altstore-repo/
```
with your actual GitHub Pages URL, which will be:
```
https://YOUR-GITHUB-USERNAME.github.io/YOUR-REPO-NAME/
```

Also update the `identifier` field with your own reverse-domain string (e.g., `com.yourname.altsource`).

### Step 4 — Commit and push

Push all files to the `main` branch. GitHub Actions will:
- ✅ Validate your `apps.json`
- ✅ Deploy to GitHub Pages automatically

Your repo URL will be live at:
```
https://YOUR-GITHUB-USERNAME.github.io/YOUR-REPO-NAME/apps.json
```

### Step 5 — Add the source to AltStore or SideStore

**AltStore:**
1. Open AltStore → Browse → Sources
2. Tap **+**
3. Paste your `apps.json` URL
4. Tap **Add Source**

**SideStore:**
1. Open SideStore → Browse → Sources
2. Tap **+**
3. Paste your `apps.json` URL
4. Tap **Add Source**

---

## 📱 Adding Your First Real App

### Step 1 — Get your IPA

You need a `.ipa` file for your app. This can be:
- An app you've built and exported from Xcode (Ad Hoc or Development)
- An open-source app you've compiled yourself
- Any valid IPA you have the rights to distribute

### Step 2 — Name the IPA file

Use this naming convention:
```
com.bundle.identifier-1.0.0.ipa
```
Example: `com.myname.myapp-1.2.0.ipa`

### Step 3 — Upload the IPA

Drop the file into the `apps/` folder in your GitHub repository.

### Step 4 — Prepare the icon

- Size: **512×512 pixels**, PNG format
- Name it: `com.bundle.identifier.png`
- Place in the `icons/` folder

### Step 5 — Edit apps.json

Use either:
- **admin.html** (recommended) — open it in a browser, edit visually, export the JSON, replace `apps.json`
- Or edit `apps.json` directly

### Step 6 — Push changes

Commit and push. GitHub Actions deploys automatically in ~60 seconds.

---

## 🔄 Publishing an App Update

1. **Upload the new IPA** → `apps/com.bundle.id-2.0.0.ipa`
2. **Update icon** if it changed → `icons/com.bundle.id.png`
3. **Open admin.html** → find your app → click Edit → click "Add Version"
4. Fill in the new version number, date, download URL, and release notes
5. Click **Save App**, then **Export JSON**
6. Replace `apps.json` in your repo
7. Push to GitHub

AltStore and SideStore will automatically show the update in the **Updates** tab for users who have installed the app.

> **Important:** Never change a `bundleIdentifier` after an app has been published. AltStore tracks apps by bundle ID. Changing it is treated as a completely different app.

---

## 🛠 Using admin.html Locally

`admin.html` is a pure static file. To use it locally with live `apps.json` loading:

**Option A — Python:**
```bash
cd altstore-repo
python3 -m http.server 8080
# Open http://localhost:8080/admin.html
```

**Option B — Node.js / npx:**
```bash
cd altstore-repo
npx serve .
# Open http://localhost:3000/admin.html
```

**Option C — Live Server (VS Code):**
Right-click `admin.html` → Open with Live Server

> ⚠ Opening `admin.html` directly as a `file://` URL will not load `apps.json` due to browser CORS rules. Always use a local server.

---

## 📐 apps.json Field Reference

### Top-level fields

| Field | Type | Required | Description |
|---|---|---|---|
| `name` | string | ✅ | Repository display name |
| `identifier` | string | ✅ | Unique reverse-domain ID |
| `subtitle` | string | | Short tagline |
| `description` | string | | Full description |
| `iconURL` | string | | URL to repo icon |
| `headerURL` | string | | URL to header image |
| `website` | string | | Your website URL |
| `tintColor` | string | | Hex color (e.g. `#007AFF`) |
| `featuredApps` | array | | Bundle IDs to feature |
| `apps` | array | ✅ | List of apps |
| `news` | array | | News items |

### App fields

| Field | Type | Required | Description |
|---|---|---|---|
| `name` | string | ✅ | App display name |
| `bundleIdentifier` | string | ✅ | Unique bundle ID |
| `developerName` | string | | Developer display name |
| `subtitle` | string | | One-line description |
| `localizedDescription` | string | | Full description |
| `iconURL` | string | | URL to 512×512 PNG icon |
| `tintColor` | string | | Hex accent color |
| `screenshotURLs` | array | | Screenshot image URLs |
| `versions` | array | ✅ | Version history |
| `appPermissions` | object | | Entitlements/privacy info |

### Version fields

| Field | Type | Required | Description |
|---|---|---|---|
| `version` | string | ✅ | Version string (e.g. `1.2.3`) |
| `date` | string | ✅ | ISO 8601 date |
| `downloadURL` | string | ✅ | Direct URL to IPA |
| `localizedDescription` | string | | Release notes |
| `size` | number | | File size in bytes |
| `minOSVersion` | string | | Minimum iOS version |
| `maxOSVersion` | string | | Maximum iOS version |

---

## 🔒 Keeping URLs Relative

All `downloadURL` and `iconURL` values in `apps.json` support relative paths, which means your repository will work correctly on any fork without changing URLs:

```json
"downloadURL": "apps/com.example.myapp-1.0.0.ipa",
"iconURL": "icons/com.example.myapp.png"
```

When loaded by AltStore, URLs are resolved relative to the `apps.json` location.

---

## ❓ Troubleshooting

**AltStore says "Source is invalid"**
- Check that `apps.json` is valid JSON (use the Preview tab in admin.html)
- Make sure `name`, `identifier`, and `apps` fields are present
- Try pasting your JSON into [jsonlint.com](https://jsonlint.com)

**App doesn't show an update**
- Verify the new version number is higher (AltStore compares version strings)
- Make sure the `bundleIdentifier` hasn't changed
- Wait a few minutes for AltStore to refresh

**Icons not loading**
- Icons must be accessible via HTTPS
- Check the path is correct (case-sensitive on GitHub Pages)
- Use a 512×512 PNG for best results

**GitHub Actions fails**
- Check the Actions tab in your GitHub repo for the error log
- Most common cause: invalid JSON in `apps.json`

---

## 📄 License

This repository template is released under the MIT License. You are free to use it for personal and commercial repositories.

# RD Expense Tracker

Personal expense management app — hosted on GitHub Pages, with optional OneDrive or Google Drive sync.

---

## Quick Start (GitHub Pages)

1. **Fork or create a new repo** on GitHub
2. **Upload** `index.html` (and this README) to the repo root
3. Go to **Settings → Pages**
4. Under *Source*, select **GitHub Actions**
5. Push any commit to `main` — the app auto-deploys
6. Your app is live at: `https://YOUR_USERNAME.github.io/YOUR_REPO_NAME`

The `.github/workflows/deploy.yml` file handles all deployment automatically.

---

## Storage Options

The app supports three storage backends. You can switch between them any time in **Storage Settings**.

### Option A — Local Downloads (zero setup)
No configuration needed. Reports export as `.xlsx` files and receipts download as individual images directly to your Downloads folder. Best for quick setup.

### Option B — OneDrive (Microsoft 365)

1. Go to [portal.azure.com](https://portal.azure.com) and sign in
2. Navigate to **Azure Active Directory → App Registrations → New Registration**
3. Name it `RD Expense Tracker`
4. Set Redirect URI to: `https://YOUR_USERNAME.github.io/YOUR_REPO_NAME`
5. Under **API Permissions**, add:
   - `Microsoft Graph → Delegated → Files.ReadWrite`
   - `Microsoft Graph → Delegated → User.Read`
6. Copy the **Application (client) ID**
7. Open `index.html`, find the `CFG` block near the top, and set:
   ```js
   MSAL_CLIENT_ID: 'paste-your-client-id-here',
   MSAL_TENANT:    'common',   // or your tenant ID for org accounts
   ```
8. The MSAL.js library is already loaded — uncomment the `msalApp` lines in `doMSLogin()` to enable real auth

Receipts sync to: `OneDrive / {Folder} / {ReportName}_Receipts /`

### Option C — Google Drive

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a new project or select an existing one
3. Enable **Google Drive API** and **Google Picker API**
4. Go to **Credentials → Create Credentials → OAuth 2.0 Client ID**
5. Application type: **Web Application**
6. Authorized JavaScript origins: `https://YOUR_USERNAME.github.io`
7. Copy the **Client ID** and **API Key**
8. Open `index.html`, find the `CFG` block, and set:
   ```js
   GOOGLE_CLIENT_ID: 'paste-your-client-id-here',
   GOOGLE_API_KEY:   'paste-your-api-key-here',
   ```
9. Uncomment the Google GIS lines in `doGoogleLogin()` to enable real auth

Receipts sync to: `My Drive / {Folder} / {ReportName}_Receipts /`

---

## Updating the App Version

When you make changes, update the `CFG` block at the top of `index.html`:

```js
const CFG = {
  APP_VERSION:  'v1.1.0',       // bump this on every release
  APP_RELEASED: 'May 2026',     // update the month
  ...
};
```

The version number appears in:
- The bottom version bar of the app
- Every exported Excel file (Expenses sheet header + Summary sheet)
- The export filename: `ReportName_v1.1.0_Report.xlsx`

Commit and push — GitHub Actions redeploys in ~30 seconds.

---

## Demo Login

For the demo build (no real auth configured), use:
- **Email:** any valid email address
- **Password:** `demo`

To require a real password, replace the `doLogin()` check in `index.html` with your preferred auth method.

---

## Data & Privacy

All expense data is stored in your **browser's localStorage** on your device. Nothing is sent to any server unless you configure OneDrive or Google Drive sync. Clearing browser data will clear the app — back up by exporting reports regularly.

---

## File Structure

```
rd-expense/
├── index.html                  ← entire app (single file)
├── README.md
└── .github/
    └── workflows/
        └── deploy.yml          ← auto-deploy to GitHub Pages
```

---

## Version History

| Version | Date | Notes |
|---------|------|-------|
| v1.0.0 | April 2026 | Initial release — reports, expenses, receipts, Excel export, OneDrive/Google Drive/Local storage, login |

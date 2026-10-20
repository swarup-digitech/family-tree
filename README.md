# न्यौपाने वंशावली — your own web address

These files are a small front page for the family tree. It opens your Apps Script site inside it, so
**your Google Sheet, admin panel and members keep working exactly as before.** What you gain:

- your own address, for example `https://vamshavali.com` or `https://family.yourname.com`
- a real **Install** button on Android phones and computers, with the family icon and the name **वंशावली**
- the option to publish on the Play Store later (see the end)

Nothing private is stored on GitHub. These files contain only the page design, the icons and your Apps Script link.

---

## Step 1 — Put your Apps Script link in `index.html`

1. Open your Apps Script project and choose **Deploy → Manage deployments**. Copy the **Web app** URL
   (it starts with `https://script.google.com/macros/s/` and ends in `/exec`).
2. Open `index.html` in any text editor and find this line near the top:
   ```js
   const APP_URL = 'PASTE_YOUR_WEB_APP_URL_HERE';
   ```
   Replace the text between the quotes with your link. Save the file.

## Step 2 — Upload to GitHub

1. Sign in at github.com and choose **+ → New repository**.
2. Give it any name, for example `family-tree`. Set it to **Public**, because free GitHub Pages needs a public repository.
   It only holds these design files, not family data.
3. Choose **Add file → Upload files**. Drag in **everything in this folder**: `index.html`, `manifest.webmanifest`,
   `sw.js`, `privacy.html`, `.nojekyll`, and the `icons` and `.well-known` folders. Then choose **Commit changes**.
   - If your computer hides `.nojekyll`, create it on GitHub instead: **Add file → Create new file**, name it `.nojekyll`,
     leave it empty and commit.
4. Go to **Settings → Pages**. Under **Build and deployment**, set **Source** to *Deploy from a branch*, then choose
   **Branch: main** and the **/ (root)** folder, and choose **Save**.
   After a minute the site is live at `https://<your-username>.github.io/family-tree/`. Open it to check.

## Step 3 — Connect your domain

### 3a. Tell GitHub the domain
In the repository, go to **Settings → Pages → Custom domain**. Type your domain, for example `vamshavali.com` or
`family.yourname.com`, and choose **Save**. GitHub adds a file called `CNAME` to the repository; leave it there.

### 3b. Add DNS records at your domain company
These go wherever you bought the domain (GoDaddy, Namecheap, Hostinger, Cloudflare, Mercantile, and so on), in the
**DNS** or **Manage DNS** page. Delete any old `A`, `AAAA` or `CNAME` records for the same name first.

**If you use a sub-domain** such as `family.yourname.com` (easiest):

| Type  | Name / Host | Value / Points to          |
|-------|-------------|----------------------------|
| CNAME | `family`    | `<your-username>.github.io` |

Use only your username here, without the repository name.

**If you use the main domain** such as `vamshavali.com`, add all eight records below, plus the `www` record:

| Type  | Name / Host | Value                  |
|-------|-------------|------------------------|
| A     | `@`         | `185.199.108.153`      |
| A     | `@`         | `185.199.109.153`      |
| A     | `@`         | `185.199.110.153`      |
| A     | `@`         | `185.199.111.153`      |
| AAAA  | `@`         | `2606:50c0:8000::153`  |
| AAAA  | `@`         | `2606:50c0:8001::153`  |
| AAAA  | `@`         | `2606:50c0:8002::153`  |
| AAAA  | `@`         | `2606:50c0:8003::153`  |
| CNAME | `www`       | `<your-username>.github.io` |

Using Cloudflare? Set these records to **DNS only** (grey cloud) until Step 3c is finished.

### 3c. Turn on HTTPS
DNS changes take from a few minutes up to 24 hours. Back in **Settings → Pages**, wait until it says your site is
live at your domain, then tick **Enforce HTTPS**. If the box can't be ticked yet, GitHub is still issuing the
certificate; try again later. HTTPS is required for the Install button and for *Near me* location.

### 3d. Protect the domain (recommended)
Go to your **profile picture → Settings → Pages → Add a domain**. GitHub shows a `TXT` record. Add it at your domain
company and choose **Verify**. This stops anyone else's GitHub repository from using your domain.

## Step 4 — Tell the family tree about the new address

In Apps Script, open `Code.gs` and set the address in the `SITE` block at the top:
```js
publicUrl: 'https://vamshavali.com'
```
Then choose **Deploy → Manage deployments → ✏️ Edit**, set **Version: New version**, and choose **Deploy**.
After this, the admin panel's **View website** link and the WhatsApp login message you send to members use your domain.

---

## Good to know

- **Updates:** changes to the Sheet or Apps Script show up on your domain automatically, so you never need to touch
  GitHub again, unless you change the Apps Script link itself.
- **Admin panel:** keep using your Apps Script link with `?page=admin`. Keep it to yourself; it is not shown on your domain.
- **Members log in once more:** browsers keep logins separately for each web address. When a member first opens your
  domain, they log in again and tick *Keep me logged in*. After that, it is remembered as before.
- **Near me on phones:** some phones block GPS inside an embedded page. If that happens, the *Near me* window still
  lets members type a place name instead.
- **Install:** on Android (Chrome) and on computers, an **Install** button appears. On iPhone, the banner shows
  *Share → Add to Home Screen*.
- **Privacy page:** `https://<your-domain>/privacy.html`. App stores ask for this.

## Later: Play Store (optional)

With your own domain, the Play Store route is straightforward:
1. Open pwabuilder.com, enter your domain, and choose **Package for stores → Android**. Download the package.
2. The package contains an `assetlinks.json` file. Upload it into this repository's `.well-known` folder,
   so it is served at `https://<your-domain>/.well-known/assetlinks.json`.
3. Upload the `.aab` file to Google Play Console and complete the closed test (12 testers for 14 days for new
   personal accounts).

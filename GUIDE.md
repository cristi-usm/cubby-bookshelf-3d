# 3D Cubby Bookshelf — Setup & Deploy to Vercel

## Prerequisites

You need these installed on your machine:

- **Node.js** (v18+) → https://nodejs.org
- **Git** → https://git-scm.com
- **Vercel CLI** (optional, for CLI deploy) → installed in step 3

Check if you have them:

```bash
node -v
git --version
```

---

## Step 1 — Get the project files onto your machine

Unzip the downloaded project folder (or recreate it). Your folder should look like this:

```
cubby-bookshelf-3d/
├── public/
│   └── index.html      ← the full 3D viewer
├── package.json
└── vercel.json
```

Open a terminal and navigate into the project:

```bash
cd cubby-bookshelf-3d
```

---

## Step 2 — Run it locally

No `npm install` needed — it's a pure static site using a CDN for Three.js.

```bash
npx serve public -l 3000
```

Open your browser to **http://localhost:3000**. You should see the 3D bookshelf with orbit controls, explode button, and drawer toggle.

> **Tip:** If port 3000 is taken, use any other port: `npx serve public -l 5000`

---

## Step 3 — Create a Git repo

```bash
git init
git add .
git commit -m "Initial commit: 3D cubby bookshelf viewer"
```

---

## Step 4 — Push to GitHub

### Option A: Using GitHub CLI

```bash
gh repo create cubby-bookshelf-3d --public --push --source=.
```

### Option B: Manually

1. Go to https://github.com/new
2. Create a new repo named `cubby-bookshelf-3d`
3. Do NOT initialize with README (you already have files)
4. Run:

```bash
git remote add origin https://github.com/YOUR_USERNAME/cubby-bookshelf-3d.git
git branch -M main
git push -u origin main
```

---

## Step 5 — Deploy to Vercel

### Option A: Vercel Dashboard (easiest)

1. Go to https://vercel.com and sign in (or create a free account)
2. Click **"Add New..." → Project**
3. Click **"Import Git Repository"** and select `cubby-bookshelf-3d`
4. Vercel auto-detects the settings. Verify:
   - **Framework Preset:** Other
   - **Output Directory:** `public`
5. Click **Deploy**
6. Wait ~30 seconds. Done! You'll get a URL like `cubby-bookshelf-3d.vercel.app`

### Option B: Vercel CLI

Install the CLI first:

```bash
npm i -g vercel
```

Then deploy:

```bash
vercel
```

It will ask you a few questions:

```
? Set up and deploy? → Y
? Which scope? → (pick your account)
? Link to existing project? → N
? Project name? → cubby-bookshelf-3d
? In which directory is your code located? → ./
? Override settings? → N
```

Vercel reads `vercel.json` and knows to serve from `public/`.

To deploy to **production** (get your .vercel.app URL):

```bash
vercel --prod
```

---

## Step 6 — Custom domain (optional)

1. In the Vercel dashboard, open your project
2. Go to **Settings → Domains**
3. Add your domain (e.g., `bookshelf.yourdomain.com`)
4. Point your DNS to Vercel's nameservers (Vercel shows you the exact records)

---

## Making changes

After editing `public/index.html`:

```bash
git add .
git commit -m "Updated bookshelf design"
git push
```

Vercel auto-deploys on every push to `main`. Your site updates in ~15 seconds.

---

## Project structure explained

| File | Purpose |
|---|---|
| `public/index.html` | The entire app — HTML + CSS + Three.js scene in one file |
| `package.json` | Defines the `dev` script for local preview |
| `vercel.json` | Tells Vercel to serve `public/` as the site root |

The Three.js library loads from a CDN (`cdnjs.cloudflare.com`), so there are no node_modules or build steps. The entire project is a single HTML file.

---

## Troubleshooting

**Black screen / nothing renders:**
- Check browser console for errors (F12 → Console)
- Make sure WebGL is enabled (type `chrome://gpu` in Chrome)

**Vercel shows 404:**
- Verify `vercel.json` has `"outputDirectory": "public"`
- Make sure `public/index.html` exists

**Slow on mobile:**
- The scene uses soft shadows. You can reduce `shadow.mapSize` from 1024 to 512 in the script for better mobile performance

**Port already in use:**
- Use a different port: `npx serve public -l 8080`

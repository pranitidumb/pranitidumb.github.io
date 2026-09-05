# Personal Website — Setup & Deploy Guide

This uses **GitHub Pages + Jekyll**. Jekyll turns plain Markdown files into
a styled website automatically — you write content, GitHub builds the
actual HTML/CSS for you on every push. No local build tools needed.

## Step 1: Create a special repository

GitHub Pages treats one specific repo name as your personal site's root URL.

1. Go to https://github.com/new
2. Name it **exactly**: `pranitidumb.github.io` (your GitHub username +
   `.github.io` — this exact naming is what makes it your main site URL
   instead of a project sub-page)
3. Public, no README initialization (same as before)
4. Click **Create repository**

## Step 2: Add these files to your project folder

Take the files I've generated (`_config.yml`, `index.md`, `blog.md`,
`reading-list.md`, and the `_posts` folder) and put them in a new local
folder, e.g. `~/Desktop/personal-website`.

## Step 3: Push it to GitHub

Same flow as your book recommender:

```
cd ~/Desktop/personal-website
git init
git add .
git commit -m "Initial site"
git remote add origin https://github.com/pranitidumb/pranitidumb.github.io.git
git branch -M main
git push -u origin main
```

## Step 4: Enable GitHub Pages

1. On your repo's GitHub page, click **Settings**
2. In the left sidebar, click **Pages**
3. Under "Build and deployment" → Source, select **"Deploy from a branch"**
4. Branch: `main`, folder: `/ (root)` — click **Save**

## Step 5: Wait and visit your site

Give it 1-2 minutes for the first build. Then visit:
```
https://pranitidumb.github.io
```

If it 404s at first, wait another minute and refresh — the very first
deploy is sometimes slower than later updates.

---

## How to edit each part going forward

**Portfolio (home page):** edit `index.md` directly — replace the
placeholder bullet points with your real projects and bio.

**Add a blog post:** create a new file in `_posts/`, named
`YYYY-MM-DD-your-title.md` (the date matters — it controls sorting and
the post's URL). Copy the front matter format from the sample post.

**Reading list:** edit `reading-list.md` — it's just a Markdown list, add
or remove books freely.

**After any edit**, push it live the same way:
```
git add .
git commit -m "describe what you changed"
git push
```
GitHub rebuilds automatically within a minute or so.

---

## Customizing further

- **Change the color theme:** `minima` (the theme used here) supports a
  few built-in skins. In `_config.yml`, under the `minima:` section, add:
  ```yaml
  minima:
    skin: dark
  ```
  Options: `classic` (default), `dark`, `solarized-dark`, `solarized-light`.
- **Add a custom domain later** (like `pranitidumb.com`): GitHub Pages
  supports this for free once you own a domain — Settings → Pages has a
  "Custom domain" field when you're ready.
- **Want more design control than Jekyll's themes allow?** That's a bigger
  jump (a framework like Next.js or Astro, or hand-written HTML/CSS) — a
  good "phase 2" once you're comfortable with this simpler version.

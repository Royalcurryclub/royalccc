# Royal Curry Club of Christchurch - Complete Setup Guide

This bundle contains a fully cleaned, tested, and ready-to-deploy version of your site.

## What's in this bundle

```
royalccc-main/
├── _config.yml          ← Updated with theme, plugins, navigation
├── Gemfile              ← NEW - tells Netlify which Jekyll version to use
├── netlify.toml         ← NEW - Netlify build configuration
├── index.md             ← NEW - homepage with welcome + post listing
├── contact.md           ← Rewritten with Netlify Forms (replaces broken WordPress form)
├── reviews.md           ← Rewritten to auto-list all 88 reviews
├── visitations.md       ← Cleaned (removed WordPress edit links, fixed entities)
├── scores-on-the-doors.md ← Cleaned (removed WordPress edit links, fixed entities)
├── welcome.md           ← Original (kept as-is for old URLs)
├── _posts/              ← All 88 posts, HTML entities cleaned (— & ' instead of &#8211; &amp; &#8217;)
├── admin/               ← NEW - Netlify CMS interface for your friend
│   ├── config.yml
│   └── index.html
└── wp-content/uploads/  ← Logo files only (101 duplicate sizes removed)
```

## What was fixed

1. **Post titles displayed as gibberish** — All 88 posts had WordPress HTML entities (`&#8211;`, `&amp;`, `&#8217;`) that double-escaped on render. Now display correctly as `December 2024 – Indian Fendalton & Misceos`.
2. **Missing homepage** — Created `index.md` so visiting the root URL shows something.
3. **Empty reviews page** — Now auto-generates a list of all reviews.
4. **Broken contact form** — Replaced WordPress's `wpcf7` form with a Netlify Forms version (free, includes spam filtering, submissions appear in your Netlify dashboard).
5. **Bare _config.yml** — Was 3 lines; now properly configured with theme, plugins, navigation.
6. **No theme** — Added the `minima` theme so it doesn't look like 1995.
7. **Missing Gemfile** — Netlify needs this to build Jekyll. Created.
8. **WordPress junk** — Removed `_drafts/` (revision history), plugin folders (`simply-static` 7.5MB, `really-simple-ssl`, `sucuri`, etc.), and 100+ duplicate auto-resized logo variations. Site went from **13MB → 1.8MB**.
9. **CMS not configured** — Added `admin/` folder with Netlify CMS pre-configured for editing the two tables and creating new blog posts.

## Tested

I installed Jekyll, ran `jekyll build` against this bundle, and verified:
- Homepage renders with welcome text + post list (94 pages built total)
- All 88 posts render with clean titles
- Visitations table renders (125 rows)
- Scores on the Doors table renders (95 rows)
- Reviews page auto-lists all 88 reviews
- Contact form has correct Netlify Forms attributes
- Navigation menu works
- Admin folder is served at `/admin/`

---

## Deployment Steps

### Phase 1: Upload to GitHub (5 min)

1. Go to https://github.com and sign up / log in
2. Click **+** (top right) → **New repository**
3. Name: `royalccc` (or whatever you prefer), Public, do NOT initialize with README
4. Click **Create repository**
5. On the next screen, click **uploading an existing file**
6. Drag the entire contents of the `royalccc-main` folder into the upload area
   - Make sure to include hidden files like `.gitattributes`
7. Commit message: "Initial site"
8. Click **Commit changes**

### Phase 2: Deploy to Netlify (5 min)

1. Go to https://app.netlify.com/signup → **Sign up with GitHub**
2. Authorize Netlify
3. Click **Add new site** → **Import an existing project** → **Deploy with GitHub**
4. Select your `royalccc` repository
5. Build settings should auto-detect from `netlify.toml`:
   - Build command: `jekyll build`
   - Publish directory: `_site`
6. Click **Deploy site**
7. Wait 2-3 minutes. Click the URL Netlify provides (e.g. `https://random-name-12345.netlify.app`)
8. Verify the site loads

### Phase 3: Update site URL in config (2 min)

Once you know your Netlify URL (e.g. `https://random-name-12345.netlify.app`):

1. Go back to your GitHub repo
2. Click `_config.yml`
3. Click the pencil icon to edit
4. Change line 4 from:
   ```yaml
   url: "https://royalccc.netlify.app"
   ```
   to your actual Netlify URL
5. Commit changes — Netlify will auto-rebuild

(If you set a custom domain later, change this to that.)

### Phase 4: Enable the CMS for editing (10 min)

1. In Netlify, go to your site → **Site configuration** (or **Site settings**)
2. Click **Identity** in the left sidebar
3. Click **Enable Identity**
4. Under **Registration preferences**, change to **Invite only**
5. Scroll down to **Services** → **Git Gateway** → **Enable Git Gateway**
6. Go to the **Identity** tab (top nav) → **Invite users**
7. Enter your email and your friend's email
8. You'll both get invitation emails — click the link, set a password

### Phase 5: Test the CMS

1. Go to `https://your-site.netlify.app/admin/`
2. Log in with your email/password
3. You should see:
   - **Blog Posts** (88 entries)
   - **Pages** (Visitations, Scores on the Doors, Welcome)
4. Try editing one of the tables — change something trivial, click Save → Publish
5. Wait ~60 seconds, refresh the live site to see the change

---

## Optional: Custom Domain

If you want to keep `royalccc.net`:

1. In Netlify: **Domain settings** → **Add custom domain** → enter `royalccc.net`
2. Netlify shows DNS records to add
3. Log into Namecheap → Domain List → **Manage** next to royalccc.net
4. Click **Advanced DNS**
5. Add the records Netlify gave you (typically an A record and CNAME)
6. Wait 1-24 hours for DNS to propagate
7. Netlify auto-provisions free SSL once DNS resolves

Cost: just the domain renewal (~NZD $20-25/year at Namecheap)

---

## Friend's Editing Workflow

Send your friend this short version:

> 1. Go to `https://your-site.netlify.app/admin/`
> 2. Log in with your email and password
> 3. To update a table: click **Pages** → **Visitations Table** or **Scores on the Doors** → edit → Save → Publish
> 4. To add a review: click **Blog Posts** → **New Blog Post** → fill in title, date, body → Save → Publish
> 5. Changes appear on the live site within ~60 seconds

The CMS has a built-in markdown table editor so they don't need to remember the `| pipe | syntax |`.

---

## Costs

| Item | Cost |
|---|---|
| GitHub | Free |
| Netlify hosting (100GB/mo bandwidth, plenty for this) | Free |
| Netlify Identity (up to 1000 users) | Free |
| Netlify CMS | Free |
| Netlify Forms (100 submissions/month) | Free |
| Domain (optional) | ~NZD $25/year |

**Total: $0/month** (or ~$2/month if you keep royalccc.net)

---

## Troubleshooting

**Build fails on Netlify?**
- Check the **Deploys** tab → click the failed deploy → read the log
- Most common: a typo in `_config.yml` YAML

**Friend gets "user not found" trying to log in?**
- Make sure they accepted the invitation email
- Check Netlify → Identity tab → confirm they're listed

**Site shows but tables look wrong?**
- The minima theme has basic table styling. If you want fancier tables, ask Claude later for custom CSS.

**Want a different theme?**
- Browse https://jamstackthemes.dev/ssg/jekyll/
- Replace `theme: minima` in `_config.yml` with the new theme name
- Add the new theme to the `Gemfile`

---

That's it. Upload, deploy, invite, edit. You and your friend should be running the site within an hour.

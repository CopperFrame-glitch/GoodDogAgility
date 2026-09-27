# Good Dog Agility Club — website

A plain HTML site. No database, no plugins, no updates, nothing that can break on its own.

---

## How this site is published

This repository **is** the live site. Netlify watches the `main` branch and redeploys
within a minute or two of every push. There is no build step and no zip to drag anywhere.

    edit a file  →  commit  →  push  →  live

Rolling back is just as quick: in Netlify, **Deploys → pick an earlier deploy →
Publish deploy**. Nothing on the site can break in a way you can't undo.

### Making a small change without any software

For one-off edits, such as posting a running order or closing a class, you can work
entirely in your browser:

1. Open the file here on github.com.
2. Click the pencil icon.
3. Make the change and click **Commit changes**.

Netlify picks it up and republishes on its own. Uploading a PDF works the same way:
open `trials/docs/<year>/<month>/`, then **Add file → Upload files**.

`GO-LIVE.md` in this folder has the step-by-step instructions for trial documents,
closing and reopening registration, and marking a class full.

### Hosting and email

The site is on Netlify. **Club email is separate and lives on TotalChoice (cPanel).**
Only the website records point at Netlify; the MX and mail records stay where they are.
Don't cancel TotalChoice and don't move nameservers, or email will stop.

---

## Where the images go

```
images/brand/logo_Clear_web-clear.png    site logo (header)
images/brand/GDA-Favicon.png             browser tab icon
images/facility/GDAMap.jpg               facility map (What to Expect page)
images/action/                           trial and class action shots
images/people/                           instructor headshots (square)
images/graduates/                        graduate photos
images/video/                            timer and scribe tutorials
```

Filenames in the HTML must match exactly, including capitalisation.

---

## Routine updates

### New class session (twice a year)
Open `classes.html` and `index.html`, find the schedule table, change the four date ranges. Update the announcement bar text at the top of every page — it's the line starting `<div class="ticker">`.

Also swap the Jotform URL in `classes.html` when you duplicate the registration form:
```html
<iframe title="Class registration form" src="https://pci.jotform.com/form/YOUR-NEW-FORM-ID"
```

### Adding a trial
In `trials.html`, copy one `<div class="trial">` block and edit it. Put upcoming trials at the top.

**After the event, don't delete it.** Change `class="trial"` to `class="trial past"`, move it into the Past Trials section, and add results. Deleting old trials throws away search traffic the club has already earned — that was a real problem on the old site.

Make sure the dates match everywhere: heading, premium PDF, and page title.

### Adding an instructor
In `instructors.html`, copy a `<div class="person">` block. Square photo into `images/people/`.

### Adding graduates
In `graduates.html`, copy a `<figure>` block per team. Caption with handler and dog name.

---

## Changing the look

All colours and fonts live at the top of `style.css`:

```css
:root{
  --ink:#3D1607;      /* dark brown */
  --clay:#D45113;     /* burnt orange */
  --sun:#FCC831;      /* golden yellow */
  ...
}
```

Change a value there and it updates across all 17 pages.

---

## Still to do

- [ ] Paste the dog aggression policy into `policies.html` (from `DogAgressionPolicy.html` in the backup)
- [ ] Add the board and officers list to `about.html`
- [ ] Replace instructor placeholders with real photos and fresh bios
- [ ] Add real trial dates to `trials.html`
- [ ] Fill the gallery and graduates pages
- [ ] Link the timer and scribe tutorial videos in `volunteering.html`
- [ ] Claim the Google Business Profile for the Chandler location
- [ ] Set up 301 redirects from the old URLs (`/training.html`, `/clubinfo.html`, `/faq.html`, `/shows.html`, `/membership.html`, `/what_to_bring.html`) — in Netlify, create a `_redirects` file
- [ ] Ask this session's graduates for Google reviews

---

## Rebuilding from source

`build.py` (one level up) generates every page from shared templates. If you'd rather edit copy in one place than across 17 files, edit `build.py` and run `python3 build.py`. Editing the HTML directly is fine too — the generator is a convenience, not a requirement.

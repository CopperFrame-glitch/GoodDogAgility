# Going live — 1 hour plan

## 1. Add your images (5 min)
Copy your `_UPLOAD` contents into `images/` so you have:
```
images/brand/logo_Clear_web-clear.png
images/brand/GDA-Favicon.png
images/facility/GDAMap.jpg
```
Those three are the only ones the site needs to look complete.
If your files carry a prefix (`brand-logo_Clear_web-clear.png`), rename them
to drop it — that's easier than editing every page.

## 2. Deploy (5 min)
- app.netlify.com/drop → drag this whole folder in
- Claim the site to your account
- Confirm it loads and is not password protected
  (Site configuration → Access & security)

## 3. Point the domain — DO THIS CAREFULLY (20 min + propagation)

**Do NOT change nameservers.** That would move DNS control to Netlify and
break your club email, which lives on TotalChoice.

Instead, keep DNS at TotalChoice and change only the website records:

- In Netlify: **Domain management → Add a domain** → `gooddog.org`
  Netlify will show you the values to use (an A record IP for the apex,
  and a CNAME target for www).
- In cPanel: **Zone Editor** for gooddog.org
  - Change the **A record** for `gooddog.org` to Netlify's IP
  - Change/add the **CNAME** for `www` to Netlify's target
  - **LEAVE EVERY MX RECORD EXACTLY AS IT IS** — those are your email
  - Leave any TXT records (SPF/DKIM) alone too

Propagation is usually 15–60 minutes. Netlify issues SSL automatically once
the DNS resolves; give it another few minutes after the site first responds.

## 4. Verify
- gooddog.org loads the new site
- https works (padlock in the address bar)
- **Send yourself an email at your @gooddog.org address and confirm it arrives**
- Click through all 17 pages
- Test the registration form on /classes.html

## 5. After it's live
- Submit `sitemap.xml` in Google Search Console
- Add photos to gallery.html, graduates.html, instructors.html
- Paste the dog aggression policy into policies.html
- Add real trial dates to trials.html
- Claim your Google Business Profile

## Rollback
If anything goes wrong, change the A record in cPanel back to the original
TotalChoice IP. Write that IP down before you change it.

---

## Removing the "new site" notice

Every page has a soft banner under the navigation reassuring returning
visitors they're in the right place. Leave it up for a few weeks, then remove it:

1. In each `.html` file, delete the block starting `<div class="notice">`
   and ending with the matching `</div></div>`
2. In `style.css`, delete the `.notice` block at the very bottom

Or search-and-replace across all files at once in a text editor like
Notepad++ or VS Code.

---

## Doggie Bytes balance page

Balances are looked up through a Google Apps Script web app backed by the club's
Doggie Bytes spreadsheet. The site embeds it on `doggie-bytes.html`.

Deployed URL currently in use:

```
https://script.google.com/macros/s/AKfycby4Uehu2LmTasqnrp2g_BDn2QpdWmUiceqpZ_4pyyKGltySbRVkBCd8tqyz6qpIyd_mYQ/exec
```

**Updating balances:** edit the spreadsheet. No redeployment needed. Changes appear
within about five minutes.

**Changing the URL:** if the script is ever redeployed and gets a new `/exec` URL,
search and replace it in `doggie-bytes.html`. It appears twice, once in the iframe
`src` and once in the fallback link.

**If the embed shows a blank box**, the script is refusing to be framed. In the Apps
Script editor, the `doGet` function needs:

```javascript
return HtmlService.createHtmlOutputFromFile('Index')
  .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);
```

Save, then **Deploy -> Manage deployments -> edit -> New version -> Deploy**. Keep
the same deployment so the URL doesn't change. Also confirm the deployment's "Who
has access" is set to **Anyone**.

The old PHP checker at `bytes.gooddog.org/bytes_balance.php` is no longer linked from
anywhere on the site. Leave the `bytes` DNS record alone until you've confirmed the
new lookup is working, then it can be retired.

**TotalChoice hosting still can't be cancelled** because club email lives there.

---

## Registration: closed for Fall 2026 (September 27, 2026)

All four levels are full and the Fall session has started, so registration is closed
sitewide. Team limits: Beginners 20, Advanced Beginners 16, Intermediate 12, Handlers 6.

What is currently in the closed state:

- **Ticker, every page** — `<div class="ticker closed">` reading "Registration closed".
- **`register.html`** — the Jotform embed has been removed and replaced with a closed
  notice and waiting-list instructions. The page and its URL stay live so search traffic
  and old links still land somewhere useful.
- **Class pages** — each of the four has a `<div class="fullnotice">` block, and hero
  buttons pointing at the waiting list instead of the form.
- **`index.html` / `classes.html`** — `<span class="fullflag">Full</span>` on all four
  cards and all four schedule rows (`class="rowfull"`), price-box buttons reading
  "Join the waiting list", and a closed sign-up section on `classes.html`.
- **Nav button and footer link** — both read "Waiting list".

### Reopening for the February session

1. **Put the form back on `register.html`.** Restore the `<h1>`, drop the closed notice and
   the waiting-list sections, and paste the embed back in:

   ```html
     <h1>Register for classes</h1>
     <p class="formnote">February 2027 &middot; 16 weeks &middot; $250 including club membership</p>
     <iframe
       id="jotform"
       title="February 2027 class registration form"
       src="https://pci.jotform.com/262375882204157"
       allow="geolocation; microphone; camera; fullscreen; payment"
       scrolling="no"
       frameborder="0"
       style="width:100%;border:0;display:block;height:3200px"></iframe>
     <script>
     /* Jotform posts its height as it renders; resize to match.
        If this script is blocked, the 3200px fallback above still shows the whole form. */
     window.addEventListener("message", function (e) {
       if (typeof e.data !== "string" || e.data.indexOf("setHeight") === -1) return;
       var h = parseInt(e.data.split(":")[1], 10);
       if (h > 0) document.getElementById("jotform").style.height = h + "px";
     });
     </script>
   ```

   Check with the secretary whether the Jotform is a new form for the February session. If
   it is, swap the ID in both the `src` and the footer link. Also set the Course schema
   `availability` back to `https://schema.org/InStock`, and update the title and meta
   description at the top of the page.

2. **Ticker, every page** — change `ticker closed` back to `ticker`, and the text back to
   `<strong>Registration open</strong>` with the new start date and a "Register now" link.
3. **Class pages** — delete each `<div class="fullnotice">` block and restore the hero
   buttons to
   `<a class="btn btn-a" href="register.html">Register</a><a class="btn btn-b" href="what-to-expect.html">What to bring</a>`
4. **`index.html` and `classes.html`** — remove `<span class="fullflag">Full</span>` from
   every card and schedule row, remove `class="rowfull"`, set the price-box buttons back to
   "Register now", and restore the sign-up section on `classes.html`.
5. **Nav and footer** — change "Waiting list" back to "Register".
6. **Class dates** — update the day, time and date ranges on the four class pages, both
   schedule tables, and the class cards.

To mark a single class full while registration stays open, copy the `fullnotice` block and
the `fullflag` / `rowfull` markers onto that class only, and name it in the ticker.

## Trial pages (added September 2026)

Every trial has its own permanent page in `/trials/`, named `YYYY-MM-usdaa-trial-city.html`
(regionals and team trials say so in the name). **Never delete one.** Old trial pages keep
earning search traffic, and the old WordPress URLs now redirect to them (see `_redirects`).

Trial PDFs live in `/trials/docs/YYYY/MM/`. Drop a new PDF into the folder for the month
you post it, then link to it from the trial page.

### Updating documents before a trial
Open the trial's page and find the `<ul class="docs">` list. Each row looks like this:

    <li><span class="dl">Running order</span><span class="pending">Posted a few days before the trial</span></li>

To post the document, replace the grey `<span class="pending">…</span>` with a link:

    <li><span class="dl">Running order</span><a href="docs/2026/11/GoodDogNovRunningOrder.pdf" target="_blank" rel="noopener">Running order (PDF)</a></li>

Same pattern for Trial information, Volunteer schedule, and Course maps & results.
Closing and move-up dates are in the `<dl class="facts">` block at the top of the page.
**Keep the dates identical everywhere:** page title, `<h1>`, facts block, the JSON-LD
`startDate`/`endDate`, and the premium.

### After a trial
1. On the trial page: change the eyebrow from `Upcoming trial` to `Past trial`, swap the
   intro sentence to the "This trial has finished…" wording used on older pages, delete the
   volunteer/Agility Buddy section, and add the results link.
2. On `trials.html`: move that trial's block from Upcoming into Past trials (newest first,
   under its year heading), add `past` to its class, and change its button to
   "Results and documents".

### Adding a new trial
Copy the most recent upcoming trial page, rename it with the new date, and update the title,
description, canonical URL, `<h1>`, facts, JSON-LD, the USDAA premium link, and the
previous/next links at the bottom. Add it to `trials.html` and to `sitemap.xml`.

When you add a USDAA online entry link to a trial page, keep the login reminder under it
(USDAA's entry link only works for people already signed in):

    <span class="docnote">Log in to your USDAA account first, or the link won't open the entry form.</span>

# RadLit Digest — Setup Guide

This builds you a free webpage that, once a month, lists new articles from your
journals, summarizes them, links you to the full article, and shows figure
thumbnails (for open-access articles) that open full-size when clicked.

The page has two sections, switchable at the top of the sidebar:

- **Articles** — a **sidebar** to pick a single journal or all of them together,
  **date-range buttons** (last month / 3 / 6 / 12 months / custom range), and — for
  open-access articles — a **"Detailed summary" button** that copies the article's
  full text into a ready-made prompt so you can paste it to Claude for a deeper
  write-up.
- **Videos** — recent uploads from your YouTube channels, automatically sorted
  **by topic** (with non-educational clips dropped into "Other"), each with a
  thumbnail and a link to watch.

Both sections have a **★ star button** on every item and a **★ Favorites** view
in the sidebar that collects everything you've starred.

The monthly job pulls a rolling year of articles and videos, so the date ranges
have data to show.

It uses only free services. No subscription, no credit card.

You'll do this once. After that, your monthly experience is just opening a webpage.

There are two journals to start: **AJNR** (subscription — you'll click through and
log in as normal) and **Neurographics** (open access — full-text summaries and figures).

---

## What you'll set up (about 30 minutes, one time)

1. A free GitHub account
2. A repository (a folder that holds the project) with the files I gave you
3. A free Gemini API key (this writes the summaries)
4. Turn on the automatic monthly schedule

Take it one numbered step at a time. You don't need to understand the code.

---

## Step 1 — Make a GitHub account

1. Go to https://github.com and click **Sign up**.
2. Use any email and pick a username. Choose the **Free** plan.
3. Verify your email when GitHub asks.

That's it. GitHub is just a place that stores your project files and runs the
monthly job for you, for free.

---

## Step 2 — Create the repository and add the files

1. Click the **+** in the top-right corner of GitHub → **New repository**.
2. Name it `radlit-digest`.
3. Choose **Public** (Public is required for free GitHub Pages hosting; nothing
   sensitive is stored here — your API key goes in a separate, hidden place in Step 4).
4. Click **Create repository**.

Now add the files. The easiest way:

1. On your new repository page, click **uploading an existing file** (it's a link
   in the middle of the page), or click **Add file → Upload files**.
2. Drag in these files I gave you:
   - `digest.py`
   - `journals.yaml`
3. Click **Commit changes** (green button).

The workflow file lives in a folder, so add it separately:

1. Click **Add file → Create new file**.
2. In the filename box, type exactly: `.github/workflows/digest.yml`
   (GitHub will turn the slashes into folders automatically as you type.)
3. Paste in the contents of the `digest.yml` file I gave you.
4. Click **Commit changes**.

---

## Step 3 — Get a free Gemini API key

1. Go to https://aistudio.google.com/apikey and sign in with a Google account.
2. Click **Create API key**.
3. Copy the long string it gives you. Keep it somewhere safe for the next step.

The free tier is generous and shows you the current daily limits in that console.
For two journals once a month, you'll stay well inside it. If a future busy month
ever exceeds the free limit, those articles simply show their abstract and link
(no summary), and you can have me summarize any of them on demand.

---

## Step 4 — Store your key safely (this is the secure part)

Your API key must NOT go in the code files. GitHub has a hidden vault for this
called "Secrets." Nothing here is visible to anyone, including in the public repo.

1. On your repository page, click **Settings** (top menu).
2. In the left sidebar: **Secrets and variables → Actions**.
3. Click **New repository secret**.
4. Name: `GEMINI_API_KEY`  — Value: paste your Gemini key. Click **Add secret**.

Optional but recommended (raises the PubMed rate limit, also free):

5. Get an NCBI key at https://account.ncbi.nlm.nih.gov → Account Settings → API Key Management.
6. Add another secret named `NCBI_API_KEY` with that value.
7. Add one more secret named `CONTACT_EMAIL` with your email (PubMed asks who's calling).

### For the Videos section — a free YouTube key

The video tracking needs its own free key. It's a couple more clicks than the
Gemini one because it lives in Google Cloud, but it's still free at this scale.

1. Go to https://console.cloud.google.com and sign in.
2. At the top, create a new project (any name, e.g. "radlit"). Wait a few seconds
   for it to be created, then make sure it's selected.
3. In the search bar at the top, type **YouTube Data API v3**, click it, and click
   **Enable**.
4. In the left menu go to **APIs & Services → Credentials**.
5. Click **Create credentials → API key**. Copy the key it gives you.
6. Back in your GitHub repository: **Settings → Secrets and variables → Actions →
   New repository secret**. Name it `YOUTUBE_API_KEY`, paste the key, save.

That's the only extra setup the videos need. (If you ever want to skip videos,
just don't add this key — the page will simply show only the Articles section.)

Also upload the `videos.yaml` file I gave you the same way you uploaded the others
(**Add file → Upload files**). It lists your channel and your topic list, both of
which you can edit later.

---

## Step 5 — Turn on the webpage (GitHub Pages)

1. **Settings → Pages** (left sidebar).
2. Under **Source**, choose **GitHub Actions**.

That's all. The first time the job runs it will publish your page.

---

## Step 6 — Run it once, right now, to test

1. Click the **Actions** tab (top menu).
2. If asked, click the green button to enable workflows.
3. Click **Monthly RadLit Digest** on the left → **Run workflow** → **Run workflow**.
4. Wait 1–3 minutes. A green check means success.
5. Go back to **Settings → Pages** — your webpage address is shown there
   (looks like `https://YOURNAME.github.io/radlit-digest/`).
6. Open that link on your PC and your phone. Bookmark it. Add it to your phone's
   home screen if you like — it behaves like an app.

From now on it refreshes automatically on the 1st of every month. You never have
to touch GitHub again unless you want to add journals.

---

## Adding more journals later

1. On your repository, open `journals.yaml` and click the pencil icon to edit.
2. Copy one of the existing journal blocks, change the `name`, `pubmed_ta`, and
   `open_access` line. The `pubmed_ta` is the official PubMed title abbreviation;
   find it at https://www.ncbi.nlm.nih.gov/nlmcatalog by searching the journal.
3. Click **Commit changes**. Done.

---

## A few honest notes

- **The first run takes a few minutes longer** than you might expect, because it
  pulls a full year of articles so your date ranges have data. That's normal.
- **The "Detailed summary" button (open-access articles only)** copies a prompt —
  including the article's full text — to your clipboard. Paste it into a Claude
  chat and you'll get a detailed, teaching-focused summary on demand, for free.
  Subscription articles don't have this button because only their abstract is
  freely available; for those, open the article and read the full text directly.
- **To stay inside the Gemini free tier**, the job auto-summarizes up to a set
  number of articles per run (you can change `max_summaries_per_run` in
  `journals.yaml`). Any beyond that still appear with their link; use the detailed
  button or open them directly.
- **Summaries are AI-generated.** They're a fast triage aid, not a substitute for
  reading the article. Verify anything before clinical use.
- **Videos are organized, not summarized.** The tool sorts videos by topic from
  their title and description and links you to watch them — it doesn't watch or
  summarize the video content itself (there's no reliable free way to do that).
  Topic labels are AI-assigned and occasionally land a video in the wrong bucket;
  the star button lets you keep the good ones regardless.
- **Editing your topic list or channels:** open `videos.yaml` and use the pencil
  icon, same as journals. Add a channel by its @handle; add or rename topics in
  the list. The classifier files videos into whatever topics you define.
- **Favorites** are saved in the browser you're using. Right now they're per-device
  (your phone and PC keep separate lists). Cross-device sync is a planned upgrade.
- **AJNR (subscription):** the summary is built from the freely available abstract,
  and the "Open full article" button takes you to the article where your normal
  AJNR / ClinicalKey login gives you the full text and all figures. The tool never
  logs in for you or copies paywalled content — that keeps you fully within the
  publishers' terms.
- **Neurographics (open access):** full-text summaries and figure thumbnails work
  because that content is openly and legitimately available through PubMed Central.
- **Figures** only display when the article is open access and PubMed Central hosts
  them. For subscription articles, view figures on the publisher page after logging in.
- If a monthly run ever fails (e.g., a service was down), just go to the Actions
  tab and click **Run workflow** to retry. Nothing breaks.

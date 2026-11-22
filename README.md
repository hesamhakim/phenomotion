# The public site, and why it exists

Three static pages: a home page, a privacy policy and terms. They exist because Google will not let an
**External** OAuth app leave *Testing* without a home page, a privacy policy URL and a matching authorized
domain - a console behaviour since about August 2026 that catches personal, single-user apps like this one.
Staying in Testing was the alternative, and it expires the refresh token every 7 days.

Nothing here is decorative: the privacy policy is the one Google's reviewers and the consent screen point
at, so it says exactly what the uploader touches and nothing it does not.

## Done (2026-11-21)

- Public repository **`hesamhakim/phenomotion`** created, these pages pushed to it, GitHub Pages enabled
  and serving:
  - <https://hesamhakim.github.io/phenomotion/>
  - <https://hesamhakim.github.io/phenomotion/privacy.html>
  - <https://hesamhakim.github.io/phenomotion/terms.html>
- Commits use `hesamhakim@users.noreply.github.com` rather than the real address, since commit metadata
  on a public repository is readable by anyone; GitHub still attributes them to the account.
- **The user-site root is not this project.** `hesamhakim.github.io/` returns 404. The repository of that
  name was emptied of these pages and now holds only a `.nojekyll` and a README; it exists so that
  Search Console's verification file can be served from the domain root, which is where Google looks.

This folder is its own git repository, separate from the research one - the site is public and the
research repo is not. `site/.git` is ignored by the outer repository.

## Your turn - the parts that need your Google account

**1. Verify the domain in Search Console.** Go to <https://search.google.com/search-console>, signed in as
the account that owns the Cloud project. Add a property of type **URL prefix**: `https://hesamhakim.github.io/`.
Choose either method and hand me the result - I can push it in seconds:

- **HTML file**: it offers a file like `google1a2b3c4d5e.html`. Tell me its name and contents and I will
  push it to the **`hesamhakim.github.io`** repository, so it serves from the domain root where Google
  looks for it; then click Verify.
- **HTML tag**: it gives a `<meta name="google-site-verification" content="...">`. That only works if the
  property is the subdirectory `https://hesamhakim.github.io/phenomotion/`, since the tag has to be in the
  page Google fetches. Paste it to me either way and I will put it where it belongs.

Verify the **domain-root** property (`https://hesamhakim.github.io/`) if Google gives you the choice -
that is the form the Authorized domains field wants.

**2. Fill the Branding page** (Google Auth Platform → Branding):

| Field | Value |
|---|---|
| Application home page | `https://hesamhakim.github.io/phenomotion/` |
| Application privacy policy link | `https://hesamhakim.github.io/phenomotion/privacy.html` |
| Application terms of service link | `https://hesamhakim.github.io/phenomotion/terms.html` |
| Authorized domains → Add domain | `hesamhakim.github.io` |

Leave the **App logo** empty - uploading one forces the app into verification, which is not otherwise
needed. Save.

**3. Audience → Publish app.** The button should now be active. Afterwards the consent screen shows
"Google hasn't verified this app"; **Advanced → Go to … (unsafe)** is the expected path for your own app,
and refresh tokens stop expiring after seven days.

**4. Run the login** (the Mac acts only as a browser; the token lands on the HPC):

```bash
pip install google-auth-oauthlib
cd /scratch/g/he301766/projects/PhenoMotion_visualization_ecosystem_for_biology/phenomotion
python tools/youtube_login_browser.py ~/Downloads/client_secret_desktop.json
```

Then on the HPC, `mv phenomotion/tools/youtube_token.json ~/.config/phenomotion/youtube_token.json`.

## One thing you may want to change

`privacy.html` ends by pointing at the consent screen's contact address rather than printing an email on a
public page, where it would be scraped. If you would rather show one, say so and I will add it - there is
an HTML comment marking the spot.

## Changing the pages later

```bash
cd /scratch/g/he301766/projects/PhenoMotion_visualization_ecosystem_for_biology/site
# edit, then
git add -A && git commit -m "..." && git push   # remote: hesamhakim/phenomotion
```

Pages rebuilds within a minute or so.

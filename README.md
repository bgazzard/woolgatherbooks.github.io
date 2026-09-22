# GitHub Pages redirect for the Etsy OAuth

Etsy needs the redirect URI to be **https on a real domain** — no localhost, no IP
addresses. A free GitHub Pages site gives you one that is genuinely yours, so the
authorisation code never passes through anyone else's server.

## Setup, once

1. **Create a GitHub account** if you do not have one. The username becomes part of the
   domain, so pick something you are happy with: `woolgatherbooks` would give
   `woolgatherbooks.github.io`.

2. **Create a new PUBLIC repository named exactly `<username>.github.io`.**
   That exact name is what makes it a user site — any other name serves from a
   subdirectory instead. Tick "Add a README" so the repo is not empty.

3. **Add this page** at the path `oauth/redirect/index.html`:
   - In the repo, click **Add file → Create new file**
   - Type `oauth/redirect/index.html` as the filename (the slashes create the folders)
   - Paste the contents of `index.html` from this folder
   - Commit

4. **Wait a minute**, then check it loads:
   `https://<username>.github.io/oauth/redirect`
   It should show an "Authorisation code" card saying no code is present yet.

5. **Register that URL** as a redirect URI on your Etsy app, exactly:
   `https://<username>.github.io/oauth/redirect`

## Then authorise

    python scripts/etsy_auth.py --key <keystring> --redirect https://<username>.github.io/oauth/redirect

Approve in the browser. Etsy sends you back to your own page, which displays the code with
a copy button. Paste the full URL into the terminal when asked.

## Why a page rather than a 404

The flow works with any registered https URL — the code is in the address bar either way.
This page just makes it readable, with "copy full URL" and "copy code only" buttons, and it
tells you plainly if Etsy returned an error instead.

Nothing secret is exposed by hosting it publicly: the page is static, it only reads the
query string in the visitor's own browser, and the code is single-use, expires in minutes,
and is useless without the PKCE verifier, which never leaves your machine.

# mwololo-space

Static pages that satisfy Google's OAuth consent screen requirements for the personal
`mwololo-backup` tool, so the Google Drive backup remote can run on a self-owned
`client_id` instead of rclone's shared one (retired during 2026).

## Files

| file | served at | purpose |
|---|---|---|
| `public/index.html` | `/` | Home page: what the app is, who uses it, which scope it requests |
| `public/privacy.html` | `/privacy.html` | Privacy policy incl. the Google API Limited Use statement |
| `public/terms.html` | `/terms.html` | Terms of service |
| `public/style.css` | `/style.css` | One shared stylesheet, no external assets, no trackers |
| `public/robots.txt` | `/robots.txt` | Allows indexing; the pages must be publicly reachable |

Only `public/` is published. Keeping the pages in a subdirectory means a direct
upload can never push `.git/` to the web host.

## Why these pages exist

Google requires, for an external app published to *In production*:

1. an **Application home page** hosted on a domain the owner has verified;
2. an **Application privacy policy link** on that same domain, linked from the home page;
3. an **Application terms of service link**;
4. the domain registered under **Authorized domains** *before* the URLs are pasted.

The app name on the consent screen and `<h1>` here must match. Today both read
`mwololo-backup`.

## Deploy (Cloudflare Pages)

The `mwololo.space` zone is on Cloudflare (`bart.ns.cloudflare.com`,
`fay.ns.cloudflare.com`), so Pages can attach the apex and create the DNS record
itself. `public/` is the build output directory in both routes below.

**Route A — direct upload.** Used for the first deployment, and for any edit that
should not wait for a Git build. The API token lives outside the repository, at
`~/.config/cloudflare/token` (mode 600), and is never committed.

```
CLOUDFLARE_API_TOKEN=$(cat ~/.config/cloudflare/token) \
  wrangler pages deploy public --project-name mwololo-space --branch main
```

**Route B — Git-connected Pages.** Dashboard → Workers & Pages → Create → Pages →
Connect to Git → pick this repository. Framework preset: **None**. Build command:
empty. Build output directory: `public`.

Either route ends the same way: Custom domains → Set up a domain → `mwololo.space`,
then confirm `https://mwololo.space/`, `/privacy.html` and `/terms.html` return 200.

Route A needs `wrangler` (`npm i -g wrangler`). Route B needs nothing local; the
site is plain static files.

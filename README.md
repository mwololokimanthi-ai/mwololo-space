# mwololo-space

Static pages that satisfy Google's OAuth consent screen requirements for the personal
`mwololo-backup` tool, so the Google Drive backup remote can run on a self-owned
`client_id` instead of rclone's shared one (retired during 2026).

## Files

| file | served at | purpose |
|---|---|---|
| `index.html` | `/` | Home page: what the app is, who uses it, which scope it requests |
| `privacy.html` | `/privacy.html` | Privacy policy incl. the Google API Limited Use statement |
| `terms.html` | `/terms.html` | Terms of service |
| `style.css` | `/style.css` | One shared stylesheet, no external assets, no trackers |

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
`fay.ns.cloudflare.com`), so Pages can attach the apex and create the DNS record itself.

1. Cloudflare dashboard → Workers & Pages → Create → Pages → Connect to Git.
2. Pick this repository. Framework preset: **None**. Build command: empty.
   Build output directory: `/`.
3. Deploy, then Custom domains → Set up a domain → `mwololo.space`.
4. Confirm `https://mwololo.space/`, `/privacy.html` and `/terms.html` return 200.

Node is not needed; the site is plain static files.

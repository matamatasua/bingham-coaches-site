# Bingham MTB Team Contacts

One-scan contact picker for the Bingham Mountain Bike coaching staff and team leadership.

## What's in here

```
/
  index.html          Landing page with 6 contact buttons
  logo.png            Cleaned Bingham MTB chainring logo
  _headers            Cloudflare Pages MIME type config
  vcf/
    rodney-tupai.vcf
    matt-michaelis.vcf
    danielle-tupai.vcf
    melissa-darby.vcf
    brooke-haas.vcf
    ashley-thomas.vcf
    all-coaches.vcf
```

## Deploy to Cloudflare Pages

This project is intended to deploy as its own Cloudflare Pages project at a subdomain of mockletdesign.com (for example, `coaches.mockletdesign.com`). Keeping it separate from the main React site avoids SPA routing conflicts.

Steps:

1. Push this repo to GitHub
2. Cloudflare dashboard, Workers and Pages, Create project, Connect to Git
3. Build settings: no build command, output directory `/` (the whole repo)
4. Deploy
5. Add custom domain: `coaches.mockletdesign.com` (or whatever subdomain you choose)
6. In Cloudflare DNS, add a CNAME record pointing the subdomain to the Pages project hostname

## How the _headers file works

Cloudflare Pages reads `_headers` at the site root and applies the rules on every request. The config sets `Content-Type: text/vcard; charset=utf-8` for all `.vcf` files. That tells iOS and Android "this is a contact," so phones respond with the native Add Contact prompt instead of a generic download.

## Why no download attribute on the links

The HTML uses plain `<a href="...vcf">` without `download=`. That is intentional. With the right MIME type in place, phones handle the file directly. iOS Safari shows the Add Contact preview, Android hands off to the Contacts app after downloading. Adding `download=` would force disk save and hide the native contact prompt.

## Android behavior note

Android Chrome will still download the .vcf file (Chrome does not expose a contact handler the way Safari does). After scanning the QR and tapping a coach, the Android user taps the download notification, picks the Contacts app, and taps Import. This is the best we can do without a native app. iOS users get the one-tap experience.

## Testing checklist

1. Deploy to Pages
2. Visit the live URL on desktop, confirm page loads and looks right
3. On iPhone, tap a contact card. Expect native Add Contact preview
4. On Android, tap a contact card. Expect download notification, then Import via Contacts app
5. Generate QR code from the page URL, test the full scan flow on both platforms

## Updating contacts later

Edit the matching file in `vcf/`, update the displayed number in `index.html`, redeploy. No QR code regeneration needed because the QR points to the page, not the files.

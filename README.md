# Icon Packs - Wallpapers

A collection of wallpapers for icon pack apps, hosted via GitHub raw URLs.

## Structure

- ios_icons/ — iOS wallpapers (255 images, optimized as WebP)
  - wallpapers.json — Manifest with GitHub raw URLs
  - wallpapers/ — Optimized WebP images

- pixel_icons/ — Google Pixel wallpapers (313 images, optimized as WebP)
  - wallpapers.json — Manifest with GitHub raw URLs
  - wallpapers/ — Optimized WebP images

- samsung_icons/ — Samsung Galaxy wallpapers (251 images, optimized as WebP)
  - wallpapers.json — Manifest with GitHub raw URLs
  - wallpapers/ — Optimized WebP images

## Adding a new icon pack

1. Create a new folder (e.g., oneplus_icons/)
2. Use the WALLPAPER_HOSTING_GUIDE.md for the full process
3. Convert images to WebP and generate wallpapers.json
4. Commit and push to GitHub

## App URLs

Each icon pack has its own wallpapers.json for your app:

- iOS: https://raw.githubusercontent.com/samshady/icon-packs-wallpapers/main/ios_icons/wallpapers.json
- Pixel: https://raw.githubusercontent.com/samshady/icon-packs-wallpapers/main/pixel_icons/wallpapers.json
- Samsung: https://raw.githubusercontent.com/samshady/icon-packs-wallpapers/main/samsung_icons/wallpapers.json

## Deployment

After pushing to GitHub, files are served via raw.githubusercontent.com (no size limits, no CDN throttling).

Total: 819 wallpapers across 3 icon packs

# Icon Packs - Wallpapers

A collection of wallpapers for icon pack apps, hosted via GitHub + jsDelivr CDN.

## Structure

- ios_icons/ — iOS wallpapers (275 images, optimized as WebP)
  - wallpapers.json — Manifest with jsDelivr URLs
  - wallpapers/ — Optimized WebP images

## Adding a new icon pack

1. Create a new folder (e.g., android_icons/)
2. Add a wallpapers/ subfolder with your WebP images
3. Create a wallpapers.json with entries formatted like:
   {
     "name": "Wallpaper Name",
     "author": "Author",
     "url": "https://cdn.jsdelivr.net/gh/USERNAME/REPO@main/PACK_NAME/wallpapers/FILENAME.webp",
     "thumbnail": "https://cdn.jsdelivr.net/gh/USERNAME/REPO@main/PACK_NAME/wallpapers/FILENAME.webp",
     "copyright": "License"
   }
4. Update your app's json_url to point to the new manifest

## Deployment

After pushing to GitHub, files are served via:
https://cdn.jsdelivr.net/gh/USERNAME/REPO@main/PATH

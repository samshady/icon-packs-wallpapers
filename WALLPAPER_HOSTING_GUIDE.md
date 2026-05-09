# Wallpaper Hosting Setup Guide

A step-by-step guide to preparing and hosting wallpaper collections for icon packs via GitHub raw URLs.

## Prerequisites

- A GitHub account with SSH key configured
- ~/.ssh/config set up with your host alias (e.g., github-samshady)
- Python 3 with Pillow installed (pip install Pillow)
- A folder of wallpaper images (JPG/PNG)

---

## Step 1: Folder Structure

In your main repo (e.g., icon-packs-wallpapers), create:

- PACK_NAME/ (e.g., ios_icons/)
  - wallpapers/ (place your raw JPG/PNG images here)
  - wallpapers.json (will be generated later)

---

## Step 2: Convert Images to WebP & Generate JSON

Run this Python script from the repo root. It will:
1. Read all images from PACK_NAME/wallpapers/
2. Convert them to WebP format (removes originals)
3. Generate wallpapers.json with GitHub raw URLs

Create a file named convert.py with this content (or run this from a Python shell):

import os
import json
from PIL import Image
from collections import Counter

PACK = "PACK_NAME"  # Change this to your folder name
USER = "samshady"   # Change to your GitHub username
REPO = "icon-packs-wallpapers"  # Change to your repo name

wall_dir = f"{PACK}/wallpapers"
manifest = []

for fname in sorted(os.listdir(wall_dir)):
    if fname.startswith('.') or not fname.lower().endswith(('.jpg', '.jpeg', '.png', '.webp', '.gif')):
        continue

    name_no_ext = os.path.splitext(fname)[0]
    src_path = os.path.join(wall_dir, fname)

    # Clean filename: remove parentheses, spaces -> underscores
    clean_name = name_no_ext.replace("(", "").replace(")", "").replace(" ", "_")

    img = Image.open(src_path)
    if img.mode in ('RGBA', 'LA', 'P'):
        img = img.convert('RGBA')
    elif img.mode not in ('RGB',):
        img = img.convert('RGB')

    webp_path = os.path.join(wall_dir, f"{clean_name}.webp")
    counter = 1
    while os.path.exists(webp_path):
        webp_path = os.path.join(wall_dir, f"{clean_name}_{counter}.webp")
        counter += 1

    img.save(webp_path, 'WEBP', quality=85)
    os.remove(src_path)

    webp_name = os.path.basename(webp_path)
    old_size = os.path.getsize(src_path) if os.path.exists(src_path) else 0
    new_size = os.path.getsize(webp_path)

    entry = {
        "name": os.path.splitext(webp_name)[0],
        "author": "Author",
        "url": f"https://raw.githubusercontent.com/{USER}/{REPO}/main/{PACK}/wallpapers/{webp_name}",
        "thumbnail": f"https://raw.githubusercontent.com/{USER}/{REPO}/main/{PACK}/wallpapers/{webp_name}",
        "copyright": "License"
    }
    manifest.append(entry)
    print(f"Converted: {fname} -> {webp_name} ({new_size/1024:.0f} KB)")

with open(f"{PACK}/wallpapers.json", "w") as f:
    json.dump(manifest, f, indent=2)

print(f"Done - {len(manifest)} wallpapers processed")

---

## Step 3: Commit and Push to GitHub

cd /path/to/icon-packs-wallpapers
git add PACK_NAME/
git commit -m "Add PACK_NAME wallpapers"
git push

---

## Step 4: Update Your App

In your Android Studio project (frames_setup.xml or equivalent), set:

json_url = https://raw.githubusercontent.com/USERNAME/REPO/main/PACK_NAME/wallpapers.json

---

## Step 5: Verify

Test the URL in your browser:

https://raw.githubusercontent.com/USERNAME/REPO/main/PACK_NAME/wallpapers.json

The JSON should load as plain text. Test a wallpaper URL directly to confirm it returns the image.

---

## Important Notes

- GitHub raw URLs have no file size limit (unlike jsDelivr's 50 MB per-file cap)
- If your total repo exceeds ~1 GB, consider splitting into separate repos
- Always make your GitHub repo Public (raw URLs won't work on private repos)
- WebP format saves 60-90% space vs JPG/PNG with minimal quality loss

## Troubleshooting

### SSH Permission Denied

Check your ~/.ssh/config has the right host alias and fix key permissions:

chmod 600 ~/.ssh/YOUR_KEY_FILE
ssh -T git@YOUR_HOST_ALIAS

### 404 on wallpaper URLs

The JSON must exactly match the filenames. Re-run the conversion script to rebuild the JSON.

### Duplicate wallpaper names

The script automatically renames duplicates by adding _1, _2, etc. to avoid overwrites.

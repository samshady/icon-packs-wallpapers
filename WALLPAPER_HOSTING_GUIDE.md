# Wallpaper Hosting Setup Guide

A step-by-step guide to preparing and hosting wallpaper collections for icon packs via GitHub + jsDelivr CDN.

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
2. Convert them to WebP format
3. Generate wallpapers.json with jsDelivr CDN URLs

Create a file named convert.py with this content:

import os
import json
from PIL import Image

PACK = "PACK_NAME"  # Change this to your folder name
USER = "samshady"   # Change to your GitHub username
REPO = "icon-packs-wallpapers"  # Change to your repo name

src_dir = f"{PACK}/wallpapers"
dst_dir = f"{PACK}/wallpapers"
manifest = []

for fname in os.listdir(src_dir):
    if not fname.lower().endswith(('.jpg', '.jpeg', '.png', '.webp')):
        continue
    
    name = os.path.splitext(fname)[0]
    src_path = os.path.join(src_dir, fname)
    
    try:
        img = Image.open(src_path)
        if img.mode in ('RGBA', 'LA'):
            img = img.convert('RGBA')
        elif img.mode != 'RGB':
            img = img.convert('RGB')
        
        webp_name = f"{name}.webp"
        dst_path = os.path.join(dst_dir, webp_name)
        img.save(dst_path, 'WEBP', quality=85)
        
        entry = {
            "name": name,
            "author": "Author",
            "url": f"https://cdn.jsdelivr.net/gh/{USER}/{REPO}@main/{PACK}/wallpapers/{webp_name}",
            "thumbnail": f"https://cdn.jsdelivr.net/gh/{USER}/{REPO}@main/{PACK}/wallpapers/{webp_name}",
            "copyright": "License"
        }
        manifest.append(entry)
        print(f"Converted: {fname}")
    except Exception as e:
        print(f"Error: {fname} - {e}")

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

json_url = https://cdn.jsdelivr.net/gh/USERNAME/REPO@main/PACK_NAME/wallpapers.json

---

## Step 5: Verify

Wait 1-2 minutes for jsDelivr to cache, then test the URL in your browser. The JSON should load as plain text showing all wallpaper entries.

---

## Troubleshooting

### SSH Permission Denied

Check your ~/.ssh/config has the right host alias and run:

chmod 600 ~/.ssh/YOUR_KEY_FILE
ssh -T git@YOUR_HOST_ALIAS

### jsDelivr not loading

Make sure your GitHub repo is set to Public. jsDelivr can only serve public repos.

### Duplicate wallpaper names

If you have multiple files with the same name (e.g., two different Unsplash images), the script will overwrite them. Rename files to be unique before running.

# Tutorial: Professional Wallpaper Hosting (GitHub + jsDelivr)

This guide shows you exactly how to move away from Google Drive to a faster, more reliable hosting method for your icon pack wallpapers.

## 1. Create the GitHub Repository
1. Log in to [GitHub](https://github.com/).
2. Click **New** to create a repository.
3. Name it (e.g., `kimono-wallpapers`).
4. **IMPORTANT**: Make sure it is set to **Public**.
5. Click **Create repository**.

## 2. Upload your Wallpapers
1. In your new repository, click **Add file** > **Upload files**.
2. Upload all your wallpaper images (e.g., `iphone_blue.webp`, `abstract_red.jpg`).
3. Click **Commit changes**.

## 3. Create the `wallpapers.json`
Create a file named `wallpapers.json` in that same repository. Here is exactly how it should look:

```json
[
  {
    "name": "iPhone Blue",
    "author": "Kimono",
    "url": "https://cdn.jsdelivr.net/gh/USERNAME/REPO@main/iphone_blue.webp",
    "thumbnail": "https://cdn.jsdelivr.net/gh/USERNAME/REPO@main/iphone_blue.webp"
  },
  {
    "name": "Abstract Red",
    "author": "Kimono",
    "url": "https://cdn.jsdelivr.net/gh/USERNAME/REPO@main/abstract_red.jpg",
    "thumbnail": "https://cdn.jsdelivr.net/gh/USERNAME/REPO@main/abstract_red.jpg"
  }
]
```

## 4. How to get the "Raw" Link (The jsDelivr Magic)
You **never** use the link in your browser's address bar. Instead, you "construct" it using this formula:

**Formula:** `https://cdn.jsdelivr.net/gh/YOUR_GITHUB_USER/YOUR_REPO_NAME@main/FILENAME`

### Example:
- **Your User**: `kimonodesign`
- **Your Repo**: `kimono-wallpapers`
- **File**: `wallpapers.json`
- **Resulting Link**: `https://cdn.jsdelivr.net/gh/kimonodesign/kimono-wallpapers@main/wallpapers.json`

## 5. Put the link in Android Studio
Go to `app/src/main/res/values/frames_setup.xml` and update the `json_url`:

```xml
<string name="json_url">https://cdn.jsdelivr.net/gh/kimonodesign/kimono-wallpapers@main/wallpapers.json</string>
```

---

## Why this works:
- **GitHub** acts as your hard drive (storing the files).
- **jsDelivr** acts as your delivery truck (it takes your GitHub files and provides a "Raw" link that is optimized for apps).
- **The App** sees a perfect, direct link to the data and loads it instantly without any "Google Drive" security warnings or throttling.

# 🚀 Cloudflare R2 CDN Sync

This repository automatically syncs static assets from the `cdn/` folder to a [Cloudflare R2](https://developers.cloudflare.com/r2/) bucket and serves them through a custom domain (e.g. `https://cdn.fiqry.dev`).

## ✨ Features

- 📂 Store static assets in the `cdn/` directory of this repo
- 🔄 GitHub Actions automatically sync changes to Cloudflare R2
- 🌍 Files are served globally via Cloudflare CDN at your custom domain
- ⚡ Immutable cache headers for long-lived assets
- 🛠 Easy to extend with image resizing or versioned paths

## 📁 Repository Structure

```text
.
├── cdn/                  # Place all your assets here
│   ├── images/
│   │   └── example.png
│   └── logos/
│       └── logo.svg
└── .github/
    └── workflows/
        └── cloudflare-r2-workflows.yml # CI/CD sync workflow

```

## 🔑 Setup

### 1. Create R2 Bucket
- Go to Cloudflare Dashboard → **R2 → Buckets → Create bucket**
- Example: `assets`

### 2. Attach Custom Domain
- Go to **Bucket → Settings → Custom Domains → Add**
- Example: `cdn.fiqry.dev`

### 3. Generate API Keys
- **R2 → Manage R2 API Tokens → Create API Token**
- Save:
  - `Account ID`
  - `Access Key ID`
  - `Secret Access Key`

### 4. Add GitHub Secrets
In your repo → **Settings → Secrets and variables → Actions**:

- `R2_ACCOUNT_ID`
- `R2_ACCESS_KEY_ID`
- `R2_SECRET_ACCESS_KEY`

## ⚙️ Workflow

The GitHub Action `.github/workflows/cloudflare-r2-workflows.yml`:

- Triggers on push to files in `cdn/**`
- Syncs assets to R2
- Optionally sets immutable cache headers for images

Example:

```yaml
aws s3 sync cdn s3://$R2_BUCKET \
  --endpoint-url https://$R2_ACCOUNT_ID.r2.cloudflarestorage.com \
  --delete
````

## 🌐 Accessing Files

Your assets are accessible via your custom domain:

```
https://cdn.fiqry.dev/images/example.png
```

## 🧰 Development Notes

* Use hashed filenames (e.g., `logo.ab12cd.svg`) for safe immutable caching
* For mutable files, configure shorter `Cache-Control`
* Optional: Add a Worker in front of R2 for [Image Resizing](https://developers.cloudflare.com/images/image-resizing/) or authentication

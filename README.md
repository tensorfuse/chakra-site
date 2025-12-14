# CHAKRA Manifesto

> Software is a language now.

## Local Development

```bash
# Install dependencies
npm install

# Watch for changes (development)
npm run watch:css

# Build for production
npm run build
```

## Deploy to GitHub Pages

### Option 1: Deploy `/dist` folder (Recommended)

1. **Create a GitHub repo** and push this project:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/chakra-manifesto.git
   git push -u origin main
   ```

2. **Go to your repo Settings → Pages**

3. **Under "Build and deployment":**
   - Source: **Deploy from a branch**
   - Branch: **main**
   - Folder: **/dist**

4. **Click Save** — your site will be live at `https://YOUR_USERNAME.github.io/chakra-manifesto/`

### Option 2: Use GitHub Actions (Auto-build)

1. Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build
        run: npm run build
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

2. Go to **Settings → Pages → Source** and select **GitHub Actions**

3. Push to main — it auto-deploys!

## Project Structure

```
chakra-site/
├── src/
│   ├── index.html      # Source HTML
│   └── input.css       # Tailwind + custom CSS
├── dist/               # Built files (deploy this)
│   ├── index.html
│   └── output.css
├── package.json
├── tailwind.config.js
└── README.md
```

## No More CDN Warning ✓

This project uses the Tailwind CLI to compile CSS at build time, eliminating the `cdn.tailwindcss.com should not be used in production` warning.

---

*The weird inherit the feed.*

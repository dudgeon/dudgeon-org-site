# dudgeon.org

Minimal static site hosted on GitHub Pages with custom domain.

## Project Files

- **README.md** (this file) - Setup guide and usage instructions
- **TASKS.md** - Project task tracking and backlog
- **claude.md** - Technical context for Claude Code (project conventions, architecture notes)

## Structure

```
public/               # Published root (served at dudgeon.org)
├── index.html       # Homepage
├── assets/          # Shared CSS, images, etc.
│   └── style.css
└── tools/           # Tools section
    └── index.html   # Tools landing page
```

## Adding a New Tool

### Option 1: Folder in This Repo

To add a tool directly in this repository:

1. Navigate to `public/tools/` in GitHub
2. Click "Add file" → "Create new file"
3. Name it `your-tool-name/index.html`
4. Add your tool's HTML (link to `/assets/style.css` or use inline styles)
5. Commit to main
6. Update `public/tools/index.html` to add a link to your new tool

Example structure:
```
public/tools/
├── index.html           # Landing page listing all tools
├── example-tool/
│   └── index.html
└── your-tool-name/
    ├── index.html
    ├── script.js        # Optional
    └── styles.css       # Optional tool-specific styles
```

### Option 2: Pull from Separate Repos (Future)

For tools maintained in separate repositories, you can use GitHub Actions to automatically pull their built output during deployment.

**Approach:**

1. Each tool repo has its own build process outputting to `/dist` or `/public`
2. Modify `.github/workflows/deploy.yml` to:
   - Checkout this repo
   - Checkout tool repos into temporary directories
   - Copy tool artifacts into `public/tools/<tool-name>/`
   - Upload combined `public/` folder to Pages

**Example workflow addition:**

```yaml
- name: Checkout tool repos
  run: |
    git clone https://github.com/dudgeon/tool-alpha.git /tmp/tool-alpha
    git clone https://github.com/dudgeon/tool-beta.git /tmp/tool-beta

- name: Copy tool artifacts
  run: |
    mkdir -p public/tools/tool-alpha
    cp -r /tmp/tool-alpha/dist/* public/tools/tool-alpha/
    mkdir -p public/tools/tool-beta
    cp -r /tmp/tool-beta/dist/* public/tools/tool-beta/
```

**Benefits:**
- Each tool can have its own repo, issues, PRs, versioning
- Tools can use different frameworks/build processes
- Main site repo stays clean and minimal
- CI/CD tests each tool independently before integrating

**Note:** This requires build steps and is NOT needed for simple static tools. Start with Option 1 and migrate tools to separate repos only when they become complex enough to warrant it.

## Local Development

Since this is pure static HTML/CSS/JS with no build step:

1. Clone the repo
2. Open `public/index.html` in your browser
3. Or use any static server: `python -m http.server 8000` from `/public`

## Custom Domain Setup

See DNS configuration section below. The `public/CNAME` file tells GitHub Pages to serve this site at dudgeon.org.

## DNS Configuration

**A Records (Apex domain):**
```
@ → 185.199.108.153
@ → 185.199.109.153
@ → 185.199.110.153
@ → 185.199.111.153
```

**CNAME Record (www subdomain):**
```
www → dudgeon.github.io.
```

**Important:** Do not modify existing MX or TXT records for Google Workspace email.

## GitHub Pages Setup (Phone-Friendly Instructions)

### Enable GitHub Pages

1. In your repo, tap "Settings" → "Pages"
2. Under "Build and deployment":
   - Source: Select "GitHub Actions"
3. Under "Custom domain":
   - Enter: `dudgeon.org`
   - Tap "Save"
4. Once DNS is verified, check "Enforce HTTPS"

### Configure DNS Records

⚠️ **CRITICAL:** Only ADD the records below. Do NOT delete or modify existing MX, TXT, or SPF records for Google Workspace.

In your domain registrar's DNS settings:

1. Add 4 A records for apex (@):
   - @ → 185.199.108.153
   - @ → 185.199.109.153
   - @ → 185.199.110.153
   - @ → 185.199.111.153

2. Add CNAME for www:
   - www → dudgeon.github.io.

DNS propagation can take up to 48 hours. GitHub will automatically redirect www to the apex domain.

## Deployment

Changes pushed to the `main` branch automatically deploy via GitHub Actions. Check the "Actions" tab to monitor deployment status.

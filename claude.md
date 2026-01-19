# Claude Context: dudgeon.org Site

## Project Overview

This is a minimal static site for dudgeon.org hosted on GitHub Pages. The site serves as a personal homepage and a hub for hosting various web-based tools.

**Live URL**: https://dudgeon.org (once DNS is configured)
**Repo**: https://github.com/dudgeon/dudgeon-org-site

## Key Constraints & Requirements

### DNS & Email
- **CRITICAL**: dudgeon.org uses Google Workspace for email
- When modifying DNS, NEVER touch existing MX, TXT, or SPF records
- Only add/modify web-related A and CNAME records
- Custom domain: apex (dudgeon.org) is canonical; www redirects to apex

### Deployment
- GitHub Pages with GitHub Actions as source
- Deploys automatically on push to `main` branch
- Publishes from `/public` folder only (keeps repo files private)
- No build step required (pure static HTML/CSS/JS)

## Project Structure

```
dudgeon-org-site/
├── .github/workflows/
│   └── deploy.yml           # GitHub Actions: deploys /public to Pages
├── public/                  # PUBLISH ROOT - everything here is public
│   ├── .nojekyll           # Prevents Jekyll processing
│   ├── CNAME               # Contains: dudgeon.org
│   ├── index.html          # Homepage (hello world + link to tools)
│   ├── assets/
│   │   └── style.css       # Shared minimal styling
│   └── tools/
│       └── index.html      # Tools landing page
├── README.md               # User-facing setup guide
└── claude.md               # This file (Claude context)
```

## Design Patterns & Conventions

### File Organization
- **Publish root**: `/public` only (prevents .github/, README, etc. from being served)
- **Shared assets**: `/public/assets/` for CSS, images, fonts
- **Tools structure**: `/public/tools/<tool-name>/` for each tool
- **No Jekyll**: `.nojekyll` file disables GitHub's default Jekyll processing

### HTML/CSS Conventions
- All pages link to `/assets/style.css` for consistent styling
- Mobile-first, responsive design using system fonts
- Minimal, clean aesthetic (light gray background, white cards)
- Relative links use root-relative paths (e.g., `/tools/` not `./tools/`)

### Tool Structure Options

#### Option 1: Simple Static Tool (Recommended)
```
public/tools/
└── my-tool/
    ├── index.html          # Tool UI
    ├── script.js           # Optional JS
    └── style.css           # Optional tool-specific styles
```

Update `public/tools/index.html` to add link to new tool.

#### Option 2: Multi-Repo (Future)
For complex tools with their own repos/build processes:
- Tool repos output to `/dist` or `/public`
- Modify `.github/workflows/deploy.yml` to clone tool repos
- Copy tool artifacts into `public/tools/<tool-name>/` before deployment
- See README.md for example workflow code

## GitHub Pages Configuration

### Current Status
- Branch: `main`
- Source: GitHub Actions (not "Deploy from a branch")
- Custom domain: `dudgeon.org` (set in repo settings + CNAME file)
- HTTPS: Enforced once DNS propagates

### Required DNS Records
```
A records (apex):
@ → 185.199.108.153
@ → 185.199.109.153
@ → 185.199.110.153
@ → 185.199.111.153

CNAME (www):
www → dudgeon.github.io.
```

## Development Workflow

### Adding a New Tool
1. Create folder: `public/tools/<tool-name>/`
2. Add `index.html` and any assets
3. Update `public/tools/index.html` to link to new tool
4. Commit and push to `main`
5. Deployment happens automatically (1-2 minutes)

### Editing Content
1. Edit files directly in `/public`
2. Commit and push to `main`
3. Monitor deployment in Actions tab
4. Changes live at dudgeon.org within minutes

### Local Testing
Since there's no build step:
- Open `public/index.html` directly in browser
- Or run: `python -m http.server 8000` from `/public`
- Or use any static file server

## Important Notes for Claude

### Phone-First User
- User works from phone frequently
- Cannot run CLI commands locally
- All changes must be committable via GitHub web UI or Claude Code
- Keep file structure simple and flat where possible

### No Over-Engineering
- Keep it minimal (current structure is intentionally simple)
- Don't add frameworks unless explicitly requested
- No build step unless tools require it
- Avoid abstractions for one-off uses

### Tool Philosophy
- Each tool is self-contained in its folder
- Tools can be vanilla HTML/CSS/JS or use CDN libraries
- No shared JS framework (tools can each use what they need)
- Tools should link back to `/tools/` for navigation

## Future Enhancements (Not Implemented Yet)

- Multi-repo tool integration via GitHub Actions
- Shared JavaScript utilities (if multiple tools need them)
- Analytics (if requested)
- Dark mode toggle (if requested)
- RSS feed for new tools (if requested)

## Quick Reference Commands

```bash
# View current structure
tree public/

# Check deployment status
gh workflow view "Deploy to GitHub Pages"

# View live site (once DNS configured)
open https://dudgeon.org
```

## Contact & Context
- Owner: dudgeon (GitHub username)
- Domain: dudgeon.org
- Email: Google Workspace on dudgeon.org (DO NOT BREAK)
- Created: January 2026

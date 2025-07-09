# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Jekyll blog using the Chirpy theme. It's a personal technical blog with posts organized by categories (CSS, Frontend, DB, AWS, etc.). The site is built with Jekyll and uses Ruby gems for dependencies.

## Development Commands

### Local Development
```bash
# Start development server
make dev
# or
jekyll serve

# Start with custom host
bash tools/run -H 0.0.0.0

# Run in production mode
bash tools/run --production
```

### Build and Test
```bash
# Build site for production
JEKYLL_ENV=production bundle exec jekyll build

# Run tests (build + html validation)
bash tools/test

# Lint SCSS
npm run lint:scss

# Build CSS and JS assets
npm run build
```

### Package Management
```bash
# Install Ruby dependencies
bundle install

# Install Node.js dependencies
npm install

# Update dependencies
bundle update
```

## Content Structure

### Blog Posts
- Located in `_posts/` directory
- Organized by category folders: `CSS/`, `Frontend/`, `DB/`, `AWS/`, etc.
- File naming: `YYYY-MM-DD-title.md`
- Posts support Korean content and technical writing

### Assets
- Images: `assets/images/YYYY-MM-DD-post-title/`
- Post images follow the pattern: `assets/images/2024-08-29-CORS/cors-simple-request.png`

### Configuration
- Main config: `_config.yml`
- Site title: "shineoov"
- Timezone: Asia/Seoul
- Theme: jekyll-theme-chirpy

## Key Files and Directories

- `_config.yml`: Main Jekyll configuration
- `_posts/`: Blog post markdown files
- `_layouts/`: HTML templates
- `_includes/`: Reusable HTML components
- `_sass/`: SCSS styles
- `assets/`: Static assets (images, CSS, JS)
- `tools/`: Build and development scripts

## Commit Message Style

Follow the existing commit message patterns:
- `post: [description]` for new posts
- `fix: [description]` for bug fixes
- `CSS: [description]` for CSS-related changes
- Include Korean descriptions when appropriate

## Content Guidelines

Posts are written in Korean and English, focusing on technical topics. Common categories include:
- CSS fundamentals and properties
- Frontend development (Canvas, JavaScript)
- Database concepts (isolation levels, query optimization)
- AWS and cloud services
- Git and development workflow

When adding new posts, ensure proper front matter with category, tags, and date fields.
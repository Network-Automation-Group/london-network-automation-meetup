# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hugo static site for the London Network Automation Meetup (LNAM) using the Event theme. The site integrates with Sessionize (event management platform) to automatically generate event content including sessions, speakers, and schedules.

**Key Details:**
- Event Date: October 9, 2025
- Venue: AWS Experience, 60 Holborn Viaduct, London
- Sessionize ID: `veiahmfo`
- Live URL: https://networkautomation.group/

## Commands

### Development
```bash
# Run development server
hugo server

# Build for production
hugo --minify

# Build with specific base URL
hugo --minify --baseURL "https://networkautomation.group/"
```

### Theme Development (themes/event/)
```bash
# Install theme dependencies
cd themes/event && npm install

# Run theme tests
cd themes/event && npx playwright test

# Lint theme code  
cd themes/event && npx eslint --fix
```

## Architecture

### Hugo Site Structure
- **Root hugo.yaml**: Main site configuration with event details, menus, and theme parameters
- **themes/event/**: Complete Hugo Event theme (no longer a submodule - converted to regular files)
- **content/**: Event-specific content pages
- **static/**: Static assets (logos, images)
- **layouts/**: Custom layout overrides for the theme

### Theme Integration
The Event theme generates content dynamically from Sessionize API during build:
- **Sessions**: Auto-generated from Sessionize event data
- **Speakers**: Auto-generated speaker profiles and pages
- **Schedule**: Interactive filterable event schedule

### Key Configuration Areas

**hugo.yaml structure:**
```yaml
params:
  themes:
    event:
      sessionizeId: "veiahmfo"  # Controls data source
      colors: {...}            # Theme color customization
      socialLinks: {...}       # Event social media links
      callToAction: {...}      # Registration/ticket links
```

**Content Generation:**
- Theme fetches data from Sessionize API during hugo build
- Creates `/sessions/`, `/speakers/` pages automatically  
- Organizer info pulled from `organizers: pete-crocker` setting

### Deployment
- **GitHub Actions**: `.github/workflows/hugo.yml` deploys to GitHub Pages
- **Build triggers**: Push to main branch, manual workflow dispatch
- **Hugo version**: Currently pinned to v0.128.0 in CI

## Important Notes

### Submodule Cleanup
The `themes/event` directory was previously a git submodule but has been converted to regular files. All submodule references have been cleaned up.

### Content Updates
Since the site pulls data from Sessionize, content updates happen by:
1. Updating the Sessionize event
2. Rebuilding the site (automatic via GitHub Actions)

### Theme Customization
- Colors and branding configured in `hugo.yaml`
- Custom layouts can be added to root `layouts/` to override theme defaults
- Theme-specific assets in `themes/event/assets/`
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hugo static site for the London Network Automation Meetup (LNAM) using the Event theme. The site integrates with Sessionize (event management platform) to automatically generate event content including sessions, speakers, and schedules.

**Key Details:**

- Next Event: January 29, 2026
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

# Run theme tests (Playwright end-to-end tests)
cd themes/event && npx playwright test

# Run Playwright tests with UI
cd themes/event && npx playwright test --ui

# Lint theme code (ESLint with Prettier)
cd themes/event && npx eslint --fix

# Format theme code
cd themes/event && npx prettier --write .
```

## Architecture

### Hugo Site Structure

- **hugo.yaml**: Main site configuration with event details, menus, and theme parameters
- **themes/event/**: Complete Hugo Event theme (no longer a submodule - converted to regular files)
  - Contains theme assets, layouts, styles, tests (playwright.config.ts), and linting config (eslint.config.js)
- **content/**: Event-specific markdown pages (about.md, code-of-conduct.md, location.md)
- **assets/**: Site-specific assets (logos/lnam.png, images/background.png) - processed by Hugo pipelines
- **static/**: Static files served as-is (aws.png sponsor logo, favicon.ico, slack.png)
- **layouts/**: Custom layout overrides for the theme (partials/, _default/, sessions/)
- **data/**: Empty - dynamic data fetched from Sessionize API during build
- **i18n/**: Internationalization overrides (currently en.yaml)

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

- Theme fetches data from Sessionize API during `hugo` build
- Creates `/sessions/`, `/speakers/`, `/schedule/` pages automatically
- Organizer info pulled from `organizers: pete-crocker` setting in hugo.yaml
- No local data files needed - all event data comes from Sessionize

### Deployment

- **GitHub Actions**: `.github/workflows/hugo.yml` deploys to GitHub Pages
- **Build triggers**: Push to main branch, manual workflow dispatch
- **Hugo version**: Pinned to v0.128.0 (extended version) in CI
- **Automatic rebuilds**: Consider scheduling rebuilds to sync latest Sessionize changes

## Important Notes

### Updating Event Details

**For a new event, update these settings in hugo.yaml:**
- `params.themes.event.sessionizeId` - Sessionize event ID
- `params.themes.event.startDate` and `endDate`
- `params.themes.event.callToAction.other.url` - Ticket registration URL
- Event address and map details

**Session/speaker content updates:**
Since the site pulls data from Sessionize API, content updates happen by:
1. Updating the Sessionize event (sessions, speakers, schedule)
2. Rebuilding the site (automatic via GitHub Actions on push, or manual trigger)

### Theme Customization

- **Colors**: Configured in `hugo.yaml` under `params.themes.event.colors`
- **Logos and assets**: Place in `assets/` directory (processed by Hugo) or `static/` (served as-is)
- **Custom layouts**: Add to root `layouts/` to override theme defaults
- **Menus**: Configured in `hugo.yaml` under `languages.en.menus`

### Submodule History

The `themes/event` directory was previously a git submodule but has been converted to regular files. All submodule references have been cleaned up.

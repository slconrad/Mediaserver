# Mediaserver AI Coding Assistant Instructions

## Project Overview
Galaxy Media Server is a static landing page and access portal for a private Plex media server. The project consists of three files:
- `index.html` - Landing page with hero section, media categories grid, about section, and Plex access modal
- `styles.css` - Complete responsive styling with gradients, animations, and dark theme
- `README.md` - Basic project description

This is a **frontend-only project** with no build process, backend, or framework dependencies.

## Architecture & Key Components

### HTML Structure (`index.html`)
- **Header**: Sticky navigation with logo and branding
- **Hero Section**: Call-to-action with "Start Watching" button
- **Media Sections**: Two category grids (Classics & Binge-Worth Series) using Font Awesome icons instead of images
- **About Section**: Long-form content describing the service
- **Modal Dialog**: "Access Galaxy Media Server" overlay triggered by CTA button, links to Plex.tv
- **Footer**: Copyright and tagline

Key pattern: Modal triggers on `.cta-buttons .btn` click and closes via X button or outside click.

### CSS Design System (`styles.css`)
Uses CSS custom properties for theming:
```css
--color-primary: #00d4ff (cyan)
--color-secondary: #9d4edd (purple)
--color-background: gradient (dark blue/purple)
--color-surface: semi-transparent white
```

**Design Patterns:**
- Glassmorphism: `backdrop-filter: blur(10px)` on header, cards, modal
- Icon grids instead of image assets (reduces dependencies)
- Smooth animations: hover transforms, fade-ins, header pulse effect
- Responsive breakpoints: 768px (tablet) and 480px (mobile)
- Gradient text headers using `-webkit-background-clip: text`

### JavaScript Functionality (`index.html` inline)
Minimal vanilla JS for modal management:
1. Click "Start Watching" → open modal
2. Click X or outside modal → close
3. "Go to Plex.tv" button opens Plex.tv in new tab

## Development Patterns

### Styling Convention
- **Colors**: Use CSS variables, not hex values directly (except in gradients)
- **Spacing**: Use multiples of 8px or 4px increments
- **Typography**: System font stack: `-apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto'`
- **Icons**: Font Awesome 6.4.0 (CDN), use `<i>` tags with `fa-solid`/`fa-regular` classes

### Responsive Design
- Mobile-first not used; desktop breakpoints are 768px and 480px
- Media grid changes from 3 columns → 1 column on tablet
- Font sizes scale significantly on mobile (64px → 36px for h1)
- Use `aspect-ratio` for responsive image containers (16/9 for media posters)

### Component Patterns
- **Media Cards**: Flex container with image area (9:16 ratio) + info section
- **Buttons**: `.btn` base class + `.btn-primary`/`.btn-secondary` modifiers
- **Sections**: Each has `.section-title` with `::before` pseudo-element for star icon
- **Modals**: Fixed positioning, semi-transparent overlay, centered content with max-width

## File Modification Guide

### When Adding Content
1. New sections should follow the pattern: `<section class="section"><h2 class="section-title">...</h2><div class="media-grid">...`
2. Use section IDs for anchor links (e.g., `id="classics"`, `id="modern"`)
3. Maintain consistent padding/margins with CSS variables

### When Updating Styles
1. Add custom properties to `:root` for new colors/sizes
2. Apply media queries at file end (already separated by breakpoint)
3. Test both hover states and mobile layouts
4. Use `backdrop-filter: blur()` only on dark backgrounds (performance consideration)

### When Modifying JavaScript
- Keep modal logic vanilla JS (no jQuery dependencies)
- Modal target ID should match: `.modal` class with `getElementById()`
- Event delegation for dynamically added buttons uses `.querySelectorAll()`

## Known Limitations & Constraints
- Font Awesome loaded from CDN (requires internet, but cached after first load)
- Modal hardcoded to Plex.tv link (not configurable)
- Fixed gradient backgrounds may impact battery on dark mode devices
- Service worker caches essential assets; CDN fonts cached after first visit

## PWA (Progressive Web App) Support
As of the latest update, Galaxy Media Server includes full PWA support for app-like experience:

### What's Included
- **`manifest.json`**: Web app manifest with icons, app name, theme colors, and shortcuts
- **`sw.js`**: Service worker with cache-first strategy for offline support
- **HTML metadata**: Apple mobile app tags, theme colors, touch icons

### How It Works
1. Users can "Install" the app on mobile/desktop (Add to Home Screen, Create Shortcut)
2. App runs in standalone mode without browser UI
3. Offline access to landing page content (cached on first visit)
4. Service worker caches HTML, CSS, and Font Awesome fonts
5. Periodic update checks (every 60 seconds)

### Key Files
- `manifest.json` - App metadata, icons, shortcuts to media categories
- `sw.js` - Service worker with cache strategy and offline fallback
- `index.html` - Meta tags for iOS/Android PWA support and service worker registration

### Cache Strategy
- **Network requests**: Cache assets on first fetch, serve from cache on subsequent requests
- **CDN assets**: Font Awesome files cached locally after first load
- **Offline fallback**: Shows cached index.html or error message if network unavailable

### Testing PWA Locally
1. Serve over HTTPS (required for service workers):
   ```bash
   # Using Python 3
   python3 -m http.server --directory . 8000
   # Then access via http://localhost:8000 (service worker works on localhost)
   ```
2. Open DevTools → Application tab → Service Workers to verify registration
3. Test offline: DevTools → Network tab → check "Offline" and reload
4. Install app: Browser menu → "Install app" or "Add to Home Screen"

# Website clone

Notes on how reference sites are built — styling system, layout patterns, and interactive behavior — captured for use when cloning or rebuilding them.

# Site 1: harrisonjansma.com

Notes on how [harrisonjansma.com](https://harrisonjansma.com/) is built.

## Tech stack

- **Hand-built static HTML/CSS/JS**, hosted on GitHub Pages. No framework, no build step, no bundler.
- Fonts loaded from Google Fonts: `Inter` (400/500/600/700/800) for body text and UI, `Space Grotesk` (500/600/700) for headings/display text.
- Icon sets: Linearicons (`css/linearicons.css`) and Font Awesome (`css/font-awesome.min.css`) for social icons.
- Two CSS files split by concern:
  - `css/chrome.css` — shared header + footer, loaded on **every** page, plus the design-token `:root` block. Everything is scoped under `.site-header` / `.site-footer` so it can coexist with an older Bootstrap-based page chain without specificity fights.
  - `css/site.css` — page content for the "2026 refresh" pages (Home, About, Projects, Huck). Assumes `chrome.css` loaded first for its tokens.
- `js/site.js` — a single vanilla-JS file (no dependencies), organized as independent "arm*" functions that each no-op if their markup isn't present on the page, so one file serves every page.

## Design tokens (CSS custom properties)

Defined once in `chrome.css :root` and used everywhere:

```css
--ink:        #171a26;   /* headings, primary text */
--ink-soft:   #4a5163;   /* body text */
--muted:      #6b7385;   /* secondary / meta text */
--line:       #e8eaf2;   /* hairline borders */
--line-strong:#e0e2f0;   /* button borders, dividers */
--bg:         #ffffff;
--bg-tint:    #f7f8fc;   /* alternating section background */
--accent:     #4f46e5;   /* indigo */
--accent-2:   #7c3aed;   /* violet */
--accent-ink: #3730a3;
--dark:       #12141f;   /* footer, diagram panel */
--dark-soft:  #7c8296;   /* text on --dark */
--micro:      #8a90a3;   /* mono micro-labels on light */
--index:      #a8adbf;   /* row index numerals */
--live:       #3ddc84;   /* "live" status dot only */
--grad: linear-gradient(120deg,#4f46e5 0%,#7c3aed 100%);
--shadow-sm: 0 1px 2px rgba(23,26,38,.06), 0 2px 8px rgba(23,26,38,.05);
--shadow-md: 0 10px 30px -12px rgba(23,26,38,.22);
--mono: ui-monospace,SFMono-Regular,Menlo,monospace;
--display: 'Space Grotesk','Inter',sans-serif;
--ease: cubic-bezier(.22,.7,.2,1);
```

This is a small, consistent palette: near-black ink on white/off-white, one indigo→violet gradient as the sole accent, and a single green dot reserved for "live" indicators. Almost every color, radius, and shadow on the site traces back to one of these tokens.

## Layout system

- **Container**: `.wrap { max-width:1160px; margin:0 auto; padding:0 32px; }` — one shared content width for the whole site, defined in `chrome.css` since both the header/footer and page content use it.
- **Section rhythm**: each section gets its own top/bottom padding class (`.pad-work`, `.pad-band`, `.pad-skills`, `.pad-intro`, `.pad-exp`, `.pad-projects`, `.pad-gallery`), roughly 58–80px, tightened on mobile via a `@media (max-width:640px)` block that redefines each one.
- **Alternating backgrounds**: `.tint` / `.banded` classes swap between white and `--bg-tint` (#f7f8fc) to separate sections without hard lines, `.banded` adds hairline top/bottom borders.
- **Grid background**: `.grid-bg` (used behind the hero) is a faint 64×64px graph-paper pattern made from two overlaid linear-gradients at ~3.5% opacity.
- Breakpoints: `1020px` (tablet — collapses multi-column grids to 1–2 columns, nav spacing tightens) and `640px` (mobile — single column, reduced padding, smaller headings).

## Recurring UI patterns

- **Pill / chip system**: `.pill` (indigo-tinted, rounded-full tags for tech/keywords) and `.chip-tool` (neutral gray tags for tooling lists) — both `display:inline-block`, small caps-free text, used throughout project rows and skill cards.
- **Buttons**: `.btn` base + `.btn--grad` (gradient-filled, primary) or `.btn--ghost` (white with border, secondary), both fully rounded (`border-radius:999px`), with a subtle lift + shadow on hover.
- **Section headers**: `.sechead` pattern — a small uppercase monospace "kicker" label (e.g. "01 — Selected work"), a large `Space Grotesk` `<h2>`, and an optional muted description paragraph capped at `58ch` for readability.
- **Cards**: `.skill-card`, `.featured` (the live-demo callout), `.tl-current-card` — all share the same visual language: 1px hairline or tinted border, generous border-radius (16–20px), white or very-faintly-tinted background.
- **Row list**: `.rows` / `.row-link` — the "Selected work" project list is a set of full-width clickable rows with a monospace index number, category label, title, description, and right-aligned metrics column, laid out with CSS grid and a hover state that shifts the row right 4px with a tinted background.
- **Timeline**: `.timeline` / `.tl-row` — CSS-grid timeline (year column, a rail with dots, content) for the About page's experience history, with expandable `.exp-panel` sections animated via `max-height`/`opacity` transitions (JS-driven, not `<details>`).
- **Gallery + lightbox**: `.gallery` uses CSS multi-column (`columns:3`) for a masonry-style photo grid (Huck page), paired with a JS-driven fullscreen lightbox overlay with prev/next/escape keyboard support.
- **Sticky header**: `.site-header` is `position:sticky` with a frosted-glass effect (`backdrop-filter: blur(16px) saturate(180%)` over `rgba(255,255,255,.72)`), plus a 2px gradient hairline strip beneath it.
- **Status/live indicators**: a small pulsing green dot (`.live-dot`, `@keyframes livePulse`) used wherever something is described as real-time/live (hero status chip, architecture diagram).
- **Architecture diagram**: a hand-built animated SVG-free diagram (all divs) in the hero, showing data flowing from a source node, fanning out to three model nodes, and fanning back in — animated with small "packet" dots traveling along CSS lines (`@keyframes flowDown`) and a subtle glow pulse on the model nodes (`@keyframes nodeGlow`).

## Motion & accessibility

- **Scroll reveal**: elements marked `data-reveal` fade/slide in on scroll. Implemented defensively in three layers so content is never stuck invisible: (1) `IntersectionObserver` as the primary path, (2) a manual bounding-rect check on scroll/resize as a fallback, (3) a hard 1.5s timeout that forces everything visible if the observer never fires. The `.js-reveal` class (and thus the initial `opacity:0`) is only added via an inline `<head>` script if JS is confirmed running and `prefers-reduced-motion` is not set — so a no-JS visit never leaves content invisible.
- **`prefers-reduced-motion: reduce`** is respected globally: a media query disables all `animation`, sets `scroll-behavior:auto`, and disables hover-transform effects.
- All interactive JS (`site.js`) is structured as small independent "arm" functions (`armReveal`, `armNav`, `armDemo`, `armFilters`, `armExpanders`, `armLightbox`) each guarded by an early-return if their markup isn't present, called from one `init()` — so all pages share one JS file safely.

## Notable interactive components

- **Live demo overlay**: the "Run it here" button opens a fullscreen `<iframe>` overlay rather than navigating away. The iframe `src` is deferred into a `data-src` attribute and only set on open (so the third-party demo host isn't contacted until requested), and is cleared on close (to stop microphone/audio capture).
- **Mobile nav**: a hamburger toggle (`.nav-toggle`) shows/hides the nav list as an absolutely-positioned dropdown below 860px, closing on outside-click or viewport resize past the breakpoint.
- **Projects filter**: pill-style filter buttons (`data-filter`) show/hide project rows (`data-cat`) and whole groups (`data-group`), clearing the top margin of whichever group becomes first so filtered views don't open with dead space.

## Content structure (Home page)

1. Sticky header — logo/wordmark, nav links, external "wife's site" link, résumé CTA button.
2. Hero — avatar photo, "live" status chip, large display-font name, lead paragraph, 3-stat row, CTA buttons, and the animated architecture diagram.
3. "01 — Selected work" — a featured live-demo card, followed by a list of project rows.
4. "02 — What I work on" — a 3×2 hairline-bordered grid of capability tiles with icon + heading + description.
5. "03 — Skills & tooling" — three cards (GenAI/LLMs, ML/NLP, Systems/Leadership), each linking out to case-study pages plus a row of tool chips.
6. "04 — About me" — photo + social links alongside a pull-quote and bio paragraphs.
7. Footer — brand, contact email, page links, external links, copyright line noting "Hand-built with HTML, CSS & JavaScript · GitHub Pages".
8. Hidden full-screen demo overlay (markup present but off-screen until triggered).

# Site 2: yan-holtz.com

Notes on [yan-holtz.com](https://www.yan-holtz.com/), a freelance-engineer portfolio built on the **Start Bootstrap "Agency"** template (Bootstrap 4 + jQuery), styled via `css/agency.css`. Unlike the harrisonjansma.com header (a static sticky bar), this site's header is a full interactive canvas.

## Header (`<header class="masthead">`) — interactive breakdown

The header/hero ("masthead") is the standout interactive piece of this site: a live, mouse-reactive particle network sitting behind the hero text.

### Structure

```html
<nav class="navbar navbar-expand-lg fixed-top" id="mainNav">...</nav>

<header class="masthead">
  <div id="particles-js"></div>          <!-- particles.js canvas mounts here -->
  <div class="textlanding">...</div>      <!-- name, tagline, CTAs, social icons -->
  <div class="arrowlanding">...</div>     <!-- scroll-down affordance -->
</header>
```

- `#mainNav` is a Bootstrap `fixed-top` navbar, transparent over the hero and turning white with a shrink animation once the page scrolls (`.navbar-shrink` class toggled by `js/agency.min.js`'s scroll listener — the classic Start Bootstrap Agency behavior). Nav links use `js-scroll-trigger` for jQuery-animated smooth-scroll to in-page anchor sections (`#description`, `#services`, `#website`, `#portfolio`, `#contact`), or a plain link out to `blog.html`.
- `.textlanding` is absolutely centered (`top:50%; right:50%; transform:translate(50%,-50%)`) over the particle canvas: name/logo lockup, a `<hr>` rule, a `.hero-title` heading ("Interactive data visualization for the web."), a lead paragraph, two CTA buttons (Newsletter / Contact), and a row of circular social icons.
- `.arrowlanding` is a large downward-pointing glyph (`&#xfe40;`, teal `#69b3a2`) pinned near the bottom of the header, itself a `js-scroll-trigger` link to `#description` — a manual scroll-cue rather than an animated chevron.

### The particle background — `particles.js` (js/particles.js + js/appHome.js)

This is the interactive core of the header. `particles.js` (a third-party canvas library, MIT, no dependency on jQuery) is initialized against `#particles-js` with a config literally inlined in `js/appHome.js`:

```js
particlesJS("particles-js", {
  particles: {
    number: { value: 80, density: { enable: true, value_area: 800 } },
    color: { value: "#888888" },
    shape: { type: "circle" },
    opacity: { value: 0.5 },
    size: { value: 5, random: true },
    line_linked: { enable: true, distance: 150, color: "#888888", opacity: 0.4, width: 1 },
    move: { enable: true, speed: 3, direction: "none", out_mode: "out" }
  },
  interactivity: {
    detect_on: "canvas",
    events: {
      onhover: { enable: true, mode: "repulse" },  // particles scatter away from the cursor
      onclick: { enable: true, mode: "push" },      // clicking spawns new particles
      resize: true
    },
    modes: {
      repulse: { distance: 200 },
      push: { particles_nb: 4 }
    }
  },
  retina_detect: true
});
```

Behavior in plain terms: 80 gray dots drift slowly and freely across the canvas, drawing a thin connecting line between any two dots within 150px of each other (a classic "constellation/network" look). Moving the mouse over the canvas **repels** nearby particles away from the cursor (`repulse`, 200px radius); clicking **adds** 4 new particles at the click point (`push`). No `grab`/`bubble` modes are wired up even though they're defined in the library, so hover doesn't highlight lines and click doesn't resize particles here.

Supporting CSS (`agency.css`):
```css
#particles-js {
  width: 100%;
  height: 760px;
  background-color: white;
}
```
The canvas is a plain full-width white block sized to 760px — the particle network is the only thing drawn on it, with the hero text absolutely positioned on top via `.textlanding`.

### Other header-adjacent interactive details

- **Tooltips on social icons**: each icon link carries a `data-tooltip` attribute (e.g. "3,000+ stars across my open-source projects") rendered via a pure-CSS `::after` pseudo-element pattern (`ul.social-buttons li a[data-tooltip]::after`), shown on `:hover` with an opacity transition — not a JS/Bootstrap tooltip component, despite Bootstrap's tooltip CSS also being loaded and present in the file for the modal/portfolio parts of the page.
- **Navbar shrink-on-scroll**: `#mainNav` starts transparent with extra top/bottom padding over the hero, then gets a `.navbar-shrink` class from the bundled Agency JS once the user scrolls, snapping to a white background and tighter padding — implemented as a plain scroll-position class toggle, not an IntersectionObserver.
- **Smooth scroll**: any `.js-scroll-trigger` link (nav items, the CTA "Contact" button, the down-arrow) is intercepted by jQuery Easing to animate scroll rather than jumping, then closes the mobile nav collapse if it was open.

## Tech stack (Site 2)

- Bootstrap 4 + jQuery + jQuery Easing (`vendor/`), Start Bootstrap "Agency" template as the base (`js/agency.min.js`, `css/agency.css`).
- `particles.js` for the interactive header canvas (`js/particles.js`, configured in `js/appHome.js`).
- Leaflet.js and D3.js loaded globally (used elsewhere on the site for maps/charts, not in the header).
- Font Awesome for icons; Google Fonts `Montserrat`, `Droid Serif`, `Kaushan Script` referenced in `agency.css`.
- Third-party embeds: ConvertKit newsletter widget script, Senja testimonial widget script.

## Adapted for this portfolio (index.html Home hero)

Implemented the same idea — a mouse-reactive particle network behind the hero text — as a
dependency-free `<canvas>` script rather than pulling in the `particles.js` library, so it could
be made theme-aware (light/dark) and match the portfolio's existing indigo accent instead of
`particles.js`'s static config:

- `<canvas id="hero-particles">` sits absolutely positioned behind `#home`'s `.container`
  (`z-index:0` vs `1`), with the container given `pointer-events:none` so mouse/click events pass
  through to the canvas underneath the text.
- Particle count scales with the hero's area (`(W*H)/13000`, clamped 24–70) rather than
  `particles.js`'s fixed 80, so it stays proportionate at any viewport size.
- Same two interactions as the reference site: particles drift slowly and link with a line when
  within 130px of each other (opacity fades with distance, mirroring `line_linked`); hovering
  repels nearby particles within a 90px radius (mirrors `repulse`); clicking adds up to 4 new
  particles at the click point, capped at 110 total (mirrors `push`).
- Colors are read from `document.documentElement`'s `data-theme` attribute on every frame
  (`rgba(79,70,229,.38)` dots / `.14` lines in light mode, brighter `rgba(157,161,255,.6)` /
  `.22` in dark) so toggling the site's existing dark-mode switch re-colors the animation
  automatically — no re-init needed, unlike `particles.js` which is configured once at load.
- Respects `prefers-reduced-motion`: draws one static frame and skips the animation loop and all
  mouse/click listeners entirely, rather than just disabling CSS transitions.

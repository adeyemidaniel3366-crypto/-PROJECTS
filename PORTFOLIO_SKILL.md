# Portfolio Skill — Production-Quality Developer Portfolio Template

A comprehensive skill for creating production-quality, fully responsive personal portfolio websites with modern React tooling, scroll-driven animations, and a content-driven architecture.

---

## Skill Overview

This skill provides a complete template and methodology for building developer portfolios featuring:

- **Bold editorial design** inspired by creative director portfolios
- **Scroll-driven animations** using Framer Motion (masked text reveals, parallax, layout transitions)
- **Smooth scrolling** with Lenis, synced to Framer Motion scroll APIs
- **Content-driven architecture** — all content in a single data file (`src/data/content.js`)
- **Custom design system** with CSS variables, fluid typography, and responsive grids
- **Reduced motion support** — respects `prefers-reduced-motion` globally
- **Accessibility-first** — semantic HTML, focus management, ARIA labels, WCAG AA contrast

---

## Tech Stack (Locked)

| Category | Technology | Version | Purpose |
|----------|------------|---------|---------|
| **Framework** | React | 18 | Component-based UI with hooks |
| **Build Tool** | Vite | 5+ | Fast dev server, optimized builds |
| **Styling** | Tailwind CSS | 4 | Utility-first with `@theme` design tokens |
| **Animations** | Framer Motion | 11 | Scroll animations, layout transitions |
| **Smooth Scroll** | Lenis | 1 | Buttery-smooth scrolling |
| **Icons** | Lucide React + React Icons | Latest | Lightweight SVG + brand icons |
| **Fonts** | Google Fonts | — | Anton, Inter, Caveat, DM Mono |
| **Linting** | ESLint | 9 | Code quality |

---

## Project Structure

```
portfolio/
├── public/
│   ├── favicon.svg              # Brand favicon
│   ├── profile.png              # Hero portrait (4:5 crop, ~640x800)
│   └── projects/                # Project screenshots
│       ├── project-one.png
│       ├── project-two.png
│       └── ...
├── src/
│   ├── components/
│   │   └── AntigravityBackground.jsx  # Optional ambient background
│   ├── data/
│   │   └── content.js           # SINGLE SOURCE OF TRUTH
│   ├── App.jsx                  # Root component (all sections inline)
│   ├── App.css                  # All styles (Tailwind + custom CSS)
│   ├── index.css                # Tailwind import + global styles
│   └── main.jsx                 # Entry point
├── index.html                   # SEO meta, Open Graph, favicon
├── vite.config.js               # Vite + Tailwind plugin config
├── package.json
├── eslint.config.js
└── README.md
```

---

## Design System

### Colors (CSS Variables)

```css
:root {
  --accent: #FF3B1F;        /* Primary brand color */
  --ink: #0A0A0A;           /* Near-black text */
  --paper: #FAFAFA;         /* Off-white background */
  --soft: #F4F4F4;          /* Alternating section bands */
  --line: #E5E5E5;          /* Thin 1px dividers */
  --muted: #999999;         /* Labels, captions */
  --surface: #FFFFFF;       /* Card backgrounds */
}
```

### Dark Mode (Auto-detected)

```css
@media (prefers-color-scheme: dark) {
  :root {
    --ink: #FAFAFA;
    --paper: #0A0A0A;
    --soft: #141414;
    --line: #333333;
    --muted: #888888;
    --surface: #1A1A1A;
  }
}
```

### Typography

| Role | Font | CSS Variable |
|------|------|--------------|
| **Display** | Anton | `--heading` |
| **Body** | Inter | `--body` |
| **Signature** | Caveat | `--serif` |
| **Mono** | DM Mono | `--mono` |

### Layout

- **Max-width**: 1320px container
- **Fluid padding**: `max(6.4vw, calc((100vw - 1320px) / 2))`
- **Section spacing**: 140px vertical padding
- **Breakpoints**: 360px / 768px / 1024px / 1440px

---

## Animation Architecture

### Core Patterns

| Pattern | Implementation | Used In |
|---------|----------------|---------|
| **Masked text reveal** | `overflow: hidden` parent + `y: '110%' → 0` | Hero headlines |
| **SVG path drawing** | `pathLength: 0 → 1` on `<motion.path>` | Hero brush stroke |
| **Count-up numbers** | `IntersectionObserver` + `requestAnimationFrame` | About stats, Skills |
| **Progress bars** | `scaleX: 0 → level/100` on `whileInView` | Skill bars |
| **Layout transitions** | `layoutId` + `layout` prop | Project card → Modal |
| **Parallax** | `useTransform` with different speeds | Hero portrait, blocks |
| **Staggered entrance** | `transition: { delay: index * 0.07 }` | Grids, lists |
| **Infinite marquee** | CSS animation + pause on hover | Tech Stack |

### Reduced Motion

All animations respect `prefers-reduced-motion`:

```jsx
const reduceMotion = useReducedMotion()
// Disables: parallax, count-ups, marquee, cursor, preloader
// Shows content instantly
```

---

## Content Schema (`src/data/content.js`)

```js
export const content = {
  // Identity
  identity: {
    name: 'Full Name',
    shortName: 'Short Name',
    role: 'Software Developer',
    tagline: 'Building clean, performant software...',
    email: 'you@example.com',
    phone: '+1 (555) 000-0000',
    location: 'City, State, Country',
    basedIn: 'City, Country',
    institution: 'University, Location',
    availability: 'Available for remote & on-site work',
    coordinates: '00°00\' N / 00°00\' E',
    social: [
      { name: 'GitHub', url: 'https://github.com/username', icon: 'github' },
      { name: 'LinkedIn', url: 'https://linkedin.com/in/username', icon: 'linkedin' },
      { name: 'X', url: 'https://x.com/username', icon: 'x' },
      { name: 'Email', url: 'mailto:you@example.com', icon: 'mail' },
    ],
    seo: {
      title: 'Name | Software Developer',
      description: 'Software developer building clean, performant applications.',
      ogImage: '/profile.png',
    },
  },

  // Hero
  hero: {
    headline: ['SOFTWARE', 'DEVELOPER'],
    subtext: 'Your tagline here...',
    ctaText: 'VIEW MY WORK',
    ctaHref: '#work',
  },

  // About
  about: {
    heading: 'I build fast, clean software with a designer\'s eye.',
    paragraph: 'Your bio paragraph here...',
    signature: 'Your Name',
    signatureCaption: 'Software Developer',
    stats: [
      { value: 12, label: 'Projects shipped', note: 'Mobile · Web · Full-stack' },
      { value: 4, label: 'Years experience', note: 'Professional + freelance' },
      { value: 8, label: 'Core technologies', note: 'React · Node · Python · Go' },
    ],
    codeWindow: {
      filename: 'approach.ts',
      lines: [
        'const craft = {',
        '  clarity: \'always\',',
        '  speed: \'by design\',',
        '  details: \'matter\'',
        '}',
        'craft.ship()',
      ],
      caption: 'A considered process, from first line to final detail.',
    },
  },

  // Services (5 columns)
  services: [
    { number: '01', title: 'Mobile Development', description: '...', icon: 'smartphone' },
    { number: '02', title: 'Frontend Web', description: '...', icon: 'layout' },
    { number: '03', title: 'Backend & APIs', description: '...', icon: 'server' },
    { number: '04', title: 'DevOps & Cloud', description: '...', icon: 'cloud' },
    { number: '05', title: 'UI Engineering', description: '...', icon: 'palette' },
  ],

  // Skills
  skills: [
    { name: 'React / TypeScript', level: 95 },
    { name: 'Node.js / Backend', level: 90 },
    { name: 'Mobile Development', level: 85 },
    { name: 'DevOps / Cloud', level: 80 },
    { name: 'System Design', level: 85 },
    { name: 'Testing / QA', level: 75 },
  ],
  skillGroups: [
    { title: 'Frontend', items: ['React', 'TypeScript', 'Next.js', 'Tailwind CSS', 'Framer Motion', 'Vite'] },
    { title: 'Backend', items: ['Node.js', 'Express', 'PostgreSQL', 'Redis', 'GraphQL', 'REST APIs'] },
    { title: 'Mobile', items: ['React Native', 'Expo', 'iOS', 'Android', 'App Store', 'Play Store'] },
    { title: 'DevOps', items: ['Docker', 'Kubernetes', 'AWS', 'Vercel', 'GitHub Actions', 'Terraform'] },
    { title: 'Tools', items: ['Git', 'VS Code', 'Figma', 'Linear', 'Notion', 'Postman'] },
  ],

  // Projects
  projects: [
    {
      id: 'project-one',
      title: 'Project One',
      category: 'Web',
      type: 'Web Application',
      stack: 'React / TypeScript / Node.js',
      description: 'A brief description of what this project does and why it matters.',
      focus: ['Key feature 1', 'Key feature 2', 'Key feature 3'],
      theme: 'dark',           // 'dark' | 'red' | 'paper'
      mark: 'P1',              // Short identifier for fallback
      image: '/projects/project-one.png',
      imageFit: 'contain',     // 'contain' | 'cover'
      imageScale: 1,           // Optional scale factor
      githubUrl: 'https://github.com/username/project-one',
      liveUrl: 'https://project-one.vercel.app',
    },
    // ... more projects
  ],

  // Tech Stack Marquee
  techStack: [
    'React', 'TypeScript', 'Next.js', 'Node.js', 'PostgreSQL', 'Redis',
    'Docker', 'Kubernetes', 'AWS', 'Vercel', 'GitHub Actions', 'Tailwind CSS',
    'Framer Motion', 'React Native', 'Expo', 'GraphQL', 'Prisma', 'Zod',
  ],

  // Foundations (4-column grid)
  foundations: [
    'Computer Science Fundamentals',
    'System Design & Architecture',
    'Testing & Quality Assurance',
    'DevOps & CI/CD',
  ],

  // How I Build (3 principles)
  principles: [
    { number: '01', title: 'Clarity First', description: 'Code is read more than written...' },
    { number: '02', title: 'Performance by Default', description: 'Optimize for the user experience...' },
    { number: '03', title: 'Ship Incrementally', description: 'Small, frequent releases reduce risk...' },
  ],

  // CTA
  cta: {
    headline: ['LET\'S BUILD', 'SOMETHING GREAT.'],
    subtext: 'Have a project in mind? Let\'s talk.',
    buttonText: 'GET IN TOUCH',
    buttonHref: '#contact',
  },

  // Navigation
  navigation: [
    { label: 'Home', id: 'home' },
    { label: 'About', id: 'about' },
    { label: 'Services', id: 'services' },
    { label: 'Skills', id: 'skills' },
    { label: 'Work', id: 'work' },
    { label: 'Stack', id: 'stack' },
    { label: 'Contact', id: 'contact' },
  ],
}
```

---

## Quick Start

### 1. Initialize Project

```bash
# Create project directory
mkdir my-portfolio && cd my-portfolio

# Initialize npm
npm init -y

# Install dependencies
npm install react react-dom framer-motion lenis lucide-react react-icons
npm install -D vite @vitejs/plugin-react tailwindcss @tailwindcss/vite eslint @eslint/js eslint-plugin-react-hooks eslint-plugin-react-refresh globals
```

### 2. Configure Vite (`vite.config.js`)

```js
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'
import { defineConfig } from 'vite'

export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```

### 3. Configure Tailwind (`src/index.css`)

```css
@import 'tailwindcss';

@theme {
  --color-accent: #FF3B1F;
  --color-ink: #0A0A0A;
  --color-paper: #FAFAFA;
  --color-soft: #F4F4F4;
  --color-line: #E5E5E5;
  --color-muted: #999999;
  --color-surface: #FFFFFF;
  
  --font-display: 'Anton', 'Impact', sans-serif;
  --font-body: 'Inter', system-ui, sans-serif;
  --font-signature: 'Caveat', cursive;
  --font-mono: 'DM Mono', ui-monospace, monospace;
  
  --animate-marquee: marquee 35s linear infinite;
  --animate-marquee-static: none;
}

@keyframes marquee {
  from { transform: translateX(0); }
  to { transform: translateX(-50%); }
}

@media (prefers-reduced-motion: reduce) {
  .marquee-static { animation: none; }
}
```

### 4. Create Content File (`src/data/content.js`)

Copy the schema above and customize with your information.

### 5. Create Main App (`src/App.jsx`)

Use the provided template with all sections as inline components. Key patterns:

```jsx
// Reveal animation wrapper
const reveal = { 
  hidden: { opacity: 0, y: 28 }, 
  visible: { opacity: 1, y: 0, transition: { duration: 0.65, ease: [0.22, 1, 0.36, 1] } } 
}

function Reveal({ children, className = '', delay = 0 }) {
  return (
    <motion.div 
      className={className} 
      variants={reveal} 
      initial="hidden" 
      whileInView="visible" 
      viewport={{ once: true, margin: '-10%' }} 
      transition={{ delay }}
    >
      {children}
    </motion.div>
  )
}

// Count-up animation
function Counter({ value }) {
  const ref = useRef(null)
  const reduceMotion = useReducedMotion() ?? false
  const [count, setCount] = useState(reduceMotion ? value : 0)
  const [hasAnimated, setHasAnimated] = useState(false)

  useEffect(() => {
    if (hasAnimated) return
    const element = ref.current
    if (!element) return

    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            setHasAnimated(true)
            observer.disconnect()
            if (reduceMotion) { setCount(value); return }
            let frame
            const start = performance.now()
            const tick = (now) => {
              const progress = Math.min((now - start) / 1050, 1)
              setCount(Math.round(value * (1 - (1 - progress) ** 4)))
              if (progress < 1) frame = requestAnimationFrame(tick)
            }
            frame = requestAnimationFrame(tick)
            return () => cancelAnimationFrame(frame)
          }
        })
      },
      { rootMargin: '0px 0px -10% 0px', threshold: 0 }
    )

    observer.observe(element)
    return () => observer.disconnect()
  }, [value, reduceMotion, hasAnimated])

  return <span ref={ref}>{count}</span>
}
```

### 6. Add Styles (`src/App.css`)

Include all custom CSS for the design system, animations, and responsive layouts.

### 7. Configure HTML (`index.html`)

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Your description" />
    <meta name="theme-color" content="#0a0a0a" />
    <meta property="og:type" content="website" />
    <meta property="og:title" content="Your Name | Software Developer" />
    <meta property="og:description" content="Your description" />
    <meta property="og:image" content="/profile.png" />
    <title>Your Name | Software Developer</title>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Anton&family=Caveat:wght@500;600&family=DM+Mono:wght@400;500&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

### 8. Entry Point (`src/main.jsx`)

```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>
)
```

---

## Build & Deploy

### Development

```bash
npm run dev        # Start dev server at http://localhost:5173
npm run lint       # Lint source files
```

### Production Build

```bash
npm run build      # Outputs to dist/
npm run preview    # Preview production build locally
```

### Deploy to Vercel (Recommended)

1. Push to GitHub
2. Import in Vercel
3. Build Command: `npm run build`
4. Output Directory: `dist`
5. Deploy

### Deploy to Netlify

1. Connect repository
2. Build Command: `npm run build`
3. Publish Directory: `dist`
4. Add `_redirects` for SPA fallback:
   ```
   /*  /index.html  200
   ```

---

## Customization Guide

### Changing Colors

Edit CSS variables in `src/index.css`:

```css
@theme {
  --color-accent: #YOUR_COLOR;
  --color-ink: #YOUR_DARK;
  --color-paper: #YOUR_BG;
}
```

### Changing Fonts

1. Update Google Fonts import in `index.html`
2. Update `--font-display`, `--font-body`, `--font-signature`, `--font-mono` in `@theme`

### Adding a Project

1. Add entry to `content.projects` in `content.js`
2. Add screenshot to `public/projects/your-project.png`
3. Set `image: '/projects/your-project.png'` in project data
4. Choose `theme: 'dark' | 'red' | 'paper'`
5. Set `imageFit: 'contain' | 'cover'` and optional `imageScale`

### Modifying Animations

- **Global variants**: Edit `reveal` object in `App.jsx`
- **Per-component**: Edit transition props in section components
- **Reduced motion**: All components check `useReducedMotion()`

---

## Accessibility Checklist

- [ ] Semantic HTML5 (`header`, `nav`, `main`, `section`, `footer`)
- [ ] Proper heading hierarchy (h1 → h2 → h3)
- [ ] Focus visible states on all interactive elements
- [ ] Keyboard-navigable modals (focus trap, Escape to close)
- [ ] ARIA labels on icon-only buttons
- [ ] `prefers-reduced-motion` respected globally
- [ ] Color contrast ≥ WCAG AA
- [ ] Alt text on all images
- [ ] `lang` attribute on `<html>`

---

## License

MIT License — feel free to use as a template for your own portfolio.

---

## Credits

- **Design inspiration**: Creative director portfolio references
- **Fonts**: Google Fonts (Anton, Inter, Caveat, DM Mono)
- **Icons**: Lucide, Font Awesome (via React Icons)
- **Smooth scroll**: Lenis by @studio-freight
- **Animations**: Framer Motion by @framer

---

*Skill created for replicating production-quality developer portfolios*
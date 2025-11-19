# CLAUDE.md - AI Assistant Guide for Client Portfolio Template

## Repository Overview

**Repository Name**: `client-portfolio-template`
**Purpose**: A template repository for creating professional client portfolio websites
**Current State**: Uninitialized template (blank slate)
**Branch**: `claude/claude-md-mi5dvx8ip4807lev-015Ct7SDtZqHGUgpCe57Et9S`

## Current Repository State

This repository is currently **empty** and serves as a blank canvas for portfolio website development. It contains only:
- `.git/` - Git version control
- `README.md` - Basic project title

**No code, dependencies, or configuration exists yet.** This document establishes conventions for AI assistants to follow when building out this template.

## Intended Architecture

### Recommended Tech Stack Options

When initializing this template, consider these modern portfolio stacks:

#### Option 1: Next.js (Recommended for Full-Featured Portfolios)
- **Framework**: Next.js 14+ with App Router
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Animations**: Framer Motion
- **Content**: MDX for blog/case studies (optional)
- **Deployment**: Vercel, Netlify, or Cloudflare Pages

#### Option 2: Vite + React (Lightweight & Fast)
- **Framework**: Vite + React 18+
- **Language**: TypeScript
- **Styling**: Tailwind CSS or CSS Modules
- **Routing**: React Router
- **Deployment**: Vercel, Netlify, GitHub Pages

#### Option 3: Astro (Content-Focused Portfolios)
- **Framework**: Astro
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Features**: Island architecture, MDX support
- **Deployment**: Cloudflare Pages, Vercel

## Expected Directory Structure

Once initialized, the repository should follow this structure:

```
client-portfolio-template/
├── .git/                    # Version control
├── .github/                 # GitHub Actions workflows (if applicable)
│   └── workflows/
│       └── deploy.yml       # CI/CD pipeline
├── public/                  # Static assets
│   ├── images/              # Image assets
│   ├── resume.pdf           # Downloadable resume
│   └── favicon.ico          # Site favicon
├── src/                     # Source code
│   ├── components/          # Reusable UI components
│   │   ├── common/          # Generic components (Button, Card, etc.)
│   │   ├── layout/          # Layout components (Header, Footer, etc.)
│   │   └── sections/        # Page sections (Hero, About, Projects, etc.)
│   ├── pages/               # Page components/routes
│   ├── styles/              # Global styles and themes
│   ├── lib/                 # Utility functions and helpers
│   ├── data/                # Portfolio content data (projects, skills, etc.)
│   ├── types/               # TypeScript type definitions
│   └── App.tsx              # Root application component
├── tests/                   # Test files
│   ├── unit/                # Unit tests
│   └── e2e/                 # End-to-end tests
├── .env.example             # Environment variable template
├── .eslintrc.json           # ESLint configuration
├── .gitignore               # Git ignore patterns
├── .prettierrc              # Prettier configuration
├── CLAUDE.md                # This file
├── package.json             # Dependencies and scripts
├── README.md                # User-facing documentation
├── tsconfig.json            # TypeScript configuration
└── vite.config.ts           # Build tool configuration
```

## Key Conventions for AI Assistants

### 1. Code Style & Quality

- **TypeScript**: Always use TypeScript for type safety
- **Naming Conventions**:
  - Components: PascalCase (`HeroSection.tsx`, `ProjectCard.tsx`)
  - Utilities: camelCase (`formatDate.ts`, `validateEmail.ts`)
  - Constants: UPPER_SNAKE_CASE (`API_BASE_URL`, `MAX_PROJECTS`)
  - CSS Modules: kebab-case (`hero-section.module.css`)
- **File Organization**: Group by feature/section, not by type
- **Imports**: Use absolute imports with path aliases (e.g., `@/components`, `@/lib`)

### 2. Component Architecture

#### Component Structure
```tsx
// src/components/sections/HeroSection.tsx
import { FC } from 'react';
import styles from './HeroSection.module.css';

interface HeroSectionProps {
  name: string;
  tagline: string;
  ctaText?: string;
}

export const HeroSection: FC<HeroSectionProps> = ({
  name,
  tagline,
  ctaText = 'View Projects'
}) => {
  return (
    <section className={styles.hero}>
      <h1>{name}</h1>
      <p>{tagline}</p>
      <button>{ctaText}</button>
    </section>
  );
};
```

#### Best Practices
- Use functional components with TypeScript interfaces
- Implement proper prop typing
- Extract reusable logic into custom hooks
- Keep components focused and single-responsibility
- Use composition over inheritance

### 3. Portfolio Content Structure

Portfolio data should be centralized and typed:

```typescript
// src/data/portfolio.ts
export interface Project {
  id: string;
  title: string;
  description: string;
  technologies: string[];
  imageUrl: string;
  demoUrl?: string;
  githubUrl?: string;
  featured: boolean;
}

export interface Skill {
  category: string;
  items: string[];
}

export const projects: Project[] = [
  // Project data
];

export const skills: Skill[] = [
  // Skill data
];
```

### 4. Styling Guidelines

- **Responsive Design**: Mobile-first approach (320px → 1440px+)
- **Color Scheme**: Support light/dark mode via CSS variables or theme provider
- **Typography**: Use system font stack or carefully chosen web fonts
- **Spacing**: Consistent spacing scale (4px, 8px, 16px, 24px, 32px, 48px, 64px)
- **Accessibility**: WCAG 2.1 AA compliance minimum

### 5. Performance Optimization

- **Images**: Use modern formats (WebP, AVIF), implement lazy loading
- **Code Splitting**: Dynamic imports for route-based splitting
- **Bundle Size**: Monitor and optimize bundle size (< 100KB initial load)
- **Lighthouse Score**: Target 90+ for all metrics
- **SEO**: Implement proper meta tags, Open Graph, and structured data

### 6. Git Workflow

#### Branching Strategy
- `main` - Production-ready code
- `develop` - Development branch (if using Gitflow)
- `claude/*` - AI assistant working branches
- `feature/*` - Feature branches

#### Commit Message Convention
Follow Conventional Commits:
```
feat: add project showcase section
fix: resolve mobile navigation bug
docs: update README with setup instructions
style: format code with Prettier
refactor: reorganize component structure
test: add unit tests for contact form
chore: update dependencies
```

#### Before Committing
1. Run linter: `npm run lint`
2. Run tests: `npm test`
3. Check build: `npm run build`
4. Review changes: `git diff`

### 7. Development Scripts

Standard npm scripts to implement:

```json
{
  "scripts": {
    "dev": "vite dev",
    "build": "vite build",
    "preview": "vite preview",
    "lint": "eslint . --ext .ts,.tsx",
    "lint:fix": "eslint . --ext .ts,.tsx --fix",
    "format": "prettier --write \"src/**/*.{ts,tsx,css}\"",
    "type-check": "tsc --noEmit",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "test:coverage": "vitest --coverage"
  }
}
```

### 8. Required Configuration Files

When initializing, create these configurations:

#### `.gitignore`
```
node_modules/
dist/
.env
.env.local
.DS_Store
*.log
.vite/
.next/
out/
build/
```

#### `tsconfig.json`
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "jsx": "react-jsx",
    "strict": true,
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["src"],
  "exclude": ["node_modules"]
}
```

### 9. Portfolio-Specific Features

Essential sections for a client portfolio:

1. **Hero Section**: Name, tagline, CTA
2. **About Section**: Bio, photo, background
3. **Projects/Portfolio**: Showcase of work with filters
4. **Skills**: Technical skills organized by category
5. **Experience**: Work history and education (optional)
6. **Testimonials**: Client reviews (optional)
7. **Contact**: Email, social links, contact form
8. **Footer**: Copyright, links, additional info

### 10. Accessibility Requirements

- Semantic HTML elements (`<header>`, `<nav>`, `<main>`, `<footer>`)
- ARIA labels for interactive elements
- Keyboard navigation support (tab order, focus states)
- Alt text for all images
- Color contrast ratios (4.5:1 for text, 3:1 for large text)
- Screen reader testing

### 11. SEO Best Practices

```html
<!-- Required meta tags -->
<meta name="description" content="Portfolio description (155 chars)" />
<meta name="keywords" content="relevant, keywords" />
<meta name="author" content="Client Name" />

<!-- Open Graph -->
<meta property="og:title" content="Client Name - Portfolio" />
<meta property="og:description" content="Portfolio description" />
<meta property="og:image" content="/og-image.jpg" />
<meta property="og:url" content="https://portfolio-url.com" />

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="Client Name - Portfolio" />
```

### 12. Testing Strategy

- **Unit Tests**: Component logic, utility functions
- **Integration Tests**: Component interactions, data flow
- **E2E Tests**: Critical user journeys (navigation, contact form)
- **Visual Regression**: Screenshot comparisons (optional)
- **Accessibility Testing**: Automated a11y checks (axe, WAVE)

### 13. Deployment Checklist

Before deploying:
- [ ] All tests passing
- [ ] Lighthouse score > 90
- [ ] Accessibility audit passed
- [ ] Cross-browser testing (Chrome, Firefox, Safari, Edge)
- [ ] Mobile responsiveness verified
- [ ] Environment variables configured
- [ ] Analytics setup (if applicable)
- [ ] Error tracking configured (Sentry, etc.)
- [ ] Domain configured and SSL enabled
- [ ] README.md updated with live URL

### 14. AI Assistant Workflow

When working on this repository:

1. **Understand Context**: Review existing code and documentation
2. **Plan Changes**: Use TodoWrite to track multi-step tasks
3. **Follow Conventions**: Adhere to established patterns
4. **Write Quality Code**: Type-safe, tested, accessible
5. **Commit Frequently**: Atomic commits with clear messages
6. **Test Thoroughly**: Run tests and checks before committing
7. **Document Changes**: Update README.md and comments as needed
8. **Push to Branch**: Always push to `claude/*` branches

### 15. Common Tasks for AI Assistants

#### Initializing the Project
```bash
# Example: Initialize with Vite + React + TypeScript
npm create vite@latest . -- --template react-ts
npm install
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

#### Adding a New Section
1. Create component in `src/components/sections/`
2. Add data types in `src/types/`
3. Add content data in `src/data/`
4. Import and use in page
5. Add tests in `tests/`
6. Update documentation

#### Updating Styles
1. Follow established color scheme
2. Maintain responsive breakpoints
3. Test in light/dark mode
4. Verify accessibility

## Dependencies to Consider

### Core
- React 18+ or framework of choice
- TypeScript 5+
- Vite or Next.js

### Styling
- Tailwind CSS or styled-components
- Autoprefixer
- PostCSS

### Utilities
- clsx or classnames (for conditional classes)
- date-fns (date formatting)
- react-hook-form (forms)
- zod (validation)

### Animations
- Framer Motion
- GSAP (if complex animations needed)

### Testing
- Vitest or Jest
- Testing Library
- Playwright or Cypress (E2E)

### Development
- ESLint
- Prettier
- TypeScript ESLint
- Husky (git hooks)
- lint-staged

## Environment Variables

Create `.env.example`:
```env
# Analytics
VITE_GA_TRACKING_ID=
VITE_ANALYTICS_ID=

# Contact Form
VITE_FORM_ENDPOINT=
VITE_EMAIL_SERVICE_ID=

# Feature Flags
VITE_ENABLE_BLOG=false
VITE_ENABLE_TESTIMONIALS=true
```

## Resources

- [Web.dev - Best Practices](https://web.dev/learn)
- [MDN Web Docs](https://developer.mozilla.org/)
- [WCAG Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [React Documentation](https://react.dev/)

## Questions & Troubleshooting

### Q: What framework should I use?
**A**: For most portfolios, Next.js offers the best balance of features, performance, and SEO. For simpler portfolios, Vite + React is faster to set up.

### Q: How should I handle images?
**A**: Store in `public/images/`, use modern formats (WebP), implement lazy loading, and provide proper alt text.

### Q: Should I use a CMS?
**A**: For simple portfolios, local data files are sufficient. For clients who need to update content frequently, consider a headless CMS like Sanity or Contentful.

### Q: How do I handle contact forms?
**A**: Use a service like Formspree, Web3Forms, or implement with Netlify Forms. Avoid exposing email addresses directly.

## Maintenance

- Review and update dependencies monthly
- Test across browsers quarterly
- Audit performance and accessibility semi-annually
- Keep content fresh and relevant
- Monitor analytics and user feedback

---

**Last Updated**: 2025-11-19
**Version**: 1.0.0
**Status**: Template Uninitialized

This document should be updated as the project evolves and new conventions are established.

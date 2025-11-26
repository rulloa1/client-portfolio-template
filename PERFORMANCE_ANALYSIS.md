# Performance Analysis & Optimization Guide
## Client Portfolio Template

This document provides a comprehensive performance analysis framework and best practices for building a high-performance client portfolio website.

## Executive Summary

Portfolio websites must balance visual appeal with performance. Poor performance leads to:
- Higher bounce rates (53% of users abandon sites that take >3 seconds to load)
- Lower search rankings (Core Web Vitals are ranking factors)
- Reduced conversion rates
- Poor user experience on mobile devices

## Performance Budget Recommendations

### Target Metrics
```
First Contentful Paint (FCP):     < 1.8s
Largest Contentful Paint (LCP):   < 2.5s
Time to Interactive (TTI):         < 3.8s
Total Blocking Time (TBT):         < 200ms
Cumulative Layout Shift (CLS):     < 0.1
Speed Index:                       < 3.4s
```

### Resource Budget
```
Total Page Weight:     < 1.5 MB (initial load)
JavaScript Bundle:     < 300 KB (compressed)
CSS Bundle:           < 50 KB (compressed)
Images:               < 800 KB (initial viewport)
Fonts:                < 100 KB
HTML:                 < 50 KB
```

## Key Performance Areas

### 1. Asset Optimization

#### Images
**Issues to Avoid:**
- Uncompressed high-resolution images
- Wrong image formats (PNG for photos)
- Missing responsive images
- No lazy loading for below-fold images
- Lack of modern format support (WebP, AVIF)

**Best Practices:**
```
✓ Use WebP/AVIF with fallbacks
✓ Implement responsive images with srcset
✓ Lazy load images below the fold
✓ Use appropriate compression (80-85% quality)
✓ Serve correctly sized images (no 4K images scaled to 400px)
✓ Use CSS sprites or SVG for icons
✓ Implement blur-up or dominant color placeholders
```

**Recommended Tools:**
- ImageOptim, Squoosh, Sharp
- Cloudinary or Imgix for dynamic optimization
- Native lazy loading: `<img loading="lazy">`

#### Fonts
**Issues to Avoid:**
- Loading multiple font weights/variants
- No font-display strategy
- Blocking rendering waiting for fonts
- Large custom font files

**Best Practices:**
```
✓ Use font-display: swap
✓ Preload critical fonts
✓ Subset fonts to required characters
✓ Use system fonts when possible
✓ Limit to 2-3 font families max
✓ Use variable fonts to reduce requests
```

Example:
```html
<link rel="preload" href="/fonts/main.woff2" as="font" type="font/woff2" crossorigin>
<style>
  @font-face {
    font-family: 'Main';
    src: url('/fonts/main.woff2') format('woff2');
    font-display: swap;
  }
</style>
```

### 2. JavaScript Optimization

#### Bundle Size
**Issues to Avoid:**
- Shipping entire libraries for small features
- No tree-shaking or dead code elimination
- Duplicate dependencies
- Large polyfills for modern browsers

**Best Practices:**
```
✓ Code splitting by route
✓ Dynamic imports for heavy components
✓ Tree-shaking with ES modules
✓ Use modern JS for modern browsers
✓ Differential serving (modern + legacy bundles)
✓ Remove unused dependencies
✓ Use lighter alternatives (day.js vs moment.js)
```

#### Execution Performance
**Issues to Avoid:**
- Long tasks blocking main thread
- Unnecessary re-renders
- Inefficient animations
- Heavy JavaScript on initial load

**Best Practices:**
```
✓ Use Web Workers for heavy computation
✓ Debounce/throttle scroll and resize handlers
✓ Use CSS transforms for animations
✓ Implement virtual scrolling for long lists
✓ Minimize main thread work during load
✓ Use requestIdleCallback for non-critical work
```

### 3. CSS Optimization

**Issues to Avoid:**
- Large unused CSS (Bootstrap, Tailwind without purging)
- Render-blocking stylesheets
- CSS-in-JS runtime overhead
- Complex selectors and deep nesting

**Best Practices:**
```
✓ Critical CSS inlined in <head>
✓ Purge unused CSS (PurgeCSS, UnCSS)
✓ Use CSS containment
✓ Minimize render-blocking resources
✓ Use will-change sparingly
✓ Avoid @import in CSS
```

Example Critical CSS:
```html
<style>
  /* Inline critical styles for above-fold content */
  header { /* ... */ }
  .hero { /* ... */ }
</style>
<link rel="preload" href="/styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
```

### 4. Network Optimization

#### HTTP/2 and HTTP/3
**Best Practices:**
```
✓ Use HTTP/2 or HTTP/3
✓ Enable multiplexing
✓ Reduce domain sharding
✓ Use server push for critical resources (carefully)
```

#### Caching Strategy
**Headers:**
```
# Static Assets (hashed filenames)
Cache-Control: public, max-age=31536000, immutable

# HTML
Cache-Control: no-cache

# API Responses
Cache-Control: private, max-age=300
```

**Service Worker:**
```javascript
// Cache-first for static assets
// Network-first for HTML
// Stale-while-revalidate for API
```

#### Resource Hints
```html
<!-- DNS prefetch for third-party domains -->
<link rel="dns-prefetch" href="//analytics.example.com">

<!-- Preconnect to required origins -->
<link rel="preconnect" href="//fonts.googleapis.com">

<!-- Prefetch next page -->
<link rel="prefetch" href="/about">

<!-- Preload critical resources -->
<link rel="preload" href="/hero.jpg" as="image">
```

### 5. Rendering Optimization

#### Layout Stability (CLS)
**Issues to Avoid:**
- Images without dimensions
- Dynamically injected content
- Web fonts causing layout shifts
- Ads without reserved space

**Best Practices:**
```
✓ Set width/height on images and videos
✓ Reserve space for dynamic content
✓ Use aspect-ratio CSS property
✓ Avoid inserting content above existing content
```

Example:
```css
.image-container {
  aspect-ratio: 16 / 9;
}

img {
  width: 100%;
  height: auto;
}
```

#### Loading Strategy
**Progressive Enhancement:**
```
1. Load critical HTML/CSS first
2. Render above-fold content
3. Load JavaScript asynchronously
4. Lazy load below-fold content
5. Prefetch next-page resources
```

### 6. Framework-Specific Recommendations

#### React
```
✓ Use React.lazy() and Suspense
✓ Implement code splitting with dynamic imports
✓ Memoize expensive computations (useMemo)
✓ Prevent unnecessary re-renders (React.memo)
✓ Use React Server Components (Next.js 13+)
✓ Implement proper key props in lists
```

#### Next.js
```
✓ Use next/image for automatic optimization
✓ Implement ISR (Incremental Static Regeneration)
✓ Use getStaticProps for static pages
✓ Enable SWC compiler
✓ Use next/font for automatic font optimization
✓ Implement App Router for better performance
```

#### Vue
```
✓ Use async components
✓ Implement virtual scrolling (vue-virtual-scroller)
✓ Use v-memo for expensive renders
✓ Lazy load routes with dynamic imports
✓ Use Nuxt for SSR/SSG benefits
```

### 7. Third-Party Scripts

**Issues to Avoid:**
- Synchronous third-party scripts
- Multiple analytics tools
- Social media widgets blocking render
- Ad scripts without lazy loading

**Best Practices:**
```
✓ Load scripts asynchronously or defer
✓ Use Partytown for web workers
✓ Implement consent management
✓ Lazy load non-critical widgets
✓ Self-host when possible
```

Example:
```html
<!-- Bad -->
<script src="//analytics.example.com/script.js"></script>

<!-- Good -->
<script async src="//analytics.example.com/script.js"></script>

<!-- Better -->
<script type="text/partytown" src="//analytics.example.com/script.js"></script>
```

### 8. Mobile Optimization

**Critical Considerations:**
```
✓ Mobile-first design
✓ Touch target size (min 44x44px)
✓ Avoid fixed positioning for large elements
✓ Test on real devices, not just emulators
✓ Consider 3G/4G network conditions
✓ Optimize for battery usage
```

**Network Conditions:**
```javascript
// Adapt to network quality
if (navigator.connection?.effectiveType === '4g') {
  // Load high-quality assets
} else {
  // Load optimized assets
}
```

## Performance Monitoring

### Tools

**Development:**
- Lighthouse (Chrome DevTools)
- WebPageTest
- Chrome DevTools Performance Panel
- React DevTools Profiler
- Bundle Analyzer (webpack/vite/rollup)

**Production:**
- Google PageSpeed Insights
- Core Web Vitals (Search Console)
- Real User Monitoring (RUM)
  - Google Analytics
  - Sentry Performance
  - New Relic
  - Datadog

### Continuous Monitoring

**CI/CD Integration:**
```yaml
# Example GitHub Actions
- name: Lighthouse CI
  uses: treosh/lighthouse-ci-action@v9
  with:
    urls: |
      https://staging.example.com
    budgetPath: ./budget.json
    uploadArtifacts: true
```

**Performance Budget (budget.json):**
```json
{
  "timings": [
    {
      "metric": "interactive",
      "budget": 3800
    },
    {
      "metric": "first-contentful-paint",
      "budget": 1800
    }
  ],
  "resourceSizes": [
    {
      "resourceType": "script",
      "budget": 300
    },
    {
      "resourceType": "image",
      "budget": 800
    }
  ]
}
```

## Implementation Checklist

### Initial Setup
- [ ] Define performance budget
- [ ] Set up monitoring tools
- [ ] Configure build optimization
- [ ] Implement compression (Brotli/gzip)
- [ ] Enable HTTP/2 or HTTP/3

### Images & Media
- [ ] Implement responsive images
- [ ] Use modern formats (WebP, AVIF)
- [ ] Add lazy loading
- [ ] Optimize all images
- [ ] Add placeholders

### JavaScript
- [ ] Implement code splitting
- [ ] Configure tree-shaking
- [ ] Remove unused dependencies
- [ ] Minify and compress
- [ ] Use differential serving

### CSS
- [ ] Extract critical CSS
- [ ] Purge unused styles
- [ ] Minify and compress
- [ ] Avoid render-blocking

### Fonts
- [ ] Subset fonts
- [ ] Use font-display: swap
- [ ] Preload critical fonts
- [ ] Consider system fonts

### Caching
- [ ] Configure cache headers
- [ ] Implement service worker
- [ ] Use CDN for static assets
- [ ] Enable browser caching

### Monitoring
- [ ] Set up Lighthouse CI
- [ ] Monitor Core Web Vitals
- [ ] Implement error tracking
- [ ] Set up performance alerts

## Common Portfolio-Specific Issues

### Image Galleries
**Problem:** Heavy image galleries blocking interaction
**Solution:**
- Progressive image loading
- Intersection Observer for lazy loading
- Thumbnail generation
- Virtual scrolling for large galleries

### Animations
**Problem:** JavaScript animations causing jank
**Solution:**
- Use CSS transforms and opacity
- Leverage GPU acceleration
- Use Web Animations API
- Implement scroll-driven animations (CSS)

### Contact Forms
**Problem:** Heavy form validation libraries
**Solution:**
- Use native HTML5 validation
- Lightweight validation (Vest, Yup)
- Progressive enhancement

### Project Showcases
**Problem:** Loading all projects upfront
**Solution:**
- Pagination or infinite scroll
- Load on-demand with dynamic imports
- Prefetch on hover

## SEO Considerations

**Performance Impact on SEO:**
```
✓ Core Web Vitals are ranking factors
✓ Mobile-first indexing requires mobile performance
✓ Fast sites have better crawl budgets
✓ User experience signals affect rankings
```

**Best Practices:**
```
✓ Server-side rendering or static generation
✓ Semantic HTML
✓ Structured data (JSON-LD)
✓ Proper meta tags
✓ Sitemap and robots.txt
✓ Fast loading speeds
```

## Accessibility & Performance

**Overlapping Concerns:**
```
✓ Keyboard navigation performance
✓ Screen reader compatibility
✓ Focus management
✓ Reduced motion preferences
✓ Color contrast ratios
```

## Testing Strategy

### Performance Testing
```
1. Baseline metrics (current state)
2. Set targets based on budget
3. Implement optimizations
4. Measure improvements
5. Repeat and iterate
```

### Test Conditions
```
✓ Different devices (mobile, tablet, desktop)
✓ Various network speeds (3G, 4G, WiFi)
✓ Different browsers
✓ Throttled CPU
✓ Cold cache vs warm cache
```

## Resources

**Official Documentation:**
- [Web.dev Performance](https://web.dev/performance/)
- [MDN Web Performance](https://developer.mozilla.org/en-US/docs/Web/Performance)
- [Core Web Vitals](https://web.dev/vitals/)

**Tools:**
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [WebPageTest](https://www.webpagetest.org/)
- [PageSpeed Insights](https://pagespeed.web.dev/)
- [Chrome DevTools](https://developer.chrome.com/docs/devtools/)

**Communities:**
- [PerfPlanet](https://www.perfplanet.com/)
- [Web Performance Slack](https://webperformance.slack.com/)

## Next Steps

1. Choose your tech stack based on performance requirements
2. Set up performance monitoring from day one
3. Implement optimizations during development, not after
4. Regular performance audits
5. Track metrics over time

---

**Last Updated:** 2025-11-26
**Status:** Template - Ready for implementation

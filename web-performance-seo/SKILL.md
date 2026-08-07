---
name: web-performance-seo
description: Use when optimizing website speed, Core Web Vitals, SEO, or accessibility. Triggers on performance audits, Lighthouse scores, image optimization, caching, SEO meta tags, structured data, and search ranking improvements.
---

# Web Performance & SEO

## Core Web Vitals Focus

- LCP (Largest Contentful Paint): Aim under 2.5s
- INP (Interaction to Next Paint): Aim under 200ms
- CLS (Cumulative Layout Shift): Aim under 0.1

## Performance Checklist

- Optimize and properly size images (WebP/AVIF, responsive srcset).
- Use modern image components that handle lazy loading and priority.
- Minimize and defer non-critical JavaScript.
- Prefer server-side rendering or static generation for content pages.
- Enable compression (Brotli/Gzip) and proper caching headers.
- Reduce third-party script impact.
- Avoid layout shifts by reserving space for images, ads, and embeds.
- Use font-display: swap and preload critical fonts.

## SEO Essentials

- Unique, descriptive title tags (50-60 characters ideal).
- Meta descriptions that encourage clicks.
- Proper heading hierarchy (one H1 per page).
- Semantic HTML.
- Mobile-first responsive design.
- Fast loading on mobile networks.
- XML sitemap and robots.txt.
- Canonical tags when needed.
- Structured data (JSON-LD) for rich results where relevant.

## Technical SEO Checks

- Ensure all important pages are indexable.
- Fix broken links and redirect chains.
- Use HTTPS everywhere.
- Implement proper 404 and error pages.
- Monitor Core Web Vitals in Search Console and real-user monitoring.

## When Auditing a Site

1. Run Lighthouse (mobile + desktop).
2. Check Core Web Vitals from field data if available.
3. Review image sizes and formats.
4. Inspect JavaScript bundle size and main thread work.
5. Validate meta tags and structured data.
6. Test on real mobile devices or throttled network.

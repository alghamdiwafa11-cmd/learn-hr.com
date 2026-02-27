# HRSavvy Knowledge Hub - Deployment Guide

## Domain Configuration for hrkaf.co.uk

### Step 1: DNS Configuration in GoDaddy

1. Log in to your GoDaddy account
2. Go to **My Products** > **DNS** for hrkaf.co.uk
3. Add the following DNS records:

#### For Root Domain (hrkaf.co.uk):
```
Type: A
Name: @
Value: [Your hosting server IP]
TTL: 600 seconds
```

#### For WWW Subdomain:
```
Type: CNAME
Name: www
Value: hrkaf.co.uk
TTL: 600 seconds
```

### Step 2: Configure Vite for Production

Update `vite.config.ts`:

```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  base: '/', // For custom domain
  build: {
    outDir: 'dist',
    assetsDir: 'assets',
  }
})
```

### Step 3: Add CNAME File

Create `public/CNAME` file:
```
hrkaf.co.uk
```

### Step 4: Build and Deploy

```bash
npm run build
```

Upload the `dist` folder contents to your web hosting server.

---

## CIPD & SHRM Newsletter Integration

### Newsletter Configuration

The newsletter system is configured in `src/config/articles.ts`:

```typescript
export const externalSources = {
  cipd: {
    name: 'CIPD',
    feedUrl: 'https://www.cipd.co.uk/rss/news',
    website: 'https://www.cipd.co.uk',
    updateTypes: ['News', 'Research', 'Policy Updates', 'Events'],
  },
  shrm: {
    name: 'SHRM',
    feedUrl: 'https://www.shrm.org/rss/news',
    website: 'https://www.shrm.org',
    updateTypes: ['News', 'Resources', 'Legal Updates', 'Certification'],
  },
};

export const newsletterConfig = {
  frequency: 'weekly',
  sendDay: 'Tuesday',
  sections: [
    { id: 'featured', label: { en: 'Featured Article', ... } },
    { id: 'cipd', label: { en: 'CIPD Updates', ... } },
    { id: 'shrm', label: { en: 'SHRM Updates', ... } },
    { id: 'new', label: { en: 'New Articles', ... } },
    { id: 'resources', label: { en: 'Free Resources', ... } },
  ],
};
```

### Setting Up Newsletter Service

#### Option 1: Mailchimp Integration

1. Create a Mailchimp account
2. Set up an audience for HRKAF subscribers
3. Create automated RSS campaigns:
   - CIPD Feed: `https://www.cipd.co.uk/rss/news`
   - SHRM Feed: `https://www.shrm.org/rss/news`
4. Configure weekly send schedule

#### Option 2: Custom Newsletter Service

Use a backend service (Node.js/Python) to:
1. Fetch RSS feeds from CIPD and SHRM
2. Parse and format content
3. Send via email service (SendGrid/AWS SES)
4. Track opens and clicks

### Newsletter Sections

Each newsletter includes:

1. **Featured Article** - Editor's pick from HRKAF
2. **CIPD Updates** - Latest from CIPD
3. **SHRM Updates** - Latest from SHRM
4. **New Articles** - Recently published on HRKAF
5. **Free Resources** - Forms, formulas, frameworks

---

## AdSense Integration

### Step 1: Sign Up for AdSense

1. Go to https://www.google.com/adsense
2. Sign up with your Google account
3. Add site: `hrkaf.co.uk`
4. Verify site ownership

### Step 2: Create Ad Units

Create these ad units in AdSense:

| Slot Name | Size | Type |
|-----------|------|------|
| hero-bottom | 728x90 | Display |
| categories-bottom | 728x90 | Display |
| sidebar-1 | 300x600 | Display |
| sidebar-2 | 300x250 | Display |
| mobile-sticky | 320x100 | Display |
| footer-ad | 728x90 | Display |

### Step 3: Update Ad Component

Edit `src/components/AdBanner.tsx`:

```typescript
data-ad-client="ca-pub-YOUR_PUBLISHER_ID"
```

Replace with your actual AdSense Publisher ID.

### Step 4: Add AdSense Script

Add to `index.html` in `<head>`:

```html
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-YOUR_PUBLISHER_ID" crossorigin="anonymous"></script>
```

---

## SEO Optimization

### Meta Tags

Add to `index.html`:

```html
<meta name="description" content="HRKAF - Your comprehensive HR knowledge hub with 350+ articles, formulas, forms, and frameworks aligned with CIPD and SHRM standards.">
<meta name="keywords" content="HR, Human Resources, CIPD, SHRM, HR knowledge, HR articles, HR templates, HR formulas">
<meta property="og:title" content="HRKAF - HR Knowledge Hub">
<meta property="og:description" content="Master HR with expert knowledge from CIPD and SHRM aligned content.">
<meta property="og:url" content="https://hrkaf.co.uk">
<meta property="og:type" content="website">
```

### Sitemap

Create `public/sitemap.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://hrkaf.co.uk/</loc>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
  <!-- Add article URLs dynamically -->
</urlset>
```

### Robots.txt

Create `public/robots.txt`:

```
User-agent: *
Allow: /
Sitemap: https://hrkaf.co.uk/sitemap.xml
```

---

## Analytics Setup

### Google Analytics 4

Add to `index.html`:

```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-YOUR_TRACKING_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-YOUR_TRACKING_ID');
</script>
```

---

## Content Statistics

### Articles by Category

| Category | Article Count |
|----------|---------------|
| Talent Acquisition | 32 |
| Learning & Development | 38 |
| Employee Relations | 26 |
| Performance Management | 30 |
| Compensation & Benefits | 23 |
| Workplace Culture | 25 |
| Employee Engagement | 22 |
| Compliance & Policies | 28 |
| Delegation & Authority | 18 |
| Succession Planning | 19 |
| Career Path & OD | 24 |
| Grade & Titling | 17 |
| Job Evaluation | 20 |
| HR Strategy | 27 |
| Diversity & Inclusion | 21 |
| Wellbeing & Mental Health | 23 |
| Organization Sizing | 16 |
| Manpower Planning | 19 |
| Workforce Design | 17 |

### Resource Types

- **Formulas**: 45+ (ROI calculations, metrics, ratios)
- **Forms/Templates**: 60+ (Surveys, checklists, matrices)
- **Frameworks**: 50+ (Models, methodologies, systems)

---

## Maintenance

### Regular Updates

1. **Weekly**: Add 2-3 new articles
2. **Monthly**: Update CIPD/SHRM content
3. **Quarterly**: Review and update formulas

### Content Calendar

- Monday: Publish new article
- Tuesday: Send newsletter
- Wednesday: Social media promotion
- Thursday: Update resources
- Friday: Analytics review

---

## Support

For technical support or content suggestions:
- Email: support@hrkaf.co.uk
- Twitter: @hrkaf
- LinkedIn: HRKAF Knowledge Hub

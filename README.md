# 🌐 URL Status Checker

**Professional URL monitoring platform with enterprise features**

[![Live Site](https://img.shields.io/badge/Live-urlstatuschecker.com-aa336a)](https://urlstatuschecker.com)
[![AWS](https://img.shields.io/badge/AWS-Serverless-orange)](https://aws.amazon.com)
[![CloudFront](https://img.shields.io/badge/CDN-CloudFront-blue)](https://aws.amazon.com/cloudfront/)

## 🚀 Features

### Core Functionality
- ✅ **Batch URL Checking** - Check up to 100 URLs simultaneously
- ⭐ **One-Click Favorites** - Save and auto-check your frequently monitored URLs
- 📊 **Expandable Status Details** - Click any status code to see definitions, problems, and solutions
- 🎯 **Status Code Library** - Built-in reference for all HTTP status codes
- 📈 **Performance Metrics** - Response time tracking and analytics
- 🌍 **DNS Checker** - Global DNS propagation checking
- 📁 **File Upload** - Drag & drop TXT/CSV files with URL lists
- 📥 **CSV Export** - Download results for analysis

### User Experience
- 🎨 **Modern UI** - Clean gradient design with smooth animations
- 📱 **Mobile Responsive** - Works perfectly on all devices
- ⚡ **Real-Time Counter** - Live URL count with visual feedback
- 🔔 **Toast Notifications** - Non-intrusive status updates
- 💾 **Persistent Data** - Favorites synced to cloud via DynamoDB

### Authentication & Plans
- 🔐 **Google OAuth** - Secure sign-in
- 💳 **Stripe Integration** - Seamless subscription management
- 📊 **Usage Tracking** - Fair rate limiting per plan tier

## 💰 Pricing Tiers

| Plan | Price | URLs/Check | Daily Checks | Favorites | Features |
|------|-------|-----------|--------------|-----------|----------|
| **Free** | $0 | 100 | 4 | 100 | Basic checking |
| **Pro** | $29/mo | 100 | Unlimited | 1,000 | Email alerts, API access |
| **Business** | $99/mo | 500 | Unlimited | Unlimited | Team sharing, webhooks |

## 🏗️ Architecture

### Frontend
- **Hosting**: AWS S3 + CloudFront
- **Framework**: Vanilla JS with Tailwind CSS
- **CDN**: CloudFront distribution `E288YIEAU7MHGS`
- **Domain**: urlstatuschecker.com (via Route 53)

### Backend
- **API**: AWS API Gateway + Lambda (Node.js 18.x)
- **Database**: DynamoDB
  - `urlstatuschecker-api-users-production`
  - `urlstatuschecker-api-favorites-production`
  - `urlstatuschecker-api-urls-production`
  - `urlstatuschecker-api-status-history-production`
- **Authentication**: Google OAuth + JWT
- **Payments**: Stripe Checkout

### DNS & Infrastructure
- **DNS**: Route 53 Hosted Zone `Z0400631USJUHW2VUTMO`
- **SSL**: AWS Certificate Manager
- **Nameservers**:
  - ns-669.awsdns-19.net
  - ns-234.awsdns-29.com
  - ns-1798.awsdns-32.co.uk
  - ns-1448.awsdns-53.org

## 🔧 Deployment

### Frontend Deployment
```bash
# Upload to S3
aws s3 cp index.html s3://urlstatuschecker-com/ --content-type "text/html"

# Invalidate CloudFront cache
aws cloudfront create-invalidation --distribution-id E288YIEAU7MHGS --paths "/*"
```

### Backend Functions
- `urlstatuschecker-api-production-checkBatch` - Batch URL checking
- `urlstatuschecker-api-production-addFavorite` - Add favorites
- `urlstatuschecker-api-production-getFavorites` - Get user favorites
- `urlstatuschecker-api-production-googleAuth` - OAuth handler
- `urlstatuschecker-api-production-getDnsPropagation` - DNS checking

## 📊 Recent Improvements (Oct 2025)

### UI/UX Redesign
✅ Modern gradient buttons with hover effects
✅ One-click favorite stars (like bookmarks)
✅ Expandable status code cards with solutions
✅ 4-card summary dashboard (Online/Offline/Total/Avg Time)
✅ Smooth fade-in/slide-in animations
✅ Real-time URL counter (color-coded)
✅ Drag & drop file upload
✅ "Load Examples" quick start
✅ "Load Favorites" one-click button

### Infrastructure
✅ Route 53 hosted zone created
✅ DNS records configured
✅ CloudFront optimized
✅ S3 bucket updated
✅ Web3Forms integration

### Revenue Optimization
✅ Clearer upgrade CTAs
✅ Better value proposition
✅ Professional appearance
✅ Faster load times
✅ Mobile-friendly design

## 🎯 Revenue Strategy

### Conversion Optimization
1. **Free → Pro** - Email alerts, unlimited checks, API access
2. **Pro → Business** - Team features, webhooks, priority support
3. **Upsell Features** - DNS propagation maps, historical data, alerts

### Target Users
- **Developers** - Monitoring their APIs and services
- **DevOps Teams** - Infrastructure monitoring
- **Marketing Agencies** - Client website monitoring
- **E-commerce** - Uptime monitoring for critical pages

## 🔐 Environment Variables

```bash
# Google OAuth
GOOGLE_CLIENT_ID=242648112266-iglul54tuis9mhucsp1pmpqg0a48l8i0.apps.googleusercontent.com

# Stripe (Live Keys)
STRIPE_PUBLIC_KEY=pk_live_51Rn1ETROAbPuC5NwF4h3ybgj24JPO0Nz39f0uFHvXZ0Gt5gzWoasMjP09s16rfv2DVd8UqihGxx1Wyil0cqmIU6r00n9EI5ETs

# API
API_BASE=https://api.urlstatuschecker.com

# Web3Forms
FEEDBACK_ACCESS_KEY=941dde05-6699-4793-b170-fb81b1659e32
```

## 📝 Nameserver Migration (IONOS → Route 53)

### Steps:
1. Login to IONOS dashboard
2. Navigate to domain management for `urlstatuschecker.com`
3. Update nameservers to:
   - ns-669.awsdns-19.net
   - ns-234.awsdns-29.com
   - ns-1798.awsdns-32.co.uk
   - ns-1448.awsdns-53.org
4. Save changes (propagation takes 24-48 hours)
5. Verify with: `dig NS urlstatuschecker.com`

**Benefits:**
- ✅ 100% uptime SLA
- ✅ Fast DNS resolution (Route 53 anycast)
- ✅ Easy subdomain management
- ✅ AWS integration
- ✅ Health checks and routing policies

## 🛠️ Local Development

```bash
# Clone repository
git clone https://github.com/terrellflautt/urlstatuschecker.com.git

# Open in browser
open index.html

# Or use live server
npx live-server
```

## 📞 Support

- **Email**: snapitsaas@gmail.com
- **Feedback**: Built-in feedback form (Web3Forms)
- **Issues**: [GitHub Issues](https://github.com/terrellflautt/urlstatuschecker.com/issues)

## 🔗 Related Projects

- [SnapIT Forms](https://snapitforms.com) - Form builder
- [SnapIT URL](https://snapiturl.com) - URL shortener
- [SnapIT QR](https://snapitqr.com) - QR code generator
- [SnapIT Analytics](https://snapitanalytics.com) - Website analytics

## 📜 License

© 2025 SnapIT Software. All rights reserved.

---

**Built with ❤️ for 100% uptime monitoring**

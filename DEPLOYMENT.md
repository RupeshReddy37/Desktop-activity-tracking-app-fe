# Deployment Guide

## Prerequisites

- GitHub account with repository access
- Vercel account (sign up at [vercel.com](https://vercel.com))

## Deploying to Vercel

### Option 1: Automatic Deployment (Recommended)

1. **Sign in to Vercel**
   - Go to [vercel.com](https://vercel.com)
   - Click "Sign up" or "Sign in" with GitHub

2. **Import Project**
   - Click **"Add New"** → **"Project"**
   - Select **"Import Git Repository"**
   - Paste or select this repository: `RupeshReddy37/Desktop-activity-tracking-app-fe`

3. **Configure Build Settings**
   - **Framework Preset**: Vite (auto-detected)
   - **Build Command**: `npm run build` (auto-filled)
   - **Output Directory**: `dist` (auto-filled)
   - **Install Command**: `npm install` (auto-filled)

4. **Set Environment Variables**
   - Click **"Environment Variables"**
   - Add the following:
     ```
     VITE_API_BASE_URL=https://your-backend-api.com
     VITE_APP_NAME=Employee Monitoring Console
     VITE_ENABLE_CONSOLE_LOGS=false
     ```
   - Replace `https://your-backend-api.com` with your actual backend URL

5. **Deploy**
   - Click **"Deploy"**
   - Vercel will build and deploy your app automatically
   - Your app will be live at `https://<project-name>.vercel.app`

### Option 2: Deploy via Vercel CLI

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Link to existing project
vercel --prod
```

## Environment Variables Configuration

### Development (`.env`)
```env
VITE_API_BASE_URL=http://localhost:8081
VITE_APP_NAME=Employee Monitoring Console
VITE_ENABLE_CONSOLE_LOGS=true
```

### Production (Vercel Dashboard)
Set in **Settings** → **Environment Variables**:
```
VITE_API_BASE_URL=https://your-backend-api.com
VITE_APP_NAME=Employee Monitoring Console
VITE_ENABLE_CONSOLE_LOGS=false
```

## Auto-Deploy from GitHub

Once imported, Vercel will:
- **Automatically deploy** on every push to `main` branch
- **Create preview deployments** for pull requests
- **Rollback** on deployment failures

## Custom Domain Setup

1. Go to **Settings** → **Domains**
2. Enter your custom domain
3. Follow DNS configuration instructions
4. SSL certificate is automatically provisioned

## Backend Integration

Your frontend communicates with the backend via the `VITE_API_BASE_URL` environment variable.

**Important**: Ensure your backend:
1. Is accessible from the internet
2. Has CORS enabled for your Vercel domain
3. Uses HTTPS (required for secure cookies/tokens)

Example CORS configuration on backend:
```
Access-Control-Allow-Origin: https://your-app.vercel.app
Access-Control-Allow-Credentials: true
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
```

## Monitoring & Logs

View deployment logs:
1. Go to Vercel Dashboard
2. Select your project
3. Click **"Deployments"**
4. Click on any deployment to see logs

## Troubleshooting

### Build Failed: "VITE_API_BASE_URL is undefined"
- Ensure environment variables are set in Vercel dashboard
- Redeploy after setting environment variables

### CORS Errors in Browser Console
- Update `VITE_API_BASE_URL` to match your backend domain
- Ensure backend has CORS headers configured

### 404 Errors on Page Refresh
- Vercel automatically handles SPA routing
- Check that React Router is correctly configured

### Slow Performance
- Check Network tab in browser DevTools
- Verify Vercel Analytics is enabled
- Review bundle size: run `npm run build` locally

## Rollback to Previous Deployment

1. Go to **Deployments** in Vercel Dashboard
2. Find the deployment you want to restore
3. Click the **three dots** menu
4. Select **"Promote to Production"**

## Related Documentation

- [Vercel Vite Deployment Guide](https://vercel.com/docs/frameworks/vite)
- [Vercel Environment Variables](https://vercel.com/docs/concepts/projects/environment-variables)
- [Vercel Analytics](https://vercel.com/docs/analytics)

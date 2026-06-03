# HungryBites Deployment Guide - Render

## ✅ Completed Fixes

1. **✅ Improved MongoDB Connection** - Added proper error handling and timeouts
2. **✅ Better Server Error Handling** - Added server error listeners and proper logging
3. **✅ Health Check Endpoint** - Added `/health` endpoint for monitoring
4. **✅ CORS Configuration** - Proper origin validation from environment variables
5. **✅ Render Configuration** - Updated render.yaml with all required environment variables
6. **✅ Removed Frontend Serving** - Backend is now pure API server

## Deployed Apps Summary

### Production URLs
- **Customer Portal**: https://customer-gamma-six.vercel.app
- **Admin Portal**: https://admin-nine-lake-44.vercel.app
- **Restaurant Portal**: https://restaurant-olive-five.vercel.app
- **Landing Page**: https://hungrybites-landing-page.vercel.app
- **Backend API (Vercel)**: https://server-kappa-black-49.vercel.app

## Deploy to Render - Step by Step

### Option 1: Deploy via Render Dashboard (Recommended)

1. **Go to Render Dashboard**: https://dashboard.render.com

2. **Connect your GitHub repo**:
   - Click "New" → "Web Service"
   - Select your GitHub repository: `2303A52222/DEVOPS-FULL-STACK`
   - Click "Connect"

3. **Configure the service**:
   - **Name**: `hungrybites-backend`
   - **Root Directory**: `apps/api/server`
   - **Runtime**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Plan**: Free tier

4. **Set Environment Variables**:
   - **MONGODB_URI**: `mongodb+srv://2303a52222_db_user:NkmxXVIJgn8G9z1P@fooddelivery.ziyu76a.mongodb.net/food-del?appName=fooddelivery`
   - **JWT_SECRET**: `your_secret_key_here`
   - **STRIPE_SECRET_KEY**: `sk_test_123`
   - **STRIPE_PUBLISH_KEY**: `pk_test_123`
   - **FRONTEND_URL**: `https://customer-gamma-six.vercel.app`
   - **ADMIN_URL**: `https://admin-nine-lake-44.vercel.app`
   - **RESTAURANT_URL**: `https://restaurant-olive-five.vercel.app`
   - **CORS_ORIGIN**: `https://customer-gamma-six.vercel.app,https://admin-nine-lake-44.vercel.app,https://restaurant-olive-five.vercel.app`
   - **NODE_ENV**: `production`

5. **Click "Create Web Service"** and Render will deploy your backend!

### Option 2: Deploy via GitHub Push

The render.yaml file is already configured. Simply:
```bash
git push origin main
```

Render will automatically detect the render.yaml and deploy according to its configuration.

## After Deployment to Render

1. **Get the Render URL**: 
   - Navigate to your Render service dashboard
   - Copy the URL (e.g., `https://hungrybites-backend.onrender.com`)

2. **Update Frontend Apps Environment**:
   - Go to each frontend app in Vercel dashboard
   - Update `VITE_API_URL` to your new Render URL
   - Redeploy each frontend app

3. **Update Backend CORS**:
   - If needed, add the Render URL to the CORS_ORIGIN variable
   - Update render.yaml accordingly

## Test the Backend

Once deployed, test with:
```bash
# Health check
curl https://your-render-url.onrender.com/health

# API check
curl https://your-render-url.onrender.com/

# Get all food items
curl https://your-render-url.onrender.com/api/food/list
```

## Environment Variables Reference

| Variable | Purpose | Value |
|----------|---------|-------|
| MONGODB_URI | Database connection | Your MongoDB Atlas URL |
| JWT_SECRET | Token signing | Secure random string |
| STRIPE_SECRET_KEY | Stripe payments | Your Stripe test key |
| STRIPE_PUBLISH_KEY | Stripe frontend | Your Stripe publish key |
| CORS_ORIGIN | CORS whitelist | Comma-separated frontend URLs |
| FRONTEND_URL | Frontend reference | Customer app URL |
| ADMIN_URL | Admin reference | Admin app URL |
| RESTAURANT_URL | Restaurant reference | Restaurant app URL |
| NODE_ENV | Environment | `production` |
| PORT | Server port | `4000` |

## Troubleshooting

### MongoDB Connection Error
- ✅ Verify MongoDB Atlas IP whitelist (should include Render's IP: 0.0.0.0/0)
- ✅ Check MONGODB_URI environment variable is set correctly

### CORS Errors
- ✅ Verify CORS_ORIGIN includes all frontend URLs
- ✅ Remove trailing slashes from URLs

### 404 on API Routes
- ✅ Check service started successfully
- ✅ Verify root path is `apps/api/server`

## Next Steps

After successful Render deployment:
1. Update all frontend apps to point to new Backend URL
2. Test API connectivity from all portals
3. Monitor Render logs for any issues
4. Set up proper JWT_SECRET for production
5. Consider SSL/TLS certificates

---

**GitHub Repository**: https://github.com/2303A52222/DEVOPS-FULL-STACK
**render.yaml Location**: `/render.yaml`

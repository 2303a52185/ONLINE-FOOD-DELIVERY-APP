# Food Delivery App - Deployment Guide

This guide walks you through deploying your Food Delivery application to production using **Vercel** for Customer/Frontend, Admin, and Restaurant apps, and **Render** for Backend.

## Prerequisites
- GitHub account with repository pushed
- Vercel account (free tier available)
- Render account (free tier available)
- MongoDB Atlas account with connection string
- Stripe API keys (if using Stripe for payments)

## Part 1: Deploy Backend to Render

### Step 1: Push code to GitHub
```bash
git add .
git commit -m "Prepare for deployment"
git push origin main
```

### Step 2: Create Render account & connect GitHub
1. Go to [render.com](https://render.com)
2. Sign up / Login
3. Click "New+" → "Web Service"
4. Select your GitHub repository
5. Connect your GitHub account

### Step 3: Configure Environment Variables on Render
1. In the Render dashboard, find your "food-del-backend" service
2. Go to **Environment** tab
3. Add the following environment variables:
   - `MONGODB_URI`: Your MongoDB Atlas connection string
   - `JWT_SECRET`: A secret key for JWT (e.g., `your-secret-key-here`)
   - `STRIPE_SECRET_KEY`: Your Stripe secret key (if using payments)
   - `STRIPE_PUBLISH_KEY`: Your Stripe publishable key (if using payments)
   - `PORT`: Set to `4000`

### Step 4: Set Build & Start Commands
- **Build Command**: `npm install`
- **Start Command**: `npm start`

### Step 5: Deploy
Click "Deploy" and wait for the build to complete. Once done, you'll get a URL like:
```
https://food-del-backend.onrender.com
```

**Note the backend URL** - you'll need it for Frontend, Admin, and Restaurant deployment.

---

## Part 2: Deploy Frontend to Vercel

### Step 1: Create Vercel account & import project
1. Go to [vercel.com](https://vercel.com)
2. Sign up / Login with GitHub
3. Click "Add New..." → "Project"
4. Select your GitHub repository
5. Select the **Frontend** folder as root directory

### Step 2: Configure Environment Variables
1. Go to **Settings** → **Environment Variables**
2. Add:
   - **Key**: `VITE_API_URL`
   - **Value**: Your Render backend URL (e.g., `https://food-del-backend.onrender.com`)
   - Select "Development, Preview, Production"
3. Click "Save"

### Step 3: Deploy
Click "Deploy" - Vercel will automatically build and deploy your Frontend.

You'll get a URL like:
```
https://food-del-frontend.vercel.app
```

---

## Part 3: Deploy Admin to Vercel

### Step 1: Import Admin project
1. In Vercel dashboard, click "Add New..." → "Project"
2. Select your GitHub repository again
3. This time, select the **Admin** folder as root directory

### Step 2: Configure Environment Variables
Same as Frontend:
- **Key**: `VITE_API_URL`
- **Value**: Your Render backend URL (e.g., `https://food-del-backend.onrender.com`)

### Step 3: Deploy
Click "Deploy" - Vercel will deploy your Admin panel.

You'll get a URL like:
```
https://food-del-admin.vercel.app
```

---

## Part 4: Deploy Restaurant to Vercel

### Step 1: Import Restaurant project
1. In Vercel dashboard, click "Add New..." -> "Project"
2. Select your GitHub repository again
3. Select the **Restaurant** folder as root directory

### Step 2: Configure Environment Variables
Same as Frontend/Admin:
- **Key**: `VITE_API_URL`
- **Value**: Your Render backend URL (e.g., `https://food-del-backend.onrender.com`)

### Step 3: Deploy
Click "Deploy" - Vercel will deploy your Restaurant panel.

You'll get a URL like:
```
https://food-del-restaurant.vercel.app
```

---

## Step 4: Update .env Files for Production

After deployment, update your local .env files:

### Frontend/.env, Admin/.env & Restaurant/.env
```
VITE_API_URL=https://food-del-backend.onrender.com
```

### Backend/.env (if you create one locally)
```
MONGODB_URI=your_mongodb_atlas_connection_string
JWT_SECRET=your_secret_key
STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_PUBLISH_KEY=your_stripe_publish_key
PORT=4000
```

---

## Troubleshooting

### Backend not connecting to MongoDB
- ✅ Check MongoDB URI is correct in Render environment variables
- ✅ Ensure MongoDB Atlas IP whitelist includes Render's IP (usually "0.0.0.0/0" for free tier)

### Frontend getting CORS errors
- ✅ Verify `VITE_API_URL` environment variable is set correctly
- ✅ Check Backend CORS configuration allows requests from your Vercel domain
- ✅ In `Backend/server.js`, ensure CORS is properly configured

### Vercel deployment fails
- ✅ Check `package.json` has `"build"` script
- ✅ Verify `vercel.json` is configured correctly
- ✅ Check for console errors in Vercel logs

### Render deployment fails
- ✅ Ensure `render.yaml` is in root of Backend folder
- ✅ Check node version compatibility
- ✅ Verify `npm start` command works locally

---

## Live URLs After Deployment

| Service | URL |
|---------|-----|
| Frontend | `https://food-del-frontend.vercel.app` |
| Admin | `https://food-del-admin.vercel.app` |
| Restaurant | `https://food-del-restaurant.vercel.app` |
| Backend API | `https://food-del-backend.onrender.com` |

---

## Post-Deployment Checks

✅ Test user login & registration
✅ Test placing an order
✅ Test admin dashboard functionality
✅ Verify images load correctly
✅ Test payment flow (if using Stripe)
✅ Check console for any errors

---

## Keep Your App Updated

To push updates to production:
```bash
git add .
git commit -m "Your changes"
git push origin main
```

Both Vercel and Render will auto-redeploy on push!

---

## Support & Resources
- [Vercel Docs](https://vercel.com/docs)
- [Render Docs](https://render.com/docs)
- [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
- [Stripe Integration Guide](https://stripe.com/docs)

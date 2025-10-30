# Deployment Guide

## Prerequisites

Before deploying, ensure you have:

1. **Convex Account**: Sign up at [convex.dev](https://convex.dev)
2. **Clerk Account**: Sign up at [clerk.com](https://clerk.com)
3. **Vercel Account**: Sign up at [vercel.com](https://vercel.com)

## Step 1: Set Up Convex

\`\`\`bash
# Install Convex CLI globally
npm install -g convex

# Login to Convex
npx convex login

# Initialize Convex (creates a new project)
npx convex dev

# This will:
# - Create a new Convex project
# - Generate the convex/_generated files
# - Give you a CONVEX_URL
\`\`\`

Copy the `CONVEX_URL` from the output - you'll need this for environment variables.

## Step 2: Set Up Clerk

1. Go to [Clerk Dashboard](https://dashboard.clerk.com)
2. Create a new application
3. Go to **API Keys** section
4. Copy these values:
   - `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY`
   - `CLERK_SECRET_KEY`

5. Set up Clerk Webhook:
   - Go to **Webhooks** in Clerk Dashboard
   - Click **Add Endpoint**
   - Set endpoint URL to: `https://your-app.vercel.app/api/webhooks/clerk`
   - Subscribe to: `user.created`, `user.updated`, `user.deleted`
   - Copy the **Signing Secret** as `CLERK_WEBHOOK_SECRET`

## Step 3: Deploy to Vercel

### Option A: Deploy via Vercel Dashboard

1. Push your code to GitHub
2. Go to [Vercel Dashboard](https://vercel.com/dashboard)
3. Click **Add New Project**
4. Import your GitHub repository
5. Add environment variables:

\`\`\`env
NEXT_PUBLIC_CONVEX_URL=https://your-project.convex.cloud
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_SECRET_KEY=sk_test_...
CLERK_WEBHOOK_SECRET=whsec_...
\`\`\`

6. Click **Deploy**

### Option B: Deploy via CLI

\`\`\`bash
# Install Vercel CLI
npm install -g vercel

# Login to Vercel
vercel login

# Deploy
vercel

# Add environment variables
vercel env add NEXT_PUBLIC_CONVEX_URL
vercel env add NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY
vercel env add CLERK_SECRET_KEY
vercel env add CLERK_WEBHOOK_SECRET

# Deploy to production
vercel --prod
\`\`\`

## Step 4: Deploy Convex to Production

\`\`\`bash
# Deploy Convex functions to production
npx convex deploy --prod

# This will give you a production CONVEX_URL
# Update your Vercel environment variable with this new URL
\`\`\`

## Step 5: Update Clerk Webhook URL

After deployment, update your Clerk webhook endpoint URL to point to your production domain:
- `https://your-production-domain.vercel.app/api/webhooks/clerk`

## Step 6: Seed Initial Data

After deployment, seed your database with initial services:

\`\`\`bash
# Run the seed function via Convex dashboard
# Or use the Convex CLI:
npx convex run seed:seedServices
npx convex run seed:seedPricingKnowledge
\`\`\`

## Environment Variables Summary

| Variable | Description | Where to Get |
|----------|-------------|--------------|
| `NEXT_PUBLIC_CONVEX_URL` | Convex deployment URL | Convex Dashboard |
| `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` | Clerk publishable key | Clerk Dashboard → API Keys |
| `CLERK_SECRET_KEY` | Clerk secret key | Clerk Dashboard → API Keys |
| `CLERK_WEBHOOK_SECRET` | Clerk webhook signing secret | Clerk Dashboard → Webhooks |

## Troubleshooting

### Build Fails with "Cannot find module '@convex/_generated/api'"

**Solution**: Run Convex codegen before building:
\`\`\`bash
npx convex dev
# Wait for codegen to complete, then build
npm run build
\`\`\`

### Clerk Authentication Not Working

**Solution**: 
1. Verify all Clerk environment variables are set correctly
2. Check that webhook URL is correct and accessible
3. Ensure webhook is subscribed to `user.created` event

### Convex Functions Not Working

**Solution**:
1. Verify `NEXT_PUBLIC_CONVEX_URL` is set correctly
2. Run `npx convex deploy` to deploy functions
3. Check Convex dashboard for function logs

## Post-Deployment

1. **Create Admin User**: 
   - Sign up through your app
   - Go to Convex dashboard
   - Update the user's role to "admin" in the users table

2. **Create First Tenant**:
   - Login as admin
   - Navigate to business onboarding
   - Create your first auto detailing business

3. **Test QR Code Flow**:
   - Generate QR code for your tenant
   - Scan with mobile device
   - Complete self-assessment flow

## Monitoring

- **Convex Logs**: [dashboard.convex.dev](https://dashboard.convex.dev)
- **Vercel Logs**: [vercel.com/dashboard](https://vercel.com/dashboard)
- **Clerk Logs**: [dashboard.clerk.com](https://dashboard.clerk.com)

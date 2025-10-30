# Vercel Deployment Setup

## Critical: Convex Setup Required Before Deployment

Your build is failing because Convex needs to be initialized. Follow these steps:

## Step 1: Connect Convex Integration in v0

1. Click the **sidebar icon** (☰) on the left side of the chat
2. Select **"Connect"** from the menu
3. Find **Convex** in the integrations list
4. Click **"Connect"** and follow the prompts
5. This will automatically:
   - Create a Convex project
   - Generate the required files
   - Set the `NEXT_PUBLIC_CONVEX_URL` environment variable

## Step 2: Set Up Clerk Authentication

1. In the v0 sidebar, go to **"Vars"**
2. Add these environment variables:

### Required Clerk Variables

Get these from [Clerk Dashboard](https://dashboard.clerk.com):

\`\`\`
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_SECRET_KEY=sk_test_...
CLERK_WEBHOOK_SECRET=whsec_...
\`\`\`

**How to get these values:**

1. Go to [Clerk Dashboard](https://dashboard.clerk.com)
2. Create a new application (or select existing)
3. Go to **API Keys** → Copy the publishable and secret keys
4. Go to **Webhooks** → Add endpoint:
   - URL: `https://your-app.vercel.app/api/webhooks/clerk`
   - Events: Subscribe to `user.created`, `user.updated`, `user.deleted`
   - Copy the signing secret

## Step 3: Deploy

Once Convex is connected and Clerk variables are set:

1. Push your changes to GitHub
2. Vercel will automatically deploy
3. The build will now succeed

## Alternative: Manual Convex Setup

If you prefer to set up Convex manually:

\`\`\`bash
# Install Convex CLI
npm install -g convex

# Login to Convex
npx convex login

# Initialize and deploy
npx convex dev

# Copy the CONVEX_URL and add it to Vercel environment variables
\`\`\`

## Troubleshooting

### Error: "Cannot find module '@/convex/_generated/api'"

**Cause**: Convex hasn't generated the API files yet.

**Solution**: Connect the Convex integration in v0 (see Step 1 above).

### Error: "Cannot find module '@tailwindcss/postcss'"

**Cause**: Missing Tailwind CSS PostCSS plugin.

**Solution**: Already fixed in package.json. Just redeploy.

### Clerk Authentication Not Working

**Cause**: Missing or incorrect Clerk environment variables.

**Solution**: 
1. Verify all three Clerk variables are set in the Vars section
2. Check that the webhook URL matches your deployed domain
3. Ensure webhook is active and subscribed to user events

## Post-Deployment Checklist

- [ ] Convex integration connected
- [ ] All Clerk environment variables set
- [ ] Webhook URL updated to production domain
- [ ] First deployment successful
- [ ] Can sign in/sign up
- [ ] Database queries working

## Need Help?

If you're still having issues:
1. Check the Vercel deployment logs
2. Check the Convex dashboard for function errors
3. Verify all environment variables are set correctly
4. Open a support ticket at [vercel.com/help](https://vercel.com/help)

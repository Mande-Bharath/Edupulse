# Vercel Deployment Guide for EduPulse

## Prerequisites

- [GitHub](https://github.com) account with your code pushed
- [Vercel](https://vercel.com) account
- Your Supabase credentials
- Your Gemini API key

## Step 1: Prepare Your Repository

### 1.1 Initialize Git (if not already done)
```bash
cd /Users/mandebharath/Downloads/edupulse-main
git init
git add .
git commit -m "Initial commit - ready for Vercel deployment"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/edupulse.git
git push -u origin main
```

### 1.2 Create Environment Variables File
Add `.env.local` to your `.gitignore` (already in repo):
```bash
echo ".env.local" >> .gitignore
```

Create `.env.local` with your actual values:
```
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your_anon_key_here
VITE_GEMINI_API_KEY=your_gemini_key_here
SUPABASE_SERVICE_KEY=your_service_role_key_here
CORS_ALLOWED_ORIGINS=https://edupulse.vercel.app
```

## Step 2: Deploy to Vercel

### 2.1 Connect GitHub to Vercel
1. Go to [vercel.com](https://vercel.com)
2. Click **"Add New..."** → **"Project"**
3. Click **"Import Git Repository"**
4. Select your GitHub account and the `edupulse` repository
5. Click **"Import"**

### 2.2 Configure Environment Variables
In the Vercel dashboard for your project:

1. Go to **Settings** → **Environment Variables**
2. Add the following variables:

| Key | Value | Environments |
|-----|-------|--------------|
| `VITE_SUPABASE_URL` | Your Supabase Project URL | Production, Preview, Development |
| `VITE_SUPABASE_ANON_KEY` | Your Supabase Anon Key | Production, Preview, Development |
| `VITE_GEMINI_API_KEY` | Your Gemini API Key | Production, Preview, Development |
| `SUPABASE_URL` | Your Supabase Project URL | Production, Preview, Development |
| `SUPABASE_SERVICE_KEY` | Your Supabase Service Role Key | Production, Preview, Development |
| `CORS_ALLOWED_ORIGINS` | `https://your-deployment.vercel.app` | Production, Preview |
| `NODE_ENV` | `production` | Production |

### 2.3 Configure Build Settings (Optional)
The `vercel.json` file is pre-configured, but you can customize in Vercel dashboard:
- **Root Directory**: Leave as `.`
- **Build Command**: `npm run build && npm --prefix backend run build`
- **Output Directory**: `dist`

### 2.4 Deploy
1. Click **"Deploy"** button
2. Vercel will automatically build and deploy your project
3. You'll get a URL like `https://edupulse-xxxxx.vercel.app`

## Step 3: Verify Deployment

### 3.1 Test Frontend
- Visit your Vercel URL
- Verify the React app loads correctly
- Test login functionality

### 3.2 Test Backend API
```bash
curl https://your-deployment.vercel.app/api/health
# Should return: {"ok":true,"service":"superadmin-settings-api"}
```

### 3.3 Check Logs
In Vercel dashboard:
1. Go to your project
2. Click **"Deployments"**
3. Click on the latest deployment
4. View **"Logs"** for any errors

## Step 4: Set Custom Domain (Optional)

1. In Vercel dashboard, go to **Settings** → **Domains**
2. Add your custom domain (e.g., `edupulse.com`)
3. Follow DNS configuration instructions
4. Update `CORS_ALLOWED_ORIGINS` in environment variables with your new domain

## Troubleshooting

### Build Fails
- Check **Deployment Logs** in Vercel dashboard
- Ensure all environment variables are set
- Run locally: `npm run build` and `npm --prefix backend run build`

### API Routes Return 404
- Verify `/api/backend.ts` exists
- Check `vercel.json` rewrites configuration
- Verify backend routes are correctly exported

### CORS Errors
- Update `CORS_ALLOWED_ORIGINS` environment variable
- Check `backend/src/server.ts` CORS configuration
- Ensure frontend uses correct API base URL

### Supabase Connection Issues
- Verify `VITE_SUPABASE_URL` and keys are correct
- Check Supabase project is active
- Ensure RLS policies allow API access

## Environment Variable Reference

### Frontend Variables (Must have `VITE_` prefix)
- `VITE_SUPABASE_URL` - Your Supabase project URL
- `VITE_SUPABASE_ANON_KEY` - Public API key for client-side
- `VITE_GEMINI_API_KEY` - Google Gemini API key

### Backend Variables (No prefix)
- `SUPABASE_URL` - Supabase project URL
- `SUPABASE_SERVICE_KEY` - Service role key (more permissions)
- `CORS_ALLOWED_ORIGINS` - Comma-separated list of allowed origins
- `PORT` - Backend port (optional, defaults to 4000 locally)

## Local Development After Setup

```bash
# Install dependencies
npm install
npm --prefix backend install

# Create .env.local
cp .env.example .env.local
# Edit .env.local with your values

# Run dev server
npm run dev

# In another terminal, run backend
npm --prefix backend run dev
```

## Production Best Practices

1. **Security**
   - Never commit `.env.local`
   - Use Vercel environment variables for secrets
   - Rotate keys periodically

2. **Monitoring**
   - Enable Vercel Analytics
   - Set up error tracking (Sentry)
   - Monitor API logs in Supabase dashboard

3. **Database**
   - Enable Supabase backups
   - Set up Row-Level Security (RLS) policies
   - Monitor database logs

4. **Performance**
   - Enable caching in Vercel
   - Optimize images and assets
   - Use Vercel Analytics to identify bottlenecks

## Additional Resources

- [Vercel Documentation](https://vercel.com/docs)
- [Supabase Documentation](https://supabase.com/docs)
- [Vite Documentation](https://vitejs.dev/)
- [React Documentation](https://react.dev/)

---

**Need Help?**
- Check Vercel deployment logs
- Review your environment variables
- Run `npm run build` locally to test build process

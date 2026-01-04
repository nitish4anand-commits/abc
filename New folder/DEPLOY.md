# Deploy Astro Kundli to Vercel + Railway

## Prerequisites
- GitHub account with your astro repo pushed
- Vercel account (free, sign in with GitHub)
- Railway account (free, sign in with GitHub)

---

## Step 1: Deploy Frontend to Vercel

1. Go to https://vercel.com and sign in with GitHub
2. Click **"New Project"**
3. Select your `astro` repository
4. Configure:
   - **Framework Preset**: Next.js
   - **Project Root**: `apps/web` (important for monorepo!)
   - **Install Command**: `npm install` or `pnpm install`
   - **Build Command**: `npm run build` or `pnpm build`
   - **Output Directory**: `.next`
5. Click **Deploy**
6. Wait for deployment (2-5 minutes)
7. **Copy the Vercel URL** (e.g., `https://astro-xyz.vercel.app`)

---

## Step 2: Deploy Backend to Railway

1. Go to https://railway.app and sign in with GitHub
2. Click **"Create New Project"** → **"Deploy from GitHub repo"**
3. Select your `astro` repository
4. Railway will auto-detect services; configure:
   - Select the Python runtime
   - **Root Directory**: `apps/api`
   - **Start Command**: `uvicorn apps.api.app_main:app --host 0.0.0.0 --port $PORT`
   - (Or use the Dockerfile if Railway detects it)
5. Add environment variables (optional for now):
   - `API_PORT=8000` (Railway assigns port automatically via $PORT)
6. Click **Deploy**
7. Wait for deployment (2-5 minutes)
8. **Copy the Railway API URL** from the deployment (e.g., `https://api-xyz.railway.app`)

---

## Step 3: Connect Frontend to Backend

1. Go back to **Vercel Dashboard** → select your `astro` project
2. Click **Settings** → **Environment Variables**
3. Add a new variable:
   - **Name**: `NEXT_PUBLIC_API_BASE`
   - **Value**: Paste your Railway API URL (e.g., `https://api-xyz.railway.app`)
   - **Environments**: Select **Production** and **Preview**
4. Click **Save**
5. Click **Deployments** → select the latest deployment → **Redeploy**
6. Wait for redeploy to complete (1-2 minutes)

---

## Step 4: Test the Live Preview

1. Open your Vercel URL in browser: `https://astro-xyz.vercel.app/create-kundli`
2. Fill the form:
   - Name: "Test User"
   - Date/Time: "2020-01-01T12:00"
   - Place: "Mumbai, India" (autocomplete will suggest)
   - Click **Create Kundli**
3. You should see:
   - ✅ Success message with chart ID
   - ✅ Redirect to `/kundli/{id}` dashboard
   - ✅ Birth details, planets, mahadasha timeline loaded from backend

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Build fails on Vercel | Check logs: Vercel Dashboard → Deployments → click failed build → View Logs |
| 404 errors on `/v1/chart` | Verify `NEXT_PUBLIC_API_BASE` is set correctly and Railway is running (check Railway dashboard) |
| CORS errors | Add CORS headers in `apps/api/entrypoint.py`: `from fastapi.middleware.cors import CORSMiddleware` |
| Form submit hangs | Check browser console (F12) for errors; ensure API URL is reachable |
| pyswisseph missing on Railway | Expected; backend runs in graceful mode (no ephemeris precision). Install native lib later if needed |

---

## Next Steps (Optional Polish)

- Add a custom domain to Vercel (Settings → Domains)
- Enable automatic deployments on git push (default in Vercel)
- Monitor API logs in Railway dashboard
- Add more golden test cases and run them in CI

---

## Live URL Once Deployed
Your frontend: `https://astro-<hash>.vercel.app`
Your backend: `https://api-<hash>.railway.app`

Share the Vercel URL with anyone to show the working kundli app! 🚀

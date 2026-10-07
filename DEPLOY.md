# Deployment Guide - AI Lost & Found Matcher

## Quick Deploy to Render.com (Free)

### Option 1: One-Click Deploy (Recommended)

1. Fork this repository to your GitHub account
2. Go to [Render.com](https://render.com) and sign up for a free account
3. Click **"New +"** → **"Web Service"**
4. Connect your GitHub repository
5. Render will auto-detect the `render.yaml` and configure everything
6. Click **"Create Web Service"**
7. Wait for the build to complete (~2-3 minutes)
8. Your app will be live at `https://your-app-name.onrender.com`

### Option 2: Manual Setup

1. Create a free account at [Render.com](https://render.com)
2. Click **"New +"** → **"Web Service"**
3. Select **"Build and deploy from a Git repository"**
4. Connect your GitHub repo
5. Configure:
   - **Name**: `ai-lost-found-matcher` (or your preferred name)
   - **Environment**: `Node`
   - **Build Command**: `npm ci && npm run build`
   - **Start Command**: `npm start`
   - **Health Check Path**: `/api/health`
6. Add Environment Variables:
   - `NODE_ENV` = `production`
   - `JWT_SECRET` = (click "Generate" for a secure random value)
   - `PORT` = `4000`
7. **Important**: Add a Persistent Disk:
   - Click **"Add Disk"**
   - **Name**: `data`
   - **Mount Path**: `/opt/render/project/src/server/data`
   - **Size**: `1 GB`
8. Click **"Create Web Service"**

---

## What Gets Deployed

- **Frontend**: React + Vite (served as static files from Express)
- **Backend**: Node.js + Express API
- **Database**: SQLite (sql.js WASM) - persists to disk
- **File Uploads**: Stored in `/uploads` (also on persistent disk)

---

## Demo Credentials (After First Deploy)

The app auto-seeds demo accounts on first run:

| Role | Email | Password |
|------|-------|----------|
| Admin | admin@lostfound.demo | Admin@123 |
| User | demo@lostfound.demo | Demo@123 |

**Change these passwords immediately after first login!**

---

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `NODE_ENV` | Yes | Set to `production` |
| `JWT_SECRET` | Yes | Secure random string (32+ chars) |
| `PORT` | No | Defaults to 4000 |

---

## Local Development

```bash
# Install dependencies
npm install

# Start both frontend (Vite) and backend
npm run dev

# Or separately:
npm run server  # Backend on http://localhost:4000
npm run client  # Frontend on http://localhost:5173
```

---

## Production Build

```bash
npm run build    # Creates dist/ folder
npm start        # Runs production server
```

---

## Troubleshooting

### Build Fails
- Check Node version (requires 18+)
- Ensure all dependencies are in `package.json`

### Database Issues
- The persistent disk keeps data between deploys
- If you need to reset: delete the disk and re-create it

### Health Check Fails
- Ensure `/api/health` returns 200 OK
- Check Render logs for startup errors

### File Uploads Not Working
- Verify the persistent disk is mounted at `/opt/render/project/src/server/data`
- The `uploads` folder is created automatically

---

## Free Tier Limits (Render.com)

- **750 hours/month** (enough for 24/7)
- **512 MB RAM**
- **0.1 CPU**
- **1 GB persistent disk**
- **Spins down after 15 min inactivity** (cold start ~30s)

---

## Alternative Free Hosting

| Platform | Pros | Cons |
|----------|------|------|
| **Railway** | $5/month credit, easy | Limited free tier |
| **Fly.io** | Free allowance, global | More complex setup |
| **Vercel + Supabase** | Great for static + DB | Requires separate services |
| **Netlify + Neon** | Good DX | PostgreSQL only |

---

## Support

- Check Render logs: Dashboard → Your Service → Logs
- Health check: `https://your-app.onrender.com/api/health`
- API base: `https://your-app.onrender.com/api`
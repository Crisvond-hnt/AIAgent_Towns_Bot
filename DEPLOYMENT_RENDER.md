# 🚀 BeaverDev - Render.com Deployment Guide

## Quick Fix for Current Error

The error `Module not found "dist/index.js"` means the build step didn't run. Here's how to fix it:

### Option 1: Update Build & Start Commands in Render Dashboard

1. Go to your Render dashboard: https://dashboard.render.com
2. Select your BeaverDev service
3. Go to **Settings** tab
4. Update these settings:

**Build Command:**
```bash
bun install && bun run build
```

**Start Command:**
```bash
bun run start
```

5. Click **Save Changes**
6. Trigger a **Manual Deploy**

### Option 2: Use render.yaml (Recommended)

We've included a `render.yaml` file in the repository. Render will automatically detect it.

1. Push the updated code:
```bash
git add render.yaml DEPLOYMENT_RENDER.md
git commit -m "Add Render configuration file"
git push origin master
```

2. Render will automatically use the configuration from `render.yaml`

## Complete Render.com Deployment Steps

### Step 1: Connect Repository

1. Go to https://dashboard.render.com
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub account
4. Select repository: `Crisvond-hnt/Botaday6`

### Step 2: Configure Service

If not using `render.yaml`, manually configure:

**Basic Settings:**
- **Name**: `beaverdev-bot` (or your choice)
- **Region**: Choose closest to your users
- **Branch**: `master`
- **Runtime**: Node

**Build & Deploy:**
- **Build Command**: `bun install && bun run build`
- **Start Command**: `bun run start`

**Instance Type:**
- Free tier (for testing)
- Or Starter ($7/month for production)

### Step 3: Environment Variables

Click **"Environment"** tab and add:

```
APP_PRIVATE_DATA=<your_base64_bot_credentials>
JWT_SECRET=<your_jwt_secret>
OPENAI_API_KEY=<your_openai_api_key>
PORT=5124
NODE_ENV=production
```

⚠️ **Important**: Mark `APP_PRIVATE_DATA`, `JWT_SECRET`, and `OPENAI_API_KEY` as **Secret** (eye icon)

### Step 4: Deploy

1. Click **"Create Web Service"**
2. Render will:
   - Install Bun
   - Run `bun install`
   - Run `bun run build` (creates dist/index.js)
   - Run `bun run start` (starts the bot)

### Step 5: Watch Build Logs

You should see:
```
==> Running 'bun install'
==> Running 'bun run build'
$ bun build src/index.ts --outdir dist --target node --format esm --minify
==> Build successful

==> Running 'bun run start'
$ NODE_ENV=production bun dist/index.js
🔍 Validating AGENTS.md...
✅ AGENTS.md validated (2445 lines, 234.56 KB)
📚 Loaded knowledge source: Towns Bot SDK Complete Guide (AGENTS.md)
🦫⚡ BeaverDev - Ultimate Towns Bot SDK Assistant
🚀 Server started on port 5124
Ready to answer questions with sass and style! ☕💻
```

### Step 6: Configure Webhook in Towns

1. Copy your Render URL: `https://beaverdev-bot-xxxx.onrender.com`
2. Go to https://app.alpha.towns.com/developer
3. Find your bot
4. Update webhook URL to: `https://beaverdev-bot-xxxx.onrender.com/webhook`
5. Save changes

### Step 7: Register Slash Commands

From your local machine:
```bash
bunx towns-bot update-commands src/commands.ts YOUR_BEARER_TOKEN
```

## Troubleshooting

### Error: "Module not found dist/index.js"

**Cause**: Build command didn't run or failed

**Solution**:
1. Check Build Command is set: `bun install && bun run build`
2. Check build logs for errors
3. Ensure `tsconfig.json` and `package.json` are committed to git
4. Trigger manual deploy

### Error: "AGENTS.md not found"

**Cause**: AGENTS.md not committed to git or not in repository root

**Solution**:
```bash
git add AGENTS.md
git commit -m "Add AGENTS.md knowledge source"
git push origin master
```

### Error: "Missing environment variables"

**Cause**: Environment variables not configured in Render

**Solution**:
1. Go to Render dashboard → Your service → Environment
2. Add all required variables
3. Redeploy

### Error: "OpenAI API error"

**Cause**: Invalid API key or rate limits

**Solution**:
1. Verify `OPENAI_API_KEY` is correct
2. Check OpenAI account has credits
3. Check API key permissions

### Free Tier Sleeps After Inactivity

**Issue**: Render free tier spins down after 15 minutes of inactivity

**Solutions**:
- Upgrade to paid plan ($7/month) for always-on
- Use a ping service (not recommended for bots)
- Accept cold starts (15-30 second delay on first request)

## Health Check

Visit your bot's health endpoint:
```
https://beaverdev-bot-xxxx.onrender.com/health
```

Should return:
```json
{
  "status": "ok",
  "bot": {
    "id": "0x...",
    "appAddress": "0x...",
    "commands": 4
  },
  "knowledge": {
    "sources": [...]
  }
}
```

## Logs

View logs in Render dashboard:
1. Go to your service
2. Click **"Logs"** tab
3. Watch for errors or issues

## Updating Your Bot

```bash
# Make changes locally
git add .
git commit -m "Update bot"
git push origin master

# Render auto-deploys on push
# Watch the deployment in Render dashboard
```

## Production Checklist

- [ ] Build command set: `bun install && bun run build`
- [ ] Start command set: `bun run start`
- [ ] All environment variables configured
- [ ] AGENTS.md committed to repository
- [ ] Webhook URL configured in Towns dashboard
- [ ] Slash commands registered
- [ ] Health endpoint returns 200 OK
- [ ] Bot responds to mentions in Towns
- [ ] Logs show no errors

## Support

**Render Documentation**: https://render.com/docs  
**Render Troubleshooting**: https://render.com/docs/troubleshooting-deploys  
**Towns Bot SDK**: https://docs.towns.com/

🦫 Happy deploying! Build amazing things with Towns Protocol!


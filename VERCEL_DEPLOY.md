# 🚀 Deploy to Vercel - Step by Step Guide

Complete guide to deploy Lua Obfuscator Pro to Vercel for free!

## Prerequisites

- ✅ GitHub account (https://github.com)
- ✅ Vercel account (https://vercel.com) - free tier
- ✅ This repository pushed to GitHub
- ✅ Git installed locally

## Step 1: Push Code to GitHub

### 1.1 Create GitHub Repository

1. Go to https://github.com/new
2. Name: `lua-obfuscator-vercel`
3. Description: "Strong Lua Obfuscator Pro - Vercel Serverless"
4. Click "Create repository"

### 1.2 Push Code

```bash
# Clone this project locally
cd lua-obfuscator-vercel

# Initialize git
git init

# Add all files
git add .

# Commit
git commit -m "Initial: Lua Obfuscator Pro for Vercel"

# Add remote
git remote add origin https://github.com/YOUR_USERNAME/lua-obfuscator-vercel.git

# Push to GitHub
git branch -M main
git push -u origin main
```

✅ Your code is now on GitHub!

## Step 2: Connect to Vercel

### 2.1 Go to Vercel

1. Visit https://vercel.com
2. Sign in or create account
3. Can use GitHub login for quick signup

### 2.2 Import Project

1. Click "Add New" button → "Project"
2. Click "Import Git Repository"
3. Find your `lua-obfuscator-vercel` repo
4. Click "Import"

### 2.3 Configure Project

**Project Name:** lua-obfuscator-vercel

**Framework Preset:** Choose "Other" (we're using Flask with Python)

**Build Command:** 
```
pip install -r requirements.txt
```

**Output Directory:** Leave blank

**Environment Variables:** 
- Leave as default (already in vercel.json)

Click "Deploy"

⏳ Vercel will build and deploy! (Takes 2-5 minutes)

## Step 3: Verify Deployment

### 3.1 Get Your URL

After deployment completes:
1. Go to your Vercel Dashboard
2. Click on your `lua-obfuscator-vercel` project
3. Copy your domain (looks like: `https://lua-obfuscator-vercel.vercel.app`)

### 3.2 Test It

Visit your URL in browser - you should see the web UI! 🎉

### 3.3 Test API

```bash
curl -X POST https://YOUR_VERCEL_URL.vercel.app/api/process \
  -H "Content-Type: application/json" \
  -d '{"code":"local x=10","mode":"obfuscate","level":3}'
```

Should return JSON with obfuscated code.

## Step 4: Custom Domain (Optional)

### 4.1 Connect Domain

1. In Vercel Dashboard → Settings → Domains
2. Add your custom domain (e.g., `obfuscator.yourdomain.com`)
3. Follow DNS configuration instructions
4. SSL automatically generated!

## Step 5: Continuous Deployment

### 5.1 Auto Deploy on Push

After first deployment, every time you push to GitHub:

```bash
git add .
git commit -m "Update: better obfuscation"
git push origin main
```

Vercel automatically redeploys! ✨

### 5.2 View Logs

1. Vercel Dashboard → Your Project
2. Click "Deployments" tab
3. Click deployment to see build/runtime logs
4. Troubleshoot issues from logs

## Step 6: Monitor & Optimize

### 6.1 Check Performance

1. Dashboard → Analytics tab
2. View:
   - Total requests
   - Response times
   - Error rates
   - Bandwidth usage

### 6.2 Upgrade if Needed

Free Tier is enough for most use cases, but:
- **Pro Plan** ($20/month): Faster builds, better support
- **Enterprise**: Custom solutions

## Troubleshooting

### ❌ Build Failed

**Error: "Module not found"**
- Check `requirements.txt` lists all dependencies
- Verify no typos in package names
- Rebuild: Dashboard → Deployments → Redeploy

**Error: "vercel.json invalid"**
- Verify JSON syntax (use https://jsonlint.com)
- Check file is at project root
- Commit and push again

### ❌ Runtime Errors

**"Internal Server Error (500)"**
1. Check Vercel logs: Dashboard → Logs → Runtime
2. Look for error messages
3. Fix code locally, push to GitHub
4. Vercel auto-redeploys

**"Function timed out"**
- Increase timeout in `vercel.json` (max 30s)
- Use Level 1-2 for faster processing
- Optimize obfuscation code

### ❌ UI Not Loading

**Blank page or 404**
1. Check URL is correct
2. Open browser console (F12)
3. Look for error messages
4. Check Vercel deployment succeeded

**Slow loading**
- First request cold start (~300-500ms) is normal
- Subsequent requests are fast (<50ms)
- Vercel caches frequently used functions

## Usage Examples

### Example 1: Web UI
1. Visit your Vercel URL
2. Paste Lua code
3. Click "Process Code"
4. Copy result

### Example 2: API
```bash
# Obfuscate
curl -X POST https://your-url.vercel.app/api/process \
  -H "Content-Type: application/json" \
  -d '{
    "code": "local x = 10\nprint(x)",
    "mode": "obfuscate",
    "level": 3
  }'

# Minify
curl -X POST https://your-url.vercel.app/api/process \
  -H "Content-Type: application/json" \
  -d '{
    "code": "local x = 10\nprint(x)",
    "mode": "minify"
  }'

# Health check
curl https://your-url.vercel.app/health
```

### Example 3: Script Integration
```python
import requests

url = "https://your-url.vercel.app/api/process"

def obfuscate_lua(code, level=3):
    response = requests.post(url, json={
        "code": code,
        "mode": "obfuscate",
        "level": level
    })
    return response.json()['result']

# Use it
protected = obfuscate_lua("local x = 10")
print(protected)
```

## Advanced Configuration

### Add Rate Limiting

Edit `api/index.py`:

```python
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address

limiter = Limiter(
    app=app,
    key_func=get_remote_address,
    default_limits=["200 per day", "50 per hour"]
)

@app.route('/api/process', methods=['POST'])
@limiter.limit("10 per minute")
def process():
    ...
```

Install in `requirements.txt`:
```
flask-limiter==3.5.0
```

### Add API Key Authentication

Edit `api/index.py`:

```python
import os
from functools import wraps

def require_api_key(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        api_key = request.headers.get('X-API-Key')
        if not api_key or api_key != os.getenv('API_KEY', 'secret'):
            return {'error': 'Unauthorized'}, 401
        return f(*args, **kwargs)
    return decorated

@app.route('/api/process', methods=['POST'])
@require_api_key
def process():
    ...
```

Add environment variable in Vercel:
1. Dashboard → Settings → Environment Variables
2. Add: `API_KEY` = `your-secret-key`

### Custom Error Pages

Add to `api/index.py`:

```python
@app.errorhandler(429)
def ratelimit_handler(e):
    return {'error': 'Rate limit exceeded'}, 429
```

## Performance Tips

1. **Use Vercel Edge Functions** for ultra-fast response
2. **Enable Gzip compression** (automatic)
3. **Cache static assets** (minimal in this app)
4. **Monitor cold starts** - warm requests are <50ms
5. **Use Level 1-2** for faster processing

## Security Checklist

- ✅ No secrets in code
- ✅ Environment variables for sensitive data
- ✅ HTTPS everywhere (automatic)
- ✅ CORS configured
- ✅ Input validation
- ✅ Error messages non-revealing

## Monitoring

### Vercel Analytics
- Dashboard → Analytics
- Monitor:
  - Requests/min
  - Errors/min
  - P50/P99 latency
  - Bandwidth usage

### Real-time Logs
- Dashboard → Logs
- See live traffic and errors

## Backup & Rollback

### Backup
- Always in GitHub repository
- Vercel keeps deployment history

### Rollback
1. Dashboard → Deployments
2. Find previous deployment
3. Click "..." → "Rollback"

## Maintenance

### Update Dependencies
```bash
pip list --outdated
pip install --upgrade [package-name]
git add requirements.txt
git commit -m "Update dependencies"
git push
# Vercel auto-redeploys!
```

### Monitoring Errors
1. Dashboard → Logs → Runtime
2. Set up alerts (Vercel Pro)
3. Monitor error rate

## Need Help?

- **Vercel Docs**: https://vercel.com/docs
- **Flask Docs**: https://flask.palletsprojects.com/
- **GitHub Issues**: Your repo issues
- **Python Docs**: https://docs.python.org/3/

---

**You're now live on Vercel!** 🎉

Visit your Vercel URL and start protecting Lua code!

**Share your link**: `https://your-project.vercel.app`

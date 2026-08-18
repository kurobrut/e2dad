# 🔐 Lua Obfuscator Pro - Vercel Edition

**Production-Ready Enterprise Lua Code Protection** deployed on Vercel serverless infrastructure.

## ✨ Features

### Advanced Obfuscation Techniques
- ✅ **Variable Renaming** - Converts meaningful names to cryptic ones
- ✅ **String Encoding** - Hex/Char/Base64 encoding
- ✅ **Dead Code Injection** - Adds confusing dead code paths
- ✅ **Control Flow Obfuscation** - Wraps code in function closures
- ✅ **Number Encoding** - Converts literals to expressions
- ✅ **Comment Stripping** - Removes all documentation

### Processing Modes
| Mode | Purpose | Use Case |
|------|---------|----------|
| **Obfuscate** | Maximum protection | Game scripts, proprietary code |
| **Minify** | File size reduction | Production deployment |
| **Beautify** | Code formatting | Code review, debugging |
| **Remove Comments** | Clean code | Distribution |

### Protection Levels
| Level | Protection | Performance | Use Case |
|-------|-----------|-------------|----------|
| **1 - Light** | Comments + Minify | Very Fast ⚡ | Quick obfuscation |
| **2 - Medium** | Level 1 + Variables | Fast ⚡⚡ | Most projects |
| **3 - Strong** | Level 2 + Dead Code | Normal ⚡⚡⚡ | **Recommended** |
| **4 - Maximum** | All techniques | Slow ⚡⚡⚡⚡ | Sensitive code |

## 🚀 Quick Start

### Prerequisites
- GitHub account
- Vercel account (free tier available)
- Git installed

### Deploy in 3 Steps

#### Step 1: Create Vercel Project
1. Go to [vercel.com](https://vercel.com)
2. Click "Add New" → "Project"
3. Import your GitHub repository
4. Select "Python" as framework

#### Step 2: Deploy
Vercel will automatically:
- Read `vercel.json`
- Install dependencies from `requirements.txt`
- Deploy the serverless function
- Generate HTTPS URL

#### Step 3: Use It!
Visit your Vercel URL and start protecting code.

## 📝 API Endpoints

### POST `/api/process`
Process and protect Lua code

**Request:**
```json
{
  "code": "local x = 10\nprint(x)",
  "mode": "obfuscate",
  "level": 3
}
```

**Response:**
```json
{
  "success": true,
  "result": "local a=10;print(a)",
  "stats": {
    "original_size": 22,
    "result_size": 18,
    "reduction": "18.2%",
    "original_lines": 2,
    "result_lines": 1
  }
}
```

### GET `/api/stats`
Get API information and available features

### GET `/health`
Health check endpoint

### POST `/api/compare`
Compare different obfuscation modes

## 💻 Local Development

### Setup
```bash
# Clone repository
git clone <your-repo>
cd lua-obfuscator-vercel

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run locally
python -m flask --app api.index run
```

Visit: `http://localhost:5000`

### Testing
```bash
# Test API
curl -X POST http://localhost:5000/api/process \
  -H "Content-Type: application/json" \
  -d '{"code":"local x=10","mode":"obfuscate","level":3}'

# Test health
curl http://localhost:5000/health
```

## 🎯 Obfuscation Examples

### Example 1: Light Obfuscation (Level 1)
```lua
-- Input
local playerHealth = 100
function takeDamage(amount)
    playerHealth = playerHealth - amount
end

-- Output (comments + minify)
local playerHealth=100;function takeDamage(amount)playerHealth=playerHealth-amount end
```

### Example 2: Strong Obfuscation (Level 3)
```lua
-- Input
local playerName = "Hero"
local health = 100

-- Output
(function()local _={}...obfuscated code...end)()
```

### Example 3: Maximum Obfuscation (Level 4)
```lua
-- Includes dead code, control flow, string encoding, all techniques
-- Result is extremely difficult to reverse engineer
```

## 📊 Performance Metrics

Vercel Infrastructure:
- **Cold Start**: ~300-500ms
- **Warm Requests**: <50ms
- **Memory**: 1GB per function
- **Timeout**: 30 seconds
- **Max Code Size**: 10MB

## 🔐 Security Features

✅ **No Code Logging** - Your code is never stored or logged  
✅ **HTTPS Only** - All connections encrypted  
✅ **CORS Protected** - Prevent unauthorized access  
✅ **Rate Limiting Ready** - Can be added to middleware  
✅ **Environment Variables** - Secure configuration  

## 📚 Configuration

### Environment Variables
```bash
PYTHONUNBUFFERED=1     # Python output buffering
FLASK_ENV=production   # Flask mode
```

### Vercel Settings
Settings are in `vercel.json`:
- Memory: 1024MB per function
- Max Duration: 30 seconds
- Runtime: Python 3.11

## 🚨 Important Notes

⚠️ **Not Encryption**
- Obfuscation makes code hard to read but not impossible to reverse
- Use for legitimate protection, not security

⚠️ **Always Backup**
- Keep original source code backed up
- Obfuscation is one-way (can't be undone)

⚠️ **Test First**
- Always test obfuscated code before deployment
- Different protection levels may have different effects

## 📈 Upgrade Path

### Free Tier (Vercel)
- Unlimited deployments
- 100GB bandwidth/month
- Auto HTTPS
- Cold starts: ~300-500ms

### Pro Tier ($20/month)
- Faster cold starts
- Increased limits
- Priority support

## 🛠️ Customization

### Add Rate Limiting
```python
from flask_limiter import Limiter
limiter = Limiter(app, key_func=lambda: request.remote_addr)

@app.route('/api/process', methods=['POST'])
@limiter.limit("10 per minute")
def process():
    ...
```

### Add Authentication
```python
from functools import wraps

def require_api_key(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        api_key = request.headers.get('X-API-Key')
        if api_key != os.getenv('API_KEY'):
            return {'error': 'Unauthorized'}, 401
        return f(*args, **kwargs)
    return decorated
```

### Custom Obfuscation Levels
Edit `obfuscator.py` `obfuscate()` method to add new levels.

## 🐛 Troubleshooting

### Problem: "Module not found" error
```bash
# Solution: Ensure vercel.json buildCommand installs dependencies
pip install -r requirements.txt
```

### Problem: Slow cold starts
- **Solution**: Use Level 1-2 for faster processing
- Cold starts are normal on serverless, warm requests are fast

### Problem: 413 Payload Too Large
- **Solution**: Code limit is 10MB, split into smaller files

### Problem: Function timeout
- **Solution**: Use simpler obfuscation levels (1-2)
- Level 4 can be slow on large files

## 📞 Support

- **Issues**: GitHub Issues
- **Docs**: See README.md
- **API Help**: Check `/api/stats` endpoint

## 📄 License

MIT License - Free for personal and commercial use

## 🤝 Contributing

Contributions welcome! Areas to improve:
- [ ] Additional obfuscation techniques
- [ ] Performance optimization
- [ ] Custom encoding methods
- [ ] Web UI enhancements

## 🚀 Next Steps

1. **Deploy to Vercel**: Push to GitHub, auto-deploys!
2. **Share URL**: Give everyone access to your obfuscator
3. **Add Authentication**: Optional API key protection
4. **Monitor Usage**: Check Vercel dashboard for analytics

## 📊 Vercel Dashboard

Monitor your deployment:
1. Go to [vercel.com/dashboard](https://vercel.com/dashboard)
2. Select your project
3. View logs, analytics, deployments
4. Scale functions as needed

---

**Happy Obfuscating! 🔐** Deploy once, protect everywhere!

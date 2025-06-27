# JT808 Server - Render Deployment

## ✅ Ready for Deployment

The JT808 server has been successfully configured for Render cloud platform.

## 🚀 Quick Deploy

1. **Push to GitHub:**

   ```bash
   git add .
   git commit -m "Add Render deployment"
   git push origin master
   ```

2. **Deploy on Render:**
   - Go to [Render Dashboard](https://dashboard.render.com)
   - Click "New +" → "Blueprint"
   - Connect your GitHub repository
   - Render will auto-detect `render.yaml`

## 📦 What's Included

- **Web API Server** (Port 8100): REST API, docs, monitoring
- **JT808 Protocol Server** (Port 7100): Device connections
- **Health Checks:** `/actuator/health`
- **API Documentation:** `/doc.html`
- **WebSocket Monitor:** `/ws.html`

## 🔧 Configuration

- **Java 17** runtime
- **H2 Database** (file-based)
- **Spring Boot Actuator** for monitoring
- **Docker support** included

## 🧪 Test Your Deployment

```bash
# Health check
curl https://your-app.onrender.com/actuator/health

# API docs
open https://your-app.onrender.com/doc.html

# JT808 test (use tools in 协议文档/ folder)
# Connect to: your-app.onrender.com:7100
```

## 📚 Files Created

- `render.yaml` - Multi-service deployment
- `Dockerfile` - Container configuration
- `application-render.yml` - Render config
- `RENDER_DEPLOYMENT.md` - Detailed guide

**Status:** ✅ Ready to deploy!

# JT808 Server Deployment on Render

This guide explains how to deploy the JT808 server on Render cloud platform.

## Prerequisites

- A Render account
- Git repository with the JT808 server code
- Java 17 runtime

## Deployment Options

### Option 1: Using render.yaml (Recommended)

1. **Connect your repository to Render:**

   - Go to [Render Dashboard](https://dashboard.render.com)
   - Click "New +" and select "Blueprint"
   - Connect your GitHub/GitLab repository
   - Render will automatically detect the `render.yaml` file

2. **Automatic deployment:**

   - Render will create two services:
     - `jt808-server`: Web API server (port 8100)
     - `jt808-tcp-server`: JT808 protocol server (port 7100)

3. **Environment variables:**
   - `PORT`: Web server port (default: 8100)
   - `JT808_TCP_PORT`: JT808 TCP port (default: 7100)
   - `JT808_UDP_PORT`: JT808 UDP port (default: 7100)
   - `DATABASE_URL`: Database connection string
   - `DATABASE_USERNAME`: Database username
   - `DATABASE_PASSWORD`: Database password

### Option 2: Manual Web Service

1. **Create a new Web Service:**

   - Go to Render Dashboard
   - Click "New +" and select "Web Service"
   - Connect your repository

2. **Configure the service:**

   - **Name:** jt808-server
   - **Environment:** Java
   - **Build Command:** `mvn clean package -DskipTests`
   - **Start Command:** `java -jar jtt808-server/target/jtt808-server-1.0.0-SNAPSHOT.jar`
   - **Port:** 8100

3. **Set environment variables:**
   - `JAVA_VERSION`: 17
   - `PORT`: 8100
   - `JT808_TCP_PORT`: 7100
   - `JT808_UDP_PORT`: 7100

## Service Endpoints

### Web API (Port 8100)

- **Health Check:** `https://your-app.onrender.com/actuator/health`
- **API Documentation:** `https://your-app.onrender.com/doc.html`
- **Swagger UI:** `https://your-app.onrender.com/swagger-ui/index.html`
- **WebSocket Monitor:** `https://your-app.onrender.com/ws.html`

### JT808 Protocol (Port 7100)

- **TCP:** `your-app.onrender.com:7100`
- **UDP:** `your-app.onrender.com:7100`

## Database Configuration

The server uses H2 database by default, which stores data in files. For production:

1. **Use PostgreSQL (recommended):**

   - Create a PostgreSQL database on Render
   - Update `DATABASE_URL` to use PostgreSQL connection string
   - Add PostgreSQL dependency to `pom.xml`

2. **Use MySQL/MariaDB:**
   - Create a MySQL database on Render
   - Update configuration to use MySQL connection

## Testing the Deployment

### 1. Test Web API

```bash
# Health check
curl https://your-app.onrender.com/actuator/health

# API documentation
open https://your-app.onrender.com/doc.html
```

### 2. Test JT808 Protocol

Use the provided test tools in the `协议文档/` folder:

- Run `发包工具.exe` (Windows) or use a JT808 client
- Connect to `your-app.onrender.com:7100`
- Send test messages

### 3. Test Device Registration

```bash
# Example registration message
curl -X POST https://your-app.onrender.com/device/0100 \
  -H "Content-Type: application/json" \
  -d '{
    "phoneNumber": "12345678901",
    "provinceId": 31,
    "cityId": 115,
    "manufacturerId": "TEST",
    "terminalModel": "TEST-MODEL",
    "terminalId": "TEST123",
    "plateColor": 1,
    "plateNo": "TEST123"
  }'
```

## Monitoring and Logs

- **Logs:** Available in Render dashboard under your service
- **Metrics:** Access via `/actuator/metrics` endpoint
- **Health:** Monitor via `/actuator/health` endpoint

## Troubleshooting

### Common Issues

1. **Build fails:**

   - Check Java version (must be 17)
   - Verify Maven dependencies
   - Check build logs in Render dashboard

2. **Service won't start:**

   - Verify port configuration
   - Check environment variables
   - Review application logs

3. **JT808 connections fail:**
   - Verify TCP/UDP ports are open
   - Check firewall settings
   - Test with JT808 client tools

### Performance Optimization

1. **Memory settings:**

   - Adjust `JAVA_OPTS` for memory allocation
   - Monitor memory usage in Render dashboard

2. **Database optimization:**
   - Use connection pooling
   - Optimize queries
   - Consider read replicas for high load

## Security Considerations

1. **Authentication:**

   - Implement proper authentication for web API
   - Use HTTPS for all web traffic
   - Secure JT808 connections

2. **Data protection:**
   - Encrypt sensitive data
   - Use secure database connections
   - Implement proper logging

## Support

For issues with the JT808 server:

- Check the [original repository](https://github.com/yezhihao/jt808-server)
- Review Render documentation
- Check application logs for errors

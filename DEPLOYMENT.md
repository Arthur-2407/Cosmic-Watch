# Deployment Guide

This guide covers various deployment options for the Cosmic Watch application.

## 🐳 Docker Deployment

### Prerequisites
- Docker and Docker Compose installed
- Git

### Quick Deploy with Docker Compose

1. **Clone and navigate to the project**
   ```bash
   git clone <your-repository-url>
   cd <repository-name>
   ```

2. **Deploy with Docker Compose**
   ```bash
   docker-compose up -d
   ```

3. **Verify deployment**
   ```bash
   # Check running containers
   docker-compose ps
   
   # Check logs
   docker-compose logs -f
   ```

4. **Access the application**
   - Frontend: http://localhost:80
   - Backend API: http://localhost:3001

### Manual Docker Deployment

1. **Build backend image**
   ```bash
   cd server
   docker build -t cosmic-watch-backend .
   docker run -d -p 3001:3001 -v $(pwd)/data:/app/data cosmic-watch-backend
   ```

2. **Build frontend image**
   ```bash
   cd ../build-cosmic-watch-app
   docker build -t cosmic-watch-frontend .
   docker run -d -p 80:80 cosmic-watch-frontend
   ```

## 🚀 GitHub Pages Deployment (Frontend Only)

### Automatic Deployment

The project is configured for automatic deployment to GitHub Pages:

1. **Enable GitHub Pages**
   - Go to repository settings
   - Navigate to "Pages" section
   - Select "GitHub Actions" as source

2. **Push to main branch**
   ```bash
   git add .
   git commit -m "Deploy to GitHub Pages"
   git push origin main
   ```

3. **Access your deployed app**
   - URL: `https://<username>.github.io/<repository-name>`

### Manual Deployment

1. **Build the frontend**
   ```bash
   cd build-cosmic-watch-app
   npm install
   npm run build
   ```

2. **Deploy to GitHub Pages**
   ```bash
   npm install -g gh-pages
   gh-pages -d dist
   ```

## 🖥️ Production Server Deployment

### Prerequisites
- Linux server (Ubuntu 20.04+ recommended)
- Docker and Docker Compose
- Nginx (optional, for reverse proxy)
- SSL certificate (recommended)

### Step-by-Step Production Deployment

1. **Server Setup**
   ```bash
   # Update system
   sudo apt update && sudo apt upgrade -y
   
   # Install Docker
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   
   # Install Docker Compose
   sudo curl -L "https://github.com/docker/compose/releases/download/v2.20.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   sudo chmod +x /usr/local/bin/docker-compose
   ```

2. **Deploy Application**
   ```bash
   # Create app directory
   sudo mkdir -p /opt/cosmic-watch
   cd /opt/cosmic-watch
   
   # Clone repository
   git clone <your-repository-url> .
   
   # Deploy with Docker Compose
   sudo docker-compose up -d
   ```

3. **Set up Nginx Reverse Proxy (Optional)**
   ```bash
   # Create Nginx config
   sudo nano /etc/nginx/sites-available/cosmic-watch
   ```
   
   **Nginx Configuration:**
   ```nginx
   server {
       listen 80;
       server_name your-domain.com;
       
       location / {
           proxy_pass http://localhost:80;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
       
       location /api {
           proxy_pass http://localhost:3001;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```
   
   ```bash
   # Enable site
   sudo ln -s /etc/nginx/sites-available/cosmic-watch /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl restart nginx
   ```

4. **Set up SSL with Let's Encrypt (Recommended)**
   ```bash
   # Install Certbot
   sudo apt install certbot python3-certbot-nginx
   
   # Get SSL certificate
   sudo certbot --nginx -d your-domain.com
   ```

## 🔧 Environment Configuration

### Production Environment Variables

Create `.env` files for production:

**Backend (.env)**
```env
NODE_ENV=production
JWT_SECRET=your-super-secure-jwt-secret-key
PORT=3001
CORS_ORIGIN=https://your-domain.com
```

**Frontend (.env.production)**
```env
VITE_API_URL=https://your-domain.com/api
VITE_NASA_API_KEY=your-production-nasa-api-key
```

### Database Setup

The application uses JSON file-based database by default. For production:

1. **Ensure persistent storage**
   ```yaml
   # In docker-compose.yml
   volumes:
     - ./server/data:/app/data
   ```

2. **Backup strategy**
   ```bash
   # Create backup script
   #!/bin/bash
   DATE=$(date +%Y%m%d_%H%M%S)
   cp /opt/cosmic-watch/server/data.json /opt/cosmic-watch/backups/data_$DATE.json
   ```

## 🔄 CI/CD Pipeline

### GitHub Actions Setup

1. **Repository Secrets**
   Go to repository settings > Secrets and add:
   - `DOCKER_USERNAME`: Docker Hub username
   - `DOCKER_PASSWORD`: Docker Hub password
   - `HOST`: Production server IP
   - `USERNAME`: Server username
   - `SSH_KEY`: Server SSH private key

2. **Automatic Deployment**
   - Push to `main` branch triggers deployment
   - Pull requests run tests and security scans
   - Docker images are built and pushed to Docker Hub

### Manual Deployment Commands

```bash
# Deploy latest changes
git pull origin main
docker-compose pull
docker-compose up -d

# View logs
docker-compose logs -f

# Update specific service
docker-compose up -d --no-deps backend
```

## 📊 Monitoring and Maintenance

### Health Checks

```bash
# Check application health
curl http://localhost:3001/api/health

# Check container status
docker-compose ps

# Monitor resource usage
docker stats
```

### Log Management

```bash
# View application logs
docker-compose logs -f backend
docker-compose logs -f frontend

# Rotate logs (add to docker-compose.yml)
logging:
  driver: "json-file"
  options:
    max-size: "10m"
    max-file: "3"
```

### Backup Strategy

```bash
# Create backup script
#!/bin/bash
BACKUP_DIR="/opt/backups/cosmic-watch"
DATE=$(date +%Y%m%d_%H%M%S)

mkdir -p $BACKUP_DIR

# Backup database
cp /opt/cosmic-watch/server/data.json $BACKUP_DIR/data_$DATE.json

# Backup configurations
tar -czf $BACKUP_DIR/config_$DATE.tar.gz /opt/cosmic-watch/docker-compose.yml /opt/cosmic-watch/.env

# Clean old backups (keep last 7 days)
find $BACKUP_DIR -name "*.json" -mtime +7 -delete
find $BACKUP_DIR -name "*.tar.gz" -mtime +7 -delete
```

## 🚨 Troubleshooting

### Common Issues

1. **Port conflicts**
   ```bash
   # Check what's using ports
   sudo netstat -tulpn | grep :80
   sudo netstat -tulpn | grep :3001
   ```

2. **Permission issues**
   ```bash
   # Fix Docker permissions
   sudo usermod -aG docker $USER
   
   # Fix file permissions
   sudo chown -R $USER:$USER /opt/cosmic-watch
   ```

3. **Memory issues**
   ```bash
   # Check memory usage
   docker stats
   
   # Increase swap space if needed
   sudo fallocate -l 2G /swapfile
   sudo chmod 600 /swapfile
   sudo mkswap /swapfile
   sudo swapon /swapfile
   ```

### Performance Optimization

1. **Database optimization**
   - Regular cleanup of old data
   - Implement proper indexing if migrating to SQL

2. **Caching**
   - Implement Redis for session storage
   - Use CDN for static assets

3. **Load balancing**
   - Use multiple containers with load balancer
   - Implement horizontal scaling

## 📞 Support

For deployment issues:
1. Check the logs: `docker-compose logs`
2. Verify environment variables
3. Ensure all ports are available
4. Check firewall settings
5. Review GitHub Actions workflow logs

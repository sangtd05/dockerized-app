# Dockerized App

## Tính năng

- ✅ Express.js web server
- ✅ RESTful API endpoints
- ✅ Health check endpoint
- ✅ Docker containerization
- ✅ Docker Compose orchestration
- ✅ Nginx reverse proxy (optional)
- ✅ Development và Production environments

## Cấu trúc dự án

```
dockerized-app/
├── app.js                 # Main application file
├── package.json           # Node.js dependencies
├── Dockerfile            # Docker image configuration
├── docker-compose.yml    # Production environment
├── docker-compose.dev.yml # Development environment
├── nginx.conf            # Nginx configuration
├── .dockerignore         # Docker ignore file
└── README.md             # This file
```

## API Endpoints

- `GET /` - Welcome message và thông tin server
- `GET /health` - Health check endpoint
- `GET /api/users` - Lấy danh sách users
- `POST /api/users` - Tạo user mới

## Cài đặt và chạy

### Yêu cầu hệ thống

- Docker
- Docker Compose

### Chạy trong Production

```bash
# Build và start services
docker-compose up --build

# Chạy trong background
docker-compose up -d --build
```

Ứng dụng sẽ có sẵn tại:
- App trực tiếp: http://localhost:3000
- Qua Nginx: http://localhost:80

### Chạy trong Development

```bash
# Sử dụng development compose file
docker-compose -f docker-compose.dev.yml up --build

# Chạy trong background
docker-compose -f docker-compose.dev.yml up -d --build
```

### Các lệnh hữu ích

```bash
# Xem logs
docker-compose logs -f

# Stop services
docker-compose down

# Rebuild và restart
docker-compose up --build --force-recreate

# Xem status containers
docker-compose ps

# Vào container
docker-compose exec app sh
```

## Testing

### Test API endpoints

```bash
# Health check
curl http://localhost:3000/health

# Get users
curl http://localhost:3000/api/users

# Create user
curl -X POST http://localhost:3000/api/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Test User", "email": "test@example.com"}'
```

### Test qua Nginx

```bash
# Health check qua Nginx
curl http://localhost/health

# Get users qua Nginx
curl http://localhost/api/users
```

## Docker Commands

### Build image manually

```bash
# Build image
docker build -t dockerized-app .

# Run container
docker run -p 3000:3000 dockerized-app

# Run với environment variables
docker run -p 3000:3000 -e NODE_ENV=production dockerized-app
```

### Container management

```bash
# List containers
docker ps

# List images
docker images

# Remove containers
docker-compose down --volumes --remove-orphans

# Remove images
docker rmi dockerized-app
```

## Environment Variables

- `NODE_ENV` - Environment (development/production)
- `PORT` - Port number (default: 3000)

## Troubleshooting

### Container không start

```bash
# Check logs
docker-compose logs app

# Check container status
docker-compose ps
```

### Port conflicts

Nếu port 3000 hoặc 80 đã được sử dụng, thay đổi trong `docker-compose.yml`:

```yaml
ports:
  - "3001:3000"  # Thay đổi port host
```

### Permission issues

```bash
# Fix permissions
sudo chown -R $USER:$USER .
```

## Development

### Local development (không dùng Docker)

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Start production server
npm start
```

### Hot reload trong Docker

Development compose file đã được cấu hình với volume mounting để enable hot reload.

## Security Notes

- Container chạy với non-root user
- Health checks được cấu hình
- Nginx reverse proxy cho production
- Environment variables cho configuration
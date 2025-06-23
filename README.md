# Automagik Evolution API

A production-ready Docker setup for Evolution API with optional MinIO and transcription services.

## 🚀 Quick Start

### Basic Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/namastexlabs/automagik-evolution.git
   cd automagik-evolution
   ```

2. **Configure environment**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. **Start the services**
   ```bash
   docker-compose -f docker-compose.basic.yml up -d
   ```

4. **Access the API**
   - API: http://localhost:18080
   - Docs: http://localhost:18080/docs
   - RabbitMQ Management: http://localhost:15672

### Advanced Setup (with MinIO + Transcription)

For setups requiring S3-compatible storage and audio transcription:

```bash
docker-compose -f docker-compose.minio.yml up -d
```

Additional services:
- MinIO Console: http://localhost:9001
- MinIO API: http://localhost:9000

## 📋 Configuration

### Required Environment Variables

Copy `.env.example` to `.env` and configure:

```bash
# Essential settings
SERVER_PORT=18080
DATABASE_CONNECTION_URI=postgresql://evolution_user:your_password@postgres:5432/evolution_db
AUTHENTICATION_API_KEY=your_secure_api_key

# RabbitMQ
RABBITMQ_URI=amqp://your_user:your_password@rabbitmq:5672

# MinIO (if using docker-compose.minio.yml)
S3_ENABLED=true
S3_ACCESS_KEY=your_minio_access_key
S3_SECRET_KEY=your_minio_secret_key
S3_BUCKET=your_bucket_name
S3_ENDPOINT=minio:9000
```

### Security Considerations

- ⚠️ **Never commit `.env` files**
- 🔑 Change all default passwords
- 🔐 Use strong API keys
- 🌐 Configure CORS appropriately for production

## 🐳 Docker Compose Files

| File | Description |
|------|-------------|
| `docker-compose.basic.yml` | Basic Evolution API setup |
| `docker-compose.minio.yml` | Full setup with MinIO and transcription |

## 📊 Services Overview

### Core Services
- **PostgreSQL**: Database storage
- **Redis**: Caching layer
- **RabbitMQ**: Message queuing
- **Evolution API**: WhatsApp API server

### Optional Services (minio.yml)
- **MinIO**: S3-compatible object storage
- **Transcribe**: Audio transcription service

## 🔧 Management

### Health Checks
```bash
# Check service status
docker-compose ps

# View logs
docker-compose logs evolution_api
docker-compose logs postgres
```

### Database Management
```bash
# Access PostgreSQL
docker exec -it postgres psql -U evolution_user -d evolution_db

# Backup database
docker exec postgres pg_dump -U evolution_user evolution_db > backup.sql
```

### MinIO Management
```bash
# Access MinIO CLI
docker exec -it minio mc --help

# Create bucket
docker exec -it minio mc mb local/your-bucket-name
```

## 🚨 Troubleshooting

### Common Issues

1. **API not accessible**
   - Check port mapping in docker-compose.yml
   - Ensure SERVER_PORT matches container port

2. **Database connection failed**
   - Verify DATABASE_CONNECTION_URI
   - Check PostgreSQL container health

3. **Authentication errors**
   - Verify AUTHENTICATION_API_KEY in requests
   - Check API key format in .env

### Debug Commands
```bash
# Check container logs
docker logs evolution_api

# Test database connectivity
docker exec postgres pg_isready -U evolution_user -d evolution_db

# Test API endpoint
curl -H "apikey: your_api_key" http://localhost:18080
```

## 📚 API Documentation

Once running, access the interactive API documentation at:
- **Swagger UI**: http://localhost:18080/docs
- **Official Docs**: https://doc.evolution-api.com/v2/api-reference/get-information

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License.

## 🆘 Support

- [Evolution API Documentation](https://doc.evolution-api.com)
- [GitHub Issues](https://github.com/namastexlabs/automagik-evolution/issues)
- [Evolution API GitHub](https://github.com/EvolutionAPI/evolution-api)

---

**⚠️ Security Note**: Always review and update default credentials before production deployment.
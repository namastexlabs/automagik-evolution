# Automagik Evolution API

A production-ready Docker setup for Evolution API v2.3.0 with optional MinIO, pgAdmin, and audio transcription services.

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
   docker-compose up -d
   ```

4. **Access the services**
   - API: http://localhost:8080
   - Docs: http://localhost:8080/docs
   - RabbitMQ Management: http://localhost:15672 (user: namastex, pass: namastex8888)

### Advanced Setup (with MinIO + pgAdmin + Audio Transcription)

For setups requiring S3-compatible storage, database management UI, and audio transcription:

```bash
docker-compose -f docker-compose.minio.yml up -d
```

Additional services:
- MinIO Console: http://localhost:9001 (user: minio_access, pass: namastex8888)
- MinIO API: http://localhost:9000
- pgAdmin: http://localhost:4000 (email: admin@namastex.com, pass: namastex8888)
- Audio Converter API: http://localhost:4040

## 📋 Configuration

### Required Environment Variables

Copy `.env.example` to `.env` and configure:

```bash
# Essential settings
SERVER_PORT=8080
DATABASE_CONNECTION_URI=postgresql://evolution_user:namastex8888@postgres:5432/evolution_db
AUTHENTICATION_API_KEY=your_secure_api_key

# RabbitMQ
RABBITMQ_URI=amqp://namastex:namastex8888@rabbitmq:5672

# MinIO (if using docker-compose.minio.yml)
S3_ENABLED=true
S3_ACCESS_KEY=minio_access
S3_SECRET_KEY=namastex8888
S3_BUCKET=evolution
S3_ENDPOINT=minio:9000

# Audio Transcription (if using docker-compose.minio.yml)
AUDIO_CONVERTER_URL=http://api:4040
AUDIO_CONVERTER_API_KEY=namastex8888
```

### Security Considerations

- ⚠️ **Never commit `.env` files**
- 🔑 Change all default passwords before production
- 🔐 Use strong API keys
- 🌐 Configure CORS appropriately for production
- 🔒 Update the OpenAI API key in docker-compose.minio.yml if using transcription

## 🐳 Docker Compose Files

| File | Description |
|------|-------------|
| `docker-compose.yml` | Basic Evolution API setup with core services |
| `docker-compose.minio.yml` | Full setup with MinIO, pgAdmin, and audio transcription |

## 📊 Services Overview

### Core Services (docker-compose.yml)
- **PostgreSQL 15**: Database storage
- **Redis 7**: Caching layer
- **RabbitMQ**: Message queuing with management UI
- **Evolution API v2.3.0**: WhatsApp API server

### Additional Services (docker-compose.minio.yml)
- **MinIO**: S3-compatible object storage
- **pgAdmin**: PostgreSQL web-based management interface
- **Audio Converter API**: Audio transcription service with OpenAI/Groq support

## 🔧 Management

### Health Checks
```bash
# Check service status
docker-compose ps

# View logs
docker-compose logs evolution-api
docker-compose logs postgres
```

### Database Management

#### Using CLI:
```bash
# Access PostgreSQL
docker exec -it postgres psql -U evolution_user -d evolution_db

# Backup database
docker exec postgres pg_dump -U evolution_user evolution_db > backup.sql
```

#### Using pgAdmin (if using docker-compose.minio.yml):
1. Access pgAdmin at http://localhost:4000
2. Login with admin@namastex.com / namastex8888
3. Add server with:
   - Host: postgres
   - Port: 5432
   - Database: evolution_db
   - Username: evolution_user
   - Password: namastex8888

### MinIO Management
```bash
# Access MinIO CLI
docker exec -it minio mc --help

# Configure MinIO client
docker exec -it minio mc alias set local http://localhost:9000 minio_access namastex8888

# Create bucket
docker exec -it minio mc mb local/evolution
```

## 🚨 Troubleshooting

### Common Issues

1. **API not accessible**
   - Check port mapping in docker-compose.yml
   - Ensure SERVER_PORT is set to 8080
   - Verify Evolution API container is running: `docker ps`

2. **Database connection failed**
   - Verify DATABASE_CONNECTION_URI format
   - Check PostgreSQL container health: `docker-compose ps`
   - Ensure postgres service is healthy before Evolution API starts

3. **Authentication errors**
   - Verify AUTHENTICATION_API_KEY in requests
   - Check API key format in .env
   - Use correct header: `apikey: your_api_key`

4. **RabbitMQ connection issues**
   - Wait for RabbitMQ healthcheck to pass
   - Check credentials match environment variables
   - Access management UI to verify queues

### Debug Commands
```bash
# Check container logs
docker logs evolution_api

# Test database connectivity
docker exec postgres pg_isready -U evolution_user -d evolution_db

# Test API endpoint
curl -H "apikey: your_api_key" http://localhost:8080/manager/status

# Check RabbitMQ status
docker exec rabbitmq rabbitmq-diagnostics check_port_connectivity
```

## 📚 API Documentation

Once running, access the interactive API documentation at:
- **Swagger UI**: http://localhost:8080/docs
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

**⚠️ Security Note**: Always review and update default credentials before production deployment. The default passwords (namastex8888) should be changed immediately.
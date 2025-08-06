# Deploy.tz Deployment Guide

This guide will help you deploy your Django community application to deploy.tz.

## Prerequisites
- A deploy.tz account
- Git repository with your code
- Docker installed locally (optional, for testing)

## Deployment Steps

### 1. Prepare Your Environment Variables
In your deploy.tz dashboard, set these environment variables:

```
DEBUG=False
SECRET_KEY=your-super-secret-key-here
ALLOWED_HOSTS=*.deploy.tz,deploy.tz
DATABASE_URL=postgresql://user:password@host:port/database
GOOGLE_CLIENT_ID=573256159763-dnme43v2sjbmuoajhu5pbup5k0c7v80s.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=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
```

### 2. Deploy Configuration
- **Port**: 3000 (as specified in Dockerfile)
- **Build Command**: `docker build -t aru-community .`
- **Start Command**: `gunicorn --bind 0.0.0.0:3000 --workers 3 community.wsgi:application`

### 3. Database Setup
Deploy.tz typically provides PostgreSQL. The DATABASE_URL will be automatically set.

### 4. Static Files
Static files are handled by WhiteNoise and collected during the Docker build process.

### 5. Domain Configuration
Once deployed, your app will be available at:
- `https://your-app-name.deploy.tz`

## Local Testing

To test the Docker configuration locally:

```bash
# Build the image
docker build -t aru-community .

# Run the container
docker run -p 3000:3000 -e DEBUG=True aru-community
```

Then visit `http://localhost:3000`

## Troubleshooting

### Common Issues:

1. **404 Error**: Usually caused by incorrect ALLOWED_HOSTS or static file configuration
2. **Static Files Not Loading**: Ensure WhiteNoise is properly configured
3. **Database Connection Error**: Check DATABASE_URL environment variable
4. **CSRF Error**: Ensure CSRF_TRUSTED_ORIGINS includes your domain

### Debug Steps:
1. Check deploy.tz logs for detailed error messages
2. Verify all environment variables are set correctly
3. Ensure the port (3000) matches your configuration
4. Check that static files are collected during build

## Files Created for Deployment:
- `Dockerfile` - Container configuration
- `.dockerignore` - Files to exclude from Docker build
- `.env.production` - Production environment variables template
- `deploy.sh` - Deployment preparation script

## Security Notes:
- Generate a new SECRET_KEY for production
- Use environment variables for sensitive data
- Enable HTTPS in production (set SECURE_SSL_REDIRECT=True)

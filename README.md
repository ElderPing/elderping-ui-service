# ElderPing UI Service

## Overview

The UI Service is a React-based frontend application for the ElderPing platform. It provides a user-friendly interface for elderly users and their family members to access all ElderPing features including health tracking, reminders, alerts, and family management.

## Technology Stack

- **Frontend Framework**: React
- **Build Tool**: Vite
- **Language**: JavaScript
- **Styling**: CSS/TailwindCSS
- **Web Server**: Nginx (Alpine Linux)
- **Container**: Docker (multi-stage build)
- **Security**: Non-root user execution (USER nginx)

## Features

- User authentication and login
- Health data visualization and tracking
- Medication reminder management
- Emergency alert system
- Family member management
- Real-time notifications
- Responsive design for mobile and desktop
- Accessibility features for elderly users

## Pages/Components

### Authentication
- Login page
- Registration page
- Password reset

### Dashboard
- User dashboard with overview
- Health metrics summary
- Recent alerts
- Upcoming reminders

### Health
- Health data entry forms
- Health history charts
- Medication tracking
- Health reports

### Reminders
- Reminder creation
- Reminder calendar view
- Reminder history
- Medication schedules

### Alerts
- Emergency alert button
- Alert history
- Alert status tracking
- Family notification settings

### Family
- Family member management
- Invite family members
- Family link management
- Caregiver dashboard

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `VITE_API_BASE_URL` | Backend API base URL | Yes |
| `VITE_AUTH_SERVICE_URL` | Auth service URL | No |
| `VITE_HEALTH_SERVICE_URL` | Health service URL | No |
| `VITE_REMINDER_SERVICE_URL` | Reminder service URL | No |
| `VITE_ALERT_SERVICE_URL` | Alert service URL | No |

## Docker Image

- **Repository**: `arunnsimon/elderpinq-ui-service`
- **Tags**: 
  - `dev-latest` - Development builds from develop branch
  - `prod-latest` - Production builds from main branch
  - `<version>` - Release tags

## CI/CD Pipeline

The service uses GitHub Actions for continuous integration and deployment:

1. **Security Scanning**
   - SAST (Static Application Security Testing)
   - SCA (Software Composition Analysis)
   - Trivy vulnerability scanning

2. **Docker Build & Publish**
   - Multi-stage Docker build (Node.js build + Nginx serve)
   - Push to Docker Hub
   - Tagged based on branch (dev-latest/prod-latest)

3. **GitOps Deployment**
   - Updates Helm chart image tag in elderping-k8s-charts
   - ArgoCD automatically syncs changes

## Kubernetes Deployment

### Helm Chart
Located in `elderping-k8s-charts/microservices/ui-service/`

**Resources:**
- Deployment with 2 replicas
- Service (ClusterIP on port 80)
- HorizontalPodAutoscaler (2-5 replicas, 70% CPU target)

**Configuration:**
- Namespace: elderping-dev (dev) / elderping-prod (prod)
- Resource requests: 50m CPU, 64Mi memory
- Resource limits: 200m CPU, 128Mi memory
- Liveness/Readiness probes on / endpoint

## Dockerfile

Multi-stage build:
1. **Build Stage**: Node.js 22 Alpine - Build React application
2. **Serve Stage**: Nginx 1.25 Alpine - Serve static files

Security:
- Runs as `nginx` user (non-root)
- Optimized Nginx configuration for React SPA
- Proper MIME types and caching headers

## Security Features

- **Non-root container**: Runs as `nginx` user (not root)
- **HTTPS**: SSL/TLS termination at Kong Gateway
- **JWT Authentication**: Token-based authentication with backend
- **CORS**: Configured for backend API access
- **Secure Headers**: Security headers via Nginx

## Development

### Local Setup
```bash
# Install dependencies
npm install

# Set environment variables
cp .env.example .env
# Edit .env with your values

# Run development server
npm run dev

# Run tests
npm test

# Build for production
npm run build

# Preview production build
npm run preview
```

### Docker Build
```bash
# Build image
docker build -t elderping-ui-service .

# Run container
docker run -p 80:80 --env-file .env elderping-ui-service
```

## Monitoring

- **Health Check**: `GET /` returns application status
- **Metrics**: Exposed for Prometheus scraping
- **Logs**: Collected by Loki
- **Dashboards**: Grafana dashboards for monitoring
- **Performance**: Web Vitals monitoring

## Accessibility

- WCAG 2.1 AA compliance
- Large touch targets for elderly users
- High contrast color scheme
- Screen reader support
- Keyboard navigation
- Font size adjustment

## Troubleshooting

### Common Issues

**API Connection Failed**
- Verify VITE_API_BASE_URL is set correctly
- Check backend services are accessible
- Verify Kong Gateway routing configuration

**Build Failed**
- Check Node.js version (requires Node.js 22)
- Verify all dependencies are installed
- Check for TypeScript/JavaScript errors

**Container Not Starting**
- Check pod logs: `kubectl logs <pod-name> -n elderping-dev`
- Verify Nginx configuration is valid
- Check resource limits are sufficient

## Contributing

1. Create feature branch from develop
2. Make changes and test locally
3. Commit with descriptive message
4. Push to feature branch
5. Create pull request to develop

## License

Proprietary - ElderPing Platform

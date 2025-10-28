# Docker & Jenkins Setup untuk Kasir Flutter

## Prerequisites
- Docker Desktop installed
- Jenkins (optional, bisa pakai Docker)

## Quick Start - Manual Docker Build

### Build Docker Image
```bash
docker build -t kasir-flutter .
```

### Run Container
```bash
docker run -d -p 8080:80 --name kasir-flutter-app kasir-flutter
```

### Access Application
Buka browser: `http://localhost:8080`

## Using Docker Compose

### Start Application
```bash
docker-compose up -d
```

### Stop Application
```bash
docker-compose down
```

## Setup Jenkins dengan Docker

### 1. Run Jenkins Container
```bash
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts
```

### 2. Get Initial Admin Password
```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

### 3. Access Jenkins
Buka: `http://localhost:8080`

### 4. Install Required Plugins
- Docker Pipeline
- Git Plugin
- GitHub Plugin

### 5. Create Pipeline Job
1. New Item → Pipeline
2. Nama: `kasir-flutter-pipeline`
3. Pipeline → Pipeline script from SCM
4. SCM: Git
5. Repository URL: `https://github.com/yasminnaila/kasir-flutter.git`
6. Branch: `*/main`
7. Script Path: `Jenkinsfile`

## Useful Docker Commands

### View Running Containers
```bash
docker ps
```

### View Logs
```bash
docker logs kasir-flutter-app
```

### Stop Container
```bash
docker stop kasir-flutter-app
```

### Remove Container
```bash
docker rm kasir-flutter-app
```

### Remove Image
```bash
docker rmi kasir-flutter
```

### Enter Container Shell
```bash
docker exec -it kasir-flutter-app sh
```

## CI/CD Workflow

1. Developer push code ke GitHub
2. Jenkins webhook triggered (auto atau manual)
3. Jenkins pull latest code
4. Build Docker image
5. Run tests (if any)
6. Deploy new container
7. Old container stopped & removed

## Troubleshooting

### Port Already in Use
```bash
# Find process using port 8080
netstat -ano | findstr :8080

# Kill the process
taskkill /PID <PID> /F
```

### Permission Issues (Linux/Mac)
```bash
sudo usermod -aG docker $USER
```

### Clean Up Everything
```bash
docker system prune -a
```

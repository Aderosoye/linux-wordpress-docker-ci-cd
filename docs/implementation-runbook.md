# Implementation Runbook

## 1. Prepare the Linux Server
Install Docker and Compose plugin, enable Docker service, and confirm the daemon is healthy.

Example checks:
```bash
sudo systemctl enable --now docker
sudo systemctl status docker
sudo docker version
sudo docker compose version
```

## 2. Create a Deployment Directory
```bash
sudo mkdir -p /opt/wordpress-stack
sudo chown -R $USER:$USER /opt/wordpress-stack
```

## 3. Copy Project Files
Clone the repo or sync it through CI/CD into `/opt/wordpress-stack`.

## 4. Configure Environment Variables
```bash
cp .env.example .env
nano .env
```
Update database credentials and deployment values.

## 5. Start the Stack
```bash
docker compose up -d
```

## 6. Validate Containers
```bash
docker compose ps
docker logs wordpress-nginx --tail 50
docker logs wordpress-app --tail 50
docker logs wordpress-db --tail 50
```

## 7. Access WordPress
Open the server IP or mapped DNS name in the browser and complete the WordPress installation page.

## 8. Run Health Checks
```bash
bash scripts/health-check.sh
```

## 9. Test Backup Process
```bash
export MYSQL_ROOT_PASSWORD='RootPassword123!'
bash scripts/backup.sh
```

## 10. Deploy Updates
Push changes to the repository and let the configured CI/CD workflow sync the project and restart the stack.

# Troubleshooting Guide

## Problem: Site is not reachable in the browser
Check:
```bash
docker compose ps
ss -tulpn | grep ':80'
curl -I http://localhost
```
Possible causes:
- Nginx container not running
- Host firewall or cloud security group blocking port 80
- Wrong published port in Compose

## Problem: Nginx is up but WordPress returns errors
Check:
```bash
docker logs wordpress-nginx --tail 50
docker logs wordpress-app --tail 50
```
Possible causes:
- Bad fastcgi configuration
- WordPress container not healthy
- Wrong document root or PHP upstream target

## Problem: WordPress cannot connect to database
Check:
```bash
docker logs wordpress-db --tail 50
docker logs wordpress-app --tail 50
```
Possible causes:
- Wrong database credentials in `.env`
- Database container unhealthy
- Volume or initialization issue

## Problem: CI/CD deployment fails over SSH
Check:
- SSH private key format
- host key trust
- remote user permissions
- deployment path ownership

Useful commands:
```bash
chmod 600 ~/.ssh/id_rsa
ssh -v user@server
```

## Problem: Container starts but data disappears
Cause:
- missing or broken volume configuration

Check:
```bash
docker volume ls
docker inspect wordpress-db
docker inspect wordpress-app
```

## Problem: Repository clones, but expected directory is missing
Cause:
- wrong clone target, wrong branch, or confusion about nested folders

Check:
```bash
pwd
ls -la
find . -maxdepth 2 -type d
```

# Security Hardening Notes

## Baseline Controls
- Restrict inbound access to ports 22 and 80/443 only.
- Use SSH keys instead of password authentication.
- Store secrets in CI/CD secrets or server environment files, not in Git.
- Keep Docker images updated and pinned to known versions where appropriate.
- Back up both WordPress data and database content regularly.

## Host Hardening
- Use a dedicated deployment user.
- Limit sudo access.
- Apply system updates regularly.
- Review running services and disable what is not required.
- Monitor disk usage and container logs.

## Container Hardening
- Avoid unnecessary port exposure.
- Use named volumes instead of fragile ad-hoc bind mounts where possible.
- Separate web, app, and database services.
- Rotate credentials when promoting environments.

## Pipeline Safety
- Use repository secrets for SSH keys and remote host details.
- Validate file structure before deployment.
- Exclude `.env` from source control.
- Keep deployment deterministic: same files, same commands, same target path.

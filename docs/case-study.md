# Case Study: Linux Administration, Docker, Containers, CI/CD, and WordPress Deployment

## Executive Summary
This project showcases the design and deployment of a WordPress environment on a Linux server using Docker containers, with Nginx handling web traffic and MariaDB providing the database backend. The solution was packaged with a CI/CD workflow for validation and remote deployment, turning a basic web application setup into a more structured operations exercise.

The case study demonstrates practical execution across five areas:
- Linux administration
- Docker and container operations
- WordPress deployment and service wiring
- CI/CD implementation
- Troubleshooting and operational hardening

## Business Goal
The goal was to create a clean, repeatable deployment pattern for a widely used CMS platform while showing the ability to work across both application delivery and systems operations.

Instead of installing WordPress directly on the host, the stack was containerized to improve portability, isolate services, simplify redeployment, and make the environment easier to maintain.

## Technical Scope
The implementation covered:
- Linux host preparation
- Docker Engine and Docker Compose usage
- Multi-container WordPress stack deployment
- Reverse proxy configuration with Nginx
- Persistent data storage through Docker volumes
- Remote deployment through SSH and rsync
- Pipeline-based validation and deployment
- Basic operational checks, backup logic, and troubleshooting

## Environment Design
The stack separates responsibilities clearly:
- **Nginx** receives HTTP requests and serves as the frontend web layer.
- **WordPress (PHP-FPM)** processes the application logic.
- **MariaDB** stores WordPress data.
- **Docker volumes** preserve WordPress and database data across container restarts.
- **CI/CD pipeline** validates the repository and deploys updates to the Linux host.

This design mirrors a simple but realistic production-style separation of concerns.

## Linux Administration Work Performed
The Linux administration side of the project involved the sort of tasks that matter in real environments, not just textbook commands. Key areas included:

### 1. Host Preparation
The server had to be ready for containerized services, which meant installing Docker, enabling services, validating package dependencies, and ensuring the deployment user had the right access model.

### 2. Service Control
System services and startup behavior had to be managed correctly. This includes enabling required services, verifying daemon status, and making sure the application stack comes back after restarts.

### 3. File Permissions and Ownership Awareness
Practical issues around permissions are common in Linux operations. This project required attention to file paths, writable application directories, SSH key permissions, and deployment-safe access models.

### 4. Remote Access and Deployment Connectivity
SSH-based deployment required correct key setup, trust establishment, and host-side readiness for non-interactive deployment.

### 5. Troubleshooting Mindset
When services fail, the real skill is methodical validation: checking logs, verifying ports, testing process state, confirming service dependencies, and isolating whether the issue is application-side, reverse-proxy-side, or host-side.

## Containerization Strategy
Containerization was used to reduce host-level application complexity and provide cleaner separation between web, app, and database services.

### Why Docker Was the Right Fit
- Faster and more consistent deployment
- Clear service boundaries
- Easier rollback and rebuild behavior
- Simpler environment portability
- Reduced host pollution from direct package installs

### Service Breakdown
- **wordpress-db**: MariaDB container with persistent volume storage
- **wordpress-app**: WordPress PHP-FPM container for the application runtime
- **wordpress-nginx**: Nginx container exposing HTTP traffic and passing PHP requests to WordPress

### Persistence Design
Without persistence, restarting containers would destroy important application state. Docker volumes were therefore used for:
- Database storage
- WordPress content and configuration storage

That is the difference between a toy container demo and something you can actually operate.

## CI/CD Design
The CI/CD layer was intentionally simple and realistic.

### Pipeline Objectives
- Validate the repository structure before deployment
- Reject obviously broken commits
- Copy the latest deployment package to the server
- Restart the stack using Docker Compose

### Validation Stage
The pipeline checks for the presence of required deployment files such as:
- `docker-compose.yml`
- Nginx configuration
- deployment scripts

That prevents dumb failures from hitting the server.

### Deployment Stage
Deployment is performed over SSH using `rsync`, followed by a remote `docker compose up -d` command. This is a straightforward pattern for small to mid-sized self-managed environments.

## WordPress Delivery Outcome
The result is a functioning WordPress stack that can be:
- brought up with Docker Compose,
- served through Nginx,
- persisted across restarts,
- updated through a pipeline,
- and maintained with basic operational scripts.

## Challenges Encountered
This kind of project always exposes the gap between theory and operations. The most common issues in this deployment pattern include:
- containers starting but the site still being unreachable,
- misconfigured ports or security rules,
- reverse proxy misrouting,
- SSH authentication failures in deployment,
- wrong file paths after cloning a repository,
- service startup failures after config edits,
- and permission issues that break application writes.

Those are not edge cases. That is normal systems work.

## Troubleshooting Approach
The project was approached with a layered troubleshooting model:
1. Confirm host reachability.
2. Confirm container state.
3. Confirm port exposure.
4. Confirm reverse proxy behavior.
5. Confirm application logs.
6. Confirm database connectivity.
7. Confirm firewall or cloud security rules.
8. Confirm file paths, permissions, and mounted volumes.

That sequence matters because random troubleshooting wastes time.

## Security and Operational Considerations
Even in a small WordPress stack, there are foundational controls worth applying:
- do not store secrets directly in the repository,
- use non-root remote deployment where possible,
- restrict inbound access to required ports only,
- keep container images updated,
- back up both database and application data,
- validate SSH key permissions,
- and minimize direct changes on the server outside version-controlled deployment files.

## Skills Demonstrated
This project demonstrates hands-on capability in:
- Linux systems administration
- Docker and Docker Compose
- container networking and persistence
- Nginx web service configuration
- WordPress deployment architecture
- CI/CD pipeline structuring
- shell scripting for operations
- remote deployment and troubleshooting

## Final Takeaway
This was not just a WordPress install. It was an infrastructure exercise showing how Linux administration, containers, web services, automation, and operational discipline fit together.

That is the real value of the project: the stack is simple enough to understand, but broad enough to prove practical engineering range.

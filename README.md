# 777373
## Server Configuration Overview
This Virtual Machine hosts a static web server as outlined in the Secure Digital Infrastructure Module from the University of Hull
### 1. Web Server
- **Web server**: Nginx 1.24 (Ubuntu)
- **Root directory**: `/srv/www`
- **Default static site**:
- Served at `/student/` URL
- File: `/srv/www/student.txt` contains student number `777373`
- MIME type: `text/plain`
- **Virtual hosts**:
- `stu-777373-vm1.net.dcs.hull.ac.uk` serves static content
- **Permissions**:
- Default static files: root-owned, read-only for others
- Marketing upload directory: writable by `marketing` user
### 2. Docker
- **Repository**: `https://github.com/sbrl/SDI-Docker.git`
- **Container name**: `sdi-app`
- **Docker image built from**: `Dockerfile` in repository
- **Exposed port in container**: 3000
- **Reverse-proxy configuration**:
- Subdomain: `docker.stu-777373-vm1.net.dcs.hull.ac.uk`
- Requests to this subdomain forwarded via Nginx to container
- **Persistence**:
- Restart policy: `unless-stopped`
- Container starts automatically on boot
### 3. Nginx Reverse-Proxy Configuration
- **Default host**:
- Serves static files
- Handles requests to `stu-777373-vm1.net.dcs.hull.ac.uk` and the VM IP
- **Docker host**:
- Handles requests to `docker.stu-777373-vm1.net.dcs.hull.ac.uk`
- Reverse-proxies to Docker container running HTTP server on port 3000
- **Access & error logs**:
- `/var/log/nginx/srv_www_access.log`
- `/var/log/nginx/srv_www_error.log`
### 4. Maintenance Stuff
- **Check Nginx status**:
sudo systemctl status nginx
- **Test Site**:
curl -i -H "Host: stu-777373-vm1.net.dcs.hull.ac.uk" http://10.31.224.59/student/
- **Test Docker**:
curl -i -H "Host: docker.stu-777373-vm1.net.dcs.hull.ac.uk" http://10.31.224.59
- **Restart Dockers**:
sudo docker restart sdi-app

# 661982_A25_T1: Secure Digital Infrastructure-ACW-777373
## Server Configuration Overview
This Virtual Machine hosts a static web server as outlined in the Secure Digital Infrastructure Module from the University of Hull.
### 1. Web Server Information
- **Web server**: Nginx 1.24 (Ubuntu)
- **Root directory**: `/srv/www`
- **Default static site**:
- Served at `/student/` URL
- File Directory: `/srv/www/student.txt`
- MIME type: `text/plain`
- **Virtual hosts**:
- `stu-777373-vm1.net.dcs.hull.ac.uk` serves static content
- **Permissions**:
- Default static files: root-owned, read-only for others
- Marketing upload directory: writable by `marketing` user
### 2. Nginx Reverse-Proxy Configuration
- **Default host**:
- Serves static files
- Handles requests to `stu-777373-vm1.net.dcs.hull.ac.uk` and the VM IP
- **Docker host**:
- Handles requests to `docker.stu-777373-vm1.net.dcs.hull.ac.uk`
- Reverse-proxies to Docker container running HTTP server on port 3000
- **Access & error logs**:
- `/var/log/nginx/srv_www_access.log`
- `/var/log/nginx/srv_www_error.log`
### 3. Docker Information
- **Repository**: `https://github.com/sbrl/SDI-Docker.git`
- **Container name**: `sdi-app`
- **Exposed port in container**: 3000
- **Reverse-proxy configuration**:
- Subdomain: `docker.stu-777373-vm1.net.dcs.hull.ac.uk`
- Requests to this subdomain forwarded via Nginx to container
- **Persistence**:
- Restart policy: `unless-stopped`
- Container starts automatically on boot (tested)
### 4. Maintenance Commands/Tests
- **Check Nginx status**:
sudo systemctl status nginx
sudo nginx -t
- **Reload Nginx**
sudo systemctl reload nginx
- **Test Site**:
curl -i http://127.0.0.1:3000
- **Restart Dockers**:
sudo docker restart sdi-app
- **Reboor Virtual machine**
  sudo reboot

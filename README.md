# Flapi - Cms Hub Visitor Analytics - Umami

## ⚙️ Setup Environment :
1. Go to connect SSH to VPS : 51.75.248.97 (via le Server extern au cluster K3S)
2. Download and Install Docker in VPS : https://docs.docker.com/engine/install/debian/ (for debian)
3. Download and Install NGINX in VPS : sudo apt install nginx
4. Create directory "umami-flapi" and cd directory
5. Clone project Umami (changer la version du tag si besoin) :
   ```bash
   git clone -b v2.18.1 git@github.com:umami-software/umami.git
   ```
6. Run Umami :
   ```bash
   sudo docker compose up -d
   ```
7. Go to directory nginx for config : /etc/nginx/sites-available
8. Add conf in NGINX (/etc/nginx/sites-available/default.conf) :
   ```bash
   server {
      server_name umami.flapi.org;
      
      #listen 443 ssl http2 ipv6only=on;
      #listen [::]:443 ssl http2 ipv6only=on;

      location / {
       proxy_pass http://localhost:3000/;
      }
   }

   server {
      server_name umami.flapi.org;
      
      listen 80;
      listen [::]:80;
      
      location ^~ /.well-known/acme-challenge {
        allow all;
        default_type "text/plain";
        root /var/www/html;
      }
      
      location / {
        return 301 https://$host$request_uri;
      }
   }
   ```
9. Download and Install certbot for certificat HTTPS :
   ```bash
   sudo apt-get update
   sudo apt-get install certbot python3-certbot-nginx

   sudo certbot --nginx -d umami.flapi.org

   sudo apt-get install cron
   sudo systemctl enable cron
   sudo systemctl start cron
   
   sudo crontab -e
   0 */12 * * * certbot renew --quiet

   sudo nginx -t
   sudo systemctl reload nginx
   ```

<br /><br /><br /><br />


## 🔄 Update Umami :
1. Go to connect SSH to VPS : 51.75.248.97 (via le Server extern au cluster K3S)
2. Go to directory :
   ```bash
   cd /home/debian/umami-flapi
   ```
3. Run commands :
   ```bash
   sudo docker compose pull
   sudo docker compose up --force-recreate -d
   ```

<br /><br /><br /><br />


## 🚀 Production :
1. Go to connect SSH to VPS : 51.75.248.97 (via le Server extern au cluster K3S)
2. Go to directory :
   ```bash
   cd /home/debian/umami-flapi
   ```
3. Run commands :
   ```bash
   sudo docker compose up -d
   ```

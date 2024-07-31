To serve your HTML file with a custom domain name like "cropcraftclub.xyz" using Nginx on Ubuntu, you need to configure your DNS settings and Nginx. Here’s a step-by-step guide:

### 1. Configure DNS
First, make sure your domain "cropcraftclub.xyz" is pointing to your server's IP address. This can be done through your domain registrar’s DNS management interface. Create an `A` record with the following details:

- **Name**: `@` or leave it empty (depends on your registrar)
- **Type**: `A`
- **Value**: Your server's IP address
- **TTL**: Default or 3600 seconds

### 2. Install Nginx
If you haven't already, install Nginx:
```bash
sudo apt update
sudo apt install nginx
```

### 3. Create your HTML file
Place your HTML file in the default web directory:
```bash
sudo nano /var/www/html/index.html
```
Add your HTML content, save, and exit the editor.

### 4. Configure Nginx for your domain
Create a new configuration file for your domain:
```bash
sudo nano /etc/nginx/sites-available/cropcraftclub.xyz
```
Add the following configuration:
```nginx
server {
    listen 80;
    listen [::]:80;

    server_name cropcraftclub.xyz www.cropcraftclub.xyz;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
Save and exit the editor.

### 5. Enable the configuration
Create a symbolic link to enable the new configuration:
```bash
sudo ln -s /etc/nginx/sites-available/cropcraftclub.xyz /etc/nginx/sites-enabled/
```

### 6. Test the configuration
Check for syntax errors:
```bash
sudo nginx -t
```
If there are no errors, restart Nginx to apply the changes:
```bash
sudo systemctl restart nginx
```

### 7. Allow HTTP traffic (if using a firewall)
If you have a firewall enabled (like UFW), allow HTTP traffic:
```bash
sudo ufw allow 'Nginx HTTP'
```

### 8. Access your domain
Open your web browser and go to `http://cropcraftclub.xyz`. You should see your HTML file displayed.

### 9. (Optional) Enable HTTPS with Let's Encrypt
To secure your site with HTTPS, you can use Let's Encrypt. First, install Certbot:
```bash
sudo apt install certbot python3-certbot-nginx
```
Then, run Certbot to obtain and install the certificate:
```bash
sudo certbot --nginx -d cropcraftclub.xyz -d www.cropcraftclub.xyz
```
Follow the prompts to complete the installation. Certbot will automatically configure Nginx to use the new certificate and set up automatic renewal.

### 10. Test HTTPS
After completing the steps, your site should be accessible via HTTPS at `https://cropcraftclub.xyz`.

This setup ensures your domain "cropcraftclub.xyz" serves the HTML file with Nginx on Ubuntu.

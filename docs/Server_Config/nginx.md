# Nginx

[Nginx](https://nginx.org/) is a powerful and widely-used open-source web server, reverse proxy server, and load balancer. Known for its efficiency and low resource usage, Nginx excels in handling concurrent connections and serving static content, making it a popular choice for high-traffic websites.

## Installation

```bash
sudo apt update
sudo apt install nginx
```


## Configuration

Nginx's main configuration file is typically located at `/etc/nginx/nginx.conf`. Additional configurations for specific sites or applications are placed in the `/etc/nginx/sites-available/` directory.

### Basic Configuration

1. **Start Nginx:**

   ```bash
   sudo systemctl start nginx
   ```

2. **Enable Nginx to start on boot:**

   ```bash
   sudo systemctl enable nginx
   ```

#### Server Block (Virtual Host)

To configure a server block for a specific domain or application, create a new configuration file within the `sites-available` directory and create a symbolic link to `sites-enabled`:

```bash
sudo nano /etc/nginx/sites-available/example.com
```

Example configuration:

```nginx
server {
    listen 80;
    server_name example.com www.example.com;

    location / {
        root /var/www/html;
        index index.html;
    }
}
```

Create a symbolic link:

```bash
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```

Restart Nginx to apply changes:

```bash
sudo systemctl restart nginx
```

This basic example serves files from `/var/www/html` for the specified domain.

#### More advanced configurations
[Nginx documentation](https://nginx.org/en/docs/).

! tip "Remember to test your configuration before restarting Nginx"

    ```bash
    sudo nginx -t
    ```


## Restricting files using Nginx. 
Lets restrict files in two locations: (`/admin`, `/private`). 


1. **Create Password Files:**
   Use the `htpasswd` tool to create password files for each location:

   ```bash
   sudo htpasswd -c /etc/nginx/.htpasswd_admin admin_user

   sudo htpasswd -c /etc/nginx/.htpasswd_private private_user
   ```
   ! warn "You will be prompted to enter and confirm passwords for each user."

2. **Configure Nginx:**
   Update your Nginx configuration:

   ```nginx
   server {
       listen 80;
       server_name example.com;

       location /admin {
           auth_basic "Admin Area";
           auth_basic_user_file /etc/nginx/.htpasswd_admin;

           # Your configuration for the admin area goes here
       }

       location /private {
           auth_basic "Private Area";
           auth_basic_user_file /etc/nginx/.htpasswd_private;

           # Your configuration for the private area goes here
       }
        ...
   }
   ```


3. **Test and Reload Nginx:**
   Test the Nginx configuration and reload:

   ```bash
   sudo nginx -t
   sudo systemctl reload nginx
   ```

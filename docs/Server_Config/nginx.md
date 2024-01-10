# Nginx

[Nginx](https://nginx.org/) is a powerful and widely-used open-source web server, reverse proxy server, and load balancer. Known for its efficiency and low resource usage, Nginx excels in handling concurrent connections and serving static content, making it a popular choice for high-traffic websites.

## Installation

```bash
sudo apt update
sudo apt install nginx
```





## Basic Application

1. **Start Nginx:**

   ```bash
   sudo systemctl start nginx
   ```

2. **Enable Nginx to start on boot:**

   ```bash
   sudo systemctl enable nginx
   ```

## Configuration

Nginx's main configuration file is typically located at `/etc/nginx/nginx.conf`. Additional configurations for specific sites or applications are placed in the `/etc/nginx/sites-available/` directory.

### User permission
Our config file starts by defining which users and groups can access the data. 

```nginx
user username groupname;
```
To change file permissions to work with Nginx, please refer to the Permissions page (link here).





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

! tip Remember to test your configuration before restarting Nginx

    `sudo nginx -t`

#### Testing the configuration. 
To test the configuration you can try using curl on localhost: `curl -f localhost:<port>`. w working configuraiton will provide you with: 
```html 
<!DOCTYPE html>
...
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
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

# FastAPI (Gunicorn)

--bind 0.0.0.0:8000 essentially tells Gunicorn to listen on all available network interfaces on port 8000. This is a common configuration when you want your Gunicorn server to be accessible from external sources, such as when deploying a web application.

For example, if you deploy a Flask, Django or FastAPI application with Gunicorn using this bind option, your application will be accessible over HTTP at http://your_server_ip:8000.



## Gunicorn YAML Config

```yaml

workers = 3              # number of workers Gunicorn will spawn 

bind = '127.0.0.1:8000'  # this is where you declare on which address your 


# gunicorn app is running.
                         # Basically where Nginx will forward the request to

pidfile = '/var/run/gunicorn/mysite.pid' # create a simple pid file for gunicorn. 

user = 'user'          # the user gunicorn will run on

daemon = True          # this is only to tell gunicorn to deamonize the server process

errorlog = '/var/log/gunicorn/error-mysite.log'    # error log

accesslog = '/var/log/gunicorn/access-mysite.log'  # access log

proc_name = 'gunicorn-mysite'            # the gunicorn process name
```
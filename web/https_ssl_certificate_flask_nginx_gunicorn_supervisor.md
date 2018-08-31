# HTTPS site with SSL certificate using Flask/Gunicorn/Nginx/Supervisord

[//]: # (Image References)
[img1]: ../images/https_ssl_certificate_flask_nginx_gunicorn_supervisor/your_connection_is_not_private.png
[img2]: ../images/https_ssl_certificate_flask_nginx_gunicorn_supervisor/CSR.png
[img3]: ../images/https_ssl_certificate_flask_nginx_gunicorn_supervisor/https.png

In this post, I will make a Flask Web App with trusted SSL certificate. A trusted SSL certificate can give your user more confident of your App. Let's do it. In addition, this post also illustrate how to setup a simple Flask Apps with Nginx/Gunicorn/Supervisord.

![Https][img3]

## Get SSL Certificate

When you make your site with HTTPS, without a trusted SSL certificate, you will see following prompt:

![Not Private][img1]

Most of SSL certificate is too expensive for individual, however you can buy one cheap SSL from [Namecheap](www.namecheap.com)

After you buy one SSL certificate, you need to activate the certificate.

How to activate the certificate, Ref: https://www.namecheap.com/support/knowledgebase/article.aspx/794/67/how-to-activate-ssl-certificate

### Generate Certificate Signing Request(CSR)

An easy way: https://decoder.link/csr_generator

Type in as:

![CSR][img2]

After you click GENERATE, save the generated `CERTIFICATE REQUEST` and `PRIVATE KEY` and `CERTIFICATE` some where you can keep. We need `CERTIFICATE REQUEST` and `PRIVATE KEY` in the next steps.

Enter `CERTIFICATE REQUEST` in Namecheap.com activate page.

Then choose the Domain Control Validation(DCV) method. This is used to prove your site is actually belong to you. There are three way to prove that, via Email/HTTP-based/DNS-based.
* Email: only accept some certain email, eg admin@your_site.com
* HTTP-based: need to put a file provided by namecheap into your site
* DNS-based(I choose): add a CNAME record into your stie's DNS records.

The DNS CNAME record will be provided after you submitted the `DCV`.

I am using `Alibaba Cloud` as the DNS service provider. So I add the CNAME record there.

After that, wait for sometime(DNS based usually cost half hour). You can see the `DCV` successful, and the `Certificate Status` will change from `In Progress` to `ISSUED`.

At that same time, you will received the SSL certificate in your registered email in namecheap. We will use this SSL certificate in the next step.

## Install SSL Certificate

The SSL Certificate email will contains a `.zip`, which containing the files:

* your_site.cert
* your_site.ca-bundle

Concatenate these two files:

`cat your_site.cert your_site.ca-bundle >> your_site_cert_chain.crt`

Copy `your_site_cert_chain.crt` and the `PRIVATE KEY` generated during CSR into your site's `/etc/ssl/`

### Deploy Flask app with Nginx using Gunicorn and Supervisord

You may ask why we need both Nginx/Gunicorn/Supervisord for a Flask app. Here are some key points:
* Recall the definition of 3 tier architecture: web server <-> app server <-> db server
* Nginx good to serve the static files (images, CSS, etc)
* Requests that need to be dynamically generated need to talk to application server.
* Nginx cannot directly talk to Flask, so we need Gunicorn
* Supervisord is not a must have components, it makes the Flask App run as a service.
* Components summaries
  * Flask: App server backend
  * Nginx: Web Server & Reverse Proxy Server
  * Gunicorn: Deploy Flask Apps, a Python WSGI HTTP Server
  * Supervisor: Monitor and control Gunicorn process
* Ref: [Why do I need Nginx and something like Gunicorn?](https://serverfault.com/questions/331256/why-do-i-need-nginx-and-something-like-gunicorn)

### Install required packages:

I am using Ubuntu 16.04.

`$ sudo apt-get install nginx supervisor python-pip python-virtualenv`

Create virtual env:

`$ virtualenv flask`

Activate virtual env:

`$ source flask/bin/activate`

### Install Flask

`$ pip install Flask`

Make a working folder for Apps

`$ mkdir -p flask/work/hello_world`

Code for you Apps:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return "Hello World, I am a Machine Learning Architect."

if __name__ == '__main__':
    app.run(debug=True)
```


Run Flask Apps:

`$ python app.py`

But it is not safe to use the Flask development server for a production environment. So we need Gunicorn to server our python code.

### Setup Gunicorn

`$ pip install gunicorn`

Start Gunicorn to serve our Flask app

``` bash
cd flask/work/hello_world
gunicorn app:app -b 192.168.0.xx:8000
```

Change `192.168.0.xx` to your local IP.

### Using Supervisord to monitor our Flask Apps

`sudo vim /etc/supervisor/conf.d/gunicorn.conf`

Add config as follow:

```
[program:flask_hello_world]
directory=/home/user/flask/work/hello_world
command=/home/user/flask/bin/gunicorn app:app -b 192.168.0.xx:8000
autostart=true
autorestart=true
stderr_logfile=/home/user/flask/work/hello_world/log/hello_world.err.log
stdout_logfile=/home/user/flask/work/hello_world/log/hello_world.out.log
```

Change `user` to your actual folder.

Enable the Supervisord

``` bash
sudo supervisorctl reload
sudo service supervisor restart
```

And also you can check the status of supervisrd as

`sudo supervisorctl status`

BTW: every time you made change to your apps, you need to `sudo service supervisor restart`

### Setup Nginx server block for Flask Apps

`$ sudo vim /etc/nginx/conf.d/virtual.conf`

Add following config:

```
# Redirect http to https
server {
    listen 80;
    server_name your_site.com;

    rewrite ^(.*)$  https://$host$1  permanent;
}

# Define HTTPS
server {
    listen 443;
    server_name your_site.com;

    ssl on;
    ssl_certificate /etc/ssl/your_site_cert_chain.crt;
    ssl_certificate_key /etc/ssl/your_site.key;

    client_max_body_size 4G;
    access_log /opt/your_site/logs/nginx_access.log;
    error_log /opt/your_site/logs/nginx_error.log;

    location / {
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;  # <-
        proxy_set_header Host $http_host;
        proxy_redirect off;

        if (!-f $request_filename) {
            proxy_pass http://192.168.0.xx:8000;
            break;
        }
    }
}
```

Restart Nginx:

```
sudo nginx -t
sudo service nginx restart
```

BTW: Everytime you make change to your `virtual.conf`, you need to `sudo service nginx restart` to make it take effect.

### Congratulations! Now we have a trusted HTTPS Flask Apps online:

![Https][img3]

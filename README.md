# Udacity Project 3 - Deploying to Linux Servers
This project is the last of three projects in the **Udacity Full Stack Web Developer Nanodegree program** as part of the **[one million arab coders initiative](http://www.arabcoders.ae)**.

## Purpose of the project
The purpose of the project is to **deploy** the application built in project 2 **to a linux server**. We are to take security into considerations when creating the server for our project aswell as using best practice methods to our best ability. In the project we use amazon lightsail service to create a ubuntu server. And to publish the appliaction we are to use apache2 and mod_wsgi, aswell as postgresql as database.



### SSH into server and understand the project
Create a new GitHub repository and add a file named README.md.
Your README.md file should include all of the following:

**1. The IP address and SSH port so your server can be accessed by the reviewer.**

**IP-adress and SSH:** (Will be deleted when course is finished!)

STATIC-IP: 52.29.113.63
Port: 2200
```
Command: ssh -i ".ssh/grader" grader@52.29.113.63 -p 2200
```


**2. The complete URL to your hosted web application.**
```
URL: http://www.52.29.113.63.xip.io
```

**3. A summary of software you installed and configuration changes made.**
Most programes installed are from the requirements.txt file that i got from the VAGRANT vertual enviroment that i developed the app.
but a list of the most important are given here below.

**List:** 
- sqlalchemy
- passlib
- requests
- oauth2client
- flask
- flask_httpauth
- Redis
- Apache2
- mod_wsgi
- postgresql

**Configurations:**
>**The wsgi-file:**
Path: (/var/www/FlaskApp/myapp.wsgi)
```
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/FlaskApp/FlaskApp")

from app import app as application

application.secret_key = 'super_secret_key'
```


>**Apache config file:**
Path: /etc/apache2/sites-available/001-ud-project3.conf
It is in the folder sites-available, but since it is enabled, its linked to file in sites-enabled.

```
<VirtualHost *:80>
                ServerName www.35.157.119.244.xip.io
                ServerAdmin admin@mywebsite.com

                WSGIScriptAlias / /var/www/FlaskApp/myapp.wsgi

                # Enable headers (for authentication)
                WSGIPassAuthorization On

                <Directory /var/www/FlaskApp/FlaskApp/>
                        Order allow,deny
                        Allow from all
                </Directory>

                Alias /static /var/www/FlaskApp/FlaskApp/static
                <Directory /var/www/FlaskApp/FlaskApp/static/>
                        Order allow,deny
                        Allow from all
                </Directory>

                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- ```WSGIScriptAlias / /var/www/FlaskApp/myapp.wsgi``` links to the python flask app.
```WSGIPassAuthorization On```is important to have otherwise authorization wont work when working with the API-endpoints. By default it is turned off so that apache wont handle password or any auth-info in the headers. I used Postman.

> **Postgresql:** After creating the postgres user and the database: include the following as the engine for connecting to the database.
```
engine = create_engine('postgresql://user:password@localhost/db_name')
```

**4. A list of any third-party resources you made use of to complete this project.**
- Postman
- xip.io (for making the url from an ip - paste into a browser)
- google (I write this because it was alot of googling and searching for solutions online and google helped alot. I found many website that helped me, but i found it thanks to google)
- Github


**5. Important links:**(No specific order, you use them as needed!)
* Getting redis to work: [Press here...](https://tecadmin.net/install-redis-ubuntu/)
* Getting the api-endpoints to work in mod_wisgi: Press [here..](https://stackoverflow.com/questions/29024415/flask-httpauth-verify-password-function-not-receiving-username-or-password) and [here..](https://code.google.com/archive/p/modwsgi/wikis/ConfigurationDirectives.wiki#WSGIPassAuthorization), in that order preferably.
* Time zone: [Press here to read...](https://askubuntu.com/questions/138423/how-do-i-change-my-timezone-to-utc-gmt/138442)
* Deploying flask-app on ubuntu server: [Press here to read ...](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
* Incase password hash is to long: [Press here to see solution ...](https://stackoverflow.com/questions/7729287/postgresql-change-the-size-of-a-varchar-column)
* In case of problem with memory when installing the requirements.txt file: [Press here](https://github.com/pypa/pipenv/issues/451)


``` OBS!  sudo tail -f /var/log/apache2/error.log``` has been very helpful for me when debugging error in the apache server. I ow alot to this command and google.

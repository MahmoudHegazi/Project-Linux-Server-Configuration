# Project-Linux-Server-Configuration

##  The IP address: 3.120.172.142
##  second server on : http://18.196.30.95/ 

##  SSH port : 2200

## HTTP port: 80

## user grader has sudo access 

![app image](https://github.com/MahmoudHegazi/Project-Linux-Server-Configuration/blob/master/working.JPG)
![certificate image](Cert.JPG)

## third-party resources I made use of to complete this projet:

1-  google Ouath2 for google signup.
2-  Flask Docs
3-  PostgreSQL Docs
4-  Apache Docs

## summary of software installed
1. apache2
2. python 2.7
3. pip
4. finger
5. python-setuptools libapache2-mod-wsgi
6. postgresql
7. Flask
8. python-pip
9. python-sqlalchemy
10. python3-oauth2client
11. python3-httplib2
12. python3
13. pip3
14. python-setuptools
15. libapache2-mod-wsgi
16. postgresql
17. git
18. python-pip
19. python-psycopg2
20. python-dev
21. google-api-python-client

## summary of configurations made:

1. Download keypair 
2. use sudo apt-get update   and   apt-get upgrade
3. sudo apt-install finger
4. sudo adduser grader
5. give grader access sudo  by creating this file using this command
> nano /etc/sudoers.d/grader  then add the following line 
> student ALL=(ALL) NOPASSWD:ALL
6. do the same for student user
7. chmod 700 .ssh
8. chmod 644 .ssh/authorized_keys I aDownloaded the key from lighsail 
9. vim /etc/ssh/sshd_config and then change Port 22 to Port 2200 , save & quit.
10. sudo service ssh restart


## Configure the Uncomplicated Firewall (UFW)
> Only allow connections for SSH (port 2200), HTTP (port 80), and NTP (port 123):
 
> sudo ufw default deny incoming
> sudo ufw default allow outgoing 
> sudo ufw allow 2200/tcp  (don't forget to add custom tcp 2200 on network in lighsail and change it in config to us port 2200)
> sudo ufw allow 80/tcp
> sudo ufw allow 123/udp
> sudo ufw enable
> sudo allow ssh
> sudo allow www


## Configure the local timezone to UTC
* Configure the time zone sudo dpkg-reconfigure tzdata
* then select your country 

## Install and configure Apache to serve a Python mod_wsgi application

1. Install Apache sudo apt-get install apache2
2. Install mod_wsgi sudo apt-get install python-setuptools libapache2-mod-wsgi
3. Restart Apache sudo service apache2 restart


## Install and configure Apache to serve a Python mod_wsgi application

* Install Apache sudo apt-get install apache2
* Install mod_wsgi sudo apt-get install python-setuptools libapache2-mod-wsgi
* Restart Apache sudo service apache2 restart
* Install and configure PostgreSQL
* Install PostgreSQL sudo apt-get install postgresql

Check if no remote connections are allowed sudo vim /etc/postgresql/9.3/main/pg_hba.conf

> Login as user "postgres" sudo su - postgres

> Get into postgreSQL shell psql

## Create a new database named catalog and create a new user named catalog in postgreSQL shell

> postgres=# CREATE DATABASE catalog;
> postgres=# CREATE USER catalog;
> Set a password for user catalog

> postgres=# ALTER ROLE catalog WITH PASSWORD 'password';

## Give user "catalog" permission to "catalog" application database

> postgres=# GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;
> Quit postgreSQL postgres=# \q

> Exit from user "postgres"

exit

* Install Git using sudo apt-get install git
* Use cd /var/www to move to the /var/www directory
* Create the application directory sudo mkdir FlaskApp
* Move inside this directory using cd FlaskApp
* Clone the Catalog App to the virtual machine git clone https://github.com/kongling893/Item_Catalog_UDACITY.git
* Rename the project's name sudo mv ./Item_Catalog_UDACITY ./FlaskApp
* Move to the inner FlaskApp directory using cd FlaskApp
* Rename website.py to __init__.py using sudo mv website.py __init__.py
* Edit database_setup.py, website.py and functions_helper.py and change engine = create_engine('sqlite:///toyshop.db') to engine = *  create_engine('postgresql://catalog:password@localhost/catalog')

> Install pip sudo apt-get install python-pip
> Use pip to install dependencies sudo pip install -r requirements.txt
> Install psycopg2 sudo apt-get -qqy install postgresql python-psycopg2
> Create database schema sudo python database_setup.py


## another steps (important)

> sudo apt-get install libapache2-mod-wsgi python-dev
> sudo a2enmod wsgi
> cd /var/www
> sudo mkdir FlaskApp

Your directory structure should now look like this:

* |----FlaskApp
* |---------FlaskApp
* |--------------static
* |--------------templates
 

clone your repo inside FlaskApp
and rename it using sudo mv Udacity_catalog FlaskApp

> cd /var/www/FlaskApp/FlaskApp
> sudo virtualenv venv
> source venv/bin/activate 
> sudo pip install Flask

## if you are facing no module error use pip install module name

Configure and Enable a New Virtual Host


> sudo nano /etc/apache2/sites-available/FlaskApp.conf


## Add the following lines of code to the file to configure the virtual host. Be sure to change the ServerName to your domain or cloud server's IP address:

```python
<VirtualHost *:80>
		ServerName mywebsite.com
		ServerAdmin admin@mywebsite.com
		WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
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


> Save and close the file.

> sudo a2ensite FlaskApp

## Create the .wsgi File

* Apache uses the .wsgi file to serve the Flask app. Move to the /var/www/FlaskApp directory 
* and create a file named flaskapp.wsgi with following commands:


> cd /var/www/FlaskApp
> sudo nano flaskapp.wsgi 

## Add the following lines of code to the flaskapp.wsgi file:

```python
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/FlaskApp/")

from FlaskApp import app as application
application.secret_key = 'Add your secret key'
```

Now your directory structure should look like this:

* |--------FlaskApp
* |----------------FlaskApp
* |-----------------------static
* |-----------------------templates
* |-----------------------venv
* |-----------------------__init__.py
* |----------------flaskapp.wsgi

Restart Apache

## Errors Handling (Big Errors):

### 1- Import Error No module name ..... (this has 3 answers):
*  detect your python app version 2 or 3
*  use apt-get install for this package first to install it on your machine
*  CD /var/www/FlaskApp/FlaskApp , and use sudo pip install module name
*  some modules not updated error like oauth2client check my solve below

## (Example1): No module named 'oauth2client' 
I'm using python3 in project to check your python verison try this pip -V python -V
1. sudo apt-get install python3-oauth2client 
2. sudo pip install --upgrade oauth2client
3. sudo -H pip install --upgrade google-api-python-client


-----------------------------------------------------------------------------------------------------

## (Example2): No module named 'httplib2'  
1. sudo pip install httplib2  (python2)
2. sudo pip install --upgrade pip
3. I was thinking after I used pip install httplib2 it would solve the problem but nope
4. I found I have to install the packge first using apt-get so to solve this error use this command
5. sudo apt-get install python-httplib2 if you still has the same error try this  command
6. pip2 install httplib2 --upgrade  (python2)
7. pip3 install httplib2 --upgrade (python3)

Please Note The command solved this problem for me was:
sudo apt-get install python3-httplib2

-----------------------------------------------------------------------------------------------------

## (Example3): No module named 'database_setup'
* this error takes from me alot to solve it the answer is simple 
* just add  .  before the database_setup
* cd /var/www/FlaskApp/FlaskApp , nano __init__.py  , add . before database_setup

from .database_setup import Base, Car, CarType, User

-----------------------------------------------------------------------------------------------------

## (Example4): FileNotFoundError:[Errno 2] No such file or directory: 'client_secret...'

* if you reched this error you are 1 step away from configure the lightsail so how we fix it.
* Please Note at the project we did we didn't specify the path for the secret key just add var with app path

* like this: basedir = '/var/www/FlaskApp/FlaskApp/'     
* CLIENT_ID = json.loads(open(basedir + 'client_secret.json', 'r').read())['web']['client_id']

----------------------------------------------------------------------------------------------------------

## Important notes:

1. after each fix or command use this: sudo service apache2 reload  
2. to check the new error after visit your IP use this command: tail -f /var/log/apache2/error.log
3. if you got error no () next print , I solve this by using the python3 print syntax Like this print("hello")
4. make sure you are using pip in the app dir cd /var/www/FlaskApp/FlaskApp
5. make it app.run() in you __init__.py.
6. make sure from the name of your secret key.json and the secret key in the project.
7. some errors for no Module name ... can be solved without using pip install module name
8. use python __init__.py to check for errors in the app or python database_setup.py to check the errors in the database
9. replace the engine in __init__.py and lotsofcars.py and database_setup.py to this
engine = create_engine('postgresql://catalog:password@localhost/catalog')

### "note catalog is database name and other catalog is the database user we created early"

## usefull Links (important):

https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
https://github.com/jungleBadger/-nanodegree-linux-server-troubleshoot/blob/master/OAuth_login/README.md
https://github.com/jungleBadger/-nanodegree-linux-server-troubleshoot/tree/master/python3%2Bvenv%2Bwsgi
https://github.com/jungleBadger/-nanodegree-linux-server
https://knowledge.udacity.com/questions/24195
https://stackoverflow.com/questions/17309288/importerror-no-module-named-requests







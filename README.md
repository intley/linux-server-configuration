# Linux Server Configuration
Linux Server Configuration involves taking a baseline installation of a Linux distribution on a virtual machine and prepare it to host your web applications, to include installing updates, securing it from a number of attack vectors and installing/configuring web and database servers.
In this case, the web application hosted was the Item Catalog application.

This project is part of the [Full Stack Web Developer Nanodegree](https://www.udacity.com/course/full-stack-web-developer-nanodegree--nd004) at _Udacity_.

## Server Details
The website can be accessed using the following URL's:

IP address: `
54.202.74.198`

SSH Port:`
2200`

URL: `http://54.202.74.198/` OR
 `http://ec2-54-202-74-198.us-west-2.compute.amazonaws.com/`

The Linux Server instance has been configured to be ssh into the port 2200, in addition to the default port of 22 in order to prevent any risks of ssh reconfiguration.

## Login Details for ssh (grader):
To login as the grader using ssh,
run the following command in your local terminal:
```
ssh grader@54.202.74.198 -p 2200 -i ~/.ssh/<privatekey>
```

The privatekey is paired with the public key stored on the server. In order to gain access, you must possess the private key.

#### Note to the Reviewer:
The private key contents will be provided in the Notes section for you to gain access.

A paraphrase has also been created for the user grader and this shall also be provided in the Notes section.

## Configuration changes
### Creating a new user:
A new user grader can be created using the following command:
```
sudo adduser grader
```

### Providing the grader sudo access:
Provide the user grader sudo access using the command:
```
sudo adduser grader sudo
```

You may verify that the user has been provided sudo access by run a simple sudo command such as:
```
sudo cat /etc/passwd
```

If your command returns an output containing user information for the linux instance, you can be certain that the grader has now been provided sudo access.

### Updating all currently installed packages:
To get information about all packages that are required to be updated, run the following command:
```
sudo apt-get update
```

The above command retrieves the update information along with the package indexes.

To update the packages with the retrieved information, run the following command:
```
sudo apt-get upgrade
```

If at login `*** System restart required ***` is displayed, run the following command to reboot the machine:
`reboot`

### Setting up a SSH key-pair for user grader:
To create a SSH key pair, run the following command in your local Terminal.
```
ssh-keygen
```

Enter the directory where you wish to save the key-pair. It is recommended for the purpose of the project to store the file as follows:
`/Users/<User:Name>/.ssh/<filename>`

Inside this directory, once you run the command, you will find two files:
`<filename>.pub` and `<filename>`

`<filename>` is the private key which we will use to gain access to the server.

`<filename>.pub` is the public key which we must store in the server.

### Storing the public key in the server:
Login as the grader, and run the following commands to create the directory where we must store the public key:

```
sudo mkdir /home/grader/.ssh
sudo cp /home/ubuntu/.ssh/authorized_keys /home/grader/.ssh/
chmod 700 /home/grader/.ssh
chmod 644 /home/grader/.ssh/authorized_keys
```
The chmod commands create the file permissions for the `.ssh` directory and the `authorized_keys` file.

Edit the authorized_keys file and append the contents of `<filename>.pub` to this file.

If you wish to be able to ssh into this linux server instance using the user ubuntu, follow the above steps similarly changing the path.

You should now be able to login as:
`ssh grader@54.202.74.198 -p 22 -i ~/.ssh/<filename>`

### Changing the ssh port to a non-default port (2200):

Edit the file /etc/ssh/sshd_config and in the line where it says `Port 22` add `Port 2200` in the next line.

Restart the ssh service:
`sudo service ssh restart`

You should now be able to login with the non-default port as follows:
`ssh grader@54.202.74.198 -p 2200 -i ~/.ssh/<filename>`

### Configuring Uncomplicated Firewall (UFW):
- First, check the status of the Firewall using the following command:
`sudo ufw status`
It should say negative.

- By default, block all incoming connections on all ports:
`sudo ufw default deny incoming`

- By default, allow all outgoing connections on all ports:
`sudo ufw default allow outgoing`

- Allow incoming connections for SSH on port 22:
`sudo ufw allow ssh`

- Allow incoming connections for SSH on port 2200:
`sudo ufw allow 2200/tcp`

- Allow incoming connections for HTTP on port 80:
`sudo ufw allow www`

- Allow incoming connections for NTP on port 123:
`sudo ufw allow ntp`

- To check all the rules added:
`sudo ufw show added`

- Now, enable the Firewall using the following command:
`sudo ufw enable`

- Verify the Firewall is enabled by checking the status:
`sudo ufw status`

### Deny root login and Enforce SSH authentication:
Edit the file /etc/ssh/sshd_config:

In the file, change the line where it says
`PermitRootLogin without-password` to `PermitRootLogin no`

To Enforce SSH authentication, make sure the following line reads as:
`PasswordAuthentication no`

Restart the SSH service:
`sudo service ssh restart`

### Change Timezone to UTC:
Run the command `date`
If the Timezone does not read as UTC, you can change it to the UTC timezone as shown below:
`sudo timedatectl set-timezone UTC`

### Installing Apache to serve a mod_wsgi application:
Apache is the HTTP Web Server that is used to serve the application:
`sudo apt-get install apache2`

Now, install the application handler mod-wsgi:
`sudo apt-get install libapache2-mod-wsgi`

### Install Git:
Install git by running the following command:
`sudo apt-get install git`

Set up git by running the following commands:
```
git config --global user.name "username"
git config --global user.email "email@domain.com"
```

### Installing Flask, SQLAlchemy and other dependencies:
- We will install the Flask dependencies and  attempt to run our Flask application in a virtual environment.
First, run the following commands:
```
sudo apt-get install python-psycopg2 python-flask
sudo apt-get install python-sqlalchemy python-pip
sudo pip install oauth2client
sudo pip install requests
sudo pip install httplib2
sudo pip install flask-seasurf
sudo source venv/bin/activate
sudo pip install Flask-SQLAlchemy
sudo pip install sqlalchemy
```

### Deploying Flask Application:
- Run the following command to enable mod-wsgi, once it is installed.
`sudo a2enmod wsgi`

##### Setting up directories for the Flask application:

- Move into the following directory bu running the following command:
`cd /var/www`

- Now let's create the application directory structure. Run the following commands:
```
sudo mkdir catalog
cd catalog
sudo mkdir catalog
cd catalog
sudo mkdir static templates
```

- Let's create a `__init__.py` file which will run our Flask application:
`sudo vi __init__.py`

- Inside the `init` file, type the following code in order to test if our Flask deployment works.
```
from flask import Flask
app = Flask(__name__)
@app.route("/")
def hello():
    return "Hello, everyone!"
if __name__ == "__main__":
    app.run()
```

- Close and save the file.

##### Installing virtualenv and Testing FlaskApplication:
- The following commands will create a virtualenv for our Flask Application.
```
sudo apt-get install python-pip
sudo pip install virtualenv
sudo virtualenv venv
```

- Install Flask in that environment by activating the virtual environment using: `source venv/bin/activate`

- Now, Install Flask using:
`sudo pip install Flask`

- Run the following command to test if the installation is successful and the app is running:
`sudo python __init__.py`

- It should display "Running on http://127.0.0.1:5000/". If you see this message, we have successfully configured the app.

- To deactivate the environment, run the command: `deactivate`

### Deploying Item Catalog into application structure:
- `cd` into the following directory:
`var/www/`

- We shall clone our git repository into this directory and then organize the application structure accordingly as follows:
`sudo git clone <git:repository link>`

- Move the important and required files into the `/var/www/catalog/catalog` directory. Make use of the `sudo mv` command to achieve this.

- Replace the `__init__.py` file with the `application.py` by using:
`sudo mv application.py __init__.py`

### Creating the wsgi Configuration:
- Change into the following directory:
`sudo cd /var/www/catalog`

- Create the wsgi file by running: `sudo vi catalog.wsgi` and add the following code:

```
!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/catalog/")

from catalog import app as application
application.secret_key = 'Add your secret key'
```

- The application secret key can be located in your Flask application file, in this case, `__init__.py`


### Application File Structure:
- Finally your file structure should read like this:
```
grader@ip-172-26-10-184:/var/www/catalog$ ls
catalog  catalog.wsgi
grader@ip-172-26-10-184:/var/www/catalog$ cd catalog/
grader@ip-172-26-10-184:/var/www/catalog/catalog$ ls
client_secrets.json  database_items.py  database_setup.py  __init__.py  static  templates  venv
```

### Apache Configuration to deploy Flask App:
- First, go to your website address (example: `http://54.202.74.198/`) and verify if you have Apache installed. If you have, then your website should display a HTML webpage that will display it works!

- Change into the `/etc/apache2/sites-enabled/000-default.conf` directory and edit the file.

- Add the following line at the end of the `<VirtualHost *:80>` block, right before the closing `</VirtualHost>` line: `WSGIScriptAlias / /var/www/html/catalog.wsgi`

- To serve the Item Catalog app using the Apache web server, a virtual host configuration file needs to be created in the directory `/etc/apache2/sites-available/`, in this case called `catalog.conf`

- The contents of the file need to be something like this:
```
<VirtualHost *:80>
        ServerName 54.202.74.198
        ServerAdmin admin@54.202.74.198
	ServerAlias ec2-54-202-74-198.us-west-2.compute.amazonaws.com
        WSGIScriptAlias / /var/www/catalog/catalog.wsgi
        <Directory /var/www/catalog/catalog/>
            Order allow,deny
            Allow from all
        </Directory>
        Alias /static /var/www/catalog/catalog/static
        <Directory /var/www/catalog/catalog/static/>
            Order allow,deny
            Allow from all
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        LogLevel warn
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

- Disable the default virtual host using:
`sudo a2dissite 000-default.conf`

- Enable the catalog application with:
`sudo a2ensite catalog.conf`

- Finally, restart the Apache server using:
`sudo service apache reload`

### Installing and Configuring PostgreSQL:

- Install PostgreSQL by running the following command:
`sudo apt-get install postgresql postgresql-contrib`

- Create a PostgreSQL user called `catalog` with:
`sudo -u postgres createuser -P catalog`

- You are prompted for a password. This creates a normal user that can't create databases, roles (users). The password was set as `catalog`.

- Create an empty database called catalog with:
`sudo -u postgres createdb -O catalog catalog`

- Update the create_engine statement line in the necessary `flask .py` files to read as:
`engine = create_engine('postgresql://catalog:catalog@localhost/catalog')`

Documentation: [PostgreSQL](https://help.ubuntu.com/community/PostgreSQL)

### Populating the Database and other fixes for Application:

- Run the following commands inside the `/var/www/catalog/catalog` directory:
```
python database_setup.py
python database_items.py
```

- The database should be now be populated, and if you visit the host address, you should be able to view the Item Catalog application.

- You will also need to update the directory for the oAuth `client_secrets` file. Also, update the credentials in `console.developers.google.com` to the server address.

### Resources:
- Udacity Forums
- [SteveWooding's repository](https://github.com/SteveWooding/fullstack-nanodegree-linux-server-config)
- [Anumsh's repository](https://github.com/anumsh/Linux-Server-Configuration)

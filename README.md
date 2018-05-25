# Setup instructions for deploying a Flask app in Digital Ocean Droplet

1. log into droplet (it is assumed you have created your droplet with ssh enabled)

    ```bash
    ssh root@<your_droplet_public_ip>
    ```
2. install apache

    ```bash
    sudo apt-get update
    sudo apt-get install apache2
    ```
3. install and enable mod_wsgi

    ```bash
    sudo apt-get install libapache2-mog-wsgi python-dev
    sudo a2enmod wsgi
    ```
4. create the flask app

     ```bash
    cd /var/www
    git clone https://github.com/nshathish/flask-lab2.git
    cd flask-lab2
    ```
5. setup virtual environment and install flask

    ```bash
    sudo apt-get install python-pip
    sudo pip install virtualenv
    sudo virtualenv venv
    source venv/bin/activate
    sudo pip install Flask
    ```
    **_try running the app_**
    ```bash
    sudo python app.py
    ```
    **_if app is running ok, deactivate environgment_**
    ```bash
    deactivate
    ```
6. serving flask app with wsgi

    **_inside /var/www/flask-lab2_**
    ```bash
    nano flask_lab2.wsgi
    ```
    **_in flask_lab2.wsgi write the following_**
    ```python
    import sys
    import logging
    logging.basicConfig(stream=sys.stderr)
    sys.path.insert(0, "/var/www/flask-lab2")
    from app import app as application
    ```
7. configure and enable virtual host

    ```bash
    sudo nano /etc/apache2/sites-available/flask-lab2.conf
    ```
    **_write the following in the flask-lab2.conf_**
    ```bash
    <VirtualHost *:80>
      ServerName flask-lab2.myserver.com
      ServerAdmin root@nshathish.com1
      WSGIScriptAlias / /var/www/flask-lab2/flask-lab2.wsgi
      WSGIDaemonProcess flask-lab2
      <Directory /var/www/flask-lab2>
          WSGIProcessGroup flask-lab2
          WSGIApplicationGroup %{GLBAL}
          Order deny,allow
          Allow from all
      </Directory>
      ErrorLog ${APACHE_LOG_DIR}/error.log
      LogLevel warn
      CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    ```
8. ## disable the default site and enable flask-lab2

    ```bash
    sudo a2dissite 000-default.conf
    sudo a2ensite flask-lab2.conf
    sudo service apache2 reload
    ```

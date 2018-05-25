# flask-lab2
1. log into droplet (it is assumed you have created your droplet with ssh enabled)

  `ssh root@<your_droplet_public_ip>`
  
2. install apache

  `sudo apt-get update`
  
  `sudo apt-get install apache2`
  
3. install and enable mod_wsgi
  
  'sudo apt-get install libapache2-mog-wsgi python-dev`
  
  `sudo a2enmod wsgi`
  
4. create the flask app

`cd /var/www`
`git clone https://github.com/nshathish/flask-lab2.git`
`cd flask-lab2`

  

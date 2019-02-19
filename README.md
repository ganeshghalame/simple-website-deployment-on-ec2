# simple-website-deployment-on-ec2
Simple angular website deployment on Amazan EC2 instance

# Setup Server configurations

Inboud Rules: Add `HTTP` Port `80` source anywhere

# Install Server

1. [Nginx installation](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04) `sudo apt install nginx -y`

    a. Go to nginx site configuration `cd /etc/nginx/site-available`
    
    b. Backup Default config file `sudo cp default default.bk`
    
    c. Setup config in `sufo vim default` file, set root `/var/www/html/dist/demo` and [fallback config](https://angular.io/guide/deployment#fallback-configuration-examples) `try_files $uri $uri/ /index.html;`
    
    d. Restart server `sudo service nginx restart`

2. [Apache2 installation](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-18-04-quickstart) `sudo apt install apache2 -y`
 
    a. Go to apache site configuration `cd /etc/apach2/site-available`
    
    b. Backup default config file `sudo cp 000-default.conf 000-default.conf.bk`
    
    c. Setup config in `sudo vim 000-default.conf` file, set `DocumentRoot /var/www/html/dist/demo`
    
    d. Add [`.htaccess`](https://angular.io/guide/deployment#fallback-configuration-examples) at root 
    
    e. Restart the server `sudo service apache2 restart`

# Create simple Angular App:

1. `ng new demo`

2. `ng serve` to test demo app

3. `ng build --prod`

# Continuous delivery

1. Jenkins/Codepipeline/Teamcity etc

2. Git clone and create build

3. Copy deliverable directly using scp/sftp etc
  
    a. Create zip of dist folder`zip -r  dist.zip dist/`
    
    2. Copy zip to destinsation server using `scp -i ~/.ssh/key.pem dist.zip  server URL:/home/ubuntu`
    
    3. Install unzip on deplotyment server `sudo apt install unzip`
    
    4. Unzip to desired directory `sudo unzip /home/ubuntu/dist.zip -d /var/www/html/` 
    
# Key
Download key from AWS console and save it to `~/.ssh` folder and then give below permission

`chmod 400 ~/.ssh/demo.pem`

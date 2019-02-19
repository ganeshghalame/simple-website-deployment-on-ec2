# simple-website-deployment-on-ec2
Simple angular website deployment on Amazan EC2 instance

# Setup Server configurations

Inboud Rules: Add `HTTP` Port `80` source anywhere

# Install Server

1. [Nginx installation](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04) `sudo apt install nginx -y`
    

2. [Apache2 installation](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-18-04-quickstart) `sudo apt install apache2 -y`
 
    Go to apache site configuration `cd /etc/apach2/site-available`
    
    Backup default config file `sudo cp 000-default.conf 000-default.conf.bk`
    
    Setup config in `sudo vim 000-default.conf` file, set `DocumentRoot /var/www/html/demo`
    
    Restart the server `sudo service apache2 restart`

# Create simple Angular App:

`ng new demo`

`ng serve` to test demo app

`ng build --prod`

# Continuous delivery

1. Jenkins/Codepipeline/Teamcity etc

2. Git clone and create build

3. Copy deliverable directly using scp/sftp etc
  
    Create zip of dist folder`zip -r  dist.zip dist/`
    
    Copy zip to destinsation server using ``scp -i ~/.ssh/key.pem dist.zip  server URL:/home/ubuntu`
    
    Install unzip on deplotyment server `sudo apt install unzip`
    
    Unzip to desired directory `sudo unzip /home/ubuntu/dist.zip -d demo/` 
    
# Key
Download key from AWS console and save it to `~/.ssh` folder and then give below permission

`chmod 400 ~/.ssh/demo.pem`

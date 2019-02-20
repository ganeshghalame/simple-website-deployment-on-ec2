# simple-website-deployment-on-ec2
Simple angular website deployment on Amazan EC2 instance

# Setup Server configurations
1. Launch instance [t2.micro freetier](https://ap-south-1.console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:)

2. Download key from AWS console and save it to `~/.ssh` folder and then give below permission **[You can download key only once ]**

    `chmod 400 ~/.ssh/demo.pem`

3. Set Inboud Rules: Add `HTTP` Port `80` source anywhere

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

# Create simple Angular App on local:

1. `ng new demo`

2. `ng serve` to test demo app

3. `ng build --prod`

# Copy Code from Local to remote server

1. **scp:
    Same syntax like copy:
    
    `scp -i ~/.ssh/demo.pem dist.zip  ubuntu@ec2-52-66-7-24.ap-south-1.compute.amazonaws.com:/home/ubuntu`

2. `rsync`: `rsync -Pav -e "ssh -i ~/.ssh/demo.pem" dist/ ubuntu@ec2-52-66-7-24.ap-south-1.compute.amazonaws.com:/home/ubuntu/dist`

3. **sftp**:
    `sftp -i ~/.ssh/demo.pem   ubuntu@ec2-52-66-7-24.ap-south-1.compute.amazonaws.com`
    `get` and `put` `lls`, `lcd` etc as per need

# Continuous delivery

1. Jenkins/Codepipeline/Codestar/Teamcity etc

2. Git clone and create build

3. Copy deliverable directly using `scp`, `sftp`, `rsync` etc
  
    a. Create zip of dist folder`zip -r  dist.zip dist/`
    
    b. Copy zip to destinsation server using `scp -i ~/.ssh/key.pem dist.zip  server URL:/home/ubuntu`
    
    c. Install unzip on deplotyment server `sudo apt install unzip`
    
    d. Unzip to desired directory `sudo unzip /home/ubuntu/dist.zip -d /var/www/html/` 
    
    e. Restart web server
    
 # Hosting website on [S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html):
    
    1. Must have readonly access to public
    
    2. `<bucket-name>.s3-website.<AWS-region>.amazonaws.com`
    
    3. ARN: Amazon Resource name
    
    4. Sample permission:
    
```javascript
{
    "Version": "2012-10-17",
    "Statement": [
    {   
        "Sid": "PublicReadGetObject",
        "Effect": "Allow",
        "Principal": "*",
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::deployment-demo-ganesh/*"
    }]
}
```

# Hosting website on [Github](https://angular.io/guide/deployment#deploy-to-github-pages)

    1. Create the build with below command:

        `ng build --prod --output-path docs --base-href /simple-website-deployment-on-ec2/`
    
    2. Copy docs/index.html and name it docs/404.html
    
    3. Commit docs directory and push it.

    4. On the GitHub project page, configure it to [publish from the docs folder](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/#publishing-your-github-pages-site-from-a-docs-folder-on-your-master-branch)

    5. Under your repository name, click  Settings, Use the Select source drop-down menu to select master as your GitHub Pages publishing source.

    6. [Online github site](https://ganeshghalame.github.io/simple-website-deployment-on-ec2/)
   


 


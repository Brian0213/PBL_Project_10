# Documentation for Project 10: LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS

- In this project you will register your website with LetsEnrcypt Certificate Authority, to automate certificate issuance you will use a shell client recommended by LetsEncrypt – cetrbot.

## Preparing prerequisites

- Configure Nginx as a Load Balancer

- Register a new domain name and configure secured connection using SSL/TLS certificates

- The 2 webservers from project 7

- Step 1: Configure Nginx As A Load Balancer

- Go to the Website below and register:

- Watch the video on how to create a freenom account:

[Freenom User Account Creation Video](https://www.youtube.com/watch?v=DkqwEcvXc3o)

[Web Url](https://www.freenom.com/en/index.html?lang=en)

[Domain Name](https://toolsibadan.ga )

- In AWS, navigate to Route 53 Dashboard to create hosted zone:

![Aws Created Zone](./imagesLoad/aws-createdzone-output.PNG)

- To connect the AWS created zone to the domain in Freenom:

- In the Freenom.com website, click on Management tools -> click on Nameservers -> select Use custom nameservers -> Namefield form should display.

- Copy the first 4 name servers in AWS into the first 4 fields in freenom.

![Aws Freenom Entry](./imagesLoad/aws-freenom-server-entry.PNG)

- At bottom of the entry form in Freenom, click on the Change Nameservers button, you should get the screenshot below:

![Change Nameservers](./imagesLoad/changes-output.PNG)

- Launch the Ubuntu Load Balancer Server:

- Copy the Public Ip and use it to create a Record in AWS Route 53:

![Route 53 Create Record](./imagesLoad/route53-ip-createrecord.PNG)

- Click on the create record button:

![Created Record](./imagesLoad/create-record-output.PNG)

Create a second record for www and the Load balance public ip:

![Route 53 Create Record](./imagesLoad/create-www-record.PNG)

- Click on the create record button:

![Created Www Record](./imagesLoad/created-www-output.PNG)

- Update the instance and Install Nginx:

`sudo apt update`

![Apt Update](./imagesLoad/apt-update.PNG)

`sudo apt install nginx -y`

![Ngix Install](./imagesLoad/nginx-install.PNG)

Both commnads can be ran together:

`sudo apt update && sudo apt install nginx -y`

- To enable and start the nginx

`sudo systemctl enable nginx && sudo systemctl start nginx`

![Ngix Enable & Start Status](./imagesLoad/nginx-install.PNG)

- Check the status of nginx:

`sudo systemctl status nginx`

![Ngix Status](./imagesLoad/nginx-status-output.PNG)

- Tp create a config file in Nginx:

`sudo nano /etc/nginx/sites-available/load_balancer.conf`

![Ngix Config](./imagesLoad/nginx-config-output.PNG)

- To remove the default site so that the reverse proxy can be redirecting to the new configuration file:

`sudo rm -f /etc/nginx/sites-enabled/default`

![Remove output](./imagesLoad/rm-f-output.PNG)

- To check whether Nginx is configured successfully:

`sudo nginx -t`

![Nginx Success](./imagesLoad/nginx-success.PNG)

- Change to the Nginx site:

`cd /etc/nginx/sites-enabled/`

![Change Nginx](./imagesLoad/cd-nginx.PNG)

- To connect Nginx available to Nginx enabled:

`sudo ln -s ../sites-available/load_balancer.conf .`

![Nginx Available & Enable Connect](./imagesLoad/nginx-available-enable-link-output.PNG)

- To confirm the success:

`ls`

![Nginx Available & Enable Success](./imagesLoad/ls-confirm.PNG)

- To show that both Nginx Available and Enable are linked:

`ll`

![Nginx Available & Enable link Success](./imagesLoad/success-output.PNG)

- Reload to Nginx:

`sudo systemctl reload nginx`

![Nginx Reload](./imagesLoad/nginx-reload-output.PNG)

- Launch the domain on a browser:

[My Domain](http://toolsibadan.ga/login.php)

![Nginx Reload](./imagesLoad/toolsibadan.ga-web-launch.PNG)

- The domain is not secured, to secure the domain run the command below:

`sudo apt install certbot -y`

![Certbot Install](./imagesLoad/certbot-install-output.PNG)

- Install a certbot dependency:

`sudo apt install python3-certbot-nginx -y`

![Python Certbot Install](./imagesLoad/python-certbot-output.PNG)

- Check the status and reload Nginx with the command below:

`sudo nginx -t && sudo nginx -s reload`

![Nginx Status Reload](./imagesLoad/status-reload-output.PNG)

- To create a certificate for the domain to make it secure:

`sudo certbot --nginx -d toolsibadan.ga -d www.toolsibadan.ga`

![Cert Created Successfully](./imagesLoad/cert-secure-output.PNG)

- Refresh the domain, it should display a connected domain:

![Domain Secured](./imagesLoad/secure-domain.PNG)

- Certificate:

![Certificate](./imagesLoad/certificate-viewer.PNG)

- Best pracice is to have a scheduled job that to run renew command periodically. Let us configure a cronjob to run the command twice a day:

`crontab -e`

![Crontab](./imagesLoad/crontab-output.PNG)

Add following line:

`* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1`

![Renewal Command](./imagesLoad/insert-output.PNG)

![New Crontab](./imagesLoad/new-crontab-output.PNG)

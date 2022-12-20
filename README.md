# Bulky
### A SMTP Server to send bulk mails

##### Bulky is a friendly guide for beginners to set up their own SMTP server to send bulk mails.

```
I get asked a lot, why to run your own SMTP server?
Pro: No sending volume limits. Many email service providers and internet service providers (ISPs) limit your daily emails, as do some web hosts. When you have your own dedicated SMTP, you can send as many emails as you need on an hourly or daily basis.
Pro: Fully monitor email delivery. No matter what happens to your email after you hit send, you will receive helpful delivery information. You can see if your messages were sent to the recipient and examine any error codes.
Pro: Your email list is private. Another advantage of running your own SMTP is that you don’t have to share email list information with anyone, maintaining your company’s and your customer’s data privacy.
```

When we say unlimited emails, this means that we can send unlimited emails from our server, there are no restrictions by companies or monthly plans to buy or so.

It's your own server, u can send as much your server can handle in terms of resources. so when you have more (CPU and RAM), you can send more emails and so on.

### Requirements to Setup SMTP Server.
In order to Build and Setup an SMTP Server, you will mainly need two things:
A Domain name and 
VPS Server with port 25 opened.


### VPS Server basic configuration.
Now we have our new Ubuntu VPS server, Let’s prepare it for out Setup.

Connect to your server, using an SSH client like putty or bitvise.

First, check your hostname:

hostname -f
If you don’t see a form of ANYTING.YOURDOMAIN.COM, then you need to change the hostname using the following command:

 sudo hostname host.domain.tld 
 Where the host is anything you want. so in my case, my sample domain for this tutorial is xmailing.me, the command will look like this:

sudo hostname postal.xmailing.me



### Map your domain name.
```
Now we have our VPS server and we set its name. Go to your Domain Provider and map your Domain to your VPS server. simply open DNS management zone and add a new A record like this:

host: server points: YOUR SERVER IP.
```



## Setup Free SMTP Server
The VPS is ready, and we can start the installation process. So in order to setup SMTP Server on our VPS, we need to install an SMTP software.

### Install Postal Free SMTP Software
#### Prerequisites
Postal runs entirely using containers which means to run Postal you'll need some software to run these containers. I recommend using Docker for this purpose but you can use whatever software you wish.

To install docker, run below commands. (NOTE: Each command starts with a – )
```
-sudo apt-get update
-sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
-curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
-echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
-sudo apt-get update
-sudo apt-get install docker-ce docker-ce-cli containerd.io
-sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
-sudo chmod +x /usr/local/bin/docker-compose
-docker-compose --version
```
#### System utilties
There are a few system utilities that you need to have installed before you'll be able to run some of the Postal commands.

`apt install git curl jq`

#### Git & installation helper repository
Make sure you have git installed on the server by running below commands.
```
-git clone https://postalserver.io/start/install /opt/postal/install
-sudo ln -s /opt/postal/install/bin/postal /usr/bin/postal
```

#### Maria DB
You can run MariaDB in a container, assuming you have Docker, using this command. Copy and paste this all in Putty.
```
docker run -d \
   --name postal-mariadb \
   -p 127.0.0.1:3306:3306 \
   --restart always \
   -e MARIADB_DATABASE=postal \
   -e MARIADB_ROOT_PASSWORD=postal \
   mariadb
   ```
This will install your postal database on MariaDB.


### RabbitMQ
RabbitMQ is responsible for dispatching messages between different processes. As with MariaDB, there are numerous ways for you to install this. For this guide, we're just going to run a single RabbitMQ worker. Copy all the below as one command and paste on putty.
```
docker run -d \
   --name postal-rabbitmq \
   -p 127.0.0.1:5672:5672 \
   --restart always \
   -e RABBITMQ_DEFAULT_USER=postal \
   -e RABBITMQ_DEFAULT_PASS=postal \
   -e RABBITMQ_DEFAULT_VHOST=postal \
   rabbitmq:3.8
```


### Installation
Run the command below and replace postal.yourdomain.com with the actual hostname you want to access your Postal web interface at. Make sure you have set up this domain with your DNS provider before continuing.

`postal bootstrap postal.YOURDOMAIN.COM`

### Initializing the database
Run the following commands to create the schema and then create your first admin user.
```
postal initialize
postal make-user
```

### Running postal
You're now ready to run Postal. You can do this by running:

`postal start`
This will run a number of containers on your machine. You can use `postal status` to see details of these components.


### Installing WEB Client
You can use any web client to run postal on your web, but in this guide, we are going to use Caddy. Install it with an SSL using below command.
```
docker run -d \
   --name postal-caddy \
   --restart always \
   --network host \
   -v /opt/postal/config/Caddyfile:/etc/caddy/Caddyfile \
   -v /opt/postal/caddy-data:/data \
   caddy
  ```
Once this has started, Caddy will issue an SSL certificate for your domain and you'll be able to immediately access the Postal web interface and login with the user you created in one of the previous steps.

### Configure Postal SMTP
Now, Open your Internet browser, and navigate to your Server IP URL or Subdomain like this:

`https://YOUR_SERVR_ADDRESS`

And then the Postal Login Screen will open, enter your email and password that you created during the setup to login.
Postal Login
And Now, you are inside Postal, click “Add Organization” to add one.

Then Click on Build Mail Server and enter the name, and set it to live mode

### Postal Domain Configuration
Now, Click On Domains to add you domain name into Postal.
Add Domain in Postal
Enter your Domain name that you want to use to send emails, and Click Create Domain.

Then, Postal will show you the Domain page with the records that you need to configure.
So, What you need to do simply, is to copy these records and paste in your DNS Zone. and then your server will be ready to send emails!







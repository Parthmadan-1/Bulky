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




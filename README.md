# Bulky
### A SMTP Server to send bulk mails

##### Bulky is a friendly guide for beginners to set up their own SMTP server to send bulk mails.

I get asked a lot, why to run your own SMTP server?
Pro: No sending volume limits. Many email service providers and internet service providers (ISPs) limit your daily emails, as do some web hosts. When you have your own dedicated SMTP, you can send as many emails as you need on an hourly or daily basis.
Pro: Fully monitor email delivery. No matter what happens to your email after you hit send, you will receive helpful delivery information. You can see if your messages were sent to the recipient and examine any error codes.
Pro: Your email list is private. Another advantage of running your own SMTP is that you don’t have to share email list information with anyone, maintaining your company’s and your customer’s data privacy.


When we say unlimited emails, this means that we can send unlimited emails from our server, there are no restrictions by companies or monthly plans to buy or so.

It's your own server, u can send as much your server can handle in terms of resources. so when you have more (CPU and RAM), you can send more emails and so on.

### Requirements to Setup SMTP Server.
In order to Build and Setup an SMTP Server, you will mainly need two things:
A Domain name
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






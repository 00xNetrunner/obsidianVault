---
sticker: emoji//2709-fe0f
banner: https://process.fs.teachablecdn.com/ADNupMnWyR7kCWRvm76Laz/resize=width:705/https://cdn.filestackcontent.com/vIuC2QSyRNCclJ8oh3aQ
tags:
  - Ethical_Hacking
  - TCM
  - phishing
---
# Table Of Contents
```table-of-contents
```

## Mailgun and Route 53 Setup. 

First Things first you must create a mailgun account. when you create your account it will look like this. This is the mailgun landing page. 

![[Pasted image 20240518000852.png]]

From here we are going to set up the services we need, Mailgun and Route 53 and the GoPhish Server. 

lets start by doing the following 
- 2 TXT Records (SPF, DKIM)
- 1 MX Record (validating mxa.mailgun.com and mxb.mailgun.com as email servers)
- 1 MX Record for email.mydomain.com specifying mailgun as the email domain
- 1 CNAME Record

From the landing page go to 

**Sending > Domains > Add New Domain**

![[Pasted image 20240518135451.png]]

type in the domain we registerd in the previous step. In this case its *zero-sec.com* 

![[Pasted image 20240518135643.png]]

From here click add domain and remember to choose US or EU, in this case i will be using US. 

Now we need to verify the domain 
![[Pasted image 20240518140003.png]]

To set up the records, we first need to make sure we are logged into AWS and on the route 53 panel.  

From the R53 Dashboard
![[Pasted image 20240518140509.png]]

Navigate too 

Hosted Zones > zero-sec.com > Create Record
> instead of zero-sec.com click on the domain you have registerd with R53

you should be on a page that looks like the following. 
![[Pasted image 20240518140848.png]]

Go back to the mail gun DNS verify page

![[Pasted image 20240518140914.png]]

Copy the following string:  `v=spf1 include:mailgun.org ~all`

Enter the string into the value box and select the record type TXT
![[Pasted image 20240518141252.png]]

click create record

Next is the **DKIM** where we will recreate the process, but we are gonna copy the value and part of the hostname. 

![[Pasted image 20240518141756.png]]

copy the follow part of the hostname. `pic._domainkey`
paste this into the record name in the create a record tab in R53

then repeat the process with the value and add it to the value box. again choose the TXT Record. 

The record should look like the following. 
![[Pasted image 20240518142147.png]]
Create Record

**MX**
![[Pasted image 20240518142514.png]]

For this record we need to copy both og these values `mxa.mailgun.org` and `mxb.mailgun.org`

when we enter these values into the value box in R53 we need to put 10 infront of it so it looks like this 

```text
10 mxa.mailgun.org
10 mxb.mailgun.org
```

for the record type set it too MX. and leave the subdomain/record name blank

when you are done should look like the following 
![[Pasted image 20240518142739.png]]

Create Record

last record is the **CNAME** Record
![[Pasted image 20240518142900.png]]

for the subdomain we will add `email`
and for the value it will be `mailgun.org` 
for the record type select `CNAME`

Should look like the following. 
![[Pasted image 20240518143145.png]]
Create Record. 

Now that we have set up all the records. click the verify DNS settings button
![[Pasted image 20240518143905.png]]

if their were no errors during the set up you should see the following screen. 
![[Pasted image 20240518144150.png]]

Unfortunately we are not done yet and still have a lot to do. 

for the page we are on navigate to Domain Settings >  SMTP Credentials
![[Pasted image 20240518144649.png]]
Click Add new SMTP user

![[Pasted image 20240518144810.png]]

I am going to create a new login called alerts, When you click create their will be a pop up at the bottom right of the window saying a password was create hit the copy button to save the password ans save this information to a text document. 


## AWS Server Setup
Lets head over to EC2 we can do this by going up the search bar and typing in EC2. 

head over to the dashboard page 
![[Pasted image 20240518153011.png]]
Click the Launch Instance Button

![[Pasted image 20240518153233.png]]

You will see this page pop up, 

Name the sever what you want. i named mine GoPhish Server. 

Select Ubuntu for the selection of AMIs under Quick Start.

for the instance type i choose t2.micro 
![[Pasted image 20240518153557.png]]

For the key pair click create new key pair link thats on the right. 
![[Pasted image 20240518153612.png]]

again name the key whatever you want. 

![[Pasted image 20240518153701.png]]

I named my GoPhishKey, when you create the keypair it will be downloaded to your computer, move this from the downloads to somewhere more important.  I make the keys read only with the following command. 

```bash
chmod 400 GoPhishKey.pem
```

If you are on Linux, a read only file will look like this when using the ls command
![[Pasted image 20240518154133.png]]

**Network Settings**
![[Pasted image 20240518154346.png]]

when setting up the network settings, allow SSH Traffic from. then click on the anywhere drop down menu and select **My IP** if you do not do this anyone will be able to connect via the SSH port. 

that's pretty much it, from here you can click *Launch Instance*

## Setting up the GoPhish server. 
To first set up the server you have to connect to it first, we can do this buy  entering the following command. 

```bash
ssh -i keypair.pem ubuntu@<--UbuntuPublicIP-->
```

when we are connected the first thing we should do is, a system update, 
we do this by entering the following

```bash
sudo apt-get update && sudo apt-get upgrade -y
```

Next we install golang and git
```bash
sudo apt install golang git -y
```

installing GoPhish
```bash
git clone https://github.com/gophish/gophish
cd gophish
go build
```

After the installation if you use the **ls** command we can see that doing this has make a executable, the screen one shown on screen. 
![[Pasted image 20240519184911.png]]

open up the **config.json** with neovim or nano
```bash
sudo nvim config.json
```

from here we need to change the 'listen_url' to 0.0.0.0:3333 from whatever it is. 
![[Pasted image 20240519185303.png]]

now we need to create a service file so that we can access the web page of go-phish
and so that it starts every time the server boots up.

**First we create the system service.** 
```bash
sudo nvim /etc/systemd/system/gophish.service
```

**Add the following lines:**
```bash
[Unit]

Description=gophish-service

[Service]

Type=simple

WorkingDirectory=/home/ubuntu/gophish/

ExecStart=/home/ubuntu/gophish/gophish

[Install]

WantedBy=multi-user.target
```

**Enable the gophish service:**
```bash
sudo systemctl enable gophish.service
```

**Start the gophish service:**
```bash
sudo systemctl start gophish.service
```

**Check the Status of the service:**
```bash
sudo systemctl status gophish.service
```

when we run this command if we use the left arrow key we will see that a password has been generated we use this password with the username admin to log into the web page. copy this password for now (we will change it later) ==2b4e8fde28674a54==
![[Pasted image 20240519191206.png]]




**Open port 3333 in AWS Security Groups for port 3333 to YOUR IP address only.**

from the Instances dashpage
![[Pasted image 20240519191926.png]]click on the Instance ID Link.  

when on the page we will see tabs at the bottom 
![[Pasted image 20240519192150.png]]
Click the Security one. 

Click Security groups
![[Pasted image 20240519192309.png]]

click inbound rules
![[Pasted image 20240519192446.png]]

![[Pasted image 20240519192538.png]]
from here first click add rule, keep it on custom TCP add 3333 to the port and from the source drop down box select MyIP

scroll down to the bottom and select Save rules.
![[Pasted image 20240519192659.png]]

**Login to the admin panel:**

Navigate to https://<publicPhishingServerIP/>:3333

![[Pasted image 20240519192956.png]]
Login with the password we saved earlier

Change the password
![[Pasted image 20240519193035.png]]

## Configuring TLS Certificates

![[Pasted image 20240519185303.png]]
from the config.json from earlier we can see the server is listening for stuff that will connect with port 80 port 80 i~~s unsecure so we will be using port 443 instead.~~ 

**Adding port 80 rule to inbound rule for firewall**
To do this head back navigate to EC2 dashboard click the instance ID and then security groups like last time we need to add another inbound rule. 

**Setting up A Record** (*This makes it so that when someone vists the link it will be a secure connections making it more trust worthy*)
Head back to R53 to create a record
![[Pasted image 20240520021011.png]]

![[Pasted image 20240520021054.png]]
Add the current server IP to the Value box and create the record *You will have to edit this each time the server starts as AWS will assign it a new IP address.*

go back to the ubuntu host via SSH and install Certbot
```bash
sudo apt install certbot
```

After its installed run this command. *Use the domain that you registerd*
```bash
sudo certbot certonly -d <domain> --manual --preferred-challenges dns
```
(**It will ask you for you personal email address**) enter it 

it will prompt you when creating this cert, say yes to terms and conditions, EFF will ask if they can send you an email about their work i just said no to this you can say yes if you like though. 

after it has successfully been set up it will ask you to create a DNS TXT Record with a assigned value. 
![[Pasted image 20240520021929.png]]
![[Pasted image 20240520021959.png]]
**DO NOT HIT ENTER YET**

`_acme-challenge.zero-sec.com.`
`jQhhz-NfKz8SQOi8Ab-f9Ph_3gu2emwKjH8p7DY9dTc`

Again go back to E53 to create a record. and and the information that it tells you to add. 
subdomain = `_acme-challenge.<domain>`
value = `jDWjkdj-aifhaifafshpi-jdaiowrpoajf-AIJDIAJWIDJ` (*Will look like this*)
record type = TXT

**This will take 60 Seconds to populate you will need to wait for it to complete, when it has populated head back to the SHH session and hit enter**

If it succesffulty creates the cert, you will see where the key and the cert is saved
![[Pasted image 20240520022834.png]]

Its important to save these as we will need to add them to the GoPhish Repo, (*config.json*)
```bash
Certificate is saved at: /etc/letsencrypt/live/zero-sec.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/zero-sec.com/privkey.pem
```

**Copy certificates to the gophish directory**
```
sudo cp /etc/letsencrypt/live/mydomain.com/privkey.pem mydomain_privkey.pem <new key name>
sudo cp /etc/letsencrypt/live/mydomain.com/fullchain.pem mydomain_fullchain.pem <new chain name>
```
 > Make sure you are in the gophish directory so that the cert and key are copied to the gophish directory, you do not have to do this but it makes things easier. 
 
open config.json with nvim or nano and add the cert path and key path to the ones you just renamed and copied to the gophish directory. 
![[Pasted image 20240520025054.png]]
> Also make sure to change the *use_tls* to "true" and set the listen_url to port `443`

**Restart gophish.service**
```bash
sudo systemctl stop gophish.service
sudo systemctl start gophish.service
```

> Go back the the inbound rule we created earlier and change the port 80 to 443 this should have been done. at the step before, but i kept it as port 80 before for testing.


Now if we head to `https://<domain>:443` we will see that we have a secure TLS Certificate
![[Pasted image 20240520025602.png]]
![[Pasted image 20240520025639.png]]

## Email Sending Profile Setup 
Head to the gophish Dashboard
![[Pasted image 20240520025831.png]]

> side note we can use the domain name to go to the admin panel now since we set up the A record earlier. 

Navigate to sending profile > New Profile. 

we are going to use the mailgun email and password we set up on a previous step. 

![[Pasted image 20240520032014.png]]

for the **Username**, **Password** and **SMTP From** we will be using the alerts@zero-sec.com and the password we saved from setting up the mailgun SMTP Credentials if you have forgotten the password or did not save it head over to mailgun and back to those settings to reset the password. 
![[Pasted image 20240520032200.png]]

For the *Host* we will use `smtp.mailgun.org:587` if you want to use a different port you can from the SMTP Credentials page we can see the available ports we can use their. 
![[Pasted image 20240520032359.png]]

back on the new sending profile form at the bottom we will see we can test the email. i just used 10-minute mail to test this. 
![[Pasted image 20240520032727.png]]

sometime the email will not get threw. gophish will not be able to tell, but if we go to mailgun and then logs we can see if the email was Deliverd. 
![[Pasted image 20240520033030.png]]
as you can see one email was and one did not make it.

go back to the form and save all the settings. 
![[Pasted image 20240520033240.png]]




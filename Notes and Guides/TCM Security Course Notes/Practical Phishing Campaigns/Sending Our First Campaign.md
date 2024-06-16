---
sticker: lucide//mail-x
color: var(--mk-color-purple)
banner: https://process.fs.teachablecdn.com/ADNupMnWyR7kCWRvm76Laz/resize=width:705/https://cdn.filestackcontent.com/vIuC2QSyRNCclJ8oh3aQ
tags:
  - phishing
  - Ethical_Hacking
  - TCM
---
**Steps to Create a Campaign:**

- Create a sender profile
- Create an Email Template
- Create a landing page template
- Create Users and Groups to send to
- Send the Campaign
- Check Mailgun for Logs as needed to see if emails bounced

we have already created a sender profile, so now will will set up an email template, i am going to use Ai to make mine. 

## Email Template

![[Pasted image 20240520033931.png]]

![[Pasted image 20240520034322.png]]

You can set up the template how you would like adding you own customisation. but when it comes to the link in my case its the Login To Your Account button i can select the text > right click > edit url

![[Pasted image 20240520034450.png]]
Add `{{.URL}}` GoPhish will generate a landing page each time a user clicks the link. 

## Landing Page

![[Pasted image 20240520035958.png]]

Once again i have used AI to generate a quick landing page. if you see a landing page you like you can import site via the import site button. in a real scenario we will be using real world websites, possibly your clients website. and landing pages.

When we have set up our landing page if we scroll down make sure Capture submitted data and capture password as been ticked

for the redirect page i am just going to use google, 
*if you are attacking a clients employee i would redirect to the companies official landing page or their homepage*
![[Pasted image 20240520040521.png]]


## Users & Groups
Go to the users&groups tab, and create a new group 
![[Pasted image 20240520040907.png]]
On this tab we can add emails we can send are phising campaign to, in this case i am just using a 10 minue email address with fake information. 

## Campaigns
![[Pasted image 20240520041020.png]]

Click on New Campaign 

![[Pasted image 20240520041135.png]]

Here select the templates we created. and set the URL to `https://domain.com:443`
change to the domain you have registerd. we set this up in the inbound rules earlier. 

click Launch

![[Pasted image 20240520041424.png]]

as we can see here we can now see all of the details for the campaign we set up. 

![[Pasted image 20240520041503.png]]

Now lets head over to the 10min email 

![[Pasted image 20240520041532.png]]

we can see the email made it through. click the link at it will send us to the landing page we set up. 

![[Pasted image 20240520041614.png]]

login with any username/password
when we hit login we will be redirected towards google.com like we set up. 
but now if we head back to the campaign dashboard we should see the recorded data. 

"Ladys and gentlemen, we got him!"

![[Pasted image 20240520041838.png]]

from here we can Complete the campaign to see the data we have captured. 
we can also select the drop down arrow next to marks name to see more details. 

![[Pasted image 20240520042001.png]]

Always remember to ask your client if anyone reported the email internally, and check the mailgun logs to see the status of the emails sent. 

Congratz you have conducted your first (maybe) phishing campaign. 

## Hardening Our Server
- Usefull blog post on how to haeden GoPhish via the server config
https://www.sprocketsecurity.com/resources/never-had-a-bad-day-phishing-how-to-set-up-gophish-to-evade-security-controls

head back to the SSH Session and make sure we are in the gophish directory. 
![[Pasted image 20240520043526.png]]

From the gophish directory, we will run the following commands. 
```bash
find . -type f -exec sed -i.bak 's/X-Gophish-Contact/X-Contact/g' {} +
find . -type f -exec sed -i.bak 's/X-Gophish-Contact/X-Contact/g' {} +
```

cd into the config directory and open up config.go in neovim or nano
```bash
cd config/
sudo nvim config.go
```

Scroll down the config.go file untrill you see const ServerName = 'gophish'
![[Pasted image 20240520044042.png]]
Change this value to `IGNORE` save then exit cd back into the gophish directory. 

**Change the RID parameter of gophish:**
```
sed -i 's/const RecipientParameter = "rid"/const RecipientParameter = "keyname"/g' models/campaign.go
```
What this does is it will change the landing page link, so when a user clicks the login button and they are bring to the landing page url, it will have /?=rid sometimes people will not notice this but SOC or Blue Teams will see this is a major red flag and a sign of a compromise 

in the above command where it says keyname we can change this to what we want, i will be using identifier after you have entered the command. 

restart the gophish service

```bash
sudo systemctl stop gophish.service

sudo systemctl restart gophish.service
```

Sometimes the service will not restart properly, if this happens, make sure you are in the gophish directory, and type in `go build` this will set up the binary again with the changes. 

remember to enable and start the service. 

i conducted another campaign as a test and as you can see everything worked. 
![[Pasted image 20240520050122.png]]
![[Pasted image 20240520050216.png]]

## Email Sender Hardening
**A useful blog post on gophish hardening:** [https://www.sprocketsecurity.com/resources/never-had-a-bad-day-phishing-how-to-set-up-gophish-to-evade-security-controls](https://www.sprocketsecurity.com/resources/never-had-a-bad-day-phishing-how-to-set-up-gophish-to-evade-security-controls)

we are going to head to Sending profiles and edit one of the templates we already have. from the form we are going add some Email headers
![[Pasted image 20240520052158.png]]

**Add the following headers to our "Sender Profile":**

- Mime-Version: IGNORE﻿
- Received: IGNORE
- X-Mailer: IGNORE
- X-Originating-IP: IGNORE
![[Pasted image 20240520052434.png]]

Save it. EzPz

> [!Forgot Password?]
>  Sometimes you forget things like passwords, sometimes you might forget you GoPhish password if this happens, make sure you have installed `sqlite3` 
>  when its installed go to the gophish directory, and enter the following command, this command will reset your password. 
>  
>  ```bash
>  sqlite3 gophish.db 'update users set hash="$2a$10$IYkPp0.QsM81lYYPrQx6W.U6oQGw7wMpozrKhKAHUBVL4mkm/EvAS" where username="admin";'
>  ```














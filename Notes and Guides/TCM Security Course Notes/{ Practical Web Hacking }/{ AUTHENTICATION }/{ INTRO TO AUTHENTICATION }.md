![[Pasted image 20240617232716.png]]
# Table Of Contents
```table-of-contents
```

## { WHAT IS AUTHENTICATION? } #:

Authentication is the process of an entity (user or machine) proving they are who they claim to be

There are three main factors used for authentication: 
- Passwords - `Something you have`
- Physical object or token ( Yubikey ) - `Something you have`
- Bio-metrics - `Something you are`

Just because a web app used one or more factors, it does NOT mean its auth mechanism is secure.

**Common Types of Authentication**
- Password-based authentication
- Multi-factor authentication
- Application behaviour / side-channel analysis
**Common weakness**
- Lack of brute-force protection
- Logic flaws


## {  OWASP TESTING GUIDE } #:

![[Pasted image 20240617233935.png|400]]

> [!important]
> The [OWASP](https://owasp.org/www-project-web-security-testing-guide/v42) testing guide will provide you useful information when you are carrying out a pentest, like best practise and what to look out for. and what you should be reporting to developers
> 


## { GUIDED LAB } #:
![[Pasted image 20240617234212.png|400]]
On you PortSwigger account login, and head to this - [lab](https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-broken-logic) we are going to be running through it. 

Open Burpsuite > Proxy > Open Browser
![[Pasted image 20240617235316.png]]

from the lab page click access lab take note of the credentials. 
![[Pasted image 20240617235530.png]]

copy the url - https://0a1e005c03573fab881f8ca1007300ca.web-security-academy.net/
and paste it into the burpsuite browser

![[Pasted image 20240617235753.png]]

>From this page we are going to head to `My account`> click Forgot password? > enter the username `wiener` > submit

when you have done all this, head back to burpsuite and from the `Proxy`, click the HTTP history tab. 
![[Pasted image 20240618000854.png]]

![[Pasted image 20240618000944.png]]
> Make sure Everything in the red box has been checked, then apply

now just look through the HTTP - History, we will mostly be looking for login requests.
![[Pasted image 20240618001825.png]]
> Secure and HttpOnly means it will only will only be sent over https, and that we cannot access the cookie via XSS

- Now look for something that says `/my-account?id=wiener`

![[Pasted image 20240618002928.png]]
> With changing data like this it will sometimes work just depends on how secure a website is, We had a target called carlos as well we can also try his name. and see what happens. 

- Next Look out for `/forgot-password`

![[Pasted image 20240618005016.png]]
> you can see the username here as well has been passed with wiener

> [!NOTE]
> Please note just now we are not doing anything, but just having a look at key info that will become usefull soon. 


---
tags:
  - Ethical_Hacking
  - OSINT
  - TCM
---
![[Pasted image 20240616193153.png]]

# ToC
```table-of-contents
```
## { USING LINKEDIN SEARCH }:

Using LinkedIn search function we can search for a company a target to gather information on.
![[Pasted image 20240616195512.png]]

This companies linked in is gonna have a lot of good information for when we get to pen testing , it has the website, What they do, how many employees(targets) they have. it has available jobs, (A hacker could send it a doc x CV with a payload on it to get a rev-shell), we could try and trick other targets we find under people to download payloads.  

![[Pasted image 20240616195753.png]]

we can see all the locations and where each of their buildings are. 

![[Pasted image 20240616195951.png]]
we can look for employees by country, where they studied, what they do, where they are skilled at and what they studied. a lot of users wont have their images wont available 
but some users have allowed to show their picture, we can then run these images though PimEye

Sometimes people get a bit silly and post their ID card on their LinkedIn saying how much they enjoyed their first day or week.
![[Pasted image 20240616201439.png]]
>PURE FUCKING GOLD

People will also post images of their desk,laptop, what OS they are on, what sort of applications they use.  ID badges, sometimes they will even have Sticky Notes with passwords if you are lucky.
>How nice of them. 

going back to the users we cannot see on a company page, we can use some google foo, and get whats in their bio and search for something like 
```
"IT Solutions Engineer at N-Able" site:linkedin.com
```

```
intext:"IT Solutions Enginnet at N-Able" site:linkedin.com
```

Or just simply
```
"IT Solutions Enginnet at N-Able"
```

![[Pasted image 20240616202301.png]]
> A Profile that we cannot access and do not have a name for

USING GOOGLE FU
We can get access to their profile
![[Pasted image 20240616202503.png]]
>BOOM full profile with more juicy information. Search through their profile for any thing useful

to find other users that work for a company we can use this search operator:
```
site:linkedin "' at N-Able"
```
> This is called a wildcard

To narrow it down
```
site:linkedin/in/ "' at N-Able"
```

>We can do this because of the URL that linkedin uses when you are on a user profile. 

![[Pasted image 20240616203106.png]]

> This is all very hit and miss, just will depend on what user you are searching for.. 

more operators you can use 
```
site:linkedin/in/ OR /pub/
```

## { OPENCORPORATES }#:

We can use this [website](https://opencorporates.com/) to find more information on the companies we look at. just search for your target and narrow the search by choosing the country they are in and what service the provide. 



![[Pasted image 20240616203729.png]]
When we click on this page, we will again get a lot of useful information 
![[Pasted image 20240616203842.png]]

we can also see parent companies and Trademark registrations: 
![[Pasted image 20240616203942.png]]

> [!info]
> When hackers find Parent companies they can also try something called a launchpad attack, this is where a hacker will target a company linked to the main target. 

> [!warning]
> You will need to login to find more information about a company, unfortunately i am an able to sign in for some reason, so im not getting the full information here, when logged in we can see current events and other more useful information.  You are also able to search for Executives at the company with this side. 
> Another usefull [website](https://www.aihitdata.com) that does something similar.



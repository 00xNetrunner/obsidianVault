---
tags:
  - OSINT
  - TCM
  - Ethical_Hacking
  - socialMediaOSINT
---
![[Pasted image 20240616161853.png]]
Again i shall assume you know your way around Instagram and how to conduct basic searches, for hashtags, photos and users. 

Let get into the the juicy stuff, 

## { PROFILE_ID}#:

like with the facebook section, we can gram a profile ID from Instagram right click > View Page Source, again it will bring you to a funky looking page. 

![[Pasted image 20240616164402.png]]

Next from the page that you just opened CTRL + F and then search for `profilePage_`

> Before you search i would right click the the source page and check `Wrap Long Line`
> I was getting strange bug where it would not highlight the correct word it would link me to a different ID (Firefox)

![[Pasted image 20240616165138.png]]

Why is this ID Important, well This profileID never changes, does not matter if the user changes their name or username, this ID can be used to find their profile, as searching their username or name will not provide the same results. 

> [!important]
> Its important to document this ID to always find the target profile. 

## { IMGINN }#:

Now, Instagram doesn't provide a way to download Images or videos, so a good tool to use for this would be imginn. 

![[Pasted image 20240616171101.png]]

we can then usetools like exiftool or use on website like pimeye
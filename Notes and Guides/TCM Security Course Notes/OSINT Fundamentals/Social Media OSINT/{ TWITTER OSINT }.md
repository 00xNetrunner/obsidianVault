---
tags: TCM
---
![[Pasted image 20240616004408.png]]

# ToC
```table-of-contents
title: 
style: nestedList # TOC style (nestedList|nestedOrderedList|inlineFirstLevel)
minLevel: 0 # Include headings from the specified level
maxLevel: 0 # Include headings up to the specified level
includeLinks: true # Make headings clickable
debugInConsole: false # Print debug info in Obsidian console
```
# { TWITTER OSINT | PART ONE }
-----
## { SOCK PUPPET ACCOUNT }
To get started with Twitter OSINT, we should first make a Sock Puppet account use the following website links to create one. 

| Website Name               | Website Link                                                       |
| -------------------------- | ------------------------------------------------------------------ |
| FAKE NAME GENERATOR        | https://www.fakenamegenerator.com/                                 |
| This Person Does Not Exist | https://www.thispersondoesnotexist.com/                            |
| 10 Minute Mail             | https://10minutemail.net/ OR https://temp-mail.org/en/10minutemail |

> [!attention]
> YOU CAN USE YOUR REAL ACCOUNT IF YOU WISH BUT THIS IS A GOOD TIME TO PRACTISE MAKING FAKE ACCOUNTS

-------
## { TWITTER ADVANCED SEARCHING }

After making our sock puppet account we can get started, most of us will have used twitter at some point and we know how to find posts via the hashtags or looking for things people have said or posts from people but twitter has an advanced search feature we can take full advantage of to conduct OSINT

To fine posts from a user, we can use:
```
from:johnDoe
```
we can also find posts to a person with:
```
to:johnDoe
```

To Find old posts possible of when the account was first created we can use:
```
from:johnDoe since:2019-02-01 until:2019-03-01
```
> Useful as you don't need to scroll through their whole account 

and if you want to find tweets to this person on certain dates you would use:
```
to:johnDoe since:2019-02-01 until:2019-03-01
```

this function isn't just limited to usernames we can find tweets that say certain things on certain dates
```
"nba draft pick" since:2019-02-01 until:2019-03-01
```

it would be good to be able to search to see if the user has mentioned something in a tweet this could be good for a number of reasons like if we are trying to confirm a certain account belongs to someone we already have information, say  our target likes nba we could use this to find tweets to fine if the user  has ever talked about the NBA
```
from:johnDoe nba
```

another great feature is we can use a geocode to find posts within a certain area
```
geocode: 30.281393081, -118.9123930203,10km
```
>This will look for posts made from and around 10km 

we can also combine these searches:
```
geocode: 30.281393081, -118.9123930203,10km from:johnDoe \ to:johnDoe
```
> will look for posts from within this area from our target, could be useful to try and pin down someones location. Can also be used to see user that have posted to our target from a certain area.

All of this can sometimes be hard to remember, in this case twitter has us covered with its advanced search feature which can be found here - [**Twitter Advanced Search**](https://twitter.com/search-advanced)

![[Pasted image 20240616020911.png]]






# { TWITTER OSINT | PART TWO }
------
## { LINK's }

| Website Name         | Website Link                                    |
| -------------------- | ----------------------------------------------- |
| OSINT Twitter- Tools | https://github.com/rmdir-rp/OSINT-twitter-tools |

> [!NOTE]
> Most of these tools don't work anymore due to Elon Musk's changes to twitter. this repo still have a lot of useful information on it. it provides a list of google dorks to use to search for tweets and users.

## { TWITTER QUERY DORKS }
| Query for search                            | Description                                                                                   |
| ------------------------------------------- | --------------------------------------------------------------------------------------------- |
| apple grapes site:twitter.com               | Tweets containing the words "apple" and "grapes" on Twitter:                                  |
| "apple and grapes" site:twitter.com         | Tweets containing the exact phrase "apple and grapes" on Twitter:                             |
| apple OR grapes site:twitter.com            | Tweets containing either the word "apple" or the word "grapes" on Twitter                     |
| apple -grapes site:twitter.com              | Tweets containing the word "apple" but not the word "grapes" on Twitter:                      |
| from:Apple grapes site:twitter.com          | Tweets from the Apple Twitter account mentioning grapes on Twitter:                           |
| to:Apple grapes site:twitter.com            | Tweets mentioning the Apple Twitter account and grapes on Twitter:                            |
| lang:en apple grapes site:twitter.com       | Tweets in English containing the words "apple" and "grapes" on Twitter:                       |
| apple :) grapes :) site:twitter.com         | Tweets with a positive sentiment about apples and grapes on Twitter:                          |
| apple :( grapes :( site:twitter.com         | Tweets with a negative sentiment about apples and grapes on Twitter:                          |
| near:New York apple grapes site:twitter.com | Tweets with a location near New York containing the words "apple" and "grapes" on Twitter     |
| url:apple.com apple grapes site:twitter.com | Tweets with a link to the Apple website containing the words "apple" and "grapes" on Twitter: |
| filter:images apple grapes site:twitter.com | Tweets with images of apples and grapes on Twitter:                                           |

By using these advanced Twitter dorks in Google with the site:twitter.com operator, you can perform more specific searches and find the information you need about apples and grapes on the platform.



-----
#OSINT #Ethical_Hacking #socialMediaOSINT


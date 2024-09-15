Wordlists can be installed with 
```
sudo apt install seclists
```

these wordlists can be found in
```
/usr/share/seclists
```

finding specific wordlists is easy just use the find command like this
```bash
find /usr/share/seclists -iname "*subdomain*"
```
OR
```bash
find /usr/share/seclists -iname "*username*"
```

To bruteforce a login page with burp suite we first need to find a website, and head to the login form.   
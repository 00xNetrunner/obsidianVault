`ris:Sword`
![[armitage1.png]]
**Armitage** is a tool developed for the Metasploit framework, serving as a graphical user interface (*GUI*) for the terminal-based program. This feature proves beneficial for individuals who prefer to minimize their reliance on the terminal interface.  
  
Two methods are available to initiate this program. The first involves selecting the "Start" option and subsequently typing `Armitage` in the search bar. Alternatively, individuals seeking a more specialized approach may input the following command in the terminal.
```bash
sudo -E java -jar /usr/share/armitage/armitage.jar
```

The next thing that should be observed is the connection box.
![[Pasted image 20240121171641.png| Figure 1 - connecting to metasploit]]

> **`ris:ErrorWarning`** Change accordingly; things are kept as the default.

Click connect when you are done.
![[Pasted image 20240121172157.png| Figure 2 - connecting to RPC Server. ]]

Now, there will be a prompt asking if there is a desire to connect to the Metasploit RCP server. The option "Yes" should be selected.

![[Pasted image 20240122015644.png| Figure 3 - Armitage GUI]]

it may take awhile for it to connect to the database, but when it finally does you will be greeted with this wonderful GUI 

> **`ris:Alert`** Sometimes Armitage will refuse to connect you can try typing:- 
> *`sudo postgresql start`* into another terminal to make sure the database is enabled. 

Now when the program has started, I normally Set the Exploit Rank to Poor, this can be done by navigating to the menu bar Armitage > Set Exploit Rank > Poor

![[Pasted image 20240122020319.png| Figure 4 - Setting exploit rank to Poor]]

Now lets find some targets!! 

Again by going to the menu bar: Hosts > NMAP Scan > Quick Scan
> Doing this will start a host discovery NAMP scan, when its done you will see multiple hosts pop up in the top black box ( Note this may differ in your case as i am using VMware )

![[Pasted image 20240122020619.png| Figure 5 - Starting Host Discovery Scan]]

![[Pasted image 20240122020745.png| Figure 6 - Entering scan range or Individual IP ]]

![[Pasted image 20240122021148.png| Figure 7 - NMAP Scan in Progress]]


![[Pasted image 20240122021248.png| Figure 8 - Found Hosts]]

You should now see the discovered hosts in on the screen, select one of them and go back to the NMAP scans and lets do a quick scan, Intense scan to find services, open ports and OS Detection. ( This will take awhile )

![[Pasted image 20240122021612.png]]

As you can see the scan was able to find a lot of open ports these ports are running different services on Server One, some of these services can be out of date.


Next lets set the exploit rank to poor, doing this we can find loads of different attack vectors 

**Armitage > Set Exploit Rank > Poor**

After this we can look for attack vectors by **Attack > Find Attacks**





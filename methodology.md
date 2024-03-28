## hunting methodology reminder
### I) Big Scope
#### I-1) subdomains
1 - Subdomain enumeration:<br>
```
python3 Sublist3r/sublist3r.py -d $1 -o $1/sub.txt > /dev/null
./crt.sh $1 > $1/ct.txt
amass enum --passive -d $1 -o $1/am.txt
github-subdomains -t <token> -d $1 | sort -u > $1/gitsubs.txt
vita

cat $1/gitsubs.txt $1/sub.txt $1/ct.txt $1/am.txt | sort -u > $1/subdomains.txt
```

2- httpx
```
cat subdomains.txt | httpx -status-code -title -tech-detect -silent | tee httpx.txt
```

3- favicon
```
cat httpx.txt | cut -d 1 -f " " | python3 favfreak.py 
```
4- Get urls

```
gau $line >> $1/urls1.txt
wayback -no-subs $line >> $1/urls2.txt
github-endpoints -t <token> -d $1 > $1/urls3.txt

waymore

cat $1/urls1.txt $1/urls2.txt $1/urls3.txt >> urls.txt
```

5- Brute force
```
# using dirsearch
cat $1/alive.txt | xargs -n 1 -I {} python3 dirsearch/dirsearch.py -u $1 -e html
# using ffuf
cat $1/alive.txt | ffuf -n 1 -I {} ffuf -u {}FUZZ -w small wordlist.txt -s
```
6- CORS test
```
corstest $1/alive.txt > $1/corscan.txt
```
7- subdomain takeover
```
subjack -w $1/subdomains.txt -t 100 -timeout 30 -o $1/subjack.txt -c fingerprints.json -ssl
```
8- xss
```
cat $1/urls.txt | grep "=" | grep -Ev "\.(js|css|txt|pdf|png|svg|gif|jpeg|jpg|ico|ttf|woff|woff2|tif)" | qsreplace newval | qsreplace -a | kxss | cut -d " " -f 9 | uniq | dalfox pipe
```

9- Javascript
```
gau <domain> | grep "\.js" | uniq | sort > js_files_url_list.txt
# gau or wayback might result in false positive , so we can use curl to quickly check for the status of the JavaScript files on the server
cat js_files_url_list.txt | parallel -j50 -q curl -w 'Status:%{http_code}\t Size:%{size_download}\t %{url_effective}\n' -o /dev/null -sk
or
cat js_files_url_list.txt | hakcheckurl # much more efficient

can use meg on gau's output js
meg --verbose --savestatus 200 "#"

use gf to find keys,urls,password,source,sinks
gf s3-buckets


# Identifying full URLs, relative paths in JavaScript files

python linkfinder.py -i https://example.com/1.js -o cli

```
10- dorks
```
https://dorksearch.com/

look for registration pages , login forums where there are functinalities to hunt

```

```
for each live server crawl , brute force , github endpoint , wybackurls
```

#### I-2) All assets

```
find root domains and for each root domain , use the stuff listed in subdomain .

find ips .

assets in cloud and extract organization name and cn name then do the previous.

autonomous system and extract domains and orgn name from ssl certs and copyright(dorks) stuff as well

port scan

dnnsdumpster
shodan , censys , cip
reverse dns lookup , reverse mx lookup , reverse whois lookup
reverse whois


```

### II) Small scope

#### signup functionality

```
**Duplicate registration / Overwrite existing user.**

- Create first account in application with email say abc@gmail.com and password.
- Logout of the account and create another account with same email and different password.
- You can even try to change email case in some case like from abc@gmail.com to Abc@gmail.com or <space>abc@gmail.com...
- Finish the creation process — and see that it succeeds
- Now go back and try to login with email and the new password. You are successfully logged in.

**DOS at Name/Password field in Signup Page**

very long string (100000 characters) can cause a denial a service on the server. This may lead to the website becoming unavailable or unresponsive. Usually this problem is caused by a vulnerable string hashing implementation. When a long string is sent, the string hashing process will result in CPU and memory exhaustion.

- Go Sign up form.
- Fill the form and enter a long string in password
- Click on enter and you’ll get 500 Internal Server error if it is vulnerable.
if debug mode is enabled you might end up finding leaks api keys , creds etc.. 
(you can use this in reset password too or any other similar case that involve calculation of hash...)

**XSS in username , email , password fiels **
- Payload for Username field : <svg/onload=confirm(1)>
- Payload for Email field : “><svg/onload=confirm(1)>”@x.y

**No rate limit in the sign up functionality **
if there is no rate limiting on signup page a malicious users can generate hundreds and thousands of fake accounts that lead to fill the application DataBase with fake accounts

you can use burp intruder or build a script to test this issue

- Capture the signup request and send it to Intruder.
- Add different emails as payload .
- Fire up Intruder, And check whether it returns 200 OK.

** Insufficient Email Verification **
Forced Browsing. (directly navigating to files which comes after verifying the email)
Response or Status Code Manipulation (302 -> 200 etc...)
there are so many ways and you can invent yours
eg
1- Sing up on the web application as attacker@mail.com
2-  You will receive a confirmation email on attacker@mail.com, do not open that link now.
3- The application may ask for confirming your email, check if it allows navigating to account settings page.
4- On settings page check if you can change the email.
5- If allowed, change the email to victim@mail.com.
6- Now you will be asked to confirm victim@mail.com by opening the confirmation link received on victim@mail.com, insted of opening the new link go to attacker@mail.com inbox and open the previous received link.
7- If the application verifies vitim@mail.com by using perivious verification link received on attacker mail, then this is a email verification bypass. 

**Path overwrite/hijacking**
If an application allows users to check their profile with direct path /{username} always try to signup with system reserved file names, such as index.php, signup.php, login.php, (or location of js files ...) etc. In some cases what happens here is, when you signup with username: index.php, now upon visiting target.tld/index.php, your profile will comeup and occupy the index.php page of an application. Similarly, if an attacker is able to signup with username login.php, Imagine login page getting takeovered.

```
#### Session issues

```
- identify the session cookie and check the domain scope if it's used in subdomains then by finding xss in a subdomain you can hijack session of the user
- Old Session Does Not Expire After Password Change or after logout
- are the tokens used predictable ? can we craft one ?
- check for session fixation i.e. value of session cookie before and after authentication
- 
```



## recon (subdomain/domains stuff...)
### subdomains/domains
```
sublist3r.py -d $1 -o $1/sub.txt > /dev/null
./crt.sh $1 > $1/ct.txt
amass enum --passive -d $1 -o $1/am.txt
github-subdomains -t <token> -d $1 | sort -u > $1/gitsubs.txt
curl "https://crt.sh/?O=LLC+Mail.Ru&output=json" | jq ".[]|.common_name"| sed "s/\"//g" | sed "s/\*\.//g"

extract orgname from domain:
nmap -p 443 --script ssl-cert mail.ru | grep "ssl-cert" | grep -Po "(?<=organizationName=)[^,]*" | cut -d "/" -f 1

https://opentunnel.net/subdomain-finder/target.com

https://hackertarget.com/find-shared-dns-servers/
```
### search engines
```
https://searx.be/search 
site:*.target.com //see Google cache
https://crt.sh/?O=orgname
site:*.tagret.com -www ...
ssl.cert.subject.cn:"domain" 200 ==> facet and filter based in title
ssl:"orgname" 200 => facet and filter based in title

asn:"AS<num>" 200
net:<netrange> 200

HTTP ASN:<here> -port:80,443,8080 // Web servers on non-standard ports (Shodan)

```
### urls

```
https://web.archive.org/web/*/target.Com/*
https://dorks.faisalahmed.me/

gau subdomain
wayback subdomain
github-endpoints -t token -d subdomain
```

### xss

```
cat urls.txt | grep "=" | grep -Ev "\.(js|css|txt|pdf|png|svg|gif|jpeg|jpg|ico|ttf|woff|woff2|tif)" | qsreplace newval | qsreplace -a | kxss | cut -d " " -f 9 | uniq | dalfox pipe
```

### cors

```
corstest alive.txt
```

### favicon hash

```
import mmh3
import requests
import codecs
 
response = requests.get('https://hn/favicon.ico')
favicon = codecs.encode(response.content,"base64")
hash = mmh3.hash(favicon)
print(hash)
```

### asn

```
https://hackertarget.com/as-ip-lookup/
```

### brute force

```
ffuf -w list.txt -u http://hn/FUZZ

ffuf -c -w ./wordlist.txt -u https://target/FUZZ -replay-proxy http://localhost:8080
ffuf -c -w ./wordlist.txt -u https://target/FUZZ -x http://localhost:8080
```

### file backups

```
Append .old or .bak to files

look for backups of all the executable files , Common variations for naming a backup are :  file.ext~, #file.ext#, ~file.ext, file.ext.bak, file.ext.tmp, file.ext.old, file.bak, file.tmp and file.old

bfac single url : bfac --url http://example.com/test.php
bfac list of urls : bfac --list testing_list.txt

```

### forgotten dbs dumps

```
/back.sql
/backup.sql
/accounts.sql
/backups.sql
/clients.sql
/customers.sql
/data.sql
/database.sql
/database.sqlite
/users.sql
/db.sql
/db.sqlite
/db_backup.sql
/dbase.sql
/dbdump.sql
setup.sql
sqldump.sql
/dump.sql
/mysql.sql
/sql.sql
/temp.sql
```


## Bypass regular login tricks
```
- Check for comments inside the page
- Check if you can directly access the restricted pages (dorks,bf,wayback...)
- Check to not send the parameters (do not send any or only 1)
- Check the PHP comparisons error: user[]=a&pwd=b , user=a&pwd[]=b , user[]=a&pwd[]=b
- check Default credentials 
- sql injection


```

## 403 401 bypass
```
- use wayback machine
- use google dorks and see the cached version of the app
- try /%2e/path or /%252e/path (this could bypass the protection by proxy)
- Try Unicode bypass:/%ef%bc%8fpath

- path fuzzing (If /path is blocked):
site.com/secret –> HTTP 403 Forbidden
site.com/SECRET –> HTTP 200 OK
site.com/secret/ –> HTTP 200 OK
site.com/secret/. –> HTTP 200 OK
site.com//secret// –> HTTP 200 OK
site.com/./secret/.. –> HTTP 200 OK
site.com/;/secret –> HTTP 200 OK
site.com/.;/secret –> HTTP 200 OK
site.com//;//secret –> HTTP 200 OK
site.com/secret.json –> HTTP 200 OK (ruby)

- using headers
X-Original-URL: /admin/console
X-Rewrite-URL: /admin/console

X-Originating-IP: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Forwarded: 127.0.0.1
Forwarded-For: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Remote-Addr: 127.0.0.1
X-ProxyUser-Ip: 127.0.0.1
X-Original-URL: 127.0.0.1
Client-IP: 127.0.0.1
True-Client-IP: 127.0.0.1
Cluster-Client-IP: 127.0.0.1
X-ProxyUser-Ip: 127.0.0.1
Host: localhost
  
```

## fingerprinting

```
Find Spring Boot servers : org:YOUR_TARGET http.favicon.hash:116323821
check for exposed actuators. If /env is available, you can probably achieve RCE. If /heapdump is accessible, you may find private keys and tokens

Find RocketMQ consoles : org:target.com http.title:rocketmq-console
we can for example find out: Additional hostnames and subdomains , Internal IP addresses, Log file locations ,Version details

find k8s : org:"target" product:"Kubernetes"
org:"target" port:"10250"

```

### js files

```
python linkfinder.py -i https://example.com/1.js -o cli
```

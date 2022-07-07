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
site:*.target.com
https://crt.sh/?O=orgname
site:*.tagret.com -www ...
ssl.cert.subject.cn:"domain" 200 ==> facet and filter based in title
ssl:"orgname" 200 => facet and filter based in title

asn:"AS<num>" 200
net:<netrange> 200

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
```

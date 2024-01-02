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

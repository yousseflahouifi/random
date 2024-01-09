## fingerprinting

```
Find Spring Boot servers : org:YOUR_TARGET http.favicon.hash:116323821
check for exposed actuators. If /env is available, you can probably achieve RCE. If /heapdump is accessible, you may find private keys and tokens

Find RocketMQ consoles : org:target.com http.title:rocketmq-console
we can for example find out: Additional hostnames and subdomains , Internal IP addresses, Log file locations ,Version details

find k8s : org:"target" product:"Kubernetes"
org:"target" port:"10250"

find jenkins servers using : favicon: 4dce888 or title: x-jenkins or X_Hudson X_Jenkins 200

find Exposed Redis Commander: favicon: -32e1326 , title: Redis Commander

org:"http://target. com" 
http.status:"<status_code>" 
product:"<Product_Name>" 
port:<Port_Number> “Service_Message” 
port:<Port_Number> “Service_Name” 
http.component:"<Component_Name>"
http.component_category:"<Component_Category> 
http.waf:"<firewall_name>" 
http.html:"<Name>" 
http.title:"<Title_Name>" 
ssl.alpn:"<Protocol>" 
http.favicon.hash:"<Favicon_Hash>" 
net:"<Net_Range>" (for e.g. 104.16.100.52/32) 
ssl.cert.subject.cn:"<http://Domain .com>" 
asn:"<ASnumber>" 
hostname:"<hosthame>" 
ip:"<IP_Address>" 
all:"<Keyword>" 
“Set-Cookie: phpMyAdmin” 
“Set-Cookie: lang=" 
“Set-Cookie: PHPSESSID" 
“Set-Cookie: webvpn” 
“Set-Cookie:webvpnlogin=1" 
“Set-Cookie:webvpnLang=en” 
“Set-Cookie: mongo-express=" 
“Set-Cookie: user_id=" 
“Set-Cookie: phpMyAdmin=" 
“Set-Cookie: _gitlab_session” 
“X-elastic-product: Elasticsearch” 
“x-drupal-cache” 
“access-control-allow-origin” 
“WWW-Authenticate”  
“X-Magento-Cache-Debug”   
“kbn-name: kibana”

```

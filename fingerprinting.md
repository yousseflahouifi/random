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



```

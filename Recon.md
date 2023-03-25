	
**Shodan:**

Go to more option in ports and you will get Facet Analysis. Change the Port option to `http.title`
`redis.key`  is also important 

```
ssl:"Facebook Inc." 200  
Ssl.cert.subject.CN:"domain.com" 200
asn:asnnumber
net:64.233.160.0/19
org:"Google"
```

**Censys:**

```
autonomous_system.asn:15169
ip:64.233.160.0/19
autonomous_system.organization:"Google Inc."
```

**Github:**

To filter results: `"bugcrowd.com" NOT www.bugcrowd` this will filter `www`  from results.
You can also do like this: `"bugcrowd.com" NOT www.bugcrowd NOT test.bugcrowd`

Wordlists: https://github.com/orwagodfather/My-WordLISTs

```
org:facebookresearch https://  
org:facebookresearch http://  
org:facebookresearch ldap  
org:facebookresearch ftp  
org:facebookresearch sftp  
org:facebookresearch host:  
org:facebookresearch login
```

```
git clone [https://github.com/internetwache/GitTools.git](https://github.com/internetwache/GitTools.git)  
cd GitTools/Dumper  
./gitdumper.sh https://domain.com/.git/ outputfoldarafter download end it cd outputfoldar  
===>  
git status  
===>  
git checkout -- .ls and check the files cat file  
```

Secret:

```
secret_key  
secretkey  
secret api  
secret token  
secret pass  
secret password  
aws secret  
client secret
Jenkins  
OTP  
oauth  
authoriztion  
password  
pwd  
ftp  
dotfiles  
JDBC  
key-keys  
send_key-keys  
send,key-keys  
token  
user  
login-singin  
passkey-passkeys  
pass  
secret  
SecretAccessKey  
app_AWS_SECRET_ACCESS_KEY AWS_SECRET_ACCESS_KEY  
credentials  
config  
security_credentials  
connectionstring  
ssh2_auth_password  
DB_PASSWORD
```

Python:
```
language:python password  
language:python pwd  
language:python ftp  
language:python dotfiles  
language:python JDBC  
language:python key-keys  
language:python send_key-keys  
language:python send,key-keys  
language:python token  
language:python user  
language:python login-singin  
language:python passkey-passkeys  
language:python pass  
language:python secret  
language:python credentials  
language:python config  
language:python security_credentials  
language:python connectionstring  
language:python ssh2_auth_password
```

Bash
```
language:bash password  
language:bash pwd  
language:bash ftp  
language:bash dotfiles  
language:bash JDBC  
language:bash key-keys  
language:bash send_key-keys  
language:bash send,key-keys  
language:bash token  
language:bash user  
language:bash login-singin  
language:bash passkey-passkeys  
language:bash pass  
language:bash secret  
language:bash credentials  
language:bash config  
language:bash security_credentials  
language:bash connectionstring  
language:bash ssh2_auth_password
```

Focus on **Recently Indexed** from SORT in right corner

**USER Recon:**

```
orwa atyat bugcrowd linkedin  
or  
check orwa user search for some ends points or some info about orwa  
by dork  
user:orwagodfather linkedin  
user:orwagodfather full name  
user:orwagodfather okta
user:orwagodfather jira
user:orwagodfather https://  
user:orwagodfather Ldap 
token
and just like these do some recon like if you wanna know about some internal links in org you can do like these  
org:bugcrowd https://  
org:bugcrowd host:
```

**Good Dorks:**

```
"site.com" password or secret or any keyword  
"target.atlassian" password or secret or any keyword  
"target.okta" password or secret or any keyword  
"corp.target" password or secret or any keyword  
"jira.target" password or secret or any keyword  
"target.onelogin" password or secret or any keyword  
"target.service-now" password or secret or any keyword
```

For Finding Domains:

```
org:facebookresearch language:python .php
org:facebookresearch ftp
org:facebookresearch Ldap
org:facebookresearch https://
```

**Password:**
```
password  
passwd  
pwd  
pass  
pw  
login
```




**SQLMAP:**

`sqlmap -r request.txt -p username --dbms="MySQL" --force-ssl --level 5 --risk 3 --dbs --hostname`



**BurpSuite Extensions:**

-   Collaborator Everywhere
-   XSS Validator
-   Wsdler
-   .NET Beautifier
-   Bypass WAF
-   J2EEScan
-   Param Miner
-   Wayback Machine
-   JS Link Finder
-   Upload Scanner
-   Nucleus Burp Extension
-   Software Vulnerability Scanner
-   Active Scan++
-   InQl Scanner
-   Nuclei

**AEM:**

```
git clone https://github.com/0ang3el/aem-hacker.git  
cd aem_hacker  
pip install -r requirements.txt
```

with these command you can get sometimes `SSRF` , `Sensitive Information` , `XSS` , `RCE`

`sudo python3 aem_hacker.py -u https://domain.com/ --host your.burpcollaborator.net`

AEM Login:

```
User:anonymous  
Pass:anonymous
```

**Spring Boot:**

Check Endpoints:

```
datahub/actuator/heapdump  
datahub/heapdump  
actuator/heapdump  
heapdump
```

```
Download the Eclipse Memory Analyzer from here [https://www.eclipse.org/mat/](https://www.eclipse.org/mat/)after downloading, run the MemoryAnalyzer.exe and open the Heapdump file downloadedafter opening the Heapdmp file click on Dominator viewstart search will find lot of database **credentials**
```


**Google Dorks URL:** https://dorks.faisalahmed.me/



## Root Domain Enumeration:

**Amass:**

https://allabouttesting.org/owasp-amass-quick-tutorial-example-usage/

Main Domains:
`amass intel -d <url> -whois`
ASN to domain:
`amass intel -asn 23314,81323`
ASN:
`amass intel -org "Google Inc."`
From CIDR:
`amass intel -ip -src -cidr 104.16.0.0/12`

**Whoxy:**

https://whoxy.com/

Put Company name in the company
`https://api.whoxy.com/?key=xxxxx&reverse=whois&company=Meta+Platforms,+Inc.`

use `mode=domains` for more like:
`https://api.whoxy.com/?key=xxxxx&reverse=whois&company=Meta+Platforms,+Inc.&mode=domains`

or see this docs : https://www.whoxy.com/reverse-whois/

**WhoisXMLApi:**

Find the name servers from whoxy and use this:

https://reverse-ns.whoisxmlapi.com/api/v1?apiKey=at_nLNKgiboJ9Bd9nmBzancfPZs8qXuB&ns=nameserver&mode=purchase



## FUZZING:

#### Fuzz everything:

`for i in $(cat all.sub); do echo""; echo "Subdomain of $i";echo "";gobuster dir -w wordlist.txt -u $i -e -o tmp ;cat tmp >> dutch.fuzz; echo ""; done`

#### CVE:

`for sub in $(cat all.sub);do echo "[*] Domain Name is => " $sub;echo "[*] Server Header is => " $(http — verify=no -h $sub | grep Server);echo " ";done`
```bash
$ subfinder -d domain.com -rL resolvers.txt -all -o subdomains.txt
$ cat subdomains.txt | tr '.' '\n' | sort -u > perms.txt
$ cat subdomains | grep "-" | tr '-' '\n' | tr '.' '\n' | sort -u >> perms.txt
```

* **AltDNS**

```
altdns -i domain.txt -w words.txt -o output.txt
```


-   **dnsgen**

```bash
$ cat subdomains.txt | dnsgen - > output.txt
$ dnsgen --wordlist perms.txt subdomains.txt > output-dnsgen.txt
```

-   **goaltdns**

```bash
$ goaltdns -w perms.txt -l subdomains.txt -o output-goaltdns.txt
```

-   **gotator**

```bash
$ gotator -sub subdomains.txt -perm perms.txt > output-gotator.txt
$ gotator --help
```

-   **ripgen** (Good)

```bash
cat subdomains.txt | ripgen > output-ripgen.txt
```

-   **Combine all output.txt files**

```shell
$ cat output*.txt | sort -u > output.txt
```

-   Use all the tools present on Internet because the core principle via which these permutations are generated is different for every tool.
-   One tool's output will vary from other tools's output.
-   Even, one word in perms.txt file matters.
-   Always keep your resolvers.txt updated.
-   Do your passive subdomain enumeration every week.
-   Keep your subdomains and output files managed.
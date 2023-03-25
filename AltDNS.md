
`python altdns.py -i input_domains.txt -o ./output/path -w altdns/words.txt`

### DNSgen:

`cat domains.txt | dnsgen - | massdns -r /path/to/resolvers.txt -t A -o J --flush 2>/dev/null`

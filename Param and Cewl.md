
For grepping Prameter wordlist:

`cat urls | unfurl format %q | cut -d "=" -f1 | sort -u > params.txt`


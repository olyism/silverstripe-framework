title: HTTP Cache Headers
summary: Set the correct HTTP cache headers for your responses.

# Caching Headers

By default, PHP adds caching headers that make the page appear purely dynamic. This isn't usually appropriate for most 
sites, even ones that are updated reasonably frequently. SilverStripe overrides the default settings with the following 
headers:

  * The `Last-Modified` date is set to be most recent modification date of any database record queried in the generation 
  of the page.
  * The `Expiry` date is set by taking the age of the page and adding that to the current time.
  * `Cache-Control` is set to `max-age=86400, must-revalidate`
  * Since a visitor cookie is set, the site won't be cached by proxies.
  * Ajax requests are never cached.

## Customizing Cache Headers

### HTTP::set_cache_age
```php
    HTTP::set_cache_age(0);
```

Used to set the max-age component of the cache-control line, in seconds. Set it to 0 to disable caching; the "no-cache" 
clause in `Cache-Control` and `Pragma` will be included.

### HTTP::register_modification_date


```php
    HTTP::register_modification_date('2014-10-10');
```

Used to set the modification date to something more recent than the default. [DataObject::__construct](api:SilverStripe\ORM\DataObject::__construct) calls 
[HTTP::register_modification_date(](api:SilverStripe\Control\HTTP::register_modification_date() whenever a record comes from the database ensuring the newest date is present.

### Vary: cache header

By default, SilverStripe will output a `Vary` header (used by upstream caches for determining uniqueness) 
that looks like

```
Cookie, X-Forwarded-Protocol, User-Agent, Accept
```

To change the value of the `Vary` header, you can change this value by specifying the header in configuration

```yml
HTTP:
  vary: ""
```





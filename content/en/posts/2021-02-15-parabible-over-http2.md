---
layout: post
title:  "Enable HTTP/2 for Parabible"
subtitle:  "It's easier than you think"
slug: "parabible-over-http2"
date:   2021-02-15 11:00:00 -0600
author: James Cuénod
header-img: "img/post-bg-04.jpg"
---


# What is HTTP/2?

As the name suggests, it's an iteration on HTTP 1.1 that uses binary instead of plain text. The most important difference, to my mind is:

> HTTP/2 is able to run multiple streams of data over the same TCP connection, avoiding the classic HTTP 1.1 head of blocking slow request and avoiding to re-instantiate TCP connections for each request/response (KeepAlive patched the problem in HTTP 1.1 but did not fully solve it). [source](https://httpd.apache.org/docs/2.4/howto/http2.html)

In essence, this means marginally faster websites. So, naturally, I wanted this on <https://parabible.com>.

# How to Enable HTTP/2

The first step was to check that `modules/mod_http2.so` module was available. Parabible is hosted on a Bitnami server. So, the path I needed to check was `/opt/bitnami/apache2/modules/mod_http2.so`. Hurray! It's there...

The next step is to make sure it's loaded. So now we edit `httpd.conf` (which was in my apache2 folder). Here's the key line I needed to **uncomment** (presumably you need to be sure that url is correct).

```
LoadModule http2_module modules/mod_http2.so
```

Finally, you've got to tell Apache to use HTTP/2. According to [their guide](https://httpd.apache.org/docs/2.4/howto/http2.html), that means something like this:

```
Protocols http/1.1
<VirtualHost ...>
    ServerName test.example.org
    Protocols h2 http/1.1
</VirtualHost>
```

(note that the `Protocols` directive[?] can be nested)

For my bitnami server, though, I didn't have any `<VirtualHost>` directives. Instead, I have a `/opt/bitnami/apps/parabible/conf/httpd-prefix.conf` file. Now, tbh, I'm not certain the `Protocols` line is supposed to go in there but that's where I put it because that's where I found my `ServerName` directive.

I used `Protocols h2 http/1.1`. This tells Apache to prefer `h2` to HTTP/1. There is another HTTP/2 code—`h2c`. But, `h2c` does not require TLS and since everyone should be using TLS (and a lot of browsers don't even support HTTP/2 if it's not over TLS), `h2` is preferable.

And voila, now I see "HTTP/2" all the way down my "Protocol" column in the dev tools. Success... Now to benchmark the performance changes.
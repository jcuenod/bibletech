---
layout: post
title:  "GoAccess Analytics"
subtitle:  "Using GoAccess to generate reports from Apache logs (or nginx, etc.)"
date:   2021-08-16 11:00:00 -0600
author: James Cuénod
header-img: "img/post-bg-04.jpg"
---

There are a lot of things to like about Google Analytics. I especially like being able to see where traffic is coming from in the world and there must be some sort of endorphin release to see the number of users "currently active." But Google Analytics is Google... So I've been looking for an alternative for quite some time and I've been using GoAccess as an experiment for a while.

[GoAccess](https://goaccess.io/) is a program that parses your log files (it can handle a bunch of different ones but my server uses Apache right now) and produces live output in the terminal like this, parsing your log files as they update:

![GoAccess Terminal Output](/bibletech/img/post-images/goaccess-terminal.png)

But GoAccess also supports writing html and json output with the switch `-o report.html`. This solves the first problem. With `-o`, we can generate an html file that we put into a public directory on our server.

The next step is setting up a systemd timer to do this. I've done that before when I automated my [Zotero backup service](https://jcuenod.github.io/bibletech/2020/06/14/zotero-github-backups/). Now it's just a matter of setting up a timer to run a service that executes something like:

```
goaccess -o /output/path/analytics.html
```

This produces a nice webpage like:

![GoAccess HTML Output](/bibletech/img/post-images/goaccess-html.png)

The last step is customizing the output. GoAccess checks your `~/.goaccessrc` file for configuration settings. It's possible to get geolocation from IP addresses if you have access to a GeoLite `.mmdb` file. You can set that up using:

```
geoip-database /path/to/GeoLite2-City.mmdb
```

Some of the other configurations that I added were to add my own IP to `exclude-ip` and then some additional tweaks to the html output:

```
html-prefs {"theme":"darkGray","perPage":20,"visitors":{"plot":{"chartType":"bar"}},"visit_time":{"plot":{"chartType":"bar"}}}
```

Finally, I added some customizations to filter results a bit.

```
anonymize-ip true
ignore-status 301
ignore-status 302

# Sort panel on initial load by visitors
sort-panel REQUESTS,BY_VISITORS,DESC
sort-panel REQUESTS_STATIC,BY_VISITORS,DESC
sort-panel HOSTS,BY_VISITORS,DESC
sort-panel OS,BY_VISITORS,DESC
sort-panel BROWSERS,BY_VISITORS,DESC
sort-panel REFERRERS,BY_VISITORS,DESC
sort-panel REFERRING_SITES,BY_VISITORS,DESC
sort-panel GEO_LOCATION,BY_VISITORS,DESC
```

The solution is not perfect. I obviously am not able to filter all the crawlers and bots out (I can tell because of how seemingly high my traffic is). But it is a useful alternative set of data that might eventually ween me off GA. Mostly so that I don't lose it, all this can be found on github at <https://github.com/jcuenod/goaccess-settings>.
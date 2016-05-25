# logstash_filters
###A set of useful logstash config filters and their asociated grok pattern files

This is a collection of filters I use very succssfully to have nice statistics of a set of services (mostly streaming related) using ELK stack (Logstash, Elasticsearch and Kibana)

Some of the grok patterns do include previous works from older versions I decided to change for several reasons:
some did not work to me, others worked but were not what I needed, so, most of them are almost entirelly rewritten, although I tried to reuse as much code as possible, and obviously it is highly probably that parts of code are found to be reused.
There are many sources of code, difficult to credit every one... so its easier to say that there are plenty of reused or modified code here, and I'm not, by far, the only author.

To use this filters, it is necessary to use previous tagging of log lines, since the filter configs do discriminate log lines to be applied on a tag basis.
This is because I work with a logstash splited .conf files setup (as recommended by many sources), instead of using a single huge .conf file, where config is split in several files at /etc/logstash/logstash.conf, which are linked (activated as needed) in /etc/logstash/conf.d

Since order matters (config is read/applied to logs up to down, making log lines be processed "sequentially") split config uses numbers to mark config order (for example, I use 01-inputs.conf to define input config, 10-tagging.conf to add service tags, and 99-outputs.conf for output config. This gives me a range from 11-XXXXX.conf to 98-YYYY.conf to add config filters to services that are applied to logs lines with propper tag).
My conf files are named like XX-somename.conf so it's easy to replace XX chars by adequate numbers.
At the start of every conf file it is easily visible the tag string that triggers that filter/conf to be applied to log lines "as they pass"

Likewise, Almost all of the filters require a grok pattern that is not included by default by logstash package. In my case (Debian), those files had to be put to /opt/logstash/patterns folder, and have propper logstash:logstash ownership on them (but this is very much Debian related, and may vary on other environments).

Finally, most of this filters do add Geolocation by using MaxMind's geoIP library that has been packaged in Debian.
It should be noted that conf files do point to geolite database file path... The file must exist at that path, and that file path may differ from system to system, and if it is not correct it causes errors in Logstash.
If geolocation is not interesting, simply comment out the geolocation part of the conf files.

The whole thing is in production in Debian wheezy and Jessie, with Losgstash 1.5.x installed from official Elasticsearch Debian repos.

#####Included conf/grok filters are tested with:
- Centovacast (Shout1.9.x, 2.x and Ice2.x)
- HaProxy
- Icecast2 (2.3.x and 2.4.x)
- Iptables (Debian wheezy and Jessie)
- Nginx
- php5-mail
- postfix
- pureftpd
- Shoutcast (1.9.8 and 2.x)
- Varnish
- WowzaStreamEngine (3.x and 4.x)

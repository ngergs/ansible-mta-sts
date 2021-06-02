MTA-STS
=========

Instance that hosts a .well-known/mta-sts.txt file for [MTA-STS](https://datatracker.ietf.org/doc/html/rfc8461) unter the mta-sts subdomain.


Requirements
------------

An ubuntu host and a domain for which the relevant DNS entries can be setup.
The mta-sts subdomain should also point to this server. The easiest setup for most use cases would be a CNAME DNS entry:
```
mta-sts 10800 IN CNAME @
```

Role Variables
--------------

* domain: A domain that you own and which A and AAAA dns records point to the server used. This is used for the nginx.conf template.
* https: If https with a letsencrypt certificate should be configure in nginx.
* hsts: If HSTS should be activated. Only relevant if https=true. max-age is configured to be 2 years.
* hsts_max_age: The HSTS max age setting, only relevant if hsts=true.
* hsts_preload: If HSTS preloading should be activated. Only relevant if https=true and hsts=true.
* expect_ct: If the Expect-CT header should be set to enforce. max-age is configured to be 2 years.
* mta_sts_mode: mta-sts mode. valid are none, testing and enforce.
* mta_sts_max_age: mta-sts max age in seconds
* mta_sts_mx_records: list of mx servers, full qulified domain names or wildcard host.

Dependencies
------------


Usage
----------------
Run with https=false to setup the server. Login and run
```
certbot --nginx
```
and aquire the certificate for the mta-sts subdomain. The rerun the script with https=true.
After the initial server setup you have to add the relevant MTA-STS DNS entries like below:
```
_mta-sts 10800 IN TXT "v=STSv1; id={{timestamp}}"
_smtp._tls 108000 IN TXT "v=TLSRPTv1; rua=mailto:tls-reports@{{domain}}"
```
If you wish may also run the provided /root/mta-sts_tlsa.sh script to generate a TLSA DNS entry.
This is not required for MTA-STS.

License
-------

MIT

Author Information
------------------

SelfEnergy

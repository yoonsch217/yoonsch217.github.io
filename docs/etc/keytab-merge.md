---
layout: default
title: How to merge two keytabs
nav_order: 2
parent: etc
---


It was the keytab problem.

I'm not sure whether I'm explaining exactly, but this is how I understood.

When sending a Spnego request, a client specifies a SPN.

In this case, SPNs were `TARGET_HOST@REALM` and `TARGET_IP@REALM`.

And the server who handles this request, the Spring app, should have a keytab which has all these SPNs.

But I found that the keytab only had a principal of `TARGET_HOST`.

Something like this:

```
$ klist -kt /path/to/keytab
Keytab name: FILE:/path/to/keytab
KVNO Timestamp         Principal
---- ----------------- --------------------------------------------------------
   3 02/03/20 14:33:39 HTTP/TARGET_HOST@REALM
   3 02/03/20 14:33:39 HTTP/TARGET_HOST@REALM
```

That should contain `HTTP/TARGET_IP@REALM`.

To implement this, the following steps were done:

- Create a new principal with `HTTP/TARGET_IP@REALM`
```
kadmin.local:  addprinc -randkey HTTP/alerts.c3s.navercorp.com@C3.NAVER.COM
```
- Create a new keytab which has the new principal
```
kadmin.local:  ktadd -k /home/irteamsu/alerts.dns.name.keytab -norandkey
```
- Merge the existing and the new keytab

```
$ sudo -i ktutil
ktutil:  read_kt /home1/irteamsu/keytab/spnego.service.keytab
ktutil:  read_kt /home1/irteamsu/alerts.dns.name.keytab
ktutil:  write_kt /home1/irteamsu/keytab/alerts.dns.name.spnego.keytab
```
Then, it shows as:

```
$ klist -kt /path/to/keytab
Keytab name: FILE:/path/to/keytab
KVNO Timestamp         Principal
---- ----------------- --------------------------------------------------------
   3 02/03/20 14:33:39 HTTP/TARGET_HOST@REALM
   3 02/03/20 14:33:39 HTTP/TARGET_HOST@REALM
   1 02/03/20 14:33:39 HTTP/TARGET_IP@REALM
```

Then I restarted the dispatcher app with the new keytab, which solved my problem.

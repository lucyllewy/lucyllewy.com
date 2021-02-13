---
title: "Sender Policy Framework"
date: 2015-10-27 21:09:23
published: true
categories: [sysadmin]
wpid: 781
---

There has been some discussion on the FreeChat IRC Network about the email system for reducing SPAM called "Sender Policy Framework". This system uses the network of servers used worldwide to transform the readable domain name (such as my thehoneymonster.com) into the computer address which uniquely globally identifies the server operating said service (in this case my own blog and XYZindustries).

That network of servers is called the "Domain Name System". DNS has the ability to publish arbitrary information assigned to specific names such as thehoneymonster.com or www.thehoneymonster.com (where the two are different entities). This arbitrary related information system (known as TXT records) is used by the SPF project to assign a special chunk of information which details the computer addresses which are allowed to send email that purports to be from thehoneymonster.com (or whichever domain you implement the scheme upon).

I have now implemented a set of SPF rules on all the domains that I have access to including a few client domains. If you find problems with sending email from a domain which I administer, please let me know and I can try to find a solution which still minimises the possibility for spammers to use your domain name for their spams' "from" address. The main issue I can foresee is where a client may be using a third-party webmail service or sending email through an ISP email-server instead of via XYZi's.

For those who have not delegated responsibility to myself over their Domain Name(s) the correct TXT record to insert for hosting on XYZi's Pressflow/Drupal cluster is as follows:

```
Name: Not Specified - leave empty
Value: v=spf1 include:xyz-network.com -all
TTL: any value, default is fine
```

And for those hosting via IND-Web.com's system, then the following is correct;

```
Name: Not Specified - leave empty
```

```
Value: v=spf1 include:ind-web.com -all
TTL: any value, default is fine
```
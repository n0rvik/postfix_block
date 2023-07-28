### Блокировки для postfix


FILE: bl_client

```
/\.(dsl).*\..*\..*/i
    REJECT Hi. Email from your address is blocked. If you are not a spammer, please let us know by email noc@firma.ru. (#001)

# ip111-71.alfamultimedia.pl
/(^ip|clients|node|pool|ppp|ptp|user|no[-]?ptr|lease|dsl|static|pontocomet|asianet|rapidanet|customers).*\..*\
    REJECT Hi. Email from your address is blocked. If you are not a spammer, please let us know by email noc@firma.ru. (#002)

# ecs-119-3-235-101.compute.hwclouds-dns.com
# lease.203.223.44.102.camnet.com.kh
/(^esc|^ip|clients|host|node|pool|ppp|ptp|user|no[-]?ptr|lease|dsl|static|pc)[-.](\d{1,3}[-.]){3}\d{1,3}\..*\.
    REJECT Hi. Email from your address is blocked. If you are not a spammer, please let us know by email noc@firma.ru. (#003)

# 37-145-207-229.broadband.corbina.ru
/(\d{1,3}[-.]){3}(\d{1,3})[-.](ptp|dsl|broadband|sg-ip\d{1,2}|retatil|dynamic|static|inter|net|nat|pontocomnet
    REJECT Hi. Email from your address is blocked. If you are not a spammer, please let us know by email noc@firma.ru. (#004)

# 91-215-171-103.dns-rus.net
/(\d{1,3}[-.]){3}(\d{1,3})[-.]dns-rus\.net/i
    REJECT Hi. Email from your address is blocked. If you are not a spammer, please let us know by email noc@firma.ru. (#005)

# 519771-cy20446.tmweb.ru
/\d{1,6}[-.]\S\S\d{1,5}\.tmweb\.ru/i
    REJECT Hi. Email from your address is blocked. If you are not a spammer, please let us know by email noc@firma.ru. (#006)

# o2.ptr4036.sg-ip2.cognism-privacy.com
/\.ptr\d{1,4}\.sg-ip\d{1}\..*\..*/i
    REJECT Hi. Email from your address is blocked. If you are not a spammer, please let us know by email noc@firma.ru. (#007)

# ecs-119-3-235-101.compute.hwclouds-dns.com
#/\S*[-.](\d{1,3}[-.]){3}\d{1,3}(\.\S*){3}/i
#    REJECT Hi. Email from your address is blocked. If you are not a spammer, please let us know by email noc@firma.ru. (#008)

# host-200-24-246-238.nedetel.net
/(^host)[-.](\d{1,3}[-.]){3}\d{1,3}\..*\.net/i
    REJECT Hi. Email from your address is blocked. If you are not a spammer, please let us know by email noc@firma.ru. (#009)
```

### Как воспользоваться


FILE: main.cf


```
smtpd_recipient_restrictions =
    ... 
    check_client_access pcre:/etc/postfix/bl_client
    ...
    permit
```

Полный список

```
smtpd_recipient_restrictions =
    reject_non_fqdn_recipient
    reject_non_fqdn_sender
    reject_unknown_sender_domain
    reject_unknown_recipient_domain
    permit_sasl_authenticated
    permit_mynetworks
    check_client_access hash:/etc/postfix/internal_networks
    check_sender_access hash:/etc/postfix/not_our_domain_as_sender
    reject_unauth_destination
    check_recipient_access hash:/etc/postfix/roleaccount_exceptions
    check_sender_access hash:/etc/postfix/rhsbl_sender_exceptions
    check_helo_access pcre:/etc/postfix/helo_checks
    check_sender_mx_access cidr:/etc/postfix/bogus_mx
    check_sender_access hash:/etc/postfix/common_spam_senderdomains
    check_sender_access regexp:/etc/postfix/common_spam_senderdomain_keywords
    check_client_access pcre:/etc/postfix/bl_client
    check_client_access cidr:/etc/postfix/bl_ip_client
    check_client_access pcre:/etc/postfix/bl_host_client
    reject_unknown_client
    reject_non_fqdn_hostname
    reject_invalid_hostname
    reject_unknown_hostname
    permit

```
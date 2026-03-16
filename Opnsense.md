## Introduction

## Cloudflare & Let\'s Encrypt & HAProxy & Nextcloud {#cloudflare_lets_encrypt_haproxy_nextcloud}

### Steps

Cloudflare:

CNAME `<b>`{=html}SUBDOMAIN.YOUR.DOMAIN`</b>`{=html}

opnsense:

1\. ACME Client: Let\'s Encrypt Wildcard YOUR.DOMAIN

2\. HAProxy: Redirect to inernal Nextcloud Server

3\. Unbound DNS: Local DNS entry for reverse proxying to nextcloud

## Plugins

### DynamicDNS / Cloudflare {#dynamicdns_cloudflare}

DDNS is used to update your public dynamic IP Address to your DDNS
Provider of choice. In this example I used Cloudflare.
[Source](https://arminreiter.com/2022/08/configure-dynamic-dns-in-opnsense-with-cloudflare/)

Config DynamicDNS

`Enabled: true`\
`Service: Cloudflare`\
`Username: E-Mail address of your account`\
`Password: Global API key for your account (Open Cloudflare > My Account > API Tokens > Global API Key > View)`\
`Zone: your.domain (e.g. arminreiter.com)`\
`Hostname: full domain name you want to update (e.g. dyn.arminreiter.com)`\
`Check ip method: Interface`\
`Force SSL: true`\
`Interface to monitor: WAN`

### IPS

<https://docs.opnsense.org/manual/how-tos/ips-feodo.html>

[Category:Firewall](Category:Firewall "Category:Firewall"){.wikilink}

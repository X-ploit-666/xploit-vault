- **URL** : https://www.thredup.com/  [403 Forbidden ] 
- **HTTPServer** : CloudFlare
- **IP** : 172.64.153.227
- **Title** : thredUP
- **Domain:** `thredup.com`
- **Registrar:** GoDaddy.com, LLC
- **Creation Date:** 2008-11-15
- **Expiration Date:** 2026-10-16
- **Domain Status:** `clientDeleteProhibited`, `clientRenewProhibited`, `clientTransferProhibited`, `clientUpdateProhibited`
- **Name Servers:** `MATT.NS.CLOUDFLARE.COM`, `ROSE.NS.CLOUDFLARE.COM`
- **DNSSEC:** unsigned
- **Registrant / Tech info:** Mostly private (Domains By Proxy)
- **Registrar abuse contacts:** `abuse@godaddy.com` / +1.4806242505

---
# Using Dig Command

```bash
dig thredup.com

;; ANSWER SECTION:
thredup.com.		300	IN	A	104.18.34.29
thredup.com.		300	IN	A	172.64.153.227

```

```bash
dig thredup.com MX

;; ANSWER SECTION:
thredup.com.		300	IN	MX	10 aspmx2.googlemail.com.
thredup.com.		300	IN	MX	5 alt1.aspmx.l.google.com.
thredup.com.		300	IN	MX	1 aspmx.l.google.com.
thredup.com.		300	IN	MX	10 aspmx3.googlemail.com.
thredup.com.		300	IN	MX	5 alt2.aspmx.l.google.com.

```

```bash
dig thredup.com NS

;; ANSWER SECTION:
thredup.com.		21600	IN	NS	matt.ns.cloudflare.com.
thredup.com.		21600	IN	NS	rose.ns.cloudflare.com.

```

```bash
dig thredup.com -x IP

;; ANSWER SECTION:
thredup.com.		290	IN	A	104.18.34.29
thredup.com.		290	IN	A	172.64.153.227

```

---


# Using wafw00f Command

- **wafw00f** : The site https://www.thredup.com/ is behind Cloudflare (Cloudflare Inc.) and/or Envoy (EnvoyProxy) WAF

---


# Using DnsEnum

```bash
dnsenum --enum thredup.com -f /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r 



-----   thredup.com   -----


Host's addresses:
__________________

thredup.com.                             300      IN    A        104.18.34.29
thredup.com.                             300      IN    A        172.64.153.227


Name Servers:
______________

rose.ns.cloudflare.com.                  21600    IN    A        172.64.32.141
rose.ns.cloudflare.com.                  21600    IN    A        173.245.58.141
rose.ns.cloudflare.com.                  21600    IN    A        108.162.192.141
matt.ns.cloudflare.com.                  21600    IN    A        173.245.59.131
matt.ns.cloudflare.com.                  21600    IN    A        108.162.193.131
matt.ns.cloudflare.com.                  21600    IN    A        172.64.33.131


Mail (MX) Servers:
___________________

aspmx2.googlemail.com.                   293      IN    A        172.253.152.26
aspmx.l.google.com.                      293      IN    A        142.251.127.26
aspmx3.googlemail.com.                   293      IN    A        142.251.127.27
alt1.aspmx.l.google.com.                 293      IN    A        172.253.152.26
alt2.aspmx.l.google.com.                 293      IN    A        142.251.127.27


Trying Zone Transfers and getting Bind Versions:
_________________________________________________


Trying Zone Transfer for thredup.com on rose.ns.cloudflare.com ... 
AXFR record query failed: FORMERR

Trying Zone Transfer for thredup.com on matt.ns.cloudflare.com ... 
AXFR record query failed: FORMERR


Scraping thredup.com subdomains from Google:
_____________________________________________



Brute forcing with /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:
_________________________________________________________________________________

Thread 11 terminated abnormally: empty label in "# Suite 300, San Francisco, California, 94105, USA..thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 11.
Thread 10 terminated abnormally: label too long in "# Priority ordered case sensative list, where entries were found .thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 10.
Thread 12 terminated abnormally: empty label in ".thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 12.
blog.thredup.com.                        30       IN    CNAME    proxy-ssl.webflow.com.
proxy-ssl.webflow.com.                   60       IN    CNAME    proxy-ssl-geo-2.webflow.com.
proxy-ssl-geo-2.webflow.com.             60       IN    A        15.161.34.42
proxy-ssl-geo-2.webflow.com.             60       IN    A        35.152.117.67
proxy-ssl-geo-2.webflow.com.             60       IN    A        15.160.106.203
login.thredup.com.                       300      IN    A        104.18.34.29
login.thredup.com.                       300      IN    A        172.64.153.227
support.thredup.com.                     300      IN    A        172.64.153.227
support.thredup.com.                     300      IN    A        104.18.34.29
help.thredup.com.                        300      IN    A        172.64.153.227
help.thredup.com.                        300      IN    A        104.18.34.29
events.thredup.com.                      300      IN    CNAME    events-branding.zoom.us.
events-branding.zoom.us.                 60       IN    A        170.114.52.84
docs.thredup.com.                        300      IN    CNAME    ghs.googlehosted.com.
ghs.googlehosted.com.                    300      IN    A        142.251.141.243
legal.thredup.com.                       300      IN    A        104.18.34.29
legal.thredup.com.                       300      IN    A        172.64.153.227
email.thredup.com.                       300      IN    CNAME    sparkpostmail.com.
sparkpostmail.com.                       60       IN    A        54.230.183.15
sparkpostmail.com.                       60       IN    A        54.230.183.83
sparkpostmail.com.                       60       IN    A        54.230.183.120
sparkpostmail.com.                       60       IN    A        54.230.183.109
newsletter.thredup.com.                  300      IN    CNAME    ext-cust.squarespace.com.
ext-cust.squarespace.com.                201      IN    A        198.49.23.144
ext-cust.squarespace.com.                201      IN    A        198.185.159.144
ext-cust.squarespace.com.                201      IN    A        198.185.159.145
ext-cust.squarespace.com.                201      IN    A        198.49.23.145
careers.thredup.com.                     300      IN    CNAME    thredup.phenompeople.net.
thredup.phenompeople.net.                60       IN    CNAME    d31uinunhwd43v.cloudfront.net.
d31uinunhwd43v.cloudfront.net.           60       IN    A        3.165.255.120
d31uinunhwd43v.cloudfront.net.           60       IN    A        3.165.255.47
d31uinunhwd43v.cloudfront.net.           60       IN    A        3.165.255.108
d31uinunhwd43v.cloudfront.net.           60       IN    A        3.165.255.80
www.thredup.com.                         300      IN    A        104.18.34.29
www.thredup.com.                         300      IN    A        172.64.153.227
mail.thredup.com.                        300      IN    CNAME    ghs.googlehosted.com.
ghs.googlehosted.com.                    300      IN    A        142.251.141.243
c.thredup.com.                           38       IN    A        98.89.108.138
c.thredup.com.                           38       IN    A        98.85.197.116
reports.thredup.com.                     300      IN    CNAME    app2.creatoriq.com.
app2.creatoriq.com.                      60       IN    A        54.177.6.100
app2.creatoriq.com.                      60       IN    A        54.219.73.201
groups.thredup.com.                      300      IN    CNAME    ghs.googlehosted.com.
ghs.googlehosted.com.                    300      IN    A        142.251.141.243
newsroom.thredup.com.                    300      IN    CNAME    ext-cust.squarespace.com.
ext-cust.squarespace.com.                219      IN    A        198.49.23.144
ext-cust.squarespace.com.                219      IN    A        198.185.159.145
ext-cust.squarespace.com.                219      IN    A        198.49.23.145
ext-cust.squarespace.com.                219      IN    A        198.185.159.144
Help.thredup.com.                        300      IN    A        104.18.34.29
Help.thredup.com.                        300      IN    A        172.64.153.227
get.thredup.com.                         300      IN    A        172.64.153.227
get.thredup.com.                         300      IN    A        104.18.34.29
status.thredup.com.                      300      IN    CNAME    rhf8lm31mg6v.stspg-customer.com.
rhf8lm31mg6v.stspg-customer.com.         60       IN    CNAME             (
status-thredup-com-2e59b274-77ad-42be-b5ba-72570ef810da.saas.atlassian.com. 60       IN    A                 (
status-thredup-com-2e59b274-77ad-42be-b5ba-72570ef810da.saas.atlassian.com. 60       IN    A                 (
status-thredup-com-2e59b274-77ad-42be-b5ba-72570ef810da.saas.atlassian.com. 60       IN    A                 (
status-thredup-com-2e59b274-77ad-42be-b5ba-72570ef810da.saas.atlassian.com. 60       IN    A                 (
Login.thredup.com.                       280      IN    A        104.18.34.29
Login.thredup.com.                       280      IN    A        172.64.153.227
C.thredup.com.                           20       IN    A        98.85.197.116
C.thredup.com.                           20       IN    A        98.89.108.138
Support.thredup.com.                     273      IN    A        172.64.153.227
Support.thredup.com.                     273      IN    A        104.18.34.29
summary.thredup.com.                     300      IN    CNAME    api2.creatoriq.com.
api2.creatoriq.com.                      60       IN    A        204.236.184.125
api2.creatoriq.com.                      60       IN    A        13.57.125.82
Blog.thredup.com.                        30       IN    CNAME    proxy-ssl.webflow.com.
proxy-ssl.webflow.com.                   60       IN    CNAME    proxy-ssl-geo-2.webflow.com.
proxy-ssl-geo-2.webflow.com.             60       IN    A        15.161.34.42
proxy-ssl-geo-2.webflow.com.             60       IN    A        15.160.106.203
proxy-ssl-geo-2.webflow.com.             60       IN    A        35.152.117.67
Events.thredup.com.                      300      IN    CNAME    events-branding.zoom.us.
events-branding.zoom.us.                 60       IN    A        170.114.52.84
frame.thredup.com.                       300      IN    CNAME    shops.myshopify.com.
shops.myshopify.com.                     3587     IN    A        23.227.38.74
labs.thredup.com.                        300      IN    CNAME    posthaven.com.
posthaven.com.                           21600    IN    A        188.93.148.37
Legal.thredup.com.                       300      IN    A        104.18.34.29
Legal.thredup.com.                       300      IN    A        172.64.153.227
ir.thredup.com.                          300      IN    CNAME    thredup.gcs-web.com.
thredup.gcs-web.com.                     300      IN    CNAME    leapfrog-ssl-42.gcs-web.com.edgekey.net.
leapfrog-ssl-42.gcs-web.com.edgekey.net. 21600    IN    CNAME             (
e63540.dsca.akamaiedge.net.              20       IN    A        2.21.2.177
e63540.dsca.akamaiedge.net.              20       IN    A        2.21.2.171
women.thredup.com.                       300      IN    A        104.18.34.29
women.thredup.com.                       300      IN    A        172.64.153.227
teen.thredup.com.                        300      IN    A        172.64.153.227
teen.thredup.com.                        300      IN    A        104.18.34.29
Careers.thredup.com.                     300      IN    CNAME    thredup.phenompeople.net.
thredup.phenompeople.net.                60       IN    CNAME    d31uinunhwd43v.cloudfront.net.
d31uinunhwd43v.cloudfront.net.           60       IN    A        3.165.255.120
d31uinunhwd43v.cloudfront.net.           60       IN    A        3.165.255.108
d31uinunhwd43v.cloudfront.net.           60       IN    A        3.165.255.47
d31uinunhwd43v.cloudfront.net.           60       IN    A        3.165.255.80
ec.thredup.com.                          300      IN    A        172.64.153.227
ec.thredup.com.                          300      IN    A        104.18.34.29
Email.thredup.com.                       300      IN    CNAME    sparkpostmail.com.
sparkpostmail.com.                       60       IN    A        54.230.183.83
sparkpostmail.com.                       60       IN    A        54.230.183.15
sparkpostmail.com.                       60       IN    A        54.230.183.109
sparkpostmail.com.                       60       IN    A        54.230.183.120
Docs.thredup.com.                        227      IN    CNAME    ghs.googlehosted.com.
ghs.googlehosted.com.                    227      IN    A        142.251.141.243
brand.thredup.com.                       300      IN    CNAME    shops.myshopify.com.
shops.myshopify.com.                     2646     IN    A        23.227.38.74
auth.thredup.com.                        300      IN    A        172.64.153.227
auth.thredup.com.                        300      IN    A        104.18.34.29
Mail.thredup.com.                        300      IN    CNAME    ghs.googlehosted.com.
ghs.googlehosted.com.                    300      IN    A        142.251.141.243
Reports.thredup.com.                     300      IN    CNAME    app2.creatoriq.com.
app2.creatoriq.com.                      60       IN    A        54.219.73.201
app2.creatoriq.com.                      60       IN    A        54.177.6.100
ae.thredup.com.                          300      IN    CNAME    shops.myshopify.com.
shops.myshopify.com.                     2601     IN    A        23.227.38.74
cal.thredup.com.                         300      IN    CNAME    ghs.googlehosted.com.
ghs.googlehosted.com.                    300      IN    A        142.251.141.243
WWW.thredup.com.                         300      IN    A        104.18.34.29
WWW.thredup.com.                         300      IN    A        172.64.153.227
Newsletter.thredup.com.                  300      IN    CNAME    ext-cust.squarespace.com.
ext-cust.squarespace.com.                122      IN    A        198.185.159.144
ext-cust.squarespace.com.                122      IN    A        198.49.23.144
ext-cust.squarespace.com.                122      IN    A        198.49.23.145
ext-cust.squarespace.com.                122      IN    A        198.185.159.145
discover.thredup.com.                    300      IN    A        104.18.34.29
discover.thredup.com.                    300      IN    A        172.64.153.227
ops.thredup.com.                         300      IN    A        104.18.34.29
ops.thredup.com.                         300      IN    A        172.64.153.227
teens.thredup.com.                       300      IN    A        104.18.34.29
teens.thredup.com.                       300      IN    A        172.64.153.227
bbb.thredup.com.                         300      IN    A        104.18.34.29
bbb.thredup.com.                         300      IN    A        172.64.153.227
notify.thredup.com.                      300      IN    CNAME    sparkpostmail.com.
sparkpostmail.com.                       60       IN    A        54.230.183.120
sparkpostmail.com.                       60       IN    A        54.230.183.83
sparkpostmail.com.                       60       IN    A        54.230.183.109
sparkpostmail.com.                       60       IN    A        54.230.183.15
try.thredup.com.                         300      IN    CNAME    try.thredup.com.herokudns.com.
Newsroom.thredup.com.                    300      IN    CNAME    ext-cust.squarespace.com.
ext-cust.squarespace.com.                300      IN    A        198.49.23.145
ext-cust.squarespace.com.                300      IN    A        198.185.159.144
ext-cust.squarespace.com.                300      IN    A        198.49.23.144
ext-cust.squarespace.com.                300      IN    A        198.185.159.145
gap.thredup.com.                         300      IN    CNAME    shops.myshopify.com.
shops.myshopify.com.                     2680     IN    A        23.227.38.74
clicks.thredup.com.                      300      IN    A        104.18.34.29
clicks.thredup.com.                      300      IN    A        172.64.153.227
EMAIL.thredup.com.                       300      IN    CNAME    sparkpostmail.com.
sparkpostmail.com.                       60       IN    A        54.230.183.109
sparkpostmail.com.                       60       IN    A        54.230.183.15
sparkpostmail.com.                       60       IN    A        54.230.183.83
sparkpostmail.com.                       60       IN    A        54.230.183.120
scorecard.thredup.com.                   300      IN    CNAME             (
foo.thredup.com.                         300      IN    CNAME             (
cos.thredup.com.                         300      IN    CNAME    shops.myshopify.com.
shops.myshopify.com.                     3129     IN    A        23.227.38.74
notifications.thredup.com.               300      IN    A        172.64.153.227
notifications.thredup.com.               300      IN    A        104.18.34.29
Groups.thredup.com.                      300      IN    CNAME    ghs.googlehosted.com.
ghs.googlehosted.com.                    300      IN    A        142.251.141.243
composer.thredup.com.                    300      IN    A        172.64.153.227
composer.thredup.com.                    300      IN    A        104.18.34.29
Summary.thredup.com.                     300      IN    CNAME    api2.creatoriq.com.
api2.creatoriq.com.                      60       IN    A        204.236.184.125
api2.creatoriq.com.                      60       IN    A        13.57.125.82
Labs.thredup.com.                        300      IN    CNAME    posthaven.com.
posthaven.com.                           21600    IN    A        188.93.148.37
Status.thredup.com.                      300      IN    CNAME    rhf8lm31mg6v.stspg-customer.com.
rhf8lm31mg6v.stspg-customer.com.         60       IN    CNAME             (
status-thredup-com-2e59b274-77ad-42be-b5ba-72570ef810da.saas.atlassian.com. 60       IN    A                 (
status-thredup-com-2e59b274-77ad-42be-b5ba-72570ef810da.saas.atlassian.com. 60       IN    A                 (
status-thredup-com-2e59b274-77ad-42be-b5ba-72570ef810da.saas.atlassian.com. 60       IN    A                 (
status-thredup-com-2e59b274-77ad-42be-b5ba-72570ef810da.saas.atlassian.com. 60       IN    A                 (
Women.thredup.com.                       300      IN    A        104.18.34.29
Women.thredup.com.                       300      IN    A        172.64.153.227
Brand.thredup.com.                       300      IN    CNAME    shops.myshopify.com.
shops.myshopify.com.                     758      IN    A        23.227.38.74
alexa.thredup.com.                       300      IN    A        104.18.34.29
alexa.thredup.com.                       300      IN    A        172.64.153.227
EC.thredup.com.                          300      IN    A        104.18.34.29
EC.thredup.com.                          300      IN    A        172.64.153.227
NewsRoom.thredup.com.                    300      IN    CNAME    ext-cust.squarespace.com.
ext-cust.squarespace.com.                300      IN    A        198.49.23.144
ext-cust.squarespace.com.                300      IN    A        198.49.23.145
ext-cust.squarespace.com.                300      IN    A        198.185.159.144
ext-cust.squarespace.com.                300      IN    A        198.185.159.145
DOCS.thredup.com.                        300      IN    CNAME    ghs.googlehosted.com.
ghs.googlehosted.com.                    300      IN    A        142.251.141.243
sftp.thredup.com.                        300      IN    A        3.219.188.240
Discover.thredup.com.                    300      IN    A        172.64.153.227
Discover.thredup.com.                    300      IN    A        104.18.34.29
SUPPORT.thredup.com.                     300      IN    A        104.18.34.29
SUPPORT.thredup.com.                     300      IN    A        172.64.153.227
IR.thredup.com.                          300      IN    CNAME    thredup.gcs-web.com.
thredup.gcs-web.com.                     300      IN    CNAME    leapfrog-ssl-42.gcs-web.com.edgekey.net.
leapfrog-ssl-42.gcs-web.com.edgekey.net. 21600    IN    CNAME             (
e63540.dsca.akamaiedge.net.              20       IN    A        2.21.2.177
e63540.dsca.akamaiedge.net.              20       IN    A        2.21.2.171
Www.thredup.com.                         214      IN    A        104.18.34.29
Www.thredup.com.                         214      IN    A        172.64.153.227
atlantis.thredup.com.                    300      IN    CNAME    public-ingress.red.thredup.cloud.
public-ingress.red.thredup.cloud.        300      IN    CNAME             (
a101aaa7b755c4400b58de99aec6b035-308636389.us-east-1.elb.amazonaws.com. 60       IN    A                 (
a101aaa7b755c4400b58de99aec6b035-308636389.us-east-1.elb.amazonaws.com. 60       IN    A                 (
a101aaa7b755c4400b58de99aec6b035-308636389.us-east-1.elb.amazonaws.com. 60       IN    A                 (
EVENTS.thredup.com.                      300      IN    CNAME    events-branding.zoom.us.
events-branding.zoom.us.                 60       IN    A        170.114.52.84
BBB.thredup.com.                         300      IN    A        104.18.34.29
BBB.thredup.com.                         300      IN    A        172.64.153.227
Frame.thredup.com.                       300      IN    CNAME    shops.myshopify.com.
shops.myshopify.com.                     3424     IN    A        23.227.38.74
hire.thredup.com.                        300      IN    CNAME             (
qjmkzf4hwy5uadm383y2vitog8k90cust.dns-782.com. 300      IN    A        67.205.154.99
AE.thredup.com.                          300      IN    CNAME    shops.myshopify.com.
shops.myshopify.com.                     3505     IN    A        23.227.38.74
NewsLetter.thredup.com.                  300      IN    CNAME    ext-cust.squarespace.com.
ext-cust.squarespace.com.                16       IN    A        198.185.159.144
ext-cust.squarespace.com.                16       IN    A        198.49.23.145
ext-cust.squarespace.com.                16       IN    A        198.185.159.145
ext-cust.squarespace.com.                16       IN    A        198.49.23.144
STATUS.thredup.com.                      300      IN    CNAME    rhf8lm31mg6v.stspg-customer.com.
rhf8lm31mg6v.stspg-customer.com.         60       IN    CNAME             (
status-thredup-com-2e59b274-77ad-42be-b5ba-72570ef810da.saas.atlassian.com. 60       IN    A                 (
status-thredup-com-2e59b274-77ad-42be-b5ba-72570ef810da.saas.atlassian.com. 60       IN    A                 (
status-thredup-com-2e59b274-77ad-42be-b5ba-72570ef810da.saas.atlassian.com. 60       IN    A                 (
status-thredup-com-2e59b274-77ad-42be-b5ba-72570ef810da.saas.atlassian.com. 60       IN    A                 (
toms.thredup.com.                        300      IN    CNAME    shops.myshopify.com.
shops.myshopify.com.                     3004     IN    A        23.227.38.74
Thread 13 terminated abnormally: empty label in ".thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 13.
pact.thredup.com.                        300      IN    CNAME    shops.myshopify.com.
shops.myshopify.com.                     3380     IN    A        23.227.38.74
EMail.thredup.com.                       300      IN    CNAME    sparkpostmail.com.
sparkpostmail.com.                       60       IN    A        54.230.183.109
sparkpostmail.com.                       60       IN    A        54.230.183.15
sparkpostmail.com.                       60       IN    A        54.230.183.120
sparkpostmail.com.                       60       IN    A        54.230.183.83
veda.thredup.com.                        300      IN    CNAME    shops.myshopify.com.
shops.myshopify.com.                     1853     IN    A        23.227.38.74
maternity.thredup.com.                   300      IN    A        104.18.34.29
maternity.thredup.com.                   300      IN    A        172.64.153.227
HELP.thredup.com.                        300      IN    A        172.64.153.227
HELP.thredup.com.                        300      IN    A        104.18.34.29
Thread 14 terminated abnormally: label too long in "_W0QQlotrZ1QQsasuperfeaturedZ1QQsocmdZListingItemListQQsocolumnlayoutZ1QQsocustoverrideZ1.thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 14.


Performing recursion:
______________________


 ---- Checking subdomains NS records ----
C.thredup.com.                           21600    IN    NS       ns-881.awsdns-46.net.
C.thredup.com.                           21600    IN    NS       ns-1356.awsdns-41.org.
C.thredup.com.                           21600    IN    NS       ns-433.awsdns-54.com.
C.thredup.com.                           21600    IN    NS       ns-1607.awsdns-08.co.uk.
d31uinunhwd43v.cloudfront.net.           21600    IN    NS       ns-1123.awsdns-12.org.
d31uinunhwd43v.cloudfront.net.           21600    IN    NS       ns-1873.awsdns-42.co.uk.
d31uinunhwd43v.cloudfront.net.           21600    IN    NS       ns-585.awsdns-09.net.
d31uinunhwd43v.cloudfront.net.           21600    IN    NS       ns-210.awsdns-26.com.
sparkpostmail.com.                       16537    IN    NS       ns-1855.awsdns-39.co.uk.
sparkpostmail.com.                       16537    IN    NS       ns-304.awsdns-38.com.
sparkpostmail.com.                       16537    IN    NS       ns-1361.awsdns-42.org.
sparkpostmail.com.                       16537    IN    NS       ns-775.awsdns-32.net.
sparkpostmail.com.                       16537    IN    NS       ns-1855.awsdns-39.co.uk.
sparkpostmail.com.                       16537    IN    NS       ns-304.awsdns-38.com.
sparkpostmail.com.                       16537    IN    NS       ns-1361.awsdns-42.org.
sparkpostmail.com.                       16537    IN    NS       ns-775.awsdns-32.net.
sparkpostmail.com.                       16537    IN    NS       ns-1855.awsdns-39.co.uk.
sparkpostmail.com.                       16537    IN    NS       ns-1361.awsdns-42.org.
sparkpostmail.com.                       16537    IN    NS       ns-304.awsdns-38.com.
sparkpostmail.com.                       16537    IN    NS       ns-775.awsdns-32.net.
posthaven.com.                           21600    IN    NS       ns-836.awsdns-40.net.
posthaven.com.                           21600    IN    NS       ns-1161.awsdns-17.org.
posthaven.com.                           21600    IN    NS       ns-1589.awsdns-06.co.uk.
posthaven.com.                           21600    IN    NS       ns-475.awsdns-59.com.
c.thredup.com.                           21599    IN    NS       ns-1607.awsdns-08.co.uk.
c.thredup.com.                           21599    IN    NS       ns-881.awsdns-46.net.
c.thredup.com.                           21599    IN    NS       ns-1356.awsdns-41.org.
c.thredup.com.                           21599    IN    NS       ns-433.awsdns-54.com.
d31uinunhwd43v.cloudfront.net.           21599    IN    NS       ns-1123.awsdns-12.org.
d31uinunhwd43v.cloudfront.net.           21599    IN    NS       ns-210.awsdns-26.com.
d31uinunhwd43v.cloudfront.net.           21599    IN    NS       ns-1873.awsdns-42.co.uk.
d31uinunhwd43v.cloudfront.net.           21599    IN    NS       ns-585.awsdns-09.net.
sparkpostmail.com.                       21600    IN    NS       ns-1855.awsdns-39.co.uk.
sparkpostmail.com.                       21600    IN    NS       ns-304.awsdns-38.com.
sparkpostmail.com.                       21600    IN    NS       ns-1361.awsdns-42.org.
sparkpostmail.com.                       21600    IN    NS       ns-775.awsdns-32.net.
posthaven.com.                           21600    IN    NS       ns-1589.awsdns-06.co.uk.
posthaven.com.                           21600    IN    NS       ns-1161.awsdns-17.org.
posthaven.com.                           21600    IN    NS       ns-836.awsdns-40.net.
posthaven.com.                           21600    IN    NS       ns-475.awsdns-59.com.
sparkpostmail.com.                       21600    IN    NS       ns-775.awsdns-32.net.
sparkpostmail.com.                       21600    IN    NS       ns-1361.awsdns-42.org.
sparkpostmail.com.                       21600    IN    NS       ns-1855.awsdns-39.co.uk.
sparkpostmail.com.                       21600    IN    NS       ns-304.awsdns-38.com.

 ----   Recursion level 1   ---- 

 Recursion on C.thredup.com ...
Thread 20 terminated abnormally: empty label in ".C.thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 20.
Thread 21 terminated abnormally: label too long in "# Priority ordered case sensative list, where entries were found .C.thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 21.
Thread 22 terminated abnormally: empty label in "# Suite 300, San Francisco, California, 94105, USA..C.thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 22.
Thread 23 terminated abnormally: label too long in "%5BNipponsei%5D%20Galaxy%20Angel%20Rune%20ED%20Single%20-%20Happy%20Flight%20%5BTomita%20Maho%5D.C.thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 23.
Thread 24 terminated abnormally: label too long in "%5BNipponsei%5D%20NARUTO%20OP9%20Single%20-%20Yura%20Yura%20%5BHearts%20Grow%5D.C.thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 24.

 Recursion on c.thredup.com ...
Thread 25 terminated abnormally: empty label in ".c.thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 25.
Thread 27 terminated abnormally: label too long in "# Priority ordered case sensative list, where entries were found .c.thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 27.
Thread 28 terminated abnormally: empty label in "# Suite 300, San Francisco, California, 94105, USA..c.thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 28.
Thread 26 terminated abnormally: label too long in "%5BNipponsei%5D%20Galaxy%20Angel%20Rune%20ED%20Single%20-%20Happy%20Flight%20%5BTomita%20Maho%5D.c.thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 26.
Thread 29 terminated abnormally: label too long in "%5BNipponsei%5D%20NARUTO%20OP9%20Single%20-%20Yura%20Yura%20%5BHearts%20Grow%5D.c.thredup.com" at /usr/share/perl5/Net/DNS/Question.pm line 79 thread 29.


Launching Whois Queries:
_________________________

 whois ip result:   104.18.34.0        ->      104.16.0.0/12
 whois ip result:   172.64.153.0       ->      172.64.0.0/13
 whois ip result:   98.89.108.0        ->      98.88.0.0/13
 whois ip result:   3.219.188.0        ->      3.208.0.0/12
 whois ip result:   98.85.197.0        ->      98.80.0.0/12


thredup.com___________

 172.64.0.0/13
 98.80.0.0/12
 3.208.0.0/12
 98.88.0.0/13
 104.16.0.0/12


Performing reverse lookup on 4194304 ip addresses:
___________________________________________________



```


# Things To Try  : 

- https://posthaven.com/util/s3params
- https://posthaven.com/sites/check_hostname?hostname=examplee
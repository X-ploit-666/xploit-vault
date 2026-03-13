
```bash
curl -I "https://www.bilt.com/"

HTTP/2 200 
cache-control: public, max-age=0, must-revalidate
content-length: 5167804
content-security-policy: child-src 'self';frame-ancestors 'self' *.biltrewards.com *.activebuilding.com *.avalonaccess.com *.ct-prod.avalonbay.com *.henrihome.com avalonaccess.com www.hqo.co www.hqo.com www.hqoapp.com www.mrcooper.com *.loftliving.com mycommunity.americancampus.com americancampus.my.site.com portal.tkclients.com resident.eliseai.com *.venn.city *.res.venn.city;frame-src 'self' https://*.bilt.com https://*.biltrewards.com https://www.datocms-assets.com/43819/ https://cdn.plaid.com https://dyscanweb.dyneti.com https://js.verygoodvault.com https://js3.verygoodvault.com https://www.google.com https://decagon.ai https://*.servisbot.com https://cardswitcher.knotapi.com https://alloysdk.alloy.co https://*.onefootprint.com https://cdn.userway.org https://sync-transcend-cdn.com https://*.jamsadr.com https://*.soul-cycle.com mailto: https://assets.duffel.com https://*.spline.design https://esignature.bluemoonforms.com https://esignpdf.s3.amazonaws.com https://www.facebook.com https://bid.g.doubleclick.net https://td.doubleclick.net https://www.googletagmanager.com https://i.liadm.com https://ct.pinterest.com https://tr.snapchat.com https://www.youtube.com https://youtu.be https://checkoutshopper-live-us.adyen.com https://pay.google.com/ https://applepay.cdn-apple.com/ https://checkout.getflex.com https://checkout.int.getflex.com https://onboarding-embed.int.getflex.com https://onboarding-embed.getflex.com https://*.braintreegateway.com https://*.braintree-api.com https://*.paypal.com https://*.mastercard.com https://*.rsa3dsauth.com https://embedded.cardless.com;
content-type: text/html
cross-origin-opener-policy: same-origin-allow-popups
date: Tue, 10 Mar 2026 00:42:43 GMT
etag: "6ccaad76cd2211d3f9e24bf20a4cedeb"
last-modified: Thu, 05 Mar 2026 17:24:31 GMT
link: <https://framerusercontent.com>; rel="preconnect", <https://framerusercontent.com>; rel="preconnect"; crossorigin=""
referrer-policy: origin
server: Vercel
strict-transport-security: max-age=31536000; includeSubDomains
vary: Accept-Encoding
x-content-type-options: nosniff
x-vercel-cache: MISS
x-vercel-id: fra1::hsvhv-1773103363251-13e81965f51f
x-xss-protection: 1; mode=block
via: 1.1 google
alt-svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000

```

----

```bash
curl  "https://www.bilt.com/api/version" 
{"deploymentId":"dpl_F9o3tmJXa5k1uw7aU7qPZnCMrktn"} 
```

----

```bash
curl "https://www.bilt.com/robots.txt"                                                        
User-agent:*
Disallow:/_next/*
Disallow:/*?*
Disallow:/*account*/
Disallow:/cardholder-agreement
Disallow:/wf/
Disallow:/wfemail*
Disallow:/card/join*
Disallow:/rewards/benefits/internal-bme/*
Disallow:/rewards/benefits/offer-builder/*
Disallow:/p/loan-administration
Disallow:/p/uwm
Disallow:/p/united-card
Disallow:/p/home-buying
Sitemap:https://www.bilt.com/sitemap.xml 
```

### Key Information from `whois bilt.com`

**Domain**

Domain: bilt.com  
Created: 1996-09-10  
Expires: 2035-01-13

**Registrar**

Registrar: MarkMonitor Inc.

- Enterprise-level registrar used by large companies.
    

**DNS Infrastructure**

ns-cloud-d1.googledomains.com  
ns-cloud-d2.googledomains.com  
ns-cloud-d3.googledomains.com  
ns-cloud-d4.googledomains.com

- DNS managed by **Google Cloud DNS / Google Domains**.
    

**Domain Security Status**

clientTransferProhibited  
clientDeleteProhibited  
clientUpdateProhibited  
serverTransferProhibited  
serverDeleteProhibited  
serverUpdateProhibited

- Registry locks enabled (prevents domain hijacking or unauthorized transfer).
    

**DNSSEC**

DNSSEC: unsigned

- DNS responses are not signed.
    

**Registrant**

Organization: DNStination Inc.  
Location: San Francisco, CA

- Domain management/privacy service.
    

---

### Infrastructure Observed (from HTTP response earlier)

Hosting/CDN: Vercel  
Frontend: Framer  
DNS: Google Cloud DNS

Possible architecture:

Google Cloud DNS  
      ↓  
Vercel CDN  
      ↓  
Framer-generated frontend  
      ↓  
Backend APIs / third-party services

---

### Dig

```bash
dig ${domain} 

;; ANSWER SECTION:
bilt.com.		21600	IN	A	34.102.215.178
```

```bash
dig ${domain} MX

;; ANSWER SECTION:
bilt.com.		16539	IN	MX	1 smtp.google.com.
```

```bash
dig ${domain} NS

;; ANSWER SECTION:
bilt.com.		21600	IN	NS	ns-cloud-d4.googledomains.com.
bilt.com.		21600	IN	NS	ns-cloud-d2.googledomains.com.
bilt.com.		21600	IN	NS	ns-cloud-d1.googledomains.com.
bilt.com.		21600	IN	NS	ns-cloud-d3.googledomains.com.

```

```bash
dig ${domain} -x IP

Domain: bilt.com  
IP: 34.102.215.178  
Reverse DNS: none (NXDOMAIN)  
Infrastructure: Google cloud / CDN
```


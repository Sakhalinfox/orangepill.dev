---
title:  "Guide to create Lightning Address redirection on your domain"
date:   2023-01-10T19:53:00-05:00
categories: 
  - lightning-guides
tags:
  - Lightning
  - LightningAddress
  - LNURL
  - NIP-05
  - LNAddress
toc: true
---

# Understanding Lightning Addresses
Lightning Address is a reusable identifier that looks like an email address and can be used to send and receive payments over the Lightning Network. It is easy to remember unlike an LNURL. Some Lightning wallets and services provide this feature where they may either let you choose an identifier on their domain or will randomly generate one for you. For example: Wallet of Satoshi is a custodial Lightning wallet that auto generates one for you such as `<randomidentifier>@walletofsatoshi.com`.  

>**Note**: This is this is not an email address and cannot be used to send or receive emails, unless you also setup an email server and create an email address with that same identifier and domain. 

To understand how Lightning Addresses are constructed have a look at the below image:
![Lightning Address illustration](https://camo.githubusercontent.com/268abc621585b68fbf1229eab51c3c9344870ec3f227a1ff237c7423ba3ba28e/68747470733a2f2f692e696d6775722e636f6d2f444956357138712e706e67)*Source: [Lightning Address GitHub readme](https://github.com/andrerfneves/lightning-address/blob/master/README.md)*

Based on the above pictorial illustration let us try to breakdown a Lightning Address with an actual example. My Lightning Address is `scarredsofa23@walletofsatoshi.com` which decodes into the following URL https://walletofsatoshi.com/.well-known/lnurlp/scarredsofa23. If you visit that URL you will see a LNURL Pay request and response in JSON format. :
```
{
	"callback":"https://livingroomofsatoshi.com/api/v1/lnurl/payreq/c8076587-f985-4b20-902f-585e629c0de2",
	"maxSendable":100000000000,
	"minSendable":1000,
	"metadata":"[[\"text/plain\",\"Pay to Wallet of Satoshi user: scarredsofa23\"],[\"text/identifier\",\"scarredsofa23@walletofsatoshi.com\"]]",
	"commentAllowed":32,
	"tag":"payRequest"
}
```

Lightning wallets that can decode or support it can send or receive payments by converting the Lightning Address based on the above response of the LNURL Pay request and will facilitate the payment or receipt of the BOLT11 Invoice. 
It is likely that the Lightning Address service provider running the LNURLp endpoint will have it running on the following URL:
> `domain.com/.well-known/lnurlp/[identifier]`

**Note**: I have used https://lightningdecoder.com/ to decode the Lightning Address.

# How to setup Lightning Address redirection
If you own a domain.com and a web server and would like to use an identifier on your domain, but would like to receive payments or to allow others to send payments to you using `<youridentifier>@yourdomain.com` then all you need to do is to setup a simple redirection on your web server.

**Note**: To keep this short, I will assume that your domain's or subdomain's DNS is already pointing to your web server using either an A record, AAAA record or CNAME and can access your web server.

## Setup on NGINX
Open `/etc/nginx/sites-enabled/<yourdomain.com>` with a text editor like nano or vim from your console that you use to SSH into your server. Replace `<yourdomain.com>` with your `domain.com`.

**Step 1:** Edit the below file
 >$ nano /etc/nginx/sites-enabled/<yourdomain.com>
```
server {
	location /.well-known/lnurlp {
		rewrite ^/.well-known/lnurlp/<youridentifier>$ https://walletofsatoshi.com/.well-known/lnurlp/<youractualwalletidentifier> redirect;
        }	
}
```

`rewrite`: Used to tell NGINX to match the URL pattern eg: `/.well-known/lnurlp/[identifier]` to another URL.

`^/.well-known/lnurlp/<youridentifier>`: Redirect from URL.

`$`: End of string.

`https://walletofsatoshi.com/.well-known/lnurlp/<youractualwalletidentifier>`: Redirect to URL. Replace `walletofsatoshi.com` with any other Lightning Address service providers URL.

This should also update the domain file in `/etc/nginx/sites-available/yourdomain.com` because of the symbolic link. 

Example config in my domain: `/etc/nginx/sites-enabled/orangepill.dev`:
```
server {
	location /.well-known/lnurlp {
		rewrite ^/.well-known/lnurlp/ezofox$ https://walletofsatoshi.com/.well-known/lnurlp/scarredsofa23 redirect;
        }	
}
```
As per the above example I can now share ezofox@orangepill.dev as my Lightning Address and this will forward or redirect payments to my original Lightning Address `scarredsofa23@walletofsatoshi.com` wallet.

**Step 2:** Reload or restart NGINX service
>$ service nginx reload  

or
>$ service nginx restart

## Setup on Apache
Redirection on Apache can be similarly achieved using the Redirect Directive. Depending on OS or your configuration you may have to edit with your text editor nano, vim or others one of these files  `/etc/apache2/sites-available/000-default.conf ` or `/etc/apache2/sites-available/example.com.conf` (In Ubuntu) or `/etc/httpd/conf.d/vhost.conf` (In CentOS)

*Source: https://www.linode.com/docs/guides/redirect-urls-with-the-apache-web-server/*
```
<VirtualHost *:80>
	ServerName www.yourdomain.com
	Redirect permanent /.well-known/lnurlp/<youridentifier> https://walletofsatoshi.com/.well-known/lnurlp/<youractualwalletidentifier>
</VirtualHost>
```
You can also use HTTP status codes for redirection like:
`Redirect 301 /.well-known/lnurlp/<youridentifier> https://walletofsatoshi.com/.well-known/lnurlp/<youractualwalletidentifier>`
This is also a permanent redirection.

You can also accomplish this by editing `.htaccess` and adding the necessary Redirect Directive as above. 

For any other web servers, you may need to read the respective documentation of those web server implementations to understand how to perform a similar permanent redirection.

If you found this post useful feel free to leave a Lightning tip at:

**Lightning Address**: ezofox@orangepill.dev

**LNURL**: ![Tipjar](https://raw.githubusercontent.com/Sakhalinfox/orangepill.dev/main/Tiplnurl.png)


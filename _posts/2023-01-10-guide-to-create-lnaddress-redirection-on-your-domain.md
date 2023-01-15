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

# How to setup Lightning Address redirection with your domain using GitHub pages to redirect to Wallet of Saotshi or GetAlby Lightning Address (Free and Easy) 

This section of the guide is for those who own a domain or a subdomain and would like to make use of GitHub pages for setting up Lightning Address redirection.

1. Point your domain or subdomain's DNS to GitHub pages by setting up the below A records on your domain providers portal:

	```
	185.199.108.153
	185.199.109.153
	185.199.110.153
	185.199.111.153
	```

	Set the default or lowest possible TTL (Time To Live) value that your domain provider allows. This just tells the DNS resolver how long to cache a DNS query before updating.

	Depending on your domain provier you may have to enter @ or leave blank the host value to use the root domain in your NIP-05 identifier. If you want to use a subdomain enter the subdomain name in that field.

	>**Note**: If GitHub updates the above IP addresses please refer to their [page](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site). Once you update your domain's DNS it may take some time to propagate, anywhere from a few minutes to several hours.

2. Register and Login to your GitHub account and visit [here](https://github.com/new) to create a new repository. Give your repository a name and make sure to select ‘Public’ repository if you want the GitHub Pages setup to be free. Using a ‘Private’ repository with GitHub pages is a paid feature and requires an upgrade.

	>**Note**: If want to use an existing repository that already points to your custom domain or you want to use GitHubs free subdomain skip to **step 3**.

3. Create a new file in your repository in the following directory path: `<repository>/.well-known/lnurlp/<yourusername>`

	**Note**: `<yourusername>` will be part of your Lightning Address `<yourusername>@yourdomain.com`. Replace it with what you want.

	**Redirect to [Wallet Of Satoshi(WOS)](https://walletofsatoshi.com/.) Lightning address**:  Add the below content to your `<yourusername>` file. Here's an [example](https://github.com/Sakhalinfox/woslnaddredirect/blob/main/.well-known/lnurlp/ezofox). To view your Wallet Of Satoshi content to paste into this file, visit this URL `https://walletofsatoshi.com/.well-known/lnurlp/<your-WOS-user>` and view the JSON raw data. Copy it and paste it into `<yourusername>` file:

	>**Note**: Your WOS User is usually in your Lightning Address: `<your-WOS-user>@walletofsatoshi.com` 

	Sample JSON that needs to go into the file: 

	```
	{
		"callback": "https://livingroomofsatoshi.com/api/v1/lnurl/payreq/<your-callback-payreq>",
		"maxSendable": 100000000000,
		"minSendable": 1000,
		"metadata": "[[\"text/plain\",\"Pay to Wallet of Satoshi user: <your-WOS-user>\"],[\"text/identifier\",\"<your-WOS-user>@walletofsatoshi.com\"]]",
		"commentAllowed": 32,
		"tag": "payRequest"
	}
	```
	**Redirect to [Getalby](https://getalby.com/) Lightning address**:  Add the below content to your `<yourusername>` file. To view your Getalby content to paste into this file, visit this URL `https://getalby.com/.well-known/lnurlp/<your-getalby-user>` and view the JSON raw data. Copy it and paste it into `<yourusername>` file. For example: [My getalby](https://getalby.com/.well-known/lnurlp/ezofox):

	>**Note**: Your Getlalby User is in your Lightning Address:`<your-getlaby-user>@getalby.com`. 

	Sample JSON that needs to go into the file: 

	```
	{
		"status": "OK",
		"tag": "payRequest",
		"commentAllowed": 255,
		"callback": "https://getalby.com/lnurlp/<your-getlaby-user>/callback",
		"metadata": "[[\"text/identifier\", \"<your-getlaby-user>@getalby.com\"], [\"text/plain\", \"Sats for <your-getlaby-user>\"]]",
		"minSendable": 1000,
		"maxSendable": 11000000000,
		"payerData": {
			"name": {
			"mandatory": false
			},
			"email": {
			"mandatory": false
			}
		}
	}
	```
	

4. Go to the ‘Settings’ page of your repository and navigate to ‘Pages’ under Code and automation section on the sub menu or modify the below URL to visit it directly: `https://github.com/<your-github-username>/<repository>/settings/pages`

	>**Note**: Replace `<your-github-username>` with your GitHub username and `<repository>` with the repository name.
	
	Under Build and deployment select source as ‘Deploy from a branch’. Select branch as ‘main’ or ‘master’, ‘/root’ and click save.

	Under Custom domain enter your domain and click save. GitHub will perform a DNS check to verify that the A records setup on your domain in **step 1** are accurate.

	>**Note**: It this fails, wait for your domain’s DNS to complete propagation and retry. DNS propagation could take anywhere from a few minutes to hours.

	Check ‘Enforce HTTPS’. This will generate an SSL certificate for your Domain from GitHub for free and assign it.

	>**Note**: At times, this may take a while to generate.

5. Create a new file _config.yml or update it to add the below lines:

	```
	include:
	- .well-known
	```
6. Try sending some Bitcoin or Sats to `<yourusername>@yourdomain.com` to test. If it works you are all done. If not, verify the above steps are completed.


# How to setup Lightning Address redirection using your web server
If you own a domain.com and a web server and would like to use an identifier on your domain, but would like to receive payments or to allow others to send payments to you using `<youridentifier>@yourdomain.com` then all you need to do is to setup a simple redirection on your web server.

**Note**: To keep this short, I will assume that your domain's or subdomain's DNS is already pointing to your web server using either an A record, AAAA record or CNAME and can access your web server.

## Setup on NGINX
Open `/etc/nginx/sites-enabled/<yourdomain.com>` with a text editor like nano or vim from your console that you use to SSH into your server. Replace `<yourdomain.com>` with your `domain.com`.

1.  Edit the below file using a text editor:

	> $ nano /etc/nginx/sites-enabled/<yourdomain.com>
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

2. Reload or restart NGINX service:
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


If you found this guide useful feel free to leave a Lightning tip at:

**Lightning Address**: ezofox@orangepill.dev

**LNURL**: ![Tipjar](https://raw.githubusercontent.com/Sakhalinfox/orangepill.dev/main/Tiplnurl.png)


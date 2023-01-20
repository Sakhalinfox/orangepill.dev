---
title:  "Guide to verify your Nostr profile with a NIP-05 identifier on your domain"
date:   2023-01-14T03:10:00-05:00
categories: 
  - nostr-guides
tags:
  - NIP-05
  - NIP-05 Identifier
  - NIPS
toc: true
---

# What is a NIP-05 Identifier?

[NIPS or **N**ostr **I**mplementation **P**ossibilities](https://github.com/nostr-protocol/nips) are a list of Nostr protocol documents which provides the guidelines for creating a Nostr client or relay implementation. 

NIP-05 is one such protocol document that describes mapping of your Nostr public Key with an internet identifier that looks like an email address and is used to verify your Nostr social profile and public key on Nostr clients.

Some of its benefits include:
- Quickens finding your Nostr profile.
- Quickens mentioning or tagging users on social clients.
- Directory lookup of other users on a particular NIP-05 identifier domain.
- Adds a level of trust by verifying your Nostr profile or pubkey with a domain or server you own, or via a trusted third-party NIP-05 identification service provider.
- Could be used to fight spam on Nostr by whitelisting trusted NIP-05 domains on a private relay.
- Most of all it looks cool on your social profile!

# Setup NIP-05 identifier on your domain

Buy a domain, use a free domain, use a subdomain, or use a GitHub Pages free subdomain and follow the steps below.

## Setting up with GitHub pages (Easy and free)

GitHub pages lets you build static websites and link your custom domain. If you intend to do so, this method will let you build a website and verify your NIP-05 identifier as well.

1. If you bought a domain or registered a free domain, login to your domain providers portal and point it to GitHub Pages by creating the following A Records:

    ```
    185.199.108.153
    185.199.109.153
    185.199.110.153
    185.199.111.153
    ```
    Set the default or lowest possible TTL (Time To Live) value that your domain provider allows. This just tells the DNS resolver how long to cache a DNS query before updating.

    Depending on your domain provier you may have to enter `@` or leave blank the host value to use the root domain in your NIP-05 identifier. If you want to use a subdomain enter the subdomain name in that field.

    >**Note**: If GitHub updates the above IP addresses please refer to their [page](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site).
    {: .notice--info}

2. Register and Login to your GitHub account and visit [here](https://github.com/new) to create a new repository. Make sure to select 'Public' repository if you want the GitHub Pages setup to be free. Using a 'Private' repository with GitHub pages is a paid feature and requires an upgrade.

    >**Note**: If want to use an existing repository that already points to your custom domain or you want to use GitHubs free subdomain skip to **step 4**.
    {: .notice--info}

3. Go to the 'Settings' page of your repository and navigate to 'Pages' under Code and automation section on the sub menu or modify the below URL to visit it directly: 
`https://github.com/<yourusername>/<repository>/settings/pages`

    >**Note**: Replace `<yourusername>` with your GitHub username and `<repository>` with the repository name.
    {: .notice--info}

    Under Build and deployment select source as 'Deploy from a branch'. Select branch as 'main' or 'master', '/root' and click save.

    Under Custom domain enter your domain and click save. GitHub will perform a DNS check to verify that the A records setup on your domain in step 1 are accurate.
    
    > **Note**: It this fails, wait for your domain's DNS to complete propagation and retry. DNS propagation could take anywhere from a few minutes to hours.
    {: .notice--info}

    Check 'Enforce HTTPS'. This will generate an SSL certificate for your Domain from GitHub for free and assign it.

    >**Note**: At times, this may take a while to generate.
    {: .notice--info}

4. Navigate to your repository's Code tab `https://github.com/<yourusername>/<repository>` and click on the button Add File > Create new file.

    On the 'Name your file...' input file type `/.well-known/nostr.json`. This means it will create the folder .well-known and a new file nostr.json in your repository.
    ```
    {
      "names": {
        "<your-nostr-username>": "<hex-public-key>"
      }
    }
    ```
    >**Note**: Replace `<your-nostr-username>` with your Nostr social username and `<hex-public-key>`. To convert npub to hex public key use [damus.io/key](https://damus.io/key) or [key-convrtr](https://github.com/rot13maxi/key-convertr) tool. Do not paste your private keys into these tools.
    {: .notice--info}

    Create a new file `_config.yml` or update it to add the below line:

    ```
    include:
      - .well-known
    ```
5. Open your Nostr social client like [Damus](https://damus.io/), [astra.ninja](https://astral.ninja), [snort.social](https://snort.social) and update your NIP-05 identifier as `<your-nostr-username>@<yourdomain/subdomain>.com`. 

    If you used GitHub Page's free subdomain it should be: `<your-nostr-username>@<yourgithubusername.github.io>`.

## Setting up with your web server (Semi-Advanced)

If you self-host, own a webserver, VPS or Cloud server  this guide should provide the setup details. This guide assumes that you already have NGINX or Apache installed and setup for your operating system:

### Setup with NGINX

1. Point your Domain DNS A record (IPv4) or AAAA record (IPv6) to your server's IP address.

    Ensure that both @.yourdomain.com (@ = root domain) and www.yourdomain.com are both setup to point to the same A record or AAAA record.

2. SSH to your server and navgiate to `/var/www/<yourdomain.com>` or any other path that you have configured for your domain's webserver file contents. 

    Create a new folder `.well-known` and a file `nostr.json` within it - `/var/www/<yourdomain.com/.well-known/nostr.json`. Edit the file with a text editor like vim or nano on Linux or any other text editors on other Operating Systems. Add the following contents and save it:

        
        {
          "names": {
            "<your-nostr-username>": "<hex-public-key>"
          }
        }
        

      >**Note**: Replace `<your-nostr-username>` with your Nostr social username and `<hex-public-key>`. To convert npub to hex public key use [damus.io/key](https://damus.io/key) or [key-convrtr](https://github.com/rot13maxi/key-convertr) tool. Do not paste your private keys into these tools.
      {: .notice--info}

3. Verify your NGINX configuration file exists for your domain in the following path: `/etc/nginx/sites-available/yourdomain.com` and generate an SSL certificate.

    To generate a free SSL certificate for your domain use [Certbot](https://certbot.eff.org/instructions). Visit that link and select 'Nginx' and your Operating System for installation instructions. Generating an SSL certificate for your domain is necessary because some Nostr clients may validate your NIP-05 identifier nostr.json file via a HTTPS only connection. 

      > **Note**: Ensure that your NGINX web server is running and accessible on the internet by browsing using the IP address of your web server. You must configure and open ports 80 and 443 on your firewalls or router as necessary or else certbot will not be able to connect to your server to generate the SSL certificate. Also, ensure both `yourdomain.com` and `www.yourdomain.com` are accessible. There is a 5 attempt per hour limit. So to save time, verify all of these.
      {: .notice--info}
    
    Run the command (on Linux as sudo):
    ```
    certbot --nginx -d yourdomain.com -d www.yourdomain.com
    ```
    Once completed, you will see a successful output message and your `/etc/nginx/sites-available/yourdomain.com` file should be automatically updated to listen to port 443 and the ssl_certificate details. 

4. Enabling CORS 'GET' method in Nginx can be done by editing the nginx configuration file or by editing just the domain's configuration file. In this guide we will edit the domain's file. Open the file `/etc/nginx/sites-available/yourdomain.com` with your text editor, add the following lines between the server tags and save it:

    ```
    server {

      location /.well-known {
        if ($request_method = 'GET') {
          add_header 'Access-Control-Allow-Origin' '*';
      }

    }
    ```
    This will add the CORS permission for the 'GET' HTTP method which will enable Nostr clients to run a GET method to fetch the Nostr.json file contents in the response.

    Run the following to test nginx configuration for errors:

    ```
    sudo nginx -t
    ```
    Restart or reload NGINX:
    ```
    sudo systemctl reload nginx
    OR
    sudo systemctl restart nginx
    ```
5. Test your CORS setup with [test-cors.org](https://www.test-cors.org/). Input the entire nostr.json URL into the Remote URL field, choose the HTTP method as 'GET' and click send request button. This should output as `200 OK`.

6. Open your Nostr social client like [Damus](https://damus.io/), [astra.ninja](https://astral.ninja), [snort.social](https://snort.social) and update your NIP-05 identifier as `<your-nostr-username>@<yourdomain/subdomain>.com`. 

      In [snort.social](https://snort.social) you should see a 'Green check' next to your NIP-05 Identifier indicating that it was successfully setup and accessible with the above defined CORS permission. 

### Setup with Apache

1. Point your Domain DNS A record (IPv4) or AAAA record (IPv6) to your server's IP address.

    Ensure that both @.yourdomain.com (@ = root domain) and www.yourdomain.com are both setup to point to the same A record or AAAA record.

2. SSH to your server and navgiate to `/var/www/<yourdomain.com>/public_html` or any other path that you have configured for your domain's webserver file contents. 

    Create a new folder `.well-known` and a file `nostr.json` within it - `/var/www/<yourdomain.com>/public_html/.well-known/nostr.json`. Edit the file with a text editor like vim or nano on Linux or any other text editors on other Operating Systems. Add the following contents and save it:

        
        {
          "names": {
            "<your-nostr-username>": "<hex-public-key>"
          }
        }
        

      >**Note**: Replace `<your-nostr-username>` with your Nostr social username and `<hex-public-key>`. To convert npub to hex public key use [damus.io/key](https://damus.io/key) or [key-convrtr](https://github.com/rot13maxi/key-convertr) tool. Do not paste your private keys in these tools.
      {: .notice--info}

3. Verify your Apache configuration file exists for your domain in the following path: `/etc/apache2/sites-available/yourdomain.conf` and generate an SSL certificate.

    To generate a free SSL certificate for your domain use [Certbot](https://certbot.eff.org/instructions). Visit that link and select 'Apache' and your Operating System for installation instructions. Generating an SSL certificate for your domain is necessary because some Nostr clients may validate your NIP-05 identifier nostr.json file via a HTTPS only connection. 

      > **Note**: Ensure that your Apache web server is running and accessible on the internet by browsing using the IP address of your web server. You must configure and open ports 80 and 443 on your firewalls or router as necessary or else certbot will not be able to connect to your server to generate the SSL certificate. Also, ensure both `yourdomain.com` and `www.yourdomain.com` are accessible. There is a 5 attempt per hour limit. So to save time, verify all of these.
      {: .notice--info}
    
    Run the command (on Linux as sudo):
    ```
    certbot --apache -d yourdomain.com -d www.yourdomain.com
    ```
    Once completed, you will see a successful output message and your Apache configuration file should be automatically updated to listen to port 443 and the ssl_certificate details. 

4. We need to allow CORS permissions for origin, headers and GET HTTP method. In Apache this can be done multiple ways:
    - By editing Apache config files directly - /etc/apache2/httpd.conf OR etc/apache2/apache2.conf OR /etc/httpd/httpd.conf OR /etc/httpd/conf/httpd.conf (depeding on how you installed Apache these locations vary)
    - Or by editing .htaccess (We will use this for this guide as it is simpler and only applies to your domain on the webserver)

    1. First we need to install headers module for Apache to enable CORS:

        In Ubuntu/Debian run in console to enable headers module:
        ```
        sudo a2enmod headers
        ```
        In CentOS/Redhat/Fedora edit httpd.conf Apache configuration file to uncomment the following line and save:
        ```
        LoadModule headers_module modules/mod_headers.so
        ```
    2. Open .htaccess file located in `/var/www/<yourdomain.com>/public_html/.htaccess`, add the lines and save file:
        ```
        <If "%{REQUEST_URI} =~ m#^/.well-known/#">
        Header always set Access-Control-Allow-Origin "*"
        Header always set Access-Control-Allow-Headers "*" 
        Header always set Access-Control-Allow-Methods "GET" 
        </If> 
        
        ```
    3. Test Apache Configuration:
        ```
        sudo apachectl -t
        ```
    4. Restart Apache web server:
        ```
        sudo systemctl restart apache2
        ```

5. Test your CORS setup with [test-cors.org](https://www.test-cors.org/). Input the entire nostr.json URL into the Remote URL field, choose the HTTP method as 'GET' and click send request button. This should output as `200 OK`.

6. Open your Nostr social client like [Damus](https://damus.io/), [astra.ninja](https://astral.ninja), [snort.social](https://snort.social) and update your NIP-05 identifier as `<your-nostr-username>@<yourdomain/subdomain>.com`. 

      In [snort.social](https://snort.social) you should see a 'Green check' next to your NIP-05 Identifier indicating that it was successfully setup and accessible with the above defined CORS permission.


If you found this guide useful feel free to leave a Lightning tip at:

**Lightning Address**: ezofox@orangepill.dev, OR
**LNURL**: ![Tipjar](https://raw.githubusercontent.com/Sakhalinfox/orangepill.dev/main/Tiplnurl.png)
{: .notice--success}




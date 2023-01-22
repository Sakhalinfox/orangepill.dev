---
title:  "Guide to generate and manage Nostr keys and sign events"
date:   2023-01-22T03:10:00-05:00
categories: 
  - nostr-guides
tags:
  - Public key
  - Private key
  - Nostr
  - Key
  - Key management
  - NIP 19
toc: true
ios_orion_images:
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_1.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_1.jpeg
    alt: "iOS Orion browser setup step 1"
    title: "iOS Orion browser setup step 1"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_2.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_2.jpeg
    alt: "iOS Orion browser setup step 2"
    title: "iOS Orion browser setup step 2"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_3.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_3.jpeg
    alt: "iOS Orion browser setup step 3"
    title: "iOS Orion browser setup step 3"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_4.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_4.jpeg
    alt: "iOS Orion browser setup step 4"
    title: "iOS Orion browser setup step 4"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_5.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_5.jpeg
    alt: "iOS Orion browser setup step 5"
    title: "iOS Orion browser setup step 5"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_6.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_6.jpeg
    alt: "iOS Orion browser setup step 6"
    title: "iOS Orion browser setup step 6"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_7.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_7.jpeg
    alt: "iOS Orion browser setup step 7"
    title: "iOS Orion browser setup step 7"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_8.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_8.jpeg
    alt: "iOS Orion browser setup step 8"
    title: "iOS Orion browser setup step 8"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_9.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_9.jpeg
    alt: "iOS Orion browser setup step 9"
    title: "iOS Orion browser setup step 9"
ios_nostore_images:
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_nostore1.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_nostore1.jpeg
    alt: "iOS Nostore setup step 1"
    title: "iOS Nostore setup step 1"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_nostore2.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_nostore2.jpeg
    alt: "iOS Nostore setup step 2"
    title: "iOS Nostore setup step 2"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_nostore3.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_nostore3.jpeg
    alt: "iOS Nostore setup step 3"
    title: "iOS Nostore setup step 3"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_nostore4.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_nostore4.jpeg
    alt: "iOS Nostore setup step 4"
    title: "iOS Nostore setup step 4"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_nostore5.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_nostore5.jpeg
    alt: "iOS Nostore setupsetup step 5"
    title: "iOS Nostore setup setup step 5"
  - url: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_nostore6.jpeg
    image_path: /assets/images/2023-01-20-guide-nostr-key-generation-and-management/ios_nostore6.jpeg
    alt: "iOS Nostore setup setup step 6"
    title: "iOS Nostore setup setup step 6"
---

# Understanding Nostr keys

Nostr keypair consists of a public and private key. The public key is your identifier and can be used by clients to find and display your published content. The private key is used to sign events and clients verify these signatures. 

**For example**: In a Nostr social media client like [astral.ninja](https://astral.ninja) you can create a new post or a note, and it gets signed with your private key. Once published, your post should be accessible with your public key, your social profile identifier.

There are two formats for the keypair as mentioned in [NIP-19](https://github.com/nostr-protocol/nips/blob/master/19.md):

- Bech32 encoded public key `npub....` and private key `nsec....`
- 32-bytes hex encoded public or private key

>**Important**: It is necessary to safeguard your private key from loss, theft and prying eyes. Generate, store and input it securely. Do not paste your private key into any web clients.
{: .notice--danger}

With some tools it is also possible to generate Proof of Work (POW) public keys (mined public key) with certain accepted characters. This can be used to generate a vanity public key with your name, as long as your name as the accepted chracters, or to generate a public key with leading zeros indicating that some POW is performed to generate it. 

>**Note**: One spam reduction measure would be for Nostr relays to whitelist POW generated public keys with a minimum POW bits. This could indicate that with an average computer it could take few minutes to several hours or days to generate depending on the length of the vanity name or the leading zeros (POW bits). This may or may not eliminate spam.
{: .notice--info}

# Tools to generate Nostr keypair offline

The first step is to securely generate the keypair offline, not through web clients. Genering the key offline ensures that it does not get leaked, lost or stored online. Although web clients must not store your private keys remotely, this can never be guaranteed. A malicious web client can steal your private keys. Also, security vulnerabilities in web clients can leak your private keys if you have inputted it directly into the web client.

Use an offline or airgapped device while using the below tools to generate your keypair. Do not use any of the private keys generated in this guide as everyone has access to it.

>**Warning**: For privacy and security reasons it is recommended to output the keypair, especially the private key, to a text file from the console. This is because the console maintains a history of all your commands and can store the private key in the console history file for your OS.
{: .notice--warning}

1. Using openssl command to generate a 32 byte hex private key offline:

    - Install [openssl](https://blog.devolutions.net/2020/09/tutorial-manually-installing-openssl-on-windows-linux-macos/) for your OS

    - Run the following command in your console:

      ```
      openssl rand -hex 32 > hexprivatekey.txt

      Example output to text file: b86bdbc13deb52eb1172e7b2c9821b362r83973efc37cf2ee1cd56ceabdafda8b
      ```

    You can then use a tool like [key-convertr](https://github.com/rot13maxi/key-convertr) to convert the hex private key into bech32 encoded private key `nsec....`, if needed. You can clone the [repository](https://github.com/rot13maxi/key-convertr.git), build it using [cargo](https://doc.rust-lang.org/cargo/getting-started/installation.html) and run it. The simpler alternative is to run it using docker:

    - Install [docker](https://dockerwebdev.com/tutorials/install-docker/) for your OS
    
    - Run the following command:

      ```
      docker run --rm ghcr.io/rot13maxi/key-convertr:main --kind nsec "$(cat hexprivatefrom the tools you hkey.txt)" > bech32privatekey.txt

      Example output to text file:
      nsec1hp4ahsfaadfwkytju7evnqsmxc5rjul0cd709msu64kw40d0m29s2zx8kf
      ```
    
      >**Note**: Replace hexprivatekey.txt file the text file containing your 32 byte hex private key.
      {: .notice--info}

2. [Python-nostr](https://github.com/jeffthibault/python-nostr) - A python library lets you generate a keypair using a simple script:

    - Install [python3](https://realpython.com/installing-python/) and [pip](https://realpython.com/what-is-pip/#reinstalling-pip-when-errors-occur) for your OS
    - Install python-nostr via pip in console:

      ```
      pip install nostr
      ```
    - Create a new `.py` file `generate.py` and paste the below contents and save the file:
      ```
      from nostr.key import PrivateKey

      private_key = PrivateKey()
      public_key = private_key.public_key
      print(f"32 Byte hex Private key: {private_key.hex()}")
      print(f"32 Byte hex Public key: {public_key.hex()}")
      print(f"Bech32 Private key: {private_key.bech32()}")
      print(f"Bech32 Public key: {public_key.bech32()}")

      ```
      *Source: Modified from python-nostr GitHub [README](https://github.com/jeffthibault/python-nostr/blob/main/README.md)*

    - Run the python script offline. This should generate and save the Bech32 encoded keypair in the text file `keypair.txt` within the directory that it is run in:
      ```
        python generate.py > keypair.txt
        
        OR

        python3 generate.py > keypair.txt
      ```

3. [Nostril](https://github.com/jb55/nostril) A command line utility to generate muliple nostr events. It also provides a simple 

    - Install gcc or g++ for your OS
    - Clone the repository and run `cd` to change directory to the cloned folder:

      ```
      git clone https://github.com/jb55/nostril

      cd nostril
      ```
    - Run the following command to install (may require sudo if writing to /usr/local/share on Linux)
      ```
      make install
      ```
    - Execute command to mine a Proof of Work (POW) :
      ```
      nostril --mine-pubkey --pow <difficulty> > output.txt
      ```
      >**Note**: Replace `<difficulty>` with an integer value. Entering 0 would immediately generate a 32 byte hex privatekey (secret_key) and publick key (pubkey)
      {: .notice--info}
      ```
      Example input:
      nostril --mine-pubkey --pow 0 > output.txt

      Example output to output.txt file:
      mined pubkey with 0 bits after 1 attempts, 0 ms, inf attempts per ms
      secret_key b707d88d1c0db3edeb7a48a9d7eca5f2b4a23a2470d046dbf39d2bb177ee4fa1

      {"id": "9204c8307d5be41da198d2f41a41e14295b674b49f00f15a629186fc8764200c","pubkey": "b2936d24268d18fa8c3f0204198b4e53fafacb7f759da4776d5ad3199cedeb61","created_at": 1674282042,"kind": 1,"tags": [],"content": "","sig": "9d9f56621a54af237614c9208e19f81ab98239ccd3148874fe54feaf9db51de9d485a450dfe2b11ad6158fdd7d0953059fe4fdcb374d6b17127d7313927aa2a4"}
      ```
4. Generate a Nostr private key from Bitcoin [BIP-85](https://bip85.com/) deterministic entropy derived seed. BIP-85 can derive multiple child seed phrases from one master seed phrase. Its benefits are:

  - Each derived child seed is unique using a one-way mathematical function.
  - The derived seeds are not tracable or cannot be reverse engineered to derive the master seed or other child seeds.
  - Easy to backup just the master seed and regenerate at any time any of the child seeds. This allows to maintain just one backup of the master seed and derive multiple child seeds at any time.
  - If any derived child seed is compromised, the other child seeds and the master seed are safe.
  - If you have a ColdCard you can generate BIP-85 child seeds and convert each of them to an individual Nostr private key.
  - Useful if you want to use multiple Nostr private keys to login to different clients but remember just one backup seed.

    Steps to generate Nostr 32 byte hex private key from BIP-85 child seed:

    - If you own a [ColdCard](https://coldcard.com) hardware wallet read the documentation to [setup BIP-85](https://coldcard.com/docs/bip85) and derive a child entropy seed. Here's a [video](https://www.youtube.com/watch?v=JvbtMQDPjYE) guide

    - Use an offline PC with a web browser. Open the browser console:
      ```
      Firefox shortcut (ctrl+shift+k)
      Chrome shortcut (ctrl+shift+j)
      ```
    - Paste the following code:
      ```
      [...new Uint8Array(await crypto.subtle.digest('SHA-256', new TextEncoder().encode('<your-seed-phrase-goes-here>')))].map((b) => b.toString(16).padStart(2, '0')).join('')
      ```

      *source: [brutusbond repository](https://brutusbond.nohost.me/gitea/brutus/generate_nostr_keys_with_bitcoin_wallet)*

      >**Note**: Replace `<your-seed-phrase-goes-here>` with your BIP-85 derived 12 seed words. Do not use the derived entropy child seed phrase for creating a Bitcoin Wallet.
      {: .notice--info}

    - You can then utilize the key-convertr tool and the guide from step 1 to convert it to bech32 nsec private key, if needed.

Other tools to generate offline keys:
 - [nostr-tools](https://github.com/nbd-wtf/nostr-tools)
 - [glasnostr](https://github.com/eyelight/glasnostr) - Vanity key generator
 - [nostr.rest](https://www.nostr.rest/)- Browser based, but run it offline if you want to use it
 - [nostr-pubminer](https://github.com/lacaulac/nostr-pubminer)

# Tools and clients to manage Nostr keys and authorize signing of events

## Nostr signer browser extensions
With the above tools we should now have a securely generated Nostr private key. For logging into web clients, currently there are Nostr signer browser extensions into which we will need to input the private key. They have the following benefits and limitations:

Benefits:
  
- <ins>Local browser storage of private keys</ins>: Your private key is only entered into the Nostr signer browser extension and remains configured in there and is never shared with web clients. This eliminates the need to paste your private key directly into multiple Nostr web clients and safely store them locally.
- <ins>Asks you for authorization to sign events</ins>: Nostr signer extensions will prompt you to authorize event action such as logging into the web client, publishing posts or events, replying to posts or events and sending encrypted messages. You will have options to authorize just for that action, authorize for a specific amount of time, authorize forever or reject authorization.

  These tools make use of `window.nostr` capability of the web browsers as mentioned in [NIP-07](https://github.com/nostr-protocol/nips/blob/master/07.md) to return the following without sharing your private key with the Nostr client:

  - Returns your puiblic key after you authorize.
  - Returns `id`, `public key` and `signature` for signing an event. For example: When you make a post on Nostr social clients, they will need to be signed by your private key usign the extension, and only those three values are returned to the client.
  - Returns Relay URLs
  - Returns ciphertext base64-encoded initialization vector `iv` as per [NIP-04](https://github.com/nostr-protocol/nips/blob/master/04.md) for encrypted direct messages

- <ins>Ability to store multiple profiles or keys</ins>: Nostr signer extensions will have the ability to store multiple profiles or private keys. You will be able to switch between, sign events or login to web clients with different keys.
- Can be installed on desktop browsers or mobile browsers (iOS and Android)
- Handle `nostr:` links and setup preferred relays to be used uniformly across clients.
- Generate Nostr private keys offline

Limitations:

- <ins>Can require excessive extension permissions</ins>: Most Nostr signer brwoser extensions currently require access to data from all sites you visit. Depending on your privacy setup, you may want to use a separate browser or a new browser profile for everything Nostr related with no other extensions installed.
- <ins>Requires trusting extension to not reveal or leak your stored private keys</ins>: Security vulnerabilities in software, extensions or your operating system could leak your private keys. You will need to maintian good security practices for your desktop or mobile device while using these extension on your devices like keeping your OS up to date, updating extensions to patch any known vulnerabilities and securing your browser access from others accessing it to view your private key from the extension.
- <ins>No airgapped signing of events</ins>: Invalidates the purpose of generating your private keys offline, if you need to paste them into a browser extension in an online environment. 

>**Note**: It is still early days and developments in the Nostr community and improvements in key management and derivation will be implemented gradaully. Hardware wallets or signing devices might add [support](https://twitter.com/COLDCARDwallet/status/1608886079159160834) for exporting keys, signing events via NFC, USB connection or a signature file. 
{: .notice--info}

### Nos2x, Nos2x-fox and Alby Nostr signer extensions

The [Nos2x](https://github.com/fiatjaf/nos2x) open-source extension is available for [Google Chrome and Chromium browsers](https://chrome.google.com/webstore/detail/nos2x/kpgefcfmnafjgpblomihpgmejjdanjjp). [Nos2x-fox](https://diegogurpegui.com/nos2x-fox/) is an open-source maintained extension forked from Nos2x, but for [Firefox](https://addons.mozilla.org/en-US/firefox/addon/nos2x-fox/).

Benefits of using Nos2x and Nos2x-fox:

- Lets you login to Nostr clients without sharing your private key. 
- Ability to generates new private keys.
- Displays the associated public key.
- Nos2x - Can add default preferred relays list. Not available yet in Nos2x-fox.
- Nos2x - handles `nostr:` links.

Limitations of using Nos2x and Nos2x-fox:

- No unlock password to prevent prying eyes from opening your browser and viewing the extension's options page to view your stored private key.
- Ability to revoke previously granted authorizations for Nostr web clients.


[Getalby](https://github.com/getAlby/lightning-browser-extension) is another open-source extension and a Bitcoin Lightning Wallet which also does signing of Nostr events. It is available for both [Firefox](https://addons.mozilla.org/en-US/firefox/addon/alby/) and [Chrome](https://chrome.google.com/webstore/detail/alby-bitcoin-lightning-wa/iokeahhehimjnekafflcihljlcjccdbe) browsers. 

Benefits of using Alby:

 - Lets you login to Nostr clients without sharing your private key.
 - It lets you setup an unlock password to protect your stored private key from prying eyes.
 - It encrypts the generated private key.
 - If you forget your unlock password you can reset it, relogin to your Alby account and regenerate your private key. It should generate the same private key as it did before. The Nostr key is associated with your account or node and you do not have to worry about losing your key. Read more [here](https://guides.getalby.com/overall-guide/alby-browser-extension/features/nostr).
 - Ability to revoke previous authorizations for Nostr web clients.

Limitation of using Alby:

- The Nostr key associated with the node or account is good but if it is compromised, it would be nice to generate new keys and associate the new key with the node or account. 
- Does not currently display the public key.



>**Note**: These extensions will prompt for you to allow access to all website data. This may be because the extension will need to sign your events on Nostr web client websites. If privacy and security are your concerns, use a separate browser profile or browser for your Nostr activities.
{: .notice--info}

#### Steps to setup and use on desktop

1. Install any of the extensions from the links above for your browser.
2. Visit the options page or preferences page to input your private key. This is the only place where you will need to input your Nostr private key and is a one-time setup.

    - In Firefox go to `Addons and themes` (Ctlr + Shift + A) and clcik on Extensions. Find nos2x-fox, enable it, click on the enabled extensions and go to `Preferences`. Here you can generate a private key offline or you can paste an existing private key that you had generated offline.

      If you installed Alby you will be presented with a `Get Started` page to set an unlock password. The unlock password would be required everytime you use alby to configure your Nostr private key. Set the password, sign up to Alby and login. After logging in, click open Alby's settings, under `Alby lab` there should be a Nostr section. Here you can generate a private key offline or you can paste an existing private key that you had generated offline.

    - In Chrome type in `chrome://extensions/` in the address bar. Find [nos2x](https://addons.mozilla.org/en-US/firefox/addon/nos2x-fox/), enable it, click details, click `Extension options`.  Here you can generate a private key offline or you can paste an existing private key that you had generated offline. Click save.
3. Visit any web client like [astral.ninja](https://astral.ninja), [snort.social](https://snort.social) [hamstr.to](https://hamstr.to), [yosup.app](https://yosup.app) or [iris.to](https://iris.to) and login. You will be prompted to authorize reading of your public key whe as you login. 

    >**Note**: It is recommended to authorize events individually as they happen by clicking `authorize just for this` so that you are aware of what events you are signing.
    {: .notice--info}

#### Steps to setup and use on Android devices

1. Install [Kiwi browser](https://play.google.com/store/apps/details?id=com.kiwibrowser.browser&hl=en_US&gl=US&pli=1) from Play store and install the [nos2x](https://addons.mozilla.org/en-US/firefox/addon/nos2x-fox/) or [Alby](https://chrome.google.com/webstore/detail/alby-bitcoin-lightning-wa/iokeahhehimjnekafflcihljlcjccdbe) chrome extension.
2. Go to browser settings menu, enable the extnesion from the browser's extension page, go to preferences or options and generate or paste your offline generated Nostr private key.

3. Visit any web client like [astral.ninja](https://astral.ninja), [snort.social](https://snort.social) [hamstr.to](https://hamstr.to), [yosup.app](https://yosup.app) or [iris.to](https://iris.to) and login. You will be prompted to authorize reading of your public key whe as you login. 

#### Steps to setup and use on iOS devices

1. Install [Orion browser by Kagi](https://apps.apple.com/us/app/orion-browser-by-kagi/id1484498200) from App Store (It is the only webkit browser on iOS that lets us install both Firefox and Chrome extensions).
2. Launch the browser and click on the settings button represented by three dots "`...`" on the browser's navigation bar and click `Extensions`.
3. On the extensions pop-up click 'Install Firefox Extension', search `Nos2x-Fox` or `Alby` and click add to Orion.
4. Click on browser settings button again "`...`" and you will find the extension installed on the top. Click on the extension to open its settings or options page to input or generate a private key.
5. Open a new tab in the browser and and visit any web client like [astral.ninja](https://astral.ninja), [snort.social](https://snort.social) [hamstr.to](https://hamstr.to), [yosup.app](https://yosup.app) or [iris.to](https://iris.to) and login. You will be prompted to authorize reading of your public key whe as you login. 

{% include gallery id="ios_orion_images" caption="iOS Orion browser Nostr Signer Nos2x-fox setup steps." %}

### Nostore signer Safari extension for iOS or iPad

Nostore is a Safari extension that allows managing multiple profiles consisting with which you can store and manage multiple Nostr private keys. 

>**Warning**: This extension is still in iOS Test Flight and is still in active development or alphe/beta stage. It is recommended to use it only for testing until a stable enough release is out.
{: .notice--warning}

1. Install Nostore from iOS [Test Flight](https://testflight.apple.com/join/ouPWAQAV), open it and click `Start testing`.
2. Launch Safari browser, open a Nostr web client like [snort.social](https://snort.social), click on `AA` button on the address bar, click manage extesions, enable Nostore and click `Done`.
3. Once again click on `AA` button on the address bar, click `Nostore` extension:
    - A new profile will be auto-generated with the private key. You can choose to use it or input your offline generated private key. Configure the profile name as needed and click save.
    - You can also add another profile or private key by clicking the `New` button.
    - Click `Done`.

4. Click `Login` button on [snort.social](https://snort.social) and you should see the option `Login with Extension (NIP-07)`. Click on it and you should be logged.

    >**Note**: Unlike Nos2x, Nos2x-fox or Alby, this extension at the time of writing this post does not prompt for an authorization for each event. It is forever authorized and will publish any events automatically without any prompts.
    {: .notice--info}

{% include gallery id="ios_nostore_images" caption="iOS Nostore Nostr Signer Nos2x-fox setup steps." %}

## Nostr native Social clients (beta/stable'ish)

Nostr clients for iOS, Android or desktop Operating Systems will self manage Nostr keys. The security aspects of key management on native clients depends on how well the clients store and secure private keys on the application, on the users local device and not storing them remotely. 

>**Note**: This guide will only provide a list of recommended Nostr clients and the list will be updated as more beta or stable clients are available
{: .notice--info}

### Nostr iOS clients:
- [Damus](https://testflight.apple.com/join/CLwjLxWl) - Currently on TestFlight and the beta invitation status might be full. Will be available on AppStore soon. You will need to input your offline generated private key into the client.

### Nostr Android clients:
- [Amethyst](https://github.com/vitorpamplona/amethyst) - Still in early stages of development but has account management.

### Desktop OS clients:
- [Damus for MacOS](https://testflight.apple.com/join/CLwjLxWl) - Currently on TestFlight and the beta invitation status might be full. Will be available on AppStore soon. You will need to input your offline generated private key into the client.


## Nostr native Social clients (alpha/experimantal)

Listed below are native clients that are in experimental or alpha development status, and may or may not fully feature account management.

### Nostr iOS clients:
- [Daisy - In TestFlight](https://www.neb.lol/nostr)
- [Iris - In TestFlight](https://testflight.apple.com/join/5xdoDCmG) - A wrapper of the web client [iris.to](https://iris.to)


### Nostr Android clients:
- [Nostros](https://github.com/KoalaSat/nostros/)
- [Daisy](https://www.neb.lol/nostr)
- [Nostr_Console](https://github.com/vishalxl/nostr_console) - Can be run using termux and proot-distro on [Android](https://github.com/vishalxl/nostr_console/discussions/69)
- [Nostrid - multi platform](https://github.com/lapulpeta/Nostrid)
- [Nozzle](https://github.com/kaiwolfram/Nozzle)

### Desktop OS clients:
- [Bija - for multiple desktop OS and self-hosted to run on browser](https://github.com/BrightonBTC/bija)
- [Gossip - for multiple desktop OS - built using rust-lang](https://github.com/mikedilger/gossip)
- [Nostr_console - for multiple desktop OS - built using Dart](https://github.com/vishalxl/nostr_console): It can also be run on [web browser](https://github.com/vishalxl/nostr_console/discussions/18) to access on desktop or phone
- [Nostrid - multi platform](https://github.com/lapulpeta/Nostrid)
- [Blockcore-Notes](https://github.com/block-core/blockcore-notes) - Can build and run locally or can use their web [instance](https://notes.blockcore.net)


If you found this guide useful feel free to leave a Lightning tip at:

**Lightning Address**: ezofox@orangepill.dev, OR
**LNURL**: ![Tipjar](https://raw.githubusercontent.com/Sakhalinfox/orangepill.dev/main/Tiplnurl.png)
{: .notice--success}




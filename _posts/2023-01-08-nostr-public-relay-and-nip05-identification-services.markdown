---
layout: post
title:  "Nostr Public Relay and NIP05 identification services"
date:   2023-01-08 19:14:00 -0500
categories: announcements
---
Our public Nostr relay is located in Virginia, USA, freely accessible by adding the following websocket URL: **`wss://nostr.orangepill.dev`** to your favourite Nostr clients. The relay implementation that we use is [Nostream, previously called Nostr-ts-relay](https://github.com/Cameri/nostream).

To view details about supported relay's supported NIP's visit [Nostr.watch](https://nostr.watch/relay/nostr.orangepill.dev). 

>**Note**: Visiting the above Nostr.watch link will initially show the relay as offline. This is because of how Nostr.watch functions. It first needs to connect, ping and calculate average reponse time of all relays. To do this first visit https://nostr.watch/ and wait for it to connect and calculate average response time of our relay and othrs. Once the reponse time is displayed visit the [above link](https://nostr.watch/relay/nostr.orangepill.dev).

For monitoring the uptime status of our relay and any future Nostr, Bitcoin or Lightning⚡ services that we will run, visit our uptime monitoring page [here](https://uptime.orangepill.dev). Service status checks are done every 5 minutes, response times updated every 6 hours and graphs updated every day. Incidents are logged to our [GitHub repository](https://github.com/Sakhalinfox/orangepilldevuptime) and details about service issues and resolutions will be reported under the [Issue](https://github.com/Sakhalinfox/orangepilldevuptime/issues) topics.

We also offer a NIP05 identification service with the handle `<yourname>@orangepill.dev` (this is not an email address), which at the time of writing this post is free for anyone to apply for by reaching me out via direct message on your Nostr client:  
> Find me by my pubkey: **`npub16jzr7npgp2a684pasnkhjf9j2e7hc9n0teefskulqmf42cqmt4uqwszk52`** ([Snort.social profile URL](https://snort.social/p/npub16jzr7npgp2a684pasnkhjf9j2e7hc9n0teefskulqmf42cqmt4uqwszk52)) or my NIP05 Identifier: **`ezofox@orangepill.dev`**.

This service is currently free as we are willing to assist new users of Nostr to have a seamless onboarding experience and setup process for NIP05 identifier verification. In the near future we may add paid plans in which all our NIP05 verified users will have free access to our private relay when that is available.

#### If you found this post useful feel free to tip us at our below Lightning URL or Lightning Address: scarredsofa23@walletofsatoshi.com
![Tipjar](https://raw.githubusercontent.com/Sakhalinfox/orangepill.dev/main/Tiplnurl.png)





#NEBULOchat content

![Nebulo Logo](assets/img/nebulo.png)

## Main menu

* Home
* About
* Download
* How to make a Signaling Server
* Special Thanks

## Main content

# NEBULOchat + logo
## NEBULOchat

Welcome to the official website of the Chrome Extension "Nebulo".
You’ll find more information about Nebulo itself, and how to create your own signaling server.

## About
### What makes Nebulo special?
Nebulo isn't yet another IRC client. When you use it to chat with someone else, your messages aren’t sent to a centralized server but rather sent directly to the recipients, or at worst sent (but first encrypted ;) ) via a TURN server.
It's like a P2P system.

### Why use servers then?
The signaling server is only used to connect you to other people who visit the same webpage, and shares their IP address with you, so you can send directly your message to them.
In a sense, it acts the same way as a tracker does within the BitTorrent protocol.
The STUN/TURN servers are (for now...) require to prevent network blocking between users, due to NAT and some firewalls.
The STUN servers are only used to give you, you're public IP address. TURN Server are used, if the P2P connexion is impossible, and they works like relay between you and others.
If you want better explanation: [Read this from slide 44 to slide 52](http://io13webrtc.appspot.com/#44).

### How can you do that?
This P2P-like communication works simply by using the WebRTC technology! Natively supported by recent versions of Chrome, Firefox, Opera, ...
To be more specific, this extension makes use of the [RTCMultiConnection Library](http://www.rtcmulticonnection.org/).
As for the server, [websocket-over-nodejs](https://github.com/muaz-khan/WebRTC-Experiment/tree/master/websocket-over-nodejs) is used.

### Wait, you transmit my IP address?
Well, like all P2P system, it's required. But if you use the default signaling server or the one using the wss (WebSocketSecure) protocol, your IP address is hidden between signaling server and clients.
Only people using the extension have access to it.
Some other servers can occasionally get your IP (like the STUN Google server) to connect multiples peoples behind NAT or Firewall. But if you want to protect your IP, you can alway use a VPN, tor or other spoofing stuff.

### Can you see on which website I used this?
Nope, I chose to do this a way it can't be found on the signaling server, by generating a client-side (locally on your computer) a SHA256 hash (unique to every webpage).
Then, when many people send the same hash to the signaling server, the server knows they are visiting the same webpage, but it is unable to know what page it is.

### What if there is an infected network, evil server, or something recording what I said to someone?
This could happen if someone wants to record what you said on this network,
BUT it's IMPOSSIBLE without knowing the webpage you visit, or intercepting on your side the hash identifying this webpage.

I chose to use a powerful and fast cryptographic algorithm to encrypt everything you said with the hash locally. So before your message goes out of your computer, the algorithm is named [Anubis](http://en.wikipedia.org/wiki/Anubis_%28cipher%29). 

## Download
NebuloChat is available on the Chrome Webstore:
[Available on Google WebStore](https://chrome.google.com/webstore/category/apps)
You can also download Nebulochat as .zip (and use it as developper extension):
[Download Nebulchat 1.0.3 (minified)](nebulochat.com/Nebulo_1.0.3.min.zip) – no mirror at the moment.
[Download Nebulchat 1.0.3](nebulochat.com/Nebulo_1.0.3.zip) – no mirror at the moment.
This extension is released under the term of the [MIT License](http://opensource.org/licenses/MIT).

## How to make your own Signaling Server
You will need to had a Dedicated Server, or a VPS.
You can use any operating system on your server (if it support node.JS), but I will assume that you use a fully updated Linux distribution.
I recommend Debian or CentOS.

* ### I – Add Nodesource repos:
** On Debian (as root): **
    ``` 
    apt-get install curl
    curl -sL https://deb.nodesource.com/setup | bash - 
    ```
** On CentOS (as root): **
    ``` 
    curl -sL https://rpm.nodesource.com/setup | bash - 
    ```
* ### II- Install Node.JS, npm, and screen:
** On Debian (as root): **
    ```
    apt-get install -y nodejs build-essential screen
    ```
** On CentOS (as root): **
    ```
    yum install -y nodejs gcc-c++ make screen
    ```
* ### III- Install websocket-over-nodejs and his dependencies:
 * ` npm install node-static `
 * ` npm install websocket `
 * ` npm install websocket-over-nodejs `

* ### IV- Run your signaling server:
 * ` cd node_modules/websocket-over-nodejs/ `
 * ` screen -S "signalingServer" `
 * ` nodejs signaler.js `
 
Now press Ctrl+A then press D to detach screen.
CONGRATULATIONS, you just set up your own Signaling Server, compatible with Nebulo!

* ### V- (OPTIONAL) Post-install settings
` Use wss:// (secure protocol): `

If you want to use a securer protocol, like the official server, you will need a domain name (not free...) and a signed certificate (you can get this for free from StartSSL).
Then, you have to edit the file ssl.js from the websocket-over-nodejs folder. Edit line 19-20 to point to your certificate and your private key files.
You can now run your signaling server by using "nodejs ssl.js" instead of "nodejs signaler.js".

** Change the listening port: **

You can also change the listening port by editing signaler.js or ssl.js and change the line
app.listen(...);
at the end of the file to put whatever port number you would like between the brackets.
For example, if you want to use the port "22500", you would write "app.listen(22500);".

## Special Thanks

[KuroPekoe](http://kp-sama.deviantart.com/), who designed the cool Nebulo logo.
[Galidie Clément](http://clement-galidie.fr/), who designed the extension popup.

Copyright (c) 2014 nebulochat.com. All rights reserved.
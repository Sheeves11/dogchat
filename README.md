# dogchat

## secure p2p encrypted chat in a single html file

dogchat is a completely self-contained secure messaging application that runs entirely in your browser. it uses strong rsa-2048 encryption and true peer-to-peer connections through webrtc.

![image](https://github.com/user-attachments/assets/e8e327d5-69a0-4042-94e6-c7002983323e)


## why dogchat exists

there's a lot going on in the world right now and having extreme opsec options available to the general public is important. signal and other encrypted chat apps are great for most cases, but i always had a bit of distrust for them because i can't understand their large codebases.

dogchat is made to be simple enough for anyone to understand, audit, and deploy. it's a single, static html file with no external dependencies other than peerjs for webrtc connections.

## security features

- **rsa-2048 encryption** for all messages
- **true peer-to-peer connections** via webrtc
- **no servers store messages** - all communication is direct between peers
- **no message persistence** - messages are only available during the active session
- **in-memory key storage** - private keys are stored as javascript variables that are cleared when the page is closed
- **manual public key verification** - exchange public keys through a trusted channel for max security
- **in-browser encryption** - all messages are encrypted before they're ever sent out. and they're decrypted with a key unique to your session

## quick setup

1. install apache2 on your server
2. download the `dogchat.html` file from this repository and put it in your web folder!

### for maximum security:

- exchange public keys in person or via an already secure channel
- use tor or a vpn when connecting for additional anonymity. the signal paths WILL collect your IP address. potential attackers will know who you're talking to, just not what you're talking about
- know who you're chatting with

## technical details

### encryption

dogchat uses the web cryptography api's implementation of rsa-oaep with the following specifications:

- **algorithm**: rsa-oaep
- **key length**: 2048 bits
- **hash function**: sha-256

every message is encrypted specifically for its recipient using their public key.

### networking

- powered by [peerjs](https://peerjs.com/), which simplifies webrtc connections
- uses stun servers to establish peer connections (these servers help with nat traversal but cannot see message content)
- room names are used as peer ids for the host
- participants get randomly generated peer ids

### privacy considerations

- while messages are encrypted, metadata (connection times, ips, etc.) may be visible to network observers
- webrtc can leak your real ip address even when using a vpn
- for more complete anonymity, use tor browser with this application


## how it works

1. when you open the app, it generates a new rsa key pair directly in your browser
2. when creating a room, your public key is displayed for sharing
3. when others join, a peer-to-peer webrtc connection is established
4. during connection, participants exchange public keys
5. for each message sent:
   - the message is encrypted with the recipient's public key
   - only the intended recipient can decrypt it with their private key
   - the host relays messages between participants but cannot read them
6. when you close the tab, all keys and messages are permanently deleted

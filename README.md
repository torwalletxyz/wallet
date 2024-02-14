# Tor Wallet 

The private Ethereum wallet with built-in Tor.

[![Watch the video](https://i.imgur.com/yfMzGYf.png)](https://youtu.be/wXw17wIWcWU)

## Getting started

You can use Tor Wallet as an extension and on a website

### Using the website

- Click here https://wallet.torwallet.xyz

### Installing the extension

#### Chrome (permanent developer mode)

- Download and extract the `chrome.zip` file on the (https://github.com/torwalletxyz/wallet)
- Open Chrome, open settings
- Left panel, bottom, click `Extensions`
- Top bar, right, enable `Developer mode`
- Click `Load unpacked`
- Select the folder where `chrome.zip` was extracted

#### Firefox (temporary developer mode)

- Download and extract the `firefox.zip` file on the (https://github.com/torwalletxyz/wallet)
- Open Firefox, navigate to `about:debugging`
- Left panel, click `This Firefox`
- `Temporary Extensions`, click `Load Temporary Add-on`
- Navigate to the Tor Wallet folder
- Open the folder where `firefox.zip` was extracted
- Select the `manifest.json` file


## Secure by design

### Encrypted storage

Your storage is hashed and encrypted using strong cryptography algorithms and parameters

- Cryptography algorithms are seeded by PBKDF2 with 1M+ iterations from your password
- All storage keys are hashed using HMAC-SHA256, it is impossible to retrieve the original key
- All storage values are encrypted using AES-256-GCM, each with a different ciphertext/IV

### Authenticated storage

Some critical entities like private keys and seed phrases are stored in WebAuthn and require authentication (FaceID/TouchID)

- They are encrypted before being stored in WebAuthn storage
- Their reference ID and encryption IV are stored in encrypted storage (the one we talked above)

Nobody can access your private keys or seed phrases without your password + authentication (FaceID/TouchID)

This mitigates supply-chain attacks and phishing attacks, and prevents phone-left-on-the-table attacks


### Safe Tor and TLS protocols

We use the Tor and the TLS protocols in a way that's mostly safe, even though they are not fully implemented nor audited

Keep in mind that the zero risk doesn't exist, and a highly motivated attacker could deanonymize you by doing the following steps (very unlikely to happen):

1. Owning the entry node, and logging all IP addresses using Tor Wallet, something he could know by:
  - Deep packet inspection, since our Tor/TLS packets may not be like those from regular Tor/TLS users
  - Behaviour analysis, since our Tor/TLS may not behave the same way as regular Tor/TLS implementations

2. Owning the JSON-RPC server, and logging all wallet addresses that used Tor

(Or owning the exit node, since we don't currently check TLS certificates from the JSON-RPC server, the exit node could send your packets to its own JSON-RPC server (Fixed soon))

3. Correlating IP addresses logs with wallet addresses logs, and if both sides are small enough, linking a particular IP address to a particular wallet address

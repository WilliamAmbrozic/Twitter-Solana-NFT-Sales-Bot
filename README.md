# Twitter-Solana-Sales-Bot

## Preface

[[Back to contents]](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#contents)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This bot allows for custom tweet text and fast response times. The bot utilizes MagicEden's ```GET/collections/:symbol/activities``` API call, responding to sales allowing for NFT image uploading and metadata access through MagicEden's API. This bot is designed to run on an external server. **This bot was tested on an EC2 AWS instance and a [Raspberry Pi 4](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/)**. Usable on Unix based servers.

## Contents
- [Setup](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#Setup)  
  - [Installing](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#Installing-the-Bot) 
  - [Adding Credentials](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#Adding-Credentials)
  - [Running](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#Running-the-Bot)  
- [Config (Customize)](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#Config)
- [Free Use](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#Free-Use)
- [Find Me](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#find-me)
- [Tip Jar](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#Solana-Tip-Jar)

## Demo

[[Back to contents]](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#contents)

* Used by [BR1SALESBOT](https://twitter.com/BR1SALESBOT)

## Setup

[[Back to contents]](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#contents)

## Installing the Bot

[[Back to contents]](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#contents)

Clone and install Python requirements with
```
git clone https://github.com/WilliamAmbrozic/Twitter-Solana-NFT-Sales-Bot.git
cd Twitter-Solana-NFT-Sales-Bot
pip install -r requirements.txt
```

## Adding Credentials

[[Back to contents]](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#contents)

### 1. Add Your NFT MagicEden Collection Symbol to the Config.json (CASE SENSITIVE)

![shot 1](https://imgur.com/OnbyLbV.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A collection's symbol can be found by going to https://magiceden.io/ and searching for your collection's marketplace page (click the search bar). Look at the url and copy the text appearing directly after the last dash following "marketplace" as highlighted above. This text is your collection's symbol, this is needed for the bot to know what collection to tweet out. Paste this value in the **./config/config.json** attribute called **ME_symbol** as seen [here](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#Config).

### 2. Add your Twitter Developer Credentials to the Config.json

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A. Make a Twitter account and sign up to become a Twitter developer [here](https://developer.twitter.com/).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;B. Make sure your Twitter developer account has elevated access for image uploading [here](https://developer.twitter.com/en/portal/products/elevated).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;C. Make sure that the Access Token and Secret has been created with Read and Write permissions.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;D. Place your Twitter developer credentials in **./config/config.json** as seen [here](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#Config).

## Running the Bot

[[Back to contents]](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#contents)

Run with 
```
python3 bot.py
```
Or recommended with (if you do not want an ssh exit or general exit to halt the bot)
```
nohup python3 bot.py &
```

**Please give the bot a few minutes to recieve sales.**

## Config

[[Back to contents]](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#contents)

The config file **./config/config.json** will look something like this:
```
{
  "ME_symbol": "ADD_YOUR_SYMBOL_HERE",
  "twitter_credentials": {
    "bearer_token": "ADD_YOURS_HERE",
    "consumer_key": "ADD_YOURS_HERE",
    "consumer_secret": "ADD_YOURS_HERE",
    "access_token": "ADD_YOURS_HERE",
    "access_token_secret": "ADD_YOURS_HERE"
  },
  "tweet_text": "BOOM💥 [-n] just sold for [-p] ([-f])",
  "fiat_currency": "USD",
  "TPS": 2,
  "refresh_delay": 0.1,
  "activities_per_call": 1000,
  "use_img_on_chain": false
}
```

### Optional Attributes
**tweet_text**: You can customize the text that is tweeted out with the NFT image. Tweet text follows this notation (the bot will replace the following syntax with ...):
* [-n]: The NFT name (Ex. SolGod 0001)
* [-f]: The fiat price with respect to the fiat_currency value described below (Ex. $199.19 USD)
* [-p]: The sale price in Solana (Ex. 10 SOL)
* [-o]: The buyers wallet address
* [-m]: The NFT's mint address (This can easily be appended to a MagicEden item link to get the sale link as seen in the demo like so: https://magiceden.io/item-details/[-m])
* [-i]: The NFT image url (Not needed to display the NFT image)
* [-s]: The source of the sale (Ex. magiceden_v2)
* [-b]: The blockTime the transaction occured at (Ex. 21647306453)

Ex 1. (hypothetical data), 
```
BOOM💥 [-n] just sold for [-p] ([-f])
```
would become
```
BOOM💥 SolGod #1 just sold for 50.24 SOL ($5000.37 USD)
```
Ex 2. (hypothetical data),
```
💥 [-n] SOLD for [-p] SOL ([-f])\n\n→ https://magiceden.io/item-details/[-m]"
```
would become
```
💥 SolGod #1 SOLD for 50.24 SOL ($5000.37 USD)

→ https://magiceden.io/item-details/FBoEv5Xt4fKSwPhVPt5t3HXptByesbtDWnCzE6DYe4Ys
```

**fiat_currency**: The bot currently supports the following currencies: EUR, USD, CAD, JPY, GPB, AUD, CNY, INR. Change this value to change the currency in which Solana is converted if you choose to output fiat price.

**use_img_on_chain**: By default in case the NFT image is greater than 3mb (Twitter will not allow) the bot uses the compressed image file from MagicEden's CDN (```use_img_on_chain``` == False). Change ```use_img_on_chain``` to True if you would like to use the raw image stored on ipfs, arweave, etc. **Note: In the rare case that your raw NFT file sizes are greater than 3mb, they will have to be compressed prior to being uploaded.** Please see the comment <#compress here> in the send_tweet function - place your function that compresses the ```tmp``` file there. For standard image files there are many resources online for python compression. For gifs, [this gist](https://gist.github.com/skywodd/8b68bd9c7af048afcedcea3fb1807966) may be of help.


## Free Use

[[Back to contents]](https://github.com/WilliamAmbrozic/Responsive-Solana-NFT-Sales-Bot#contents)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This bot is 100% free to use. Use of this bot for your project is encouraged. **No credit is needed**. If you find this bot useful and insist on crediting the creator, you can add @williamambrozic in the bot's Twitter bio or have the bot follow @williamambrozic on Twitter.

## Find Me

- [williamambrozic.info](https://williamambrozic.info)
- [Twitter](https://twitter.com/WilliamAmbrozic)

## Solana Tip Jar
  * 39M6R5KP6NUX4KGATWaLGVaDrmLMpzbKAQU1q59rhHoT
### Bitcoin
  * bc1qa7vkam2w4cgw8njqx976ga5ns8egsq3yzxzlrt

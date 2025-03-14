# 🖨️ Telescan

Telescan is a Telegram Bot that allows you to use your Airscan compatible 
scanner from a conversation on Telegram. It will save your scans to a directory
of your choosing, and optionally send them to you through Telegram.

![](https://github.com/kn100/telescan/raw/master/demo.gif)

## Why?

There are projects out there designed for document indexing and archiving. One
such project is [Paperless-ngx](https://github.com/paperless-ngx/paperless-ngx)
it is a great project and I use it myself. The way it's supposed to be used is
you dump your scans into an import directory, and then Paperless-ngx will tag
and index them for you. I wanted a way to remove the use of a computer from the
process. Now, my process is to scan a document using Telescan, and it will
dump it into my Paperless-ngx import directory. Paperless-ngx will then do its
thing and I can search for the document later.

## Setup

1. Create a Telegram Bot using the [BotFather](https://telegram.me/BotFather).
2. Make sure your Telegram account has a username set.
2. Use the example Docker Compose file below to get started.
3. Start a chat with your new Telegram bot.

## Docker Compose

```yaml
version: '3.8'
services:
  telescan:
    build: .
    image: ghcr.io/kn100/telescan:latest
    # Host networking is essential since we need to be able to receive Bonjour 
    # broadcasts from the scanner. If you do not use host networking, you will
    # need to set up a Bonjour reflector on your network.
    network_mode: "host"
    container_name: telescan
    environment:
      # Get your API key from the BotFather on Telegram.
      TELEGRAM_API_KEY: your-api-key
      # Get your Telegram username from your Telegram profile. You may add more
      # than one username, separated by commas, if you wish to allow multiple
      # users to use your scanner. 
      AUTHORIZED_USERS: your-telegram-username,another-telegram-username,etc
      # Set to false to deny yourself the ability to ever receive scans through
      # Telegram. True if you want to be asked on each scan whether you want to
      # receive it or not. Note that Telegram is NOT a secure messaging platform
      # and you should not use it to send sensitive documents, if you do not 
      # trust Telegram.
      SEND_SCAN_TO_CHAT: true
      # Scanner name to insist on using. If you do not specify this variable,
      # the first scanner found will be used. There is usually no need to set 
      # this variable, unless you have multiple scanners and you want to use a
      # specific one.
      # SCANNER_OVERRIDE: your-scanner
    volumes:
      - "/tmp:/tmp"
      - "/final:/final"
```

## Running without Docker
```bash
TELEGRAM_API_KEY="<some-key>" \
AUTHORIZED_USERS="some-user" \
TMP_DIR="/tmp" \
FINAL_DIR="/somewhere" \
SEND_SCAN_TO_CHAT="false" \
go run main.go
```

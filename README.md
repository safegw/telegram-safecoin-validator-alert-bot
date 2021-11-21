# telegram-safecoin-validator-alert-bot
Send an alert to your own Telegram channel if your SafeCoin validator is delinquent
# Bash script to send message to [Telegram](https://web.telegram.org/) with [Telegram Bot](https://core.telegram.org/bots/api)
#### Create telegram bot

To send a message to Telegram group or channel, you should first create your own bot. Just open Telegram, find @BotFather, start a conversation with the bot and type /start. Then follow instructions to create bot and get ```token``` to access the HTTP API. 
1.  find @BotFather in Telegram
2.  start chat with the bot
3.  type ```/start``` to get the available commands
4.  type ```/newbot``` to create a new bot
5.  choose a name for your new bot
6.  choose a username for your bot, it must end with ```bot``` . This will be later on your link. Example: https://t.me/yournewcreatedbot/
7.  Done. You will get now your own ```token``` to access the HTTP API. Please keep it safe and secure. You will need it later for the bash script
#### Create a new Channel and add your already new created bot as member in Telegram
1.  create a new channel in Telegram
##### !! set the channel to public first. After the bot has been added and set as admin, you can set the channel back to private.
2.  add your bot as member
3.  provide admin access rights to your bot
4.  set your channel back to private
5.  In order to get ```channel ID``` post any message to the channel
6.  open following link to get your ```channel ID``` https://api.telegram.org/botYOURTOKEN/getUpdates . Replace YOURTOKEN with your new token
7.  example output. Write down your ```channel ID``` you will need it later on

```
{
  "ok":true,
  "result": [
    {
      "update_id":421,
      "channel_post": {
        "message_id":34,
        "chat": {
          "id":-8547443533, // this is your channel id !! it includes the - as well !! -> -8547443533
          "title":"Notifications",
          "type":"channel"
        },
        "date":1637486719,
        "text":"my fist channel message"
      }
    }
  ]
}
```

#### Script to send message
##### Here you will need your ```channel ID``` and the ```token``` 
1. Simple test ```curl 'https://api.telegram.org/botYourToken/sendMessage?chat_id=channel_id&text=test message'```
2. create the bash script. (Thanks to phael for the hint)
3. ```cd ~/```
4. ```nano telegram-notification.sh```
```
#!/bin/bash
#you can use here your Identity or Voteaccount
validator=AAFXHYrWu6gM8vL2y4gBQHavVFN7EqWY25SrBkwAxUnh
DELIQUENT=`~/Safecoin/target/release/./safecoin validators | grep $validator | grep '!' | wc -l`

if [ $DELIQUENT == 1 ];
    then
        echo "validator is deliquent!"

curl 'https://api.telegram.org/botYOURTOKEN/sendMessage?chat_id=CHANNELID&text='$validator is delinquent''

        # SEND MAIL OR DO OTHER NOTIFICATIONS HERE
    else
        echo "validator is running"
fi
```
5.  ```chmod +x telegram-notification.sh```
6.  test it with ```./telegram-notification.sh```
7.  you should get now a notification in your private Telegram channel

#### crontab setup for an automatic check in the background 
1.  create crontab to check regularly if your validator is running ```crontab -e```
##### assuming your script is inside your home folder```~/```
2.  add following line to your crontab ```* * * * * cd ~/ && ./telegram-notification.sh```
##### this will check every minute if your validator is running and if deliquent send a message to your private Telegram channel

#### Questions? Join SafeCoin Telegram https://t.me/joinchat/HCqlLxEEFR_SQPd9VjSpBw or SafeCoin Discord https://discord.gg/c6hWAkQ

### SafeCoin realtime monitoring https://safecoin.safegw.net/ 

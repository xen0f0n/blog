---
# toc: true
layout: post
keywords: telegram bot, heroku, python
title: Deploying a Telegram Bot on Heroku to auto-post stoic quotes in a Telegram Channel
comments: true
# categories: [markdown]
hide: false
---

For the last few months I've been following [@dailystoic](https://www.instagram.com/dailystoic/){:target="_blank"} and now, while doomscrolling, I get my daily stoic wisdom one-quote-at-a-time. More recently I read Marcus Aurelius's "Meditations". I can't really say that @dailystoic was the reason for buying the book, since it'd been in my wishlist for a long time, but it certainly gave me a *daily reminder. 

Being a telegram user, I've subscribed to a few channels like [linuxgram](https://t.me/linuxgram){:target="_blank"} and [showerthoughts](https://t.me/showerthoughts){:target="_blank"}. Motivated by my recent interest in stoicism I decided as a tiny-project to try and write a simple telegram bot and use it to auto-post stoic quotes in a channel on telegram. 

## Create your telegram bot (and channel)

![]({{ site.baseurl }}/images/bot-to-autopost-in-a-telegram-channel/BotFather_1.png)

[Documentation](https://core.telegram.org/bots){:target="_blank"} is simple and clear: 
- "Just talk to [BotFather](https://t.me/botfather){:target="_blank"}"
- "Use the /newbot command to create a new bot. The BotFather will ask you for a name and username, then generate an authorization token for your new bot."
- "The token is a string along the lines of ```110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsa``` that is required to authorize the bot and send requests to the Bot API. Keep your token secure and store it safely, it can be used by anyone to control your bot."

A channel is easily created within the telegram app. A unique id defines the channel (I guess it's just its name - for me: *[@stoic_quotes](https://t.me/stoic_quotes){:target="_blank"}*)

Our bot needs admin rights in order to be able to post to the channel. So, we have to add it to the channel and make it an admin.


## Code

(Presented code can be found [here](https://github.com/xen0f0n/stoic-quotes-telegram-bot){:target="_blank"}.)

Quotes are stored in a json file in the following format and a single quote is selected randomly each time.

```json
{
  "quotes": [
    {
      "text": "If anyone tells you that a certain person speaks ill of you, do not make excuses about what is said of you but answer, \"He was ignorant of my other faults, else he would not have mentioned these alone.\"",
      "author": "Epictetus"
    },
    {
      "text": "Wealth consists not in having great possessions, but in having few wants.",
      "author": "Epictetus"
    },
    {
      "text": "There is only one way to happiness and that is to cease worrying about things which are beyond the power or our will.",
      "author": "Epictetus"
    }
    ...
```
    
```python
import json
from random import randint


def get_quote():
    with open('quotes.json') as f:
        quotes = json.load(f)['quotes']
        rand_index = randint(0, len(quotes) - 1)

        return list(quotes[rand_index].values()) # [quote, author]
```

I used two libraries:
- [python-telegram-bot](https://github.com/python-telegram-bot/python-telegram-bot){:target="_blank"} and
- [scheduler](https://github.com/dbader/schedule){:target="_blank"} (created by [danBader](https://realpython.com/team/dbader/){:target="_blank"})

In order to keep the bot's token secret (and not have it publicly on github), we can access it through an environment variable (*config vars* on Heroku).

There is no interaction with the bot through commands (`/start`, `/help`, etc). Its only purpose is to automate posting in our channel.


```python
from telegram import Bot
import schedule
import os

from utils import get_quote


def post_quote():

    # get bot and channel ids from env variables
    bot_token = os.environ.get('BOT_TOKEN')
    channel_id = os.environ.get('CHANNEL_ID')

    quote, author = get_quote()
    message = f'{quote}\n\n{author}' # format quote

    bot = Bot(token=bot_token)
    bot.sendMessage(chat_id=channel_id, text=message)


if __name__ == '__main__':

    schedule.every(8).hours.do(post_quote).tag('stoic-quote')

    while 1:
        schedule.run_pending()
```

:exclamation: You will have to create a github repo and push the code there in order to connect it directly to Heroku later. 

## Deploy on Heroku

(For this step you will need a [Heroku](https://www.heroku.com/){:target="_blank"} account)

### Create a new Heroku app 

Once logged in, select *New --> Create new app* and name it. After that, select GitHub as the *Deployment method*, and search for the target github repo.

Heroku needs a couple of things included to our repo in order to work:
- runtime.txt
- requirements.txt
- Procfile

```runtime.txt``` is optional and it's used to specify the python runtime. In our case ```python-3.6.9```. Otherwise Heroku resorts to a default runtime.

```requirements.txt``` includes our external python libraries ('dynos' aka Heroku containers need to know what to install)

```Procfile``` is used to execute a command on dyno's startup. In our case we need to run scheduler.py, Procfile contains the line:
```bash
worker: python scheduler.py
```
(*worker* is one of the available [process types](https://devcenter.heroku.com/articles/procfile){:target="_blank"})

Having done all that and pushed into our github repo, there are a couple of things left.

- Remember our secret token? We are going to store it using Heroku's [Config Vars](https://devcenter.heroku.com/articles/config-vars){:target="_blank"} (under *Settings*) which "are exposed to our app's code as environment variables". 

![]({{ site.baseurl }}/images/bot-to-autopost-in-a-telegram-channel/Heroku_1.png)

- On the *Deploy* tab, we choose the branch to deploy and whether we prefer Automatic Deploys (auto deploy every time something is pushed into that branch).

:warning: Finaly, make sure you change the worker status to <b style="color:green">ON</b>. (under the *Overview* tab). Otherwise, the worker in our Procfile will never run our script.  

---

:octocat: [Github repo](https://github.com/xen0f0n/stoic-quotes-telegram-bot){:target="_blank"}  
[Stoic Quotes](https://t.me/stoic_quotes){:target="_blank"} Telegram Channel 


> You have power over your mind - not outside events. Realize this, and you will find strength`   
>~ Marcus Aurelius